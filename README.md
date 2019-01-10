# 프로그래머를 위한 카테고리 이론
원본 사이트: [Category Theory for Programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)

---

## 목차
### 파트 1
1. \[미번역\] [합성(Composition)의 정수, 카테고리](https://bartoszmilewski.com/2014/11/04/category-the-essence-of-composition/)
2. \[미번역\] [함수와 타입](https://bartoszmilewski.com/2014/11/24/types-and-functions/)
3. \[미번역\] [다양한 종류의 카테고리들](https://bartoszmilewski.com/2014/12/05/categories-great-and-small/)
4. \[미번역\] [Kleisli 카테고리](https://bartoszmilewski.com/2014/12/23/kleisli-categories/)
5. \[미번역\] [Product와 Coproduct](https://bartoszmilewski.com/2015/01/07/products-and-coproducts/)
6. \[미번역\] [간단한 대수적 데이터 타입들](https://bartoszmilewski.com/2015/01/13/simple-algebraic-data-types/)
7. \[미번역\] [함자(Functors)](https://bartoszmilewski.com/2015/01/20/functors/)
8. \[미번역\] [함자성(Functoriality)](https://bartoszmilewski.com/2015/02/03/functoriality/)
9. \[미번역\] [함수의 종류](https://bartoszmilewski.com/2015/03/13/function-types/)
10. \[미번역\] [자연 변환](https://bartoszmilewski.com/2015/04/07/natural-transformations/)

### 파트 2
1. \[미번역\] [선언형 프로그래밍](https://bartoszmilewski.com/2015/04/15/category-theory-and-declarative-programming/)
2. \[미번역\] [Limit과 Colimit](https://bartoszmilewski.com/2015/04/15/limits-and-colimits/)
3. \[미번역\] [자유 모노이드(Monoid)](https://bartoszmilewski.com/2015/07/21/free-monoids/)
4. \[미번역\] [표현 가능 함자](https://bartoszmilewski.com/2015/07/29/representable-functors/)
5. \[미번역\] [요네다 보조정리(lemma)](https://bartoszmilewski.com/2015/09/01/the-yoneda-lemma/)
6. \[미번역\] [요네다 매장(Embedding)](https://bartoszmilewski.com/2015/10/28/yoneda-embedding/)

### 파트 3
1. \[미번역\] [사상(Morphism)에 관한 모든 것](https://bartoszmilewski.com/2015/11/17/its-all-about-morphisms/)
2. \[미번역\] [Adjunctions](https://bartoszmilewski.com/2016/04/18/adjunctions/)
3. \[미번역\] [자유 Adjunction과 잊기 쉬운 Adjunction과](https://bartoszmilewski.com/2016/06/15/freeforgetful-adjunctions/)
4. \[미번역\] [프로그래머가 정의하는 모나드(Monad)](https://bartoszmilewski.com/2016/11/21/monads-programmers-definition/)
5. \[미번역\] [모나드와 효과](https://bartoszmilewski.com/2016/11/30/monads-and-effects/)
6. \[미번역\] [카테고리스러운 모나드](https://bartoszmilewski.com/2016/12/27/monads-categorically/)
7. \[미번역\] [코모나드(Comonads)](https://bartoszmilewski.com/2017/01/02/comonads/)
8. \[미번역\] [F-Algebras](https://bartoszmilewski.com/2017/02/28/f-algebras/)
9. \[미번역\] [모나드를 위한 대수학(Algebra)](https://bartoszmilewski.com/2017/03/14/algebras-for-monads/)
10. \[미번역\] [End와 Coend](https://bartoszmilewski.com/2017/03/29/ends-and-coends/)
11. \[미번역\] [칸 확대(Kan Extension)](https://bartoszmilewski.com/2017/04/17/kan-extensions/)
12. \[미번역\] [강화된(Enriched) 카테고리](https://bartoszmilewski.com/2017/05/13/enriched-categories/)
13. \[미번역\] [Topoi](https://bartoszmilewski.com/2017/07/22/topoi/)
14. \[미번역\] [로비어 이론(Lawvere Theory)](https://bartoszmilewski.com/2017/08/26/lawvere-theories/)
15. \[미번역\] [모나드, 모노이드 그리고 카테고리](https://bartoszmilewski.com/2017/09/06/monads-monoids-and-categories/)

더 나은 타입 설정이 적용된 무료 [pdf 버전](https://github.com/hmemcpy/milewski-ctfp-pdf/)이 있으니 얼마든지 다운받으셔도 됩니다. 컬러 일러스트가 들어간 하드커버 버전은 [Blurb](http://www.blurb.com/b/9008339-category-theory-for-programmers)에서 구매하실 수 있습니다. 이 내용을 [강의하는 유튜브](https://www.youtube.com/playlist?list=PLbgaMIhjbmEnaH_LTkxLI7FMa2HsnawM_)도 있으니 언제든 수업을 들으실 수도 있습니다.

## 서문

> 얼마 전 프로그래머를 대상으로 하는 카테고리 이론에 대한 책의 아이디어가 떠올랐습니다. 정확히 말하자면 컴퓨터 과학자보다는 프로그래머를 대상으로 하려고 합니다. 미쳤다고 생각하실 수도 있는데 저도 아주 무섭습니다. 저는 과학과 엔지니어링 두 분야 모두에서 작업해본 경험이 있기 때문에 두 분야 사이에는 아주 큰 갭이 있다는 사실을 부정하지 못합니다. 그렇지만 저는 항상 무언가를 설명하려는 충동에 쌓여서 살아왔습니다. 저는 간단한 설명의 마스터인 리처드 파인만을 굉장히 존경합니다. 제가 파인만은 아니지만 최선을 다하겠습니다. 이 서문을 발행하는 것으로 그 첫걸음을 내딛으려고 합니다. 이 서문은 읽는 분들에게 카테고리 이론을 배워야하는 동기를 부여하고자 작성했습니다. 토론과 피드백은 환영입니다.

저는 다음 몇 문단에서 이 책이 여러분을 위해 쓰였음을 확신시키고, "여가 시간"에 수학의 가장 추상적인 부분 중 하나를 배워야하는 것에 대한 반대 의견을 모두 없애려고 합니다.

저의 낙관론은 몇 가지 견해에 기반합니다. 첫째, 카테고리 이론은 극도로 유용한 프로그래밍 아이디어의 보물상자입니다. 하스켈 프로그래머들은 카테고리 이론에 대해 오랫동안 접해왔는데, 이 아이디어는 천천히 다른 언어로 스며들었지만 그 과정은 너무나도 느렸습니다. 저는 전파 속도가 더 빨라졌으면 합니다.

두번째, 세상에는 아주 많은 종류의 수학이 있고 그들은 모두 다른 대상을 목표로 합니다. 미적분학이나 대수학에 대해 알레르기가 있으실 수도 있지만 그것이 카테고리 이론을 즐길 수 없는 이유가 될 순 없습니다. 저는 카테고리 이론이 프로그래머의 생각 방식에 잘 맞는 수학의 한 종류라는 것까지 주장하고 싶습니다. 그 이유는 카테고리 이론이 개별을 다루는 것보다 구조를 다루는 것에 더 적합하기 때문입니다. 카테고리 이론은 프로그램을 구성이 가능하게 만드는 구조의 종류를 다룹니다.


Composition is at the very root of category theory — it’s part of the definition of the category itself.

And I will argue strongly that composition is the essence of programming. We’ve been composing things forever, long before some great engineer came up with the idea of a subroutine.

Some time ago the principles of structured programming revolutionized programming because they made blocks of code composable.

Then came object oriented programming, which is all about composing objects.

Functional programming is not only about composing functions and algebraic data structures — it makes concurrency composable — something that’s virtually impossible with other programming paradigms.


Third, I have a secret weapon, a butcher’s knife, with which I will butcher math to make it more palatable to programmers.

When you’re a professional mathematician, you have to be very careful to get all your assumptions straight, qualify every statement properly, and construct all your proofs rigorously.

This makes mathematical papers and books extremely hard to read for an outsider.

I’m a physicist by training, and in physics we made amazing advances using informal reasoning.

Mathematicians laughed at the Dirac delta function, which was made up on the spot by the great physicist P. A. M. Dirac to solve some differential equations.

They stopped laughing when they discovered a completely new branch of calculus called distribution theory that formalized Dirac’s insights.


Of course when using hand-waving arguments you run the risk of saying something blatantly wrong, so I will try to make sure that there is solid mathematical theory behind informal arguments in this book.

I do have a worn-out copy of Saunders Mac Lane’s /Category Theory for the Working Mathematician/ on my nightstand.


Since this is category theory /for programmers/ I will illustrate all major concepts using computer code. You are probably aware that functional languages are closer to math than the more popular imperative languages.

They also offer more abstracting power.

So a natural temptation would be to say: You must learn Haskell before the bounty of category theory becomes available to you.

But that would imply that category theory has no application outside of functional programming and that’s simply not true.

So I will provide a lot of C++ examples. Granted, you’ll have to overcome some ugly syntax, the patterns might not stand out from the background of verbosity, and you might be forced to do some copy and paste in lieu of higher abstraction, but that’s just the lot of a C++ programmer.


But you’re not off the hook as far as Haskell is concerned.

You don’t have to become a Haskell programmer, but you need it as a language for sketching and documenting ideas to be implemented in C++.

That’s exactly how I got started with Haskell.

I found its terse syntax and powerful type system a great help in understanding and implementing C++ templates, data structures, and algorithms.

But since I can’t expect the readers to already know Haskell, I will introduce it slowly and explain everything as I go.


If you’re an experienced programmer, you might be asking yourself: I’ve been coding for so long without worrying about category theory or functional methods, so what’s changed?

Surely you can’t help but notice that there’s been a steady stream of new functional features invading imperative languages.

Even Java, the bastion of object-oriented programming, let the lambdas in C++ has recently been evolving at a frantic pace — a new standard every few years — trying to catch up with the changing world.

All this activity is in preparation for a disruptive change or, as we physicist call it, a phase transition.

If you keep heating water, it will eventually start boiling.

We are now in the position of a frog that must decide if it should continue swimming in increasingly hot water, or start looking for some alternatives.

<p align="center">
	<img src="img/img_preface_1.jpg" width="300" />
</p>


One of the forces that are driving the big change is the multicore revolution.

The prevailing programming paradigm, object oriented programming, doesn’t buy you anything in the realm of concurrency and parallelism, and instead encourages dangerous and buggy design.

Data hiding, the basic premise of object orientation, when combined with sharing and mutation, becomes a recipe for data races.

The idea of combining a mutex with the data it protects is nice but, unfortunately, locks don’t compose, and lock hiding makes deadlocks more likely and harder to debug.


But even in the absence of concurrency, the growing complexity of software systems is testing the limits of scalability of the imperative paradigm.

To put it simply, side effects are getting out of hand. Granted, functions that have side effects are often convenient and easy to write.

Their effects can in principle be encoded in their names and in the comments. A function called SetPassword or WriteFile is obviously mutating some state and generating side effects, and we are used to dealing with that.

It’s only when we start composing functions that have side effects on top of other functions that have side effects, and so on, that things start getting hairy.

It’s not that side effects are inherently bad — it’s the fact that they are hidden from view that makes them impossible to manage at larger scales.

Side effects don’t scale, and imperative programming is all about side effects.


Changes in hardware and the growing complexity of software are forcing us to rethink the foundations of programming.

Just like the builders of Europe’s great gothic cathedrals we’ve been honing our craft to the limits of material and structure.

There is an unfinished gothic [cathedral in Beauvais](http://en.wikipedia.org/wiki/Beauvais_Cathedral) , France, that stands witness to this deeply human struggle with limitations.

It was intended to beat all previous records of height and lightness, but it suffered a series of collapses.

Ad hoc measures like iron rods and wooden supports keep it from disintegrating, but obviously a lot of things went wrong.

From a modern perspective, it’s a miracle that so many gothic structures had been successfully completed without the help of modern material science, computer modelling, finite element analysis, and general math and physics.

I hope future generations will be as admiring of the programming skills we’ve been displaying in building complex operating systems, web servers, and the internet infrastructure.

And, frankly, they should, because we’ve done all this based on very flimsy theoretical foundations. We have to fix those foundations if we want to move forward.

<p align="center">
	<img src="img/img_preface_2.jpg" width="225" />
  <p>Ad hoc measures preventing the Beauvais cathedral from collapsing</p>
</p>

다음글: [Category: The Essence of Composition](https://bartoszmilewski.com/2014/11/04/category-the-essence-of-composition/)

---

공부 목적으로 번역을 하고 있습니다! 잘못된 점에 대한 이슈나 PR은 언제든지 환영합니다 :)
