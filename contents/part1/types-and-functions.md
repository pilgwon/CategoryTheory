# 타입과 함수
원본 사이트: [Types and Functions](https://bartoszmilewski.com/2014/11/24/types-and-functions/)

---

> 이 글은 프로그래머를 위한 카테고리 이론이라는 책의 일부입니다. 이전 글은 [합성의 정수, 카테고리](/contents/part1/category-the-essence-of-composition.md)입니다. 목차는 [여기서](/README.md) 확인하실 수 있습니다.

타입과 함수의 카테고리는 프로그래밍에서 중요한 역할을 합니다. 그럼 타입이 무엇이고 왜 타입을 알아야 하는지 알아보겠습니다.

## 타입은 어디에서 쓰이나요?

static vs dynamic 및 strong vs weak 와 같이 각 타입의 장점에 대해 논란은 들어보셨을 것입니다. 사고 실험을 통해 어떤 의미인지 보여드리겠습니다. 그것은 마치 수백만 마리의 원숭이가 행복하게 컴퓨터 앞에서 아무 키나 누르고 프로그램을 만들고 컴파일하고 실행하는 것과 같습니다.

<p align="center">
	<img src="/img/img_part1-2_1.jpg" width="258" />
</p>

머신 러닝이 있다면 원숭이가 만들어낸 바이트 조합조차도 받아들여지고 실행될 것입니다. 어휘와 문법의 에러를 찾아주는 컴파일러가 있는 고급 언어를 사용하는 우리는 감사해야 합니다. 원숭이들은 바나나 없이 가야겠지만 남겨진 프로그램은 더 유용할 것입니다. 타입 확인은 터무니없는 프로그램들이 생기지 않도록 막아주는 방어막입니다. 동적 타입 언어에선 타입 불일치는 런타임에 발견될 것이고 정적 타입 언어는 타입 불일치를 컴파일 타임에 발견할 것입니다. 옳지않은 프로그램에겐 실행할 기회도 제거해야 합니다.

그래서 우리가 만들고 싶은 것은 원숭이를 행복하게 만드는 프로그램일까요? 아니면 올바른 프로그램일까요?

원숭이들로 하는 사고 실험에선 보통 셰익스피어의 작품과 똑같은 결과를 내는 것이 목표입니다. 맞춤법과 문법 검사기가 있다면 실패할 경우를 엄청나게 줄여줄 것입니다. 로미오가 사람인 것을 타입 검사기에 정의해놓는다면 그에게 나뭇잎이 나거나 중력장 안에서 광자를 뿌리는 등의 행위는 하지 않을 것입니다.

## 결합성에 대한 타입들

카테고리 이론은 합성 화살표에 대한 내용입니다. 하지만 아무 화살표 둘이 합성할 순 없습니다. 화살표의 목표 객체는 다음 화살표의 원본 객체여야 합니다. 우리가 프로그래밍에서 한 함수의 결과값을 다른 함수로 보내는 것처럼요. 만약 목표 함수가 원본 함수에서 생성되는 데이터를 완벽하게 이해하지 못한다면 프로그램은 작동하지 않을 것입니다. 함수가 합성하려면 합성하려는 함수들의 끝이 일치해야 합니다. 언어의 타입 시스템이 더 강력할수록 타입 일치가 더 좋아진다는 것은 기계적으로 증명되기도 했으며 설명도 가능합니다.

한 가지 주장은 강력한 정적 타입 확인이 의미적으로 올바른 프로그램을 제거할 수도 있다는 것입니다. 실제로 이런 일이 일어날 확률은 극단적이지만 모든 언어가 이러한 상황을 위해 백도어같은 것을 만들어두는 것은 꼭 필요하다고 생각합니다. 심지어 하스켈에서도 `unsafeCoerce` 가 존재합니다. 그렇지만 그런 기능은 분별력 있게 사용해야 합니다. 그레고르 잠자는 프란츠 카프카 소설의 한 인물인데, 작가가 큰 갑충으로 비유하는 순간 타입 시스템이 파괴되었고 결국에 그는 죽음을 맞이하였습니다.

또 다른 주장은 타입을 다루게 하는 것은 프로그래머에게 너무 많은 짐을 지어준다는 것입니다. 저는 이러한 감정을 몇몇 C++ 이터레이터를 선언할 때 느껴봐서 공감할 수 있습니다. 컴파일러가 타입이 쓰이는 곳을 보고 추론하는 기술인 타입 추론이 있지만 그것은 예외로 치겠습니다. C++에서는 `auto` 변수를 선언하고 컴파일러가 그 변수의 타입을 알아내게 하는 방식도 존재합니다.

하스켈에선 몇없는 예외 사항을 제외하고 타입 어노테이션은 순수하게 선택입니다. 코드의 의미를 설명할 수 있으며 컴파일 에러가 이해하기 쉬워진다는 이유로 프로그래머들은 대부분 사용하는 편입니다. 이것은 하스켈에서 타입을 디자인하는 것으로부터 프로젝트를 시작하는 흔한 방식입니다. 나중에는 타입 어노테이션이 구현을 이끌고 컴파일러가 강요하는 내용이 될 것입니다.

강력한 정적 타입을 쓰는 것은 종종 코드를 테스트하지 않는 것에 대한 변명이 되기도 합니다. 하스켈 프로그래머들이 다음과 같은 말을 하는 것을 들어보셨을 수도 있습니다. "컴파일이 된다면 그건 반드시 옳아." 물론 타입이 옳은 프로그램이 옳바른 출력을 한다는 보장은 없습니다. 이러한 무신경한 태도의 결과인지 몇몇 연구에서 하스켈은 생각한 것만큼 앞선 코드 퀄리티를 보여주지 못했습니다. 저는 소프트웨어 개발자를 경제학적으로 보는 시선과 기다려주지 않는 엔드 유저 그리고 언어나 방법론에 긴 시간을 쓸 수 없는 상업적인 환경에서 버그를 고쳐야하는 압력은 코드 품질에 영향을 줘서 어느 정도 한계가 생겼다고 생각합니다. 더 나은 기준은 스케쥴을 넘어서는 프로젝트의 수 또는 기능성을 급격하게 줄여서 배포되는 프로젝트의 수를 측정하면 될 것입니다.

유닛 테스트가 강력한 타입 사용을 대체할 수 있다는 주장의 경우엔 강력한 타입 언어에서 사용되는 보통의 리팩토링인 특정 함수의 인자의 타입을 변경하는 경우를 생각해보면 됩니다. 강력한 타입 언어에선 함수의 선언만 수정하고 빌드 오류만 수정하면 됩니다. 약한 타입 언어에선 함수가 이제 다른 타입을 사용한다는 추론을 호출하는 부분에선 알 방법이 없습니다. 유닛 테스트는 몇몇 불일치는 잡아낼 수 있겠지만 테스트는 언제나 결정론적보다는 확률적인 과정이라 증명을 대신 하는 것엔 형편없습니다.

## 타입은 무엇일까요?

가장 간단하게 직관적이게 말하자면 타입은 값들의 집합입니다. Bool(하스켈의 구체 타입은 대문자로 시작합니다!)이라는 타입은 `True` 와 `False` 를 원소로 받는 집합입니다. `Char` 타입은 `'a'` 또는 `'ą'` 처럼 모든 유니코드의 집합입니다.

집합은 유한할수도 무한할수도 있습니다. `Char` 의 배열이라고도 할 수 있는 `String` 타입은 유한 집합의 한 예입니다.

`x` 를 `Integer` 라고 정의해보겠습니다.

```
x :: Integer
```

우리는 x를 정수 집합의 원소라고 말할 수 있습니다. 하스켈의 `Integer` 는 유한 집합이고 임의 정밀도 산술(Arbitrary Precision Arithmetic)을 할 수 있습니다. 하스켈엔 또 다른 유한 집합인 `Int` 가 있는데 이는 C++의 `Int` 처럼 기계 타입에 대응합니다.

순환 정의를 수반하는 다형성 함수 다형성 함수를 사용할 때 생기는 문제와 모든 집합에 대한 집합을 가질 수 없다는 사실이 타입과 집합을 구별하기 까다롭게 하는 몇 가지 애매한 점입니다. 하지만 저는 수학에 그렇게 까다로운 사람이 아닙니다. 훌륭한 점은 **Set** 이라고 불리는 집합의 카테고리가 있다는 것과 우리는 이것만 사용하면 된다는 것입니다. **Set** 의 객체는 집합이고 사상(morphism, 화살표)은 함수 역할을 합니다.

**Set** 는 아주 특별한 카테고리인데, 왜냐하면 내부에서 그것의 객체를 선택할 수 있고 그렇게 함으로서 많은 직관을 얻게 되기 때문입니다. 예를 들면 우리는 빈 집합은 원소를 가지고 있지 않다는 사실을 알고있습니다. 우리는 하나의 원소만 가지는 특별한 집합이 있다는 사실을 알고있습니다. 우리는 함수가 한 집합의 원소를 다른 집합의 원소로 매핑(map)해준다는 것을 알고있습니다. 또한 두 원소를 하나의 원소로 매핑하는 일은 가능하지만 하나의 원소를 둘 이상의 원소로 매핑하는 것은 불가능하다는 것도 알고있습니다. 저의 계획은 이러한 정보를 점진적으로 모두 잊게하고 그러한 내용을 순수한 카테고리적인 용어(객체와 화살표)로 표현할 수 있게 바꾸는 것입니다.

이상적인 세계에선 하스켈의 타입이 집합이고 하스켈의 함수가 집합 사이의 수학적인 함수라고 할 수 있습니다. 하지만 수학적인 함수는 아무 코드도 실행하지 않는다는 작은 문제가 있습니다. 그저 답을 알고 있을 뿐입니다. 하스켈의 함수는 답을 계산하게 되어있습니다. 답이 아무리 크더라도 유한한 숫자 범위 안에 포함된다면 문제가 없습니다. 하지만 재귀를 일으키는 계산이 존재하고 그것들은 절대 끝나지 않습니다. 끝나지 않는 함수를 무조건적으로 하스켈에서 없앨 수는 없습니다. 이는 유명한 정지 문제(Halting Problem)처럼 어떤 것이 끝날 함수고 어떤 것이 끝나지 않을 함수인지 구별할 수가 없기 때문입니다. 여기서 컴퓨터 과학자들은 아주 영리하게 바텀 타입(bottom type)을 생각해냈고 기호로 쓰면 `_|_` (유니코드론 ⊥)입니다. 이 "값"은 끝나지 않을 계산을 의미합니다. 아래와 같은 함수가 있다고 해보겠습니다.

```
f :: Bool -> Bool
```

위 함수는 `True`, `False` 또는 절대 끝나지 않는다는 의미의 `_|_` 를 반환할 것입니다.

흥미롭게도 여러분이 이 바텀 타입을 타입 시스템의 일부로 받아들이는 순간, 모든 런타임 에러를 간편하게 바텀 타입으로 취급하게 되고 심지어 함수가 명시적으로 이 기호를 반환하도록 하기도 합니다. 후자의 경우엔 보통 `undefined` 라는 표현으로 사용됩니다.

```
f :: Bool -> Bool
f x = undefined
```

`undefined` 는 `Bool` 을 포함하여 어떤 타입도 될 수 있기 때문에 위 정의는 아래와 같이 쓸 수도 있습니다.

```
f :: Bool -> Bool
f = undefined
```

(`x`가 없어졌습니다)

왜냐하면 바텀 타입은 `Bool->Bool` 의 일부이기도 하기 때문입니다.

바텀 타입을 반환하는 함수는 부분(partial) 정의 함수이라고 불리는데, 이는 모든 가능한 경우에 대해 유효한 결과만 반환하는 종합(total) 함수와 반대되는 내용입니다.

바텀덕분에 우리는 **Set** 대신에 하스켈의 타입과 함수들의 카테고리인 **Hask** 를 사용할 수 있을 것입니다. 이론적인 측면에서 이는 끝나지 않는 복잡함의 시작이겠지만 이 시점에 저는 만능칼을 꺼내서 이를 증명하는 부분을 잘라내려고 합니다. 프로그래밍적인 측면에서 끝나지 않는 함수와 바텀 타입은 무시해도 괜찮은 부분이고 **Hask** 를 진짜 **Set** 이라고 취급해도 괜찮습니다(글의 끝에 있는 참고문헌 부분을 확인해주세요).

## Why Do We Need a Mathematical Model?

As a programmer you are intimately familiar with the syntax and grammar of your programming language. These aspects of the language are usually described using formal notation at the very beginning of the language spec. But the meaning, or semantics, of the language is much harder to describe; it takes many more pages, is rarely formal enough, and almost never complete. Hence the never ending discussions among language lawyers, and a whole cottage industry of books dedicated to the exegesis of the finer points of language standards.

There are formal tools for describing the semantics of a language but, because of their complexity, they are mostly used with simplified academic languages, not real-life programming behemoths. One such tool called operational semantics describes the mechanics of program execution. It defines a formalized idealized interpreter. The semantics of industrial languages, such as C++, is usually described using informal operational reasoning, often in terms of an “abstract machine.”

The problem is that it’s very hard to prove things about programs using operational semantics. To show a property of a program you essentially have to “run it” through the idealized interpreter.

It doesn’t matter that programmers never perform formal proofs of correctness. We always “think” that we write correct programs. Nobody sits at the keyboard saying, “Oh, I’ll just throw a few lines of code and see what happens.” We think that the code we write will perform certain actions that will produce desired results. We are usually quite surprised when it doesn’t. That means we do reason about programs we write, and we usually do it by running an interpreter in our heads. It’s just really hard to keep track of all the variables. Computers are good at running programs — humans are not! If we were, we wouldn’t need computers.

But there is an alternative. It’s called denotational semantics and it’s based on math. In denotational semantics every programing construct is given its mathematical interpretation. With that, if you want to prove a property of a program, you just prove a mathematical theorem. You might think that theorem proving is hard, but the fact is that we humans have been building up mathematical methods for thousands of years, so there is a wealth of accumulated knowledge to tap into. Also, as compared to the kind of theorems that professional mathematicians prove, the problems that we encounter in programming are usually quite simple, if not trivial.

Consider the definition of a factorial function in Haskell, which is a language quite amenable to denotational semantics:

```
fact n = product [1..n]
```

The expression `[1..n]` is a list of integers from 1 to n. The function `product` multiplies all elements of a list. That’s just like a definition of factorial taken from a math text. Compare this with C:

```
int fact(int n) {
    int i;
    int result = 1;
    for (i = 2; i <= n; ++i)
        result *= i;
    return result;
}
```

Need I say more?

Okay, I’ll be the first to admit that this was a cheap shot! A factorial function has an obvious mathematical denotation. An astute reader might ask: What’s the mathematical model for reading a character from the keyboard or sending a packet across the network? For the longest time that would have been an awkward question leading to a rather convoluted explanation. It seemed like denotational semantics wasn’t the best fit for a considerable number of important tasks that were essential for writing useful programs, and which could be easily tackled by operational semantics. The breakthrough came from category theory. Eugenio Moggi discovered that computational effect can be mapped to monads. This turned out to be an important observation that not only gave denotational semantics a new lease on life and made pure functional programs more usable, but also shed new light on traditional programming. I’ll talk about monads later, when we develop more categorical tools.

One of the important advantages of having a mathematical model for programming is that it’s possible to perform formal proofs of correctness of software. This might not seem so important when you’re writing consumer software, but there are areas of programming where the price of failure may be exorbitant, or where human life is at stake. But even when writing web applications for the health system, you may appreciate the thought that functions and algorithms from the Haskell standard library come with proofs of correctness.

## Pure and Dirty Functions

The things we call functions in C++ or any other imperative language, are not the same things mathematicians call functions. A mathematical function is just a mapping of values to values.

We can implement a mathematical function in a programming language: Such a function, given an input value will calculate the output value. A function to produce a square of a number will probably multiply the input value by itself. It will do it every time it’s called, and it’s guaranteed to produce the same output every time it’s called with the same input. The square of a number doesn’t change with the phases of the Moon.

Also, calculating the square of a number should not have a side effect of dispensing a tasty treat for your dog. A “function” that does that cannot be easily modelled as a mathematical function.

In programming languages, functions that always produce the same result given the same input and have no side effects are called pure functions. In a pure functional language like Haskell all functions are pure. Because of that, it’s easier to give these languages denotational semantics and model them using category theory. As for other languages, it’s always possible to restrict yourself to a pure subset, or reason about side effects separately. Later we’ll see how monads let us model all kinds of effects using only pure functions. So we really don’t lose anything by restricting ourselves to mathematical functions.

## Examples of Types

Once you realize that types are sets, you can think of some rather exotic types. For instance, what’s the type corresponding to an empty set? No, it’s not C++ `void`, although this type is called `Void` in Haskell. It’s a type that’s not inhabited by any values. You can define a function that takes `Void`, but you can never call it. To call it, you would have to provide a value of the type `Void`, and there just aren’t any. As for what this function can return, there are no restrictions whatsoever. It can return any type (although it never will, because it can’t be called). In other words it’s a function that’s polymorphic in the return type. Haskellers have a name for it:

```
absurd :: Void -> a
```

(Remember, `a` is a type variable that can stand for any type.) The name is not coincidental. There is deeper interpretation of types and functions in terms of logic called the Curry-Howard isomorphism. The type `Void` represents falsity, and the type of the function `absurd` corresponds to the statement that from falsity follows anything, as in the Latin adage “ex falso sequitur quodlibet.”

Next is the type that corresponds to a singleton set. It’s a type that has only one possible value. This value just “is.” You might not immediately recognise it as such, but that is the C++ `void`. Think of functions from and to this type. A function from `void` can always be called. If it’s a pure function, it will always return the same result. Here’s an example of such a function:

```
int f44() { return 44; }
```

You might think of this function as taking “nothing”, but as we’ve just seen, a function that takes “nothing” can never be called because there is no value representing “nothing.” So what does this function take? Conceptually, it takes a dummy value of which there is only one instance ever, so we don’t have to mention it explicitly. In Haskell, however, there is a symbol for this value: an empty pair of parentheses, `()`. So, by a funny coincidence (or is it a coincidence?), the call to a function of void looks the same in C++ and in Haskell. Also, because of the Haskell’s love of terseness, the same symbol `()` is used for the type, the constructor, and the only value corresponding to a singleton set. So here’s this function in Haskell:

```
f44 :: () -> Integer
f44 () = 44
```

The first line declares that `f44` takes the type `()`, pronounced “unit,” into the type `Integer`. The second line defines `f44` by pattern matching the only constructor for unit, namely `()`, and producing the number 44. You call this function by providing the unit value `()`:

```
f44 ()
```

Notice that every function of unit is equivalent to picking a single element from the target type (here, picking the `Integer` 44). In fact you could think of `f44` as a different representation for the number 44. This is an example of how we can replace explicit mention of elements of a set by talking about functions (arrows) instead. Functions from unit to any type A are in one-to-one correspondence with the elements of that set A.

What about functions with the `void` return type, or, in Haskell, with the unit return type? In C++ such functions are used for side effects, but we know that these are not real functions in the mathematical sense of the word. A pure function that returns unit does nothing: it discards its argument.

Mathematically, a function from a set A to a singleton set maps every element of A to the single element of that singleton set. For every A there is exactly one such function. Here’s this function for `Integer`:

```
fInt :: Integer -> ()
fInt x = ()
```

You give it any integer, and it gives you back a unit. In the spirit of terseness, Haskell lets you use the wildcard pattern, the underscore, for an argument that is discarded. This way you don’t have to invent a name for it. So the above can be rewritten as:

```
fInt :: Integer -> ()
fInt _ = ()
```

Notice that the implementation of this function not only doesn’t depend on the value passed to it, but it doesn’t even depend on the type of the argument.

Functions that can be implemented with the same formula for any type are called parametrically polymorphic. You can implement a whole family of such functions with one equation using a type parameter instead of a concrete type. What should we call a polymorphic function from any type to unit type? Of course we’ll call it `unit`:

```
unit :: a -> ()
unit _ = ()
```

In C++ you would write this function as:

```
template<class T>
void unit(T) {}
```

Next in the typology of types is a two-element set. In C++ it’s called `bool` and in Haskell, predictably, `Bool`. The difference is that in C++ `bool` is a built-in type, whereas in Haskell it can be defined as follows:

```
data Bool = True | False
```

(The way to read this definition is that Bool is either True or False.) In principle, one should also be able to define a Boolean type in C++ as an enumeration:

```
enum bool {
    true,
    false
};
```

but C++ `enum` is secretly an integer. The C++11 “`enum class`” could have been used instead, but then you would have to qualify its values with the class name, as in `bool::true` and `bool::false`, not to mention having to include the appropriate header in every file that uses it.

Pure functions from `Bool` just pick two values from the target type, one corresponding to `True` and another to `False`.

Functions to `Bool` are called *predicates*. For instance, the Haskell library `Data.Char` is full of predicates like `isAlpha` or `isDigit`. In C++ there is a similar library that defines, among others, `isalpha` and `isdigit`, but these return an `int` rather than a Boolean. The actual predicates are defined in `std::ctype` and have the form `ctype::is(alpha, c)`, `ctype::is(digit, c)`, etc.

## Challenges

1. Define a higher-order function (or a function object) `memoize` in your favorite language. This function takes a pure function `f` as an argument and returns a function that behaves almost exactly like `f`, except that it only calls the original function once for every argument, stores the result internally, and subsequently returns this stored result every time it’s called with the same argument. You can tell the memoized function from the original by watching its performance. For instance, try to memoize a function that takes a long time to evaluate. You’ll have to wait for the result the first time you call it, but on subsequent calls, with the same argument, you should get the result immediately.

2. Try to memoize a function from your standard library that you normally use to produce random numbers. Does it work?

3. Most random number generators can be initialized with a seed. Implement a function that takes a seed, calls the random number generator with that seed, and returns the result. Memoize that function. Does it work?

4. Which of these C++ functions are pure? Try to memoize them and observe what happens when you call them multiple times: memoized and not.

	1. The factorial function from the example in the text.

	2. ```
	std::getchar()
	```

	3. ```
	bool f() {
  	std::cout << "Hello!" << std::endl;
    return true;
	}
	```

	4. ```
	int f(int x)
	{
		static int y = 0;
		y += x;
		return y;
	}
	```

5. How many different functions are there from `Bool` to `Bool`? Can you implement them all?

6. Draw a picture of a category whose only objects are the types `Void`, `()` (unit), and `Bool`; with arrows corresponding to all possible functions between these types. Label the arrows with the names of the functions.

## Bibliography

1. Nils Anders Danielsson, John Hughes, Patrik Jansson, Jeremy Gibbons, [Fast and Loose Reasoning is Morally Correct](http://www.cs.ox.ac.uk/jeremy.gibbons/publications/fast+loose.pdf). This paper provides justification for ignoring bottoms in most contexts.

Next: [Categories Great and Small](https://bartoszmilewski.com/2014/12/05/categories-great-and-small/)

---

공부 목적으로 번역을 하고 있습니다! 잘못된 점에 대한 이슈나 PR은 언제든지 환영합니다 :)
