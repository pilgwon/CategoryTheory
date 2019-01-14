# 프로그래머를 위한 카테고리 이론
원본 사이트: [Category Theory for Programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)

---

## 목차
### 파트 1
1. [합성(Composition)의 정수, 카테고리](/contents/part1/category-the-essence-of-composition.md)
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

합성(Composition)은 카테고리 이론 정의의 일부이며 이론의 근간이 되는 내용입니다. 저는 합성이 프로그래밍의 정수임을 강력히 주장할 것입니다. 엔지니어들이 서브루틴에 대한 아이디어를 생각하기 전까진 우리는 모든 것을 영원히 합성하고 있었습니다. 구조적 프로그래밍에 대한 규칙이 생긴 후에는 코드 블럭을 재사용할 수 있는 것 덕분에 프로그래밍에는 혁신이 일어났습니다. 그 다음엔 객체 합성에 대한 모든 것을 담은 객체 지향 프로그래밍이 생겨났습니다. 함수형 프로그래밍은 다른 프로그래밍 패러다임과 다르게 함수와 대수학적 데이터 구조뿐만 아니라 겉보기엔 불가능해보이는 것들도 합성할 수 있습니다.

세 번째로, 저에겐 비장의 무기인 부쳐스 나이프가 있습니다. 이 칼로 수학을 프로그래머들의 입맛에 맞게 잘라 붙일 것입니다. 혹시 당신이 전문적인 수학자라면 가정을 확실히 해두고 모든 내용이 적절한지 확인하고 모든 증명이 엄밀하게 구성해야 합니다. 이 문서는 외부인들에게 수학 논문과 책들이 엄청 어려워 보이도록 만들 것입니다. 제 전공은 물리학입니다. 물리학에선 비공식적 추론(informal reasoning)을 통해 놀라운 발전이 일어났습니다. 위대한 물리학자인 P. A. M. Dirac이 미분 방정식을 해결하기 위해 만든 Dirac 델타 함수를 보고 수학자들은 다들 웃지만 Dirac의 인사이트가 담겨있는 미적분학의 완전히 새로운 가지인 분포 이론을 발견한 후에는 웃을 수 없을 것입니다.

그래서 이 책의 비공식적 논쟁 뒤에는 탄탄한 수학적 이론을 바탕으로 하고자 합니다. 제 침실 탁자에는 Saunders Mac Lane의 _Category Theory for the Working Mathematician_ 이 있습니다.

이 문서는 _프로그래머를 위한_ 카테고리 이론이기 때문에 모든 주요한 개념을 컴퓨터 코드로 설명하려고 합니다. 그러면 함수형 언어가 유명한 명령형 언어보다 더 수학에 가깝다는 것을 이해하실 수 있을 것입니다. 또한 함수형 언어는 더 추상적인 파워를 제공합니다. 읽다보면 자연스럽게 카테고리 이론의 풍부한 내용을 모두 사용할 수 있기 전에 하스켈을 배워야겠다고 생각하실 수 있습니다. 하지만 그렇게 할 경우 카테고리 이론에는 함수형 프로그래밍 말고는 적용할 곳이 없다고 은연중에 느끼게 될텐데, 그것은 간단히 말하면 사실이 아닙니다. 그래서 저는 C++로 예제를 작성할 예정입니다.  못생긴 문법을 봐야하는 것은 극복해야 겠지만 패턴은 장황한 내용 사이에서 두드러지지 않을 것입니다. 그리고 더 고차원의 추상화에 복붙을 해야하는 경우도 있을 것입니다. 하지만 그건 그저 대부분의 C++ 프로그래머들이 하는 일일뿐입니다.

하지만 하스켈에 대한 고민을 내려놓지는 않아야 합니다. 하스켈 프로그래머가 될 필요는 없지만 적어도 C++로 구현할 아이디어를 스케치하고 문서화하기 위한 언어정도로는 알아야 합니다. 이게 제가 하스켈을 시작한 이유입니다. 간결한 문법과 강력한 타입 시스템이 C++ 템플릿과 데이터 구조, 알고리즘을 구현하고 이해하는데에 아주 많은 도움을 받았습니다. 그렇지만 독자들이 하스켈을 이미 알고있을거란 가정은 하지 않습니다. 저는 최대한 천천히 제가 아는 모든 것을 설명할 것입니다.


