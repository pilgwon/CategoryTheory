# 합성(Composition)의 정수, 카테고리
원본 사이트: [Category Theory for Programmers](https://bartoszmilewski.com/2014/11/04/category-the-essence-of-composition/)

---

> [지난 글](https://github.com/pilgwon/CategoryTheory/blob/master/README.md)에서 보여주신 긍정적인 반응은 엄청났습니다. 동시에 이는 저에게 많은 사람들이 기대하고 있다는 사실을 깨닫게 해줘서 부담도 됐습니다. 제가 작성할 글에 독자분들이 실망할까봐 걱정도 됩니다. 어떤 독자분들은 책이 더 실용적이라고 할 수도 있고 또 다른 독자분들은 더 추상적이라고 할 수도 있습니다. 어떤 독자분들은 C++을 싫어하고 하스켈로 만든 예제를 더 좋아할 수도 있습니다. 또 어떤 독자분들은 하스켈도 싫어해서 자바로 만든 예제를 원하실 수도 있습니다. 그러니 이 책은 모두를 만족시킬 수는 없을 것입니다. 어느 정도 타협선을 지키며 제가 지금까지 느꼈던 아하!들을 공유할 수 있는 자리가 되었으면 좋겠습니다. 기본부터 시작해볼까요?

카테고리는 당황스러울정도로 간단한 개념입니다. 카테고리는 객체와 그 객체 사이의 화살표들로 이루어져 있습니다. 그래서 카테고리는 그림으로 표현하기가 쉽습니다. 객체는 원이나 점으로 표현할 수 있고, 화살표는 화살표로 표현하면 됩니다. (다양성을 위해 저는 객체를 돼지들로, 화살표는 폭죽으로 표현하려고 합니다.) 하지만 카테고리의 정수는 합성입니다. 원하신다면 합성의 정수가 카테고리라고도 할 수 있습니다. 객체 A에서 객체 B로 가는 화살표와 객체 B에서 객체 C로 가는 화살표를 합성하면 객체 A에서 객체 C로 가는 화살표를 만들 수 있습니다.

<p align="center">
	<img src="/img/img_part1-1_1.jpg" width="520" />
  <p align="center">카테고리에서는 A에서 B로 가는 화살표와 B에서 C로 가는 화살표가 있다면 그것들의 합성을 통해 반드시 A에서 C로 직접 가는 화살표를 만들 수 있습니다. 하지만 사상(morphism)이 없다는 점에서 이 다이어그램은 완전한 카테고리라고 할 수는 없습니다.</p>
</p>

## 화살표를 함수로 보기

이미 말이 안 될 정도로 추상적인가요? 절망하지마세요. 구체적으로 말해보겠습니다. 사상(morphism)이라고도 불리는 화살표를 함수라고 생각해보겠습니다. 여기 A 타입을 받아서 B를 반환하는 f라는 함수가 있습니다. 그리고 B를 받아서 C를 반환하는 함수 g가 있습니다. 우리는 f에서 g로 결과를 넘김으로써 합성할 수 있습니다. 이제 A를 받아서 C를 반환하는 함수를 만드셨습니다.

수학에서의 합성은 g∘f와 같이 작은 원으로 표시합니다. 합성의 순서는 오른쪽에서 왼쪽입니다. 혼란스러울 수 있습니다. 이와 비슷한 문법은 Unix에서도 찾아볼 수 있습니다.

```
lsof | grep Chrome
```

또는 F#의 `>>`처럼 왼쪽에서 오른쪽으로 가는 문법을 사용하셔도 됩니다. 하지만 수학과 하스켈에서는 오른쪽에서 왼쪽으로 합성을 합니다. 그리고 g∘f는 "g after f"라고 읽으시면 됩니다.

C 코드를 통해 좀 더 분명하게 작성해보겠습니다. `A` 타입을 받아서 `B` 타입을 반환하는 함수 `f` 를 작성해보겠습니다.

```
B f(A a);
```

다른 함수도 작성해보겠습니다.

```
C g(B b);
```

그 합성 결과는 다음과 같습니다.

```
C g_after_f(A a)
{
    return g(f(a));
}
```

C로 표시한 것도 잘 보시면 오른쪽에서 왼쪽으로 합성(`g(f(a))`)하는 것을 보실 수 있습니다.

할 수만 있다면 C++ 표준 라이브러리에 두 함수를 받아서 그들의 합성을 반환하는 템플릿이 있다고 말씀드리고 싶지만 아쉽게도 없네요. 언어를 바꿔볼까요? 하스켈로 같은 내용을 작성해보겠습니다.

```
f :: A -> B
```

비슷하게 g도 작성한다면 다음과 같습니다.

```
g :: B -> C
```

위 둘을 합성해보면 아래와 같이 될 것입니다.

```
g . f
```

하스켈이 이렇게 쉽게 표현하는 것을 보고 나면 간단한 함수형 개념도 표현하지 못하는 C++은 조금 부끄러울 수도 있습니다. 하스켈에선 유니코드 문자를 사용할 수 있어서 합성을 다음과 같이 표현할 수 있습니다.

```
g ∘ f
```

심지어 유니코드 문자인 더블 콜론과 화살표도 사용할 수 있습니다.

```
f ∷ A → B
```

첫 번째 하스켈 수업입니다. 더블 콜론은 "이러한 타입을 가지고 있다..."를 의미합니다. A 함수 타입은 두 타입 사이에 화살표를 넣으면 생성 가능합니다. 그리고 두 함수 사이에 마침표 (또는 유니코드 문자 원) 를 넣으면 합성을 표현할 수 있습니다.

## 합성의 성질

어떤 카테고리의 합성이라도 반드시 만족하는 아주 중요한 두 가지 성질이 있습니다.

1. 합성은 결합이 가능합니다. 합성이 가능한 세 가지 사상(morphism)인 f,g,h가 있다고 해보겠습니다. (합성이 가능하니 각자 끝에서 끝이 이어지겠죠) 이들을 합성할 땐 괄호도 없앨 수 있습니다. 수학적으로 표현한다면 다음과 같이 표현할 수 있습니다.

```
h∘(g∘f) = (h∘g)∘f = h∘g∘f
```

하스켈의 수도 코드입니다.

```
f :: A -> B
g :: B -> C
h :: C -> D
h . (g . f) == (h . g) . f == h . g . f
```

("수도"라고 말한 이유는 각 함수에 대해 동등함이 선언되지 않았기 때문입니다.)

결합성은 함수를 다룰 때 꽤 명확하게 나오지만 다른 카테고리에서는 그만큼 명확하진 않습니다.

2. 모든 A 객체에 대해서 합성의 유닛이 되는 화살표가 존재합니다. 이 화살표는 객체 자기 자신을 가리키는 루프입니다. 합성의 유닛이 된다는 것은 A에서 시작하거나 A로 끝나는 화살표와 합성하면 똑같은 화살표를 돌려받는다는 의미입니다. A 객체에 대한 유닛 화살표는 idA(identity on A, A에 항등)라고 부릅니다. 수학적으로 표현하면 A를 받아서 B를 반환하는 f에 대해서 다음과 같이 표현할 수 있습니다.

```
f∘idA = f
```

그리고

```
idB∘f = f
```

함수를 다룰 때 항등 함수로 구현된 항등 화살표는 인자로 온 값을 다시 반환합니다. 구현은 모든 타입에 대해 동일하며 이는 이 함수가 보편적으로 다형성이라는 것을 의미합니다. C++에선 다음과 같은 템플릿으로 표현할 수 있습니다.

```
template<class T> T id(T x) { return x; }
```

물론 C++에선 그렇게 쉽지 않은데 왜냐하면 넘기는 값이 무엇인지뿐만 아니라 어떻게 넘기는지도 알아야하기 때문입니다.

하스켈에선 항등 함수가 표준 라이브러리(Prelude라고 불립니다)의 일부입니다. 다음은 그것의 선언과 정의입니다.

```
id :: a -> a
id x = x
```

보시다시피 하스켈에서 다형성 함수는 식은 죽 먹기입니다. 선언을 보면 타입을 타입 변수로 변환하기만 하면 됩니다. 약간의 요령을 알려드리자면 구체적인 타입의 이름은 언제나 대문자로 시작하고 타입 함수의 이름은 언제나 소문자 `a` 로 시작합니다. 여기서 a는 모든 타입을 나타냅니다.

하스켈 함수 정의는 정식 파라미터와 함께 함수의 이름으로 이루어져 있습니다. 여기선 `x` 하나네요. 함수의 바디 부분은 등호를 따릅니다. 이 간결함은 처음 배우는 분들에겐 놀라울 수도 있지만 완벽하게 맞아떨어진다는 것을 바로 이해하실 수 있을 것입니다. 함수의 정의와 함수 호출은 함수형 프로그래밍의 빵과 버터급으로 필수적이어서 그들의 문법은 최대한 간결해졌습니다. 인자 목록에 괄호가 없을 뿐만 아니라 인자 사이의 콤마조차도 없습니다. (이는 나중에 다중 인자를 가진 함수를 정의할 때 보실 수 있을 것입니다)

함수의 바디 부분은 언제나 표현식이 차지합니다. 함수엔 명령문은 존재하지 않습니다. 함수의 결과는 표현식입니다.

이는 하스켈의 두 번째 레슨을 결론짓습니다.

항등 조건은 다음과 같이 작성될 수 있습니다. (여기도 수도 하스켈입니다)

```
f . id == f
id . f == f
```

이런 질문이 생길 수도 있습니다. "왜 아무것도 하지 않는 항등 함수에 대해 아무도 뭐라고 하지 않는거지? 그렇다 치더라도 0은 왜 뭐라고 하는거지? 0은 아무것도 없다는 상징인데." 고대 로마인들은 0이 없는 숫자 체계를 가지고 있었고 그들은 0 없이도 오늘날까지 무너지지 않은 훌륭한 도로와 송수로를 만들 수 있었습니다.

0이나 `id` 같은 중립적인 값은 상징적인 값을 다룰 때 엄청나게 도움이 됩니다. 그게 바로 0의 개념과 친근하던 아랍인과 페르시아인들에 비해 로마인들이 대수학(algebra)을 잘하지 못했던 이유입니다.
항등 함수는 고계 함수(higher order function)에 인자로 넣거나 반환 값으로 받을 때 아주 편리합니다. 고계 함수는 함수를 상징적으로 조작하는 것을 가능하게 해주는 개념입니다. 함수의 대수학이라고 할 수 있습니다.

요약하겠습니다. 카테고리는 객체와 화살표(사상, morphism)으로 이루어져 있습니다. 화살표는 합성될 수 있으며 합성은 결합성을 가집니다. 모든 객체는 합성에서 쓰일 수 있는 유닛을 만드는 항등 화살표를 가집니다.


## Composition is the Essence of Programming

Functional programmers have a peculiar way of approaching problems. They start by asking very Zen-like questions. For instance, when designing an interactive program, they would ask: What is interaction? When implementing Conway’s Game of Life, they would probably ponder about the meaning of life. In this spirit, I’m going to ask: What is programming? At the most basic level, programming is about telling the computer what to do. “Take the contents of memory address x and add it to the contents of the register EAX.” But even when we program in assembly, the instructions we give the computer are an expression of something more meaningful. We are solving a non-trivial problem (if it were trivial, we wouldn’t need the help of the computer). And how do we solve problems? We decompose bigger problems into smaller problems. If the smaller problems are still too big, we decompose them further, and so on. Finally, we write code that solves all the small problems. And then comes the essence of programming: we compose those pieces of code to create solutions to larger problems. Decomposition wouldn’t make sense if we weren’t able to put the pieces back together.

This process of hierarchical decomposition and recomposition is not imposed on us by computers. It reflects the limitations of the human mind. Our brains can only deal with a small number of concepts at a time. One of the most cited papers in psychology, [The Magical Number Seven, Plus or Minus Two](http://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two), postulated that we can only keep 7 ± 2 “chunks” of information in our minds. The details of our understanding of the human short-term memory might be changing, but we know for sure that it’s limited. The bottom line is that we are unable to deal with the soup of objects or the spaghetti of code. We need structure not because well-structured programs are pleasant to look at, but because otherwise our brains can’t process them efficiently. We often describe some piece of code as elegant or beautiful, but what we really mean is that it’s easy to process by our limited human minds. Elegant code creates chunks that are just the right size and come in just the right number for our mental digestive system to assimilate them.

So what are the right chunks for the composition of programs? Their surface area has to increase slower than their volume. (I like this analogy because of the intuition that the surface area of a geometric object grows with the square of its size — slower than the volume, which grows with the cube of its size.) The surface area is the information we need in order to compose chunks. The volume is the information we need in order to implement them. The idea is that, once a chunk is implemented, we can forget about the details of its implementation and concentrate on how it interacts with other chunks. In object-oriented programming, the surface is the class declaration of the object, or its abstract interface. In functional programming, it’s the declaration of a function. (I’m simplifying things a bit, but that’s the gist of it.)

Category theory is extreme in the sense that it actively discourages us from looking inside the objects. An object in category theory is an abstract nebulous entity. All you can ever know about it is how it relates to other object — how it connects with them using arrows. This is how internet search engines rank web sites by analyzing incoming and outgoing links (except when they cheat). In object-oriented programming, an idealized object is only visible through its abstract interface (pure surface, no volume), with methods playing the role of arrows. The moment you have to dig into the implementation of the object in order to understand how to compose it with other objects, you’ve lost the advantages of your programming paradigm.

## Challenges

1. Implement, as best as you can, the identity function in your favorite language (or the second favorite, if your favorite language happens to be Haskell).

2. Implement the composition function in your favorite language. It takes two functions as arguments and returns a function that is their composition.

3. Write a program that tries to test that your composition function respects identity.

4. Is the world-wide web a category in any sense? Are links morphisms?

5. Is Facebook a category, with people as objects and friendships as morphisms?

6. When is a directed graph a category?

Next: [Types and Functions](https://bartoszmilewski.com/2014/11/24/types-and-functions/)

---

공부 목적으로 번역을 하고 있습니다! 잘못된 점에 대한 이슈나 PR은 언제든지 환영합니다 :)
