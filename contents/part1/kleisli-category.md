# Kleisli 카테고리
원본 사이트: [Kleisli Categories](https://bartoszmilewski.com/2014/12/23/kleisli-categories/)

---

> 이 글은 프로그래머를 위한 카테고리 이론이라는 책의 일부입니다. 이전 글은 [다양한 종류의 카테고리](/contents/part1/categories-great-and-small.md) 입니다. 목차는 [여기서](/README.md) 확인하실 수 있습니다.

## 로그의 합성

타입과 순수 함수를 카테고리로 모델링하는 것은 앞에서 보셨을 것입니다. 또한 사이드 이펙트나 순수하지 않은 함수를 모델링하는 방법에 대해서도 언급했었습니다. 실행(execution) 로그를 추적하는 함수를 예로 들어보겠습니다. 명령형 언어에선 다음과 같이 어떤 전역 변수를 변경하는 방식으로 구현할 것입니다.

```
string logger;

bool negate(bool b) {
     logger += "Not so! ";
     return !b;
}
```

이 함수는 *사이드 이펙트* 를 가지고 있기 때문에 순수한 함수가 아님을 아실 것입니다.

전역 변수를 사용하는 일은 복잡한 동시성(concurrency)이 특징인 모던 프로그래밍에선 피해야 할 사항입니다. 그러니 저런 코드를 라이브러리에 추가하는 것은 절대 하면 안될 일입니다.

이 함수를 순수하게 만들 수 있는 저희는 행운아입니다. 저흰 그저 로그만 넘기면 됩니다. 그러면 문자열을 인자로 받고 업데이트된 로그를 포함하는 문자열을 출력하는 함수를 만들어보겠습니다.

```
pair<bool, string> negate(bool b, string logger) {
     return make_pair(!b, logger + "Not so! ");
}
```

이 함수는 순수하고 사이드 이펙트도 존재하지 않습니다. 이 함수는 같은 인자를 입력하면 항상 같은 결과를 출력합니다. 필요하다면 memoize할 수도 있습니다. 하지만 누적되는 것이 운명인 로그를 memoize한다면 아마 모든 경우의 수에 대해 다 memoize해야 할 것입니다.

```
negate(true, "It was the best of times. ");
```
가 반복되는 경우까지도요.

그것은 또한 라이브러리 함수에게 어울리는 인터페이스는 아닙니다. 사용자는 호출한 결과를 무시할 수 있으니 로그를 매번 계산하는 것은 큰 부담이 아닙니다. 오히려 불편할 수 있는 부분은 항상 문자열을 입력해야한다는 것입니다.

사용자를 덜 방해할 방법은 없을까요? 관심사를 분리할 수 있는 방법은 없을까요? 위의 간단한 예제에선 negate 함수의 가장 주된 목적은 하나의 불리언 값을 뒤집는 것입니다. 로깅은 그 다음 일입니다. 그러니 로깅되는 메세지는 함수에 의존성을 가질지라도, 하나의 로그에 메세지를 쌓는 일은 분리되어야 할 고민이라는 것입니다. 정리하자면 우리에게 필요한 것은 입력할 부담은 없지만 입력된 값에 맞는 로그를 생성하는 함수입니다. 다음과 같이 작성하면 되겠네요.

```
pair<bool, string> negate(bool b) {
     return make_pair(!b, "Not so! ");
}
```

이는 함수의 호출과 다음 호출 사이에 로그가 쌓일거라는 아이디어에서 나왔습니다.

조금 더 현실적인 예시를 들어볼까요? 소문자로 이루어진 문자열을 대문자로 이루어진 문자열로 바꿔주는 함수가 있다고 해봅시다.

```
string toUpper(string s) {
    string result;
    int (*toupperp)(int) = &toupper; // toupper is overloaded
    transform(begin(s), end(s), back_inserter(result), toupperp);
    return result;
}
```

그리고 다른 하나는 문자열의 공백을 없애서 단어로 분리하는 함수가 있습니다.

```
vector<string> toWords(string s) {
    return words(s);
}
```

실제 작업은 보조 함수인 words가 해줍니다.

```
vector<string> words(string s) {
    vector<string> result{""};
    for (auto i = begin(s); i != end(s); ++i)
    {
        if (isspace(*i))
            result.push_back("");
        else
            result.back() += *i;
    }
    return result;
}
```

이제 `toUpper`와 `toWords`를 수정해서 결과값과 메세지 문자열을 동시에 가지도록 해보겠습니다.

저희는 이 함수의 결과값을 "embellished"할 것입니다. `Writer` 템플릿을 정의해서 제네릭한 방식으로 쌍으로 압축하겠습니다. 쌍은 먼저 임의의 타입인 A와 문자열로 이루어져 있습니다.

```
template<class A>
using Writer = pair<A, string>;
```

다음은 embellished 함수입니다.

```
Writer<string> toUpper(string s) {
    string result;
    int (*toupperp)(int) = &toupper;
    transform(begin(s), end(s), back_inserter(result), toupperp);
    return make_pair(result, "toUpper ");
}

Writer<vector<string>> toWords(string s) {
    return make_pair(words(s), "toWords ");
}
```

이제 이 두 함수를 합성해서 문자열을 대문자로 만들고 단어로 나누는 embellished 함수로 만들어봅시다. 그리고 모든 행동은 로그를 남겨야겠죠? 다음과 같이 만들면 되겠네요.

```
Writer<vector<string>> process(string s) {
    auto p1 = toUpper(s);
    auto p2 = toWords(p1.first);
    return make_pair(p2.first, p1.second + p2.second);
}
```

목표를 달성했습니다. 로그가 쌓이는 것은 더 이상 개별 함수의 관심사가 아닙니다. 그들은 외부에서 자신만의 메세지를 생성하고 더 큰 로그에 합칩니다.

그럼 이제 모든 함수가 이러한 방식으로 구현된 프로그램을 상상해볼까요? 그건 아마 지루하고 반복적이며 오류 발생이 쉬운 악몽이 될 것입니다. 우리 프로그래머들은 이런 문제를 어떻게 파훼해야 할 지 알고있죠? 바로 추상화입니다! 하지만 함수를 쪼개서 추상화하는 방법말고 함수 합성 자체를 추상화해야 합니다. 하지만 합성은 카테고리 이론의 정수이기 때문에 코드를 작성하기 전에 카테고리적인 시각으로 문제를 분석해보겠습니다.

## Writer 카테고리

추가적인 기능을 묶어두기 위해 함수들의 반환 타입을 embellish하는 아이디어는 굉장히 유익합니다. 예시를 보겠습니다. 기본적인 타입과 함수의 카테고리에서 시작해볼까요? 타입을 객체로 두되, 사상을 embellish된 함수라고 재정의합니다.

`int`를 받아서 `bool`을 반환하는 함수인 `isEven`을 embellish하는 것을 생각해보겠습니다. 우리는 이를 사상으로 바꿔서 embellish된 함수로 표현해보겠습니다. 중요한 점은 이 사상은 embellish된 함수가 쌍을 반환하더라도 여전히 `int`와 `bool`객체 사이를 이어주고 있다는 점입니다.

```
pair<bool, string> isEven(int n) {
     return make_pair(n % 2 == 0, "isEven ");
}
```

카테고리의 법칙에 의하면 우리는 이 사상을 `bool`에서 시작하는 다른 어떤 사상과 합성할 수 있어야 합니다. 앞에서 다뤘던 `negate`도 대상이 될 수 있겠네요.

```
pair<bool, string> negate(bool b) {
     return make_pair(!b, "Not so! ");
}
```

한 가지 명확한 것은 입력과 출력의 모양이 다르기 때문에 지금까지 해왔던 보통의 함수 합성 방식으로는 두 사상을 합성할 수 없다는 것입니다. 둘을 합성하는 함수를 만들면 다음과 같을 것입니다.

```
pair<bool, string> isOdd(int n) {
    pair<bool, string> p1 = isEven(n);
    pair<bool, string> p2 = negate(p1.first);
    return make_pair(p2.first, p1.second + p2.second);
}
```

여기 두 사상을 합칠 때 새로운 카테고리를 만들어서 합성하는 레시피를 공개합니다.

1. 첫 번째 사상에 대응하는 embellish된 함수를 작성합니다.
1. 결과로 반환된 쌍의 첫 번째 값을 추출해서 두 번째 사상에 대응하는 embellish된 함수에 넣어줍니다.
1. 첫번째 결과의 두 번째 값과 두 번째 결과의 두 번째 값을 합칩니다.
1. 합친 문자열과 최종 결과의 첫 번째 값을 쌍으로 만들어서 반환합니다.

이 합성을 C++에서 고계함수로 추상화하고 싶다면 카테고리의 세 개의 값에 대응하는 세 개의 타입을 인자로 받는 템플릿을 사용해야 합니다. 이 템플릿은 규칙에 맞고 세 번째 embellish된 함수를 만들 수 있는 두 개의 embellish된 함수를 받습니다.

```
template<class A, class B, class C>
function<Writer<C>(A)> compose(function<Writer<B>(A)> m1,
                               function<Writer<C>(B)> m2)
{
    return [m1, m2](A x) {
        auto p1 = m1(x);
        auto p2 = m2(p1.first);
        return make_pair(p2.first, p1.second + p2.second);
    };
}
```

Now we can go back to our earlier example and implement the composition of `toUpper` and `toWords` using this new template:

```
Writer<vector<string>> process(string s) {
   return compose<string, string, vector<string>>(toUpper, toWords)(s);
}
```

There is still a lot of noise with the passing of types to the `compose` template. This can be avoided as long as you have a C++14-compliant compiler that supports generalized lambda functions with return type deduction (credit for this code goes to Eric Niebler):

```
auto const compose = [](auto m1, auto m2) {
    return [m1, m2](auto x) {
        auto p1 = m1(x);
        auto p2 = m2(p1.first);
        return make_pair(p2.first, p1.second + p2.second);
    };
};
```

In this new definition, the implementation of `process` simplifies to:

```
Writer<vector<string>> process(string s){
   return compose(toUpper, toWords)(s);
}
```

But we are not finished yet. We have defined composition in our new category, but what are the identity morphisms? These are not our regular identity functions! They have to be morphisms from type A back to type A, which means they are embellished functions of the form:

```
Writer<A> identity(A);
```

They have to behave like units with respect to composition. If you look at our definition of composition, you’ll see that an identity morphism should pass its argument without change, and only contribute an empty string to the log:

```
template<class A>
Writer<A> identity(A x) {
    return make_pair(x, "");
}
```

You can easily convince yourself that the category we have just defined is indeed a legitimate category. In particular, our composition is trivially associative. If you follow what’s happening with the first component of each pair, it’s just a regular function composition, which is associative. The second components are being concatenated, and concatenation is also associative.

An astute reader may notice that it would be easy to generalize this construction to any monoid, not just the string monoid. We would use `mappend` inside `compose` and `mempty` inside `identity` (in place of `+` and `""`). There really is no reason to limit ourselves to logging just strings. A good library writer should be able to identify the bare minimum of constraints that make the library work — here the logging library’s only requirement is that the log have monoidal properties.

## Writer in Haskell

The same thing in Haskell is a little more terse, and we also get a lot more help from the compiler. Let’s start by defining the `Writer` type:

type Writer a = (a, String)
Here I’m just defining a type alias, an equivalent of a `typedef` (or `using`) in C++. The type `Writer` is parameterized by a type variable `a` and is equivalent to a pair of `a` and `String`. The syntax for pairs is minimal: just two items in parentheses, separated by a comma.

Our morphisms are functions from an arbitrary type to some `Writer` type:

```
a -> Writer b
```

We’ll declare the composition as a funny infix operator, sometimes called the “fish”:

```
(>=>) :: (a -> Writer b) -> (b -> Writer c) -> (a -> Writer c)
```

It’s a function of two arguments, each being a function on its own, and returning a function. The first argument is of the type `(a->Writer b)`, the second is `(b->Writer c)`, and the result is `(a->Writer c)`.

Here’s the definition of this infix operator — the two arguments `m1` and `m2` appearing on either side of the fishy symbol:

```
m1 >=> m2 = \x ->
    let (y, s1) = m1 x
        (z, s2) = m2 y
    in (z, s1 ++ s2)
```

The result is a lambda function of one argument `x`. The lambda is written as a backslash — think of it as the Greek letter λ with an amputated leg.

The `let` expression lets you declare auxiliary variables. Here the result of the call to `m1` is pattern matched to a pair of variables `(y, s1)`; and the result of the call to `m2`, with the argument `y` from the first pattern, is matched to `(z, s2)`.

It is common in Haskell to pattern match pairs, rather than use accessors, as we did in C++. Other than that there is a pretty straightforward correspondence between the two implementations.

The overall value of the `let` expression is specified in its `in` clause: here it’s a pair whose first component is `z` and the second component is the concatenation of two strings, `s1++s2`.

I will also define the identity morphism for our category, but for reasons that will become clear much later, I will call it `return`.

```
return :: a -> Writer a
return x = (x, "")
```

For completeness, let’s have the Haskell versions of the embellished functions `upCase` and `toWords`:

```
upCase :: String -> Writer String
upCase s = (map toUpper s, "upCase ")

toWords :: String -> Writer [String]
toWords s = (words s, "toWords ")
```

The function `map` corresponds to the C++ `transform`. It applies the character function `toUpper` to the string `s`. The auxiliary function `words` is defined in the standard Prelude library.

Finally, the composition of the two functions is accomplished with the help of the fish operator:

```
process :: String -> Writer [String]
process = upCase >=> toWords
```

## Kleisli Categories

You might have guessed that I haven’t invented this category on the spot. It’s an example of the so called Kleisli category — a category based on a monad. We are not ready to discuss monads yet, but I wanted to give you a taste of what they can do. For our limited purposes, a Kleisli category has, as objects, the types of the underlying programming language. Morphisms from type A to type B are functions that go from A to a type derived from B using the particular embellishment. Each Kleisli category defines its own way of composing such morphisms, as well as the identity morphisms with respect to that composition. (Later we’ll see that the imprecise term “embellishment” corresponds to the notion of an endofunctor in a category.)

The particular monad that I used as the basis of the category in this post is called the writer monad and it’s used for logging or tracing the execution of functions. It’s also an example of a more general mechanism for embedding effects in pure computations. You’ve seen previously that we could model programming-language types and functions in the category of sets (disregarding bottoms, as usual). Here we have extended this model to a slightly different category, a category where morphisms are represented by embellished functions, and their composition does more than just pass the output of one function to the input of another. We have one more degree of freedom to play with: the composition itself. It turns out that this is exactly the degree of freedom which makes it possible to give simple denotational semantics to programs that in imperative languages are traditionally implemented using side effects.

## Acknowledgments

I’m grateful to Eric Niebler for reading the draft and providing the clever implementation of `compose` that uses advanced features of C++14 to drive type inference. I was able to cut the whole section of old fashioned template magic that did the same thing using type traits. Good riddance! I’m also grateful to Gershom Bazerman for useful comments that helped me clarify some important points.

다음: [Product와 Coproduct](/contents/part1/product-and-coproduct.md)

---

공부 목적으로 번역을 하고 있습니다! 잘못된 점에 대한 이슈나 PR은 언제든지 환영합니다 :)
