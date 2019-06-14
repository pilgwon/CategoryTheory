# Kleisli 카테고리
원본 사이트: [Kleisli Categories](https://bartoszmilewski.com/2014/12/23/kleisli-categories/)

---

> 이 글은 프로그래머를 위한 카테고리 이론이라는 책의 일부입니다. 이전 글은 [다양한 종류의 카테고리](/contents/part1/categories-great-and-small.md) 입니다. 목차는 [여기서](/README.md) 확인하실 수 있습니다.

## 로그의 합성

타입과 순수 함수를 카테고리로 모델링하는 것과 반대로 사이드 이펙트를 가지거나 순수하지 않은 함수를 모델링하는 방법은 앞에서 알아보았습니다. 오늘은 함수를 실행한 기록을 로깅하는 일에 대해 얘기해보겠습니다. 명령형 언어였다면 다음과 같이 전역 변수를 건드리는 방식으로 구현했을 것입니다.

```
string logger;

bool negate(bool b) {
     logger += "Not so! ";
     return !b;
}
```

예시의 함수는 *사이드 이펙트*를 가지고 있기 때문에 순수 함수가 아닙니다.

복잡한 동시성(concurrency)이 특징인 현대적 프로그래밍에선 전역 변수를 사용하는 일은 피하는 것이 좋습니다. 그러니 이런 코드를 라이브러리에 추가하지 않는 방법에 대해 얘기해보도록 하겠습니다.

이 함수를 순수 함수로 만들 수 있는 저희는 행운아입니다. 저흰 그저 로그만 넘기면 됩니다. 그러면 문자열을 인자로 받고 업데이트된 로그를 포함하는 문자열을 출력하는 함수를 만들어보겠습니다.

```
pair<bool, string> negate(bool b, string logger) {
     return make_pair(!b, logger + "Not so! ");
}
```

위의 함수는 순수 함수이며 사이드 이펙트도 가지고 있지 않습니다. 게다가 같은 인자를 입력하면 항상 같은 결과를 출력하며, 필요하다면 메모이제이션할 수도 있습니다. 하지만 누적되는 것이 운명인 로그를 저장한다면 아마 모든 경우의 수에 대해 다 기억해둬야 할 것입니다.

```
negate(true, "It was the best of times. ");
```
를 반복해서 사용하는 경우까지도요.

이 방식 또한 라이브러리에 들어갈 함수에 어울리는 인터페이스가 아닙니다. 사용자는 호출한 결과를 무시할 수 있으니 로그를 매번 계산하는 것은 큰 부담이라고 할 순 없습니다. 오히려 불편할 수 있는 부분은 항상 문자열을 입력해야한다는 것입니다.

사용자를 방해하지 않을 방법은 없을까요? 관심사를 분리할 수 있는 방법은 존재하지 않는걸까요? 앞의 예제에선 negate 함수의 가장 주된 목적은 불리언 값을 하나 받으면 그 반대값을 반환하는 것입니다. 로깅은 그 다음 일입니다. 그러니 로깅되는 메세지는 함수에 의존성을 가질지라도, 하나의 로그에 메세지를 쌓는 일은 분리되어야 할 고민이라는 것입니다. 정리하자면 우리에게 필요한 것은 입력할 부담은 없지만 입력된 값에 맞는 로그를 생성하는 함수입니다. 다음과 같이 작성하면 될 것 같습니다.

```pair<bool, string> negate(bool b) {
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
2. 결과로 반환된 쌍의 첫 번째 값을 추출해서 두 번째 사상에 대응하는 embellish된 함수에 넣어줍니다.
3. 첫번째 결과의 두 번째 값과 두 번째 결과의 두 번째 값을 합칩니다.
4. 합친 문자열과 최종 결과의 첫 번째 값을 쌍으로 만들어서 반환합니다.

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

그러면 그 템플릿 방식으로 앞에서 다뤘던 `toUpper`와 `toWords`의 합성을 구현해보겠습니다.

```
Writer<vector<string>> process(string s) {
   return compose<string, string, vector<string>>(toUpper, toWords)(s);
}
```

위 방식은 `compose` 템플릿의 타입에 넘기기엔 여전히 좋지 않습니다. 반환 값을 추론할 수 있는 일반화된 람다 함수를 가진 C++ 14이상의 컴파일러가 있다면 이런 상황을 피할 수 있습니다. (이 부분도 저번 글에 이어 Eric Niebler의 작품입니다)

```
auto const compose = [](auto m1, auto m2) {
    return [m1, m2](auto x) {
        auto p1 = m1(x);
        auto p2 = m2(p1.first);
        return make_pair(p2.first, p1.second + p2.second);
    };
};
```

새롭게 정의한 부분에서 `process`의 구현체를 간단하게 설명하면 다음과 같습니다.

```
Writer<vector<string>> process(string s){
   return compose(toUpper, toWords)(s);
}
```

아직 끝이 아닙니다. 새로운 카테고리의 합성을 정의했는데 단위 사상은 무엇일까요? 보통의 단위 함수는 아닐 것입니다. 단위 사상에는 다음과 같이 표현되는 타입 A에서 타입 A로 돌아오는 사상이 필요하기 때문입니다.

```
Writer<A> identity(A);
```

단위 사상은 합성에서 항등원과 같이 작동해야 합니다. 합성의 정의를 보셨다면 단위 사상은 그들의 인자를 아무런 변화없이 넘겨야 하고 로그에는 빈 스트링을 남겨야한다는 것을 아셨을 것입니다.

```
template<class A>
Writer<A> identity(A x) {
    return make_pair(x, "");
}
```

저희가 정의한 카테고리가 타당하다는 것을 이해하기는 어렵지 않을 것입니다. 특히 우리의 합성은 결합법칙을 가지죠. 각 쌍의 첫 번째 컴포넌트에 어떤 일이 생기는지 따라가보면 아시겠지만 결합법칙이 성립하는 평범한 함수 합성입니다.

영리하신 분이라면 이러한 구조를 문자열 모노이드가 아닌 다른 어떤 모노이드로도 일반화할 수 있다는 사실을 깨달으셨을 것입니다. `+`와 `""`대신에 `compose`안의 `mappend`와 `identity`안의 `mempty`를 사용할 것입니다. 그렇게 되면 우리의 로깅이 단순한 문자열만으로 제한될 이유가 사라집니다. 좋은 라이브러리 제작자라면 라이브러리가 작동하는데에 방해되는 요소는 최대한 없애야합니다. 이제 우리의 로깅 라이브러리가 요구하는 조건은 그저 모노이드스러운 속성뿐입니다.

## Haskell의 Writer

Haskell에서도 비슷한 것이 있는데 앞에서 알아본 것보다 더 간결한 편이고 컴파일러가 주는 도움이 더 많아집니다. `Writer` 타입을 정의하는 것부터 시작해보겠습니다.

```
type Writer a = (a, String)
```

위와 같이 C++의 `typedef`(혹은 `using`)과 동일한 타입 얼라이어스를 정의했습니다. `Writer` 타입은 타입 변수 `a`로 파라미터화되었고 `a`와 `String`의 쌍과 동일합니다. 쌍을 위한 문법도 간편합니다. 그저 두 아이템을 괄호안에 넣고 쉼표로 구분해주면 됩니다.

임의의 타입에서 어떤 `Writer` 타입으로 이어지는 사상은 함수로 표현합니다.

```
a -> Writer b
```

합성을 정의할 때는 웃긴 이항 연산자(infix operator)를 사용할건데 보통 "fish 연산자"라고 불립니다.

```
(>=>) :: (a -> Writer b) -> (b -> Writer c) -> (a -> Writer c)
```

이 연산자는 두 함수를 인자로 받아서 함수를 반환합니다. 첫 번째 인자는 `(a->Writer b)`이고, 두 번째는 `(b->Writer c)`, 그리고 결과는 `(a->Writer c)`입니다.

다음은 이항 연산자의 정의입니다. 물고기 기호의 양 끝에 있는 `m1`과 `m2`는 인자로 이해하시면 됩니다.

```
m1 >=> m2 = \x ->
    let (y, s1) = m1 x
        (z, s2) = m2 y
    in (z, s1 ++ s2)
```

결과는 `x` 인자의 람다 함수입니다. 람다는 백슬래쉬로 적는데, 그리스 문자인 λ의 한 쪽 다리를 잘랐다고 생각하시면 됩니다.

`let`은 우리가 보조 변수를 선언할 수 있게 해줍니다. 여기서 `m1`을 호출한 결과는 `(y, s1)`에 해당하고, `m2`의 결과는 첫 번째 패턴의 `y`를 인자로 받아서 `(z, s2)`에 대응합니다.

C+에서 악세서리를 사용했던 것과는 다르게 Haskell에선 쌍을 패턴 매칭하는 방식이 일반적입니다. 그 외에도 C++과 Haskell에는 통하는 부분이 있습니다.

`let` 표현안에 있는 모든 값은 `in` 절에서 사용할 수 있습니다. 여기서는 쌍의 첫 번째 컴포넌트가 `z`이고 두 번째 컴포넌트는 문자열을 합친 값인 `s1++s2` 입니다.

추가적으로 단위 사상을 정의해보면 다음과 같을 것입니다. 그리고 나중을 위해 이름은 `return`으로 짓겠습니다.

```
return :: a -> Writer a
return x = (x, "")
```

완성도를 위해 Haskell 버전의 embellish된 함수인 `upCase`와 `toWords`를 작성해보겠습니다.

```
upCase :: String -> Writer String
upCase s = (map toUpper s, "upCase ")

toWords :: String -> Writer [String]
toWords s = (words s, "toWords ")
```

`map` 함수는 C++의 `transform`과 같다고 생각하시면 됩니다. 이 함수는 문자열 `s`를 `toUpper` 함수에 넣어주는 역할을 합니다. 보조 함수인 `words`는 표준 Prelude 라이브러리에 정의돼있습니다.

마지막으로 두 함수의 합성은 물고기 연산자의 도움을 받아 다음과 같이 작성할 수 있습니다.

```
process :: String -> Writer [String]
process = upCase >=> toWords
```

## Kleisli 카테고리

이 카테고리를 여기서 바로 만들었을거라고 생각하시는 분은 없을 것입니다. 앞에서 설명한 내용은 모나드에 기반한 카테고리인 Kleisli 카테고리의 예시입니다. 저희는 아직 모나드에 대해 토론할 준비가 되진 않았지만, 저는 여러분에게 그들이 무엇을 할 수 있는지 살짝 맛을 보여드리고 싶었습니다.
Kleisli 카테고리는 주어진 프로그래밍 언어의 타입을 제한된 목적의 객체로서 가지고 있습니다. 타입 A에서 타입 B로 가는 사상은 특정 embellishment를 사용해서 타입 A에서 타입 B로부터 나온 어떤 타입으로 가는 함수가 될 수 있습니다. 각 Kleisli 카테고리는 어떤 사상들을 합성하는 각자의 방식이 정의되어 있으며, 단위 사상또한 그 합성 방식을 따릅니다. (애매모호한 표현인 "embellishment"는 추후에 카테고리의 endofunctor라는 개념으로 대응될 것입니다)

이 글에서 카테고리의 기초로 사용했던 모나드는 Writer 모나드라 부르며 함수의 실행이나 로깅에 사용되었습니다. 또한 순수 계산에 이펙트를 붙이는 일반적인 상황에서 많이 사용됩니다. 프로그래밍 언어의 타입과 함수를 집합의 카테고리로 모델링할 수 있다는 것은 이전에도 보셨을 것입니다. 오늘은 그러한 모델을 사상이 embellish된 함수로 표현되고 합성이 입력된 값을 다른 곳으로 보내는 것 이상의 일을 하는 약간은 다른 카테고리로 확장시켜보았습니다. 그리고 우리는 전통적인 명령형 언어에선 사이드 이펙트를 사용해야 했던 내용을 프로그램에 간단한 표시를 넘기는 것만으로 가능하게 하는 한 단계 높은 차원의 작업을 할 수 있게 되었습니다.

다음: [Product와 Coproduct](/contents/part1/product-and-coproduct.md)

---

공부 목적으로 번역을 하고 있습니다! 잘못된 점에 대한 이슈나 PR은 언제든지 환영합니다 :)