여러분이 경험이 있는 프로그래머라면 이런 궁금증이 생길 수 있습니다. "나는 오랫동안 카테고리 이론이나 함수형 메소드에 대해 고민하지 않고 개발해왔는데 뭐가 더 좋아지는거지?" 물론 도움이 안 될 수도 있지만 새로운 함수형 기능들이 명령형 언어를 침범하는 꾸준한 흐름이 있습니다. 심지어 객체 지향 프로그래밍의 최후의 보루인 자바에서도 최근에 C++의 람다가 제정신이 아닌 속도로 발전하고 있습니다. 빠르게 바뀌는 세상을 따라잡으세요. 이 모든 활동들은 파괴적인 변화에 대한 준비라고 생각합니다. 물리학자들이 사용하는 용어로는 상전이(Phase Transition)라고 합니다. 물에 꾸준히 열이 가해진다면 결국에는 끓을 것입니다. 우리는 지금 수온이 올라가고 있는 물에서 계속 헤엄칠지 아니면 다른 대안을 찾을지를 결정하는 중인 개구리와 같습니다.

<p align="center">
	<img src="img/img_preface_1.jpg" width="300" />
</p>

이러한 큰 변화를 이끌어 가는 주된 요소 중 하나는 멀티 코어 혁신입니다. 지금 우세한 프로그래밍 패러다임인 객체 지향 프로그래밍은 동시성(concurrency)과 평행성(parallelism)의 세상에서 여러분에게 아무것도 제공하지 않고 그 대신에 위험하고 결함이 있는 디자인을 제공합니다. 객체 지향의 기본 전제 중 하나인 데이터 숨김(Data Hiding)은 데이터 경쟁의 레시피가 되었습니다. 데이터로 뮤텍스(mutex)를 조합하는 아이디어는 좋지만 불행하게도 락(lock)은 합쳐지지 않고 락을 숨기는 것은 데드락을 만들어서 디버그를 더 어렵게 만듭니다.

그러나 동시성의 부재에도 소프트웨어 시스템의 복잡도는 점점 커지고 명령형 패러다임의 확장성을 테스트하고 있습니다. 쉽게 생각해보면 사이드 이펙트는 프로그래머의 손을 벗어나는 것이 좋습니다. 물론 사이드 이펙트를 가지고 있는 함수는 편하고 작성하기도 쉽습니다. 사이드 이펙트는 원칙적으론 이름과 주석으로 그 효과를 결정합니다. 이름이 SetPassword 또는 WriteFile인 함수는 명백히 어떤 상태를 변경하고 사이드 이펙트를 생성하며 우리가 자주 다뤄오던 것들입니다. 사이드 이펙트가 일어나는 다른 함수 전에 사이드 이펙트가 일어나면 일이 복잡해지기 시작합니다. 그렇지만 사이드 이펙트가 본질적으로 나쁜 것은 아닙니다. 뷰 뒤에 숨겨져있다는 사실이 스케일이 커질수록 더 다루기 힘들게 만듭니다. 사이드 이펙트는 스케일이 달라지지 않고 명령형 프로그래밍은 모든 것이 사이드 이펙트입니다.

하드웨어의 변경과 소프트웨어 복잡도의 증가는 프로그래밍의 토대를 다시 생각하도록 만듭니다. 유럽의 위대한 고딕 성당의 건축가들처럼 우리는 우리의 실력을 갈고 닦아서 재료와 구조의 한계까지 사용할 줄 알아야 합니다. 프랑스 북서의 [Beauvais의 고딕 성당](http://en.wikipedia.org/wiki/Beauvais_Cathedral)은 아직 미완성인데 이곳에 가면 인간이 한계에 도전했던 과정을 깊게 볼 수 있습니다. 이전에 있던 모든 높이와 광량에 대한 기록을 뛰어넘으려 했지만 수차례의 붕괴 위기에 고통받았습니다.
즉석으로 만든 철 막대기나 나무 지지대는 성당이 산산조각 나는 것을 막아줬지만 많은 것이 잘못됐다는 것은 명백합니다. 현대적인 측면에서 많은 고딕 구조물들이 현대의 재료 공학과 컴퓨터 모델링, 유한 요소 분석법 그리고 수학과 물리학의 도움 없이 성공적으로 완공됐다는 것은 기적입니다. 저는 미래 세대의 사람들이 우리가 복잡한 운영 체제, 웹 서버 그리고 인터넷 인프라구조를 만들기 위해 사용한 프로그래밍 스킬을 존경했으면 하는 바람이 있습니다. 그리고 솔직히 말하면 그럴 것입니다. 왜냐하면 우리가 하는 모든 것이 아주 조잡한 이론적인 토대에 기반하고 있기 때문입니다. 앞으로 나아가려면 그 토대를 고쳐야 합니다.

<p align="center">
	<img src="img/img_preface_2.jpg" width="225" />
  <p align="center">Beauvais 성당이 무너지는 것을 막아주는 지지대</p>
</p>

다음글: [Category: The Essence of Composition](/contents/part1/category-the-essence-of-composition.md)

---

공부 목적으로 번역을 하고 있습니다! 잘못된 점에 대한 이슈나 PR은 언제든지 환영합니다 :)
