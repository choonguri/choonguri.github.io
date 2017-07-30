---
layout: post
title: (발번역) 딥러닝의 미래
comments: true
---

*원문 : [https://blog.keras.io/the-future-of-deep-learning.html](https://blog.keras.io/the-future-of-deep-learning.html)*


이 포스트는 나(저자)의 [책](Deep Learning with Python:https://www.manning.com/books/deep-learning-with-python?a_aid=keras&a_bid=76564dff) 챕터9의 섹션3를 각색하였다.

딥러닝의 한계와 미래를 다룬 두개의 포스트 시리즈중에 하나이다. 당신은 첫번째 파트는 '[딥러닝의 한계](https://choonguri.github.io/2017/07/24/the-limitations-of-deep-learning.html)'에서 읽을 수 있다.


우리가 딥뉴럴넷이 한계들과 주변에 대한 현재 상태 그리고 어떻게 동작하는지에 대한 지식들이 주어졌을때, 중단기안에 어디로 향할지 예측할 수 있을까? 여기에 순전히 개인적인 생각인데, 나는 마법구슬이 없어서 내가 예상했던 많은 것들이 현실이 되지 못할 수 도 있다. 이것은 순전히 추측에 근거한 포스트이다. 나는 그것들이 미래에 완벽하게 옳다고 입증될 것이라고 기대하기 때문이 이러한 예측들을 공유하는 것은 아니다. 다만 그것들이 현재로서는 흥미롭고 실용적이기 때문이다.

개괄적으로 말하자면, 내가 바라보는 주된 방향은 다음과 같다. 

* 범용 컴퓨터 프로그램에 가까운 모델은 현재 미분가능한 계층들보다 더 풍부한 기초 위에서 구축된다.-이것이 현재 모델의 근본적인 약점인 추론과 추상(관념)에 도달할 수 있는 방법이다.
* 위의 것을 가능케 하는 새로운 형태의 학습-모델을 단지 미분가능한 변형에서 벗어나게 허용하는 것
* 개발자가 덜 집중할 수 있는 모델-끊임없이 페달을 굴리는 것이 당신의 직업이 되어서는 안된다.
* 사전에 학습된 피쳐들과 구조를 보다더 체계적인 재사용; 재사용 가능하고 모둘화 프로그램의 서브루틴 기반의 메타 학습 시스템

덧붙여서, 이러한 고려사항들은 지금까지 딥러닝의 빵과 버터였던 지도학습 종류에 특화된 것이 아님을 유념해라. 더 정확히 말하자면, 그것들은 비지도 학습, 셀프 지도학습, 강화학습을 포함한 머신러닝의 어떤 형태에도 적용할 수 있다. 당신의 라벨이 어디에서 왔고 당신이 하는 트레이닝 과정이 무엇인지는 근본적으로 중요한 것이 아니다. 머신러닝의 다른 갈래들은 단지 같은 구조의 다른 단면일 뿐이다.

들어가 보자.

# 프로그램으로서의 모델



이전 포스트에서 아시다시피, 머신러닝 분야에서 기대할 수 있는 필요한 변혁적인 발전은 순수하게 *패턴인식을 수행*하는 모델과는 멀어지고 단지 *지역적 일반화*만 할 수 있는 것에서 *추상(관념)*과 *추론*이 가능한 *극단적 일반화*를 할 수 있는 모델로 향하는 것이다. 현재 추론의 기본 형태가 가능한 AI 프로그램들은 인간 프로그래머에 의해 모두 하드 코딩되어 있다. (즉, 검색 알고리즘과 그래프 계산, 포멀한(formal) 로직 등과 관련된 소프트웨어) 딥마인드의 알파고에서 예를 들자면, 디스플레이 위의 모든 "지능"은 전문 프로그래머들에 의해 설계되고 하드 코딩(예:Monte-Carlo tree search)되어있다. 데이터로 부터의 학습은 단지 전문화된 서브모듈(가치망과 정책망)에서 이루어진다. 그러나 미래에는 이러한 AI 시스템들이 인간의 개입 없이 충분히 배울 수 있을 것이다.

이러한 것들이 일어날 수 있는 길은 무엇일까? RNNs와 같이 잘 알려진 네트웍 형태를 생각해보자. 중요하게, RNNs는 피드포워드(feedforward) 네트웍보다 좀더 적은 제약을 가지고 있다. RNNs는 단순한 기하학적 변형 이상이기 때문이다(그것들은 *for 루프안에서 반복적으로 적용*되는 기하학적 변형이다). temporal for 루프는 그 자체가 인간 개발자들에 의해서 하드 코딩되어 있다. 그것은 네트웍의 기본적인 가설이다. 당연히, RNNs는 주로 수행하는 각 단계가 여전히 미분가능한 기하학적 변형이고, 각 단계에서 단계로 정보를 전달하는 방법이 연속적인 기하 공간(상태 벡터)이기 때문에 표현할 수 있는 범위가 여전히 극히 제한적이다. 자, for루프(단지 하드 코딩된 기하학적 메모리를 사용한 단일 하드코딩된 루프가 아닌) 처럼 프로그래밍 프리미티브와 유사한 방법으로 "증강된(augmented)" 뉴럴 네트웍, 더 정확히 말하면, 그 모델이 if문, while문, 변수 생성, 장기 기억 저장소, 소팅 수행, 고급 데이터구조(list, graph, hashtable 등등)와 같은 처리기능의 확장을 다루는데 자유로울 수 있는 방대한 프로그래밍 프리미티브 세트 모델을 생각해보자. 그러한 네트웍이 표현할 수 있는 이 프로그램 공간은 현재의 딥러닝 모델들로 표현되어질 수 있는 것보다 훨씬 더 넓고, 이 프로그램의 일부는 더 탁월한 일반화의 능력을 달성할 수 있을 것이다.

한 마디로 말해서, 우리는 한손에 "하드 코딩된 알고리즘 지능"을, 다른 한손에는 "학습된 기하학적 지능(딥러닝)"을 가지고 떠날 것이다. 우리는 대신에 *추상과 추론* 기능을 제공하는 혼합된 포멀한 알고리즘 모듈들과 *인포멀한 직관* 및 *패턴 인식* 기능을 제공하는 기하학적 모듈들을 가지게 될 것이다. 이 전체 시스템은 거의 혹은 전혀 인간의 간섭없이 학습되어 질 것이다.

내가 생각하는 AI와 관련된 하위 분야는 프로그램 합성, 특히 뉴럴 프로그램 합성에 관한 것일 지도 모른다. 프로그램 합성은 가능한 프로그램의 큰 공간을 탐구하기 위한 검색 알고리즘(유전자 프로그램 처럼 유전자 탐색이 가능한)을 사용하여 간단한 프로그램들을 자동으로 생성해내는 것에 있다. 이 검색은 프로그램이 필요한 사양들에 대한 매칭을 발견했을 때 멈추며, 종종 입-출력 쌍으로 제공되어진다. 보시다시피, 우리가 입-출력을 매칭하고 새로운 입력에 대해 일반화하는 *프로그램*을 발견한, 입-출력 쌍의 학습데이터가 주어지는 머신러닝이 크게 연상된다. 차이점은 하드코딩된 프로그램(뉴럴넷)으로 파라미터를 학습하는 대신에, 우리는 별도의 검색 프로세스를 통해 *소스코드*를 생성한다. 

![img1]({{ site.url }}/assets/future-of-dl-01.png)

**그림:** *기하학적 기초요소(패턴 인식, 직관)와 알고리즘 기초요소(추론, 검색, 기억)에 의존하는 학습된 프로그램*

# 오차역전파법과 미분가능한 계층들 저너머에

머신러닝 모델이 더욱 더 프로그램처럼 되어 간다면, 더 이상 미분가능하지 않을 것이다. 틀림없이 이 프로그램들은 서브루틴과 같은 연속적인 기하학 계층을 여전히 활용할 것이며 미분 가능할 것이지만, 전체로서의 모델은 그렇지 않을 것이다. 결과적으로, 고정되고 하드코딩된 네트웍안에서 가중치(weight)를 조절하기 위한 오차역전파법을  사용하는 것은 미래에 학습 모델을 선택하기 위한 방법이 될 수 없다. 적어도 전체적인 이야기가 될 수 없다. 우리는 미분가능하지 않은 시스템을 효과적으로 학습시키기는 것을 알아내야만 한다. 현재의 접근법에는 유전자 알고리즘, "진화 전략", 특정 강화 학습 방식, ADMM(alternating direction method of multipliers)이 포함된다. 자연스럽게, 경사하강법은 어디에도 쓰이지 않게된다. 경사 정보는 미분가능한 파라미터 함수들을 최적화하는데 유용할 것이다. 그러나 우리의 모델들은 단지 미분가능한 파라미터 함수들보다 확실히 점점 더 어마어마해지고, 따라서 자동 개발("머신러닝"에서의 "러닝")은 오차역전파법보다 더 필요할 것이다.

게다가, 오차역전파법은 종단간(end-to-end)이며 좋은 연결 변환을 학습하는데 큰 도움이 되지만, 딥뉴럴넷의 모듈화를 충분히 사용하지 못하기 때문에 계산에 있어서 약간의 비효율이 있다. 더 효율적인 것을 만들기 위한 보편적인 제조법(모듈화와 계층)이 하나 있다. 그래서 우리는 계층적 방식으로 구성된 그들 사이에 어떤 동기화 메커니즘을 가지는 분리된 트레이닝 모듈들을 도입함으로써 오차역전파법 자체를 좀더 효율적으로 만들 수 있다. 이 전략은 딥마인드의 최근 연구인 "synthetic gradients"에 다소 반영되어 있다. 나는 가까운 미래에 이 라인들을 따라 더 많은 연구가 진행되길 기대한다.

경사하강법을 사용하지 않는 효율적인 검색 프로세스를 사용하여 전체적으로 미분가능하지 않은 모델들이 학습되고 성장되는 미래를 상상할 수 있다. 반면에 미분가능한 부분들은 좀더 효율적인 오차역전파 버전을 사용한 경사하강법의 이점을 갖고 더 빨리 학습되어질 수 있을 것이다.

# 자동화(automated) 머신러닝

미래에는 모델의 아키텍쳐가 엔지니어에 의해 만들어지는 것 보다는 학습되어 질 것이다. 학습 아키텍쳐는 보다 풍부한 프리미티브와 프로그램과 유사한 머신러닝 모델을 사용하여 자동으로 진행된다. 

현재, 엔지니어들이 매우 의욕이 있다면 아키텍쳐나 돌아가는 모델(또는 최첨단 모델)을 만들기 위한 딥뉴럴넷의 하이퍼 파라미터의 튜닝에 힘써야 하지만, 딥러닝 엔지니어의 대부분의 일들이 파이썬 스크립트로 데이터를 전처리하는데에 있다. 말할 필요 없이 최적의 구조가 아니다. 다만, AI가 도움을 줄 수 있다. 불행하게도 데이터 전처리 부분은  엔지니어가 달성하려는 것에 대한 완벽한 이해와 도메인 지식들이 자주 요구되기 때문에  자동화하기 어렵다. 그러나 하이퍼파라미터의 튜닝은 간단한 검색 절차이며, 이 (튜닝되는 네트웍의 손실함수에 의해 정의되는)케이스에서 엔지니어가 달성하려고 하는 것을 이미 알고있다. 모델 노브(knob) 튜닝의 대부분을 처리할 기본 "AutoML" 시스템을  설정하는 것은 이미 일반적인 작업이다. 나는 심지어  카글(kaggle) 대회에서 우승하기 위해 몇년을 투자했다.

대부분의 기본 단계에서 이러한 시스템은 스택의 계층 개수, 순서, 각 계층의 필터 혹은 유닛들의 수를 간단히 조정한다. 이것은 "Hyperopt"(7장-[Deep Learning with Python](https://www.manning.com/books/deep-learning-with-python?a_aid=keras&a_bid=76564dff))과 같은 라이브러리를 통해 수행되어 진다. 그러나 우리는 또한 더 큰 야망을 가져야 하고, 가능한 한 적은 제약으로 밑바닥에서 부터 적절한 아키텍쳐를 배우는 것을 시도할 수 있다.

다른 중요한 AutoML의 방향은 모델 가중치와 함께 모델 구조를 학습하는 것이다. 우리가 매번 조금 다른 구조에 대해 밑바닥부터 새로운 모델을 트레이닝하는 것은 너무나 비효율적이기 때문에, 파워풀한 AutoML 시스템은 모델의 피쳐들을 트레이닝 데이터의 오차역전파법을 통해 튜닝하는 동시에 아키텍쳐를 발전시켜 나가간다. 이렇게 해서 모든 중복된 계산을 없앤다. 이러한 접근방법은 내가 이글을 쓰고 있을때 이미 나오기 시작했다.

이것들이 일어나기 시작할때, 머신러닝 엔지니어들은 실망하지 않을 것이다. 오히려 엔지니어들은 가치 창출의 체인을 더 높이 올릴 것이다. 그들은 비지니스 목표를 진정으로 반영하는 복잡한 손실함수(loss function)를  만들고, 그 모델들이 배포되는 디지털 에코시스템에 어떻게 충격을 주는지 깊은 이해를 하는데 더 노력을 시작할 것이다(예를 들면, 모델의 예측을 소비하고 모델의 학습데이터를 생성하는 사용자). 현재로서는 가장 큰 회사만 고려할 수 있는 문제이다.

# 평생 학습과 개별 서브루틴의 재사용

모델이 더 복잡해지고 더 풍부한 알고리즘 기반 위에 만들어진다면, 이 증가된 복잡성은 우리가 새로운 작업 혹은 새로운 데이터셋으로 매번 밑바닥부터 새롭게 학습하는 모델 보다 오히려 작업들 사이에서 더욱 재사용이 요구될 것이다. 확실히 약간의 데이터셋은 밑바닥부터 새롭게 복잡한 모델을 개발하기 위한 충분한 정보를 담고 있지 않으며, 이전에 발생한 데이터 셋에서 제공되는 정보를 활용하는 것이 필요하다. 새로운 책을 볼 때마다 처음부터 영어를 배우지 않는 것과 흡사하다(일어나지 않을 일이다). 그에 반해 매번 새롭게 밑바닥 부터 학습된 모델들은 현재 작업과 이전에 접한 작업들 사이에 많은 중복이 있어 매우 비효율적이다.

게다가, 최근 몇년간 반복되어 왔던 주목할 만한 관찰은 동시에 여러개가 느슨하게 연결된 태스트들을 수행하는 "같은" 모델을 학습하는 것이 각 작업에서 더 나은 성능을 얻을 수 있다. 예를 들면, 영어를 독일어로 번역하고 불어를 이탈리아어로 번역하는 것 두가지를 커버하는 뉴럴 머신 번역 모델을 학습하는 것은 각각을 학습하는 것보다 성능이 더 좋다는 결과가 있다. 같은 컨볼루션 기반을 공유하는 이미지 분할 모델과 이미지 분류를 함께 학습하는 것이 두 작업을 학습하는 것보다 좋은 결과를 가져온다. 기타 등등. 이것은 상당히 직관적이다. 거기에는 외견상 연결된 작업 사이에 항상 겹치는 *어떤* 정보가 있으며, 함께 하는 모델은 단지 특정 작업만 학습한 모델보다 각자의 개별적인 작업에 관해 더 많은 정보에 접근한다.

우리가 현재 작업 전반에 걸쳐 재사용되는 모델의 라인을 따라 수행하는 것은 공통 기능을 수행하는 모델들을 위한 사전 학습된 가중치(시각화된 피쳐 추출처럼)를 활용하는 것이다. 당신은 5장에서 보았다. 미래에 나는 아주 흔하게 되는 일반화된 버전을 기대한다. 우리는 단지 이전 피쳐들을 활용하는 것 뿐만 아니라 모델 아키텍쳐와 학습 과정도 활용한다. 모델들은 프로그램처럼 될것이기 때문에 우리는 인간 프로그래밍 언어에서 발견되는 함수들과 클래스들 같은 *프로그램 서브루틴*을 재사용하기 시작할 것이다.

오늘날 (엔지니어가 특정 문제-파이썬에서 HTTP쿼리-를 푸는)소프트웨어 개발 과정을 생각해 보면, 그들은 추상적이고 재사용 가능한 라이브러리 같은 것을 포함할 것이다. 앞으로 유사한 문제에 직면한 엔지니어들은 기존에 있는 라이브러리들을 쉽게 찾고 다운로드 하고 그들의 프로젝트에 사용할 수 있다. 같은 방법으로 미래에 메타 러닝 시스템들은 고수준의 재사용 가능한 블럭 라이브러리를 통해 새로운 프로그램을 구성 할 수 있을 것이다. 그 시스템이 여러가지 다른 작업을 위해 개발된 유사한 프로그램 서브 루틴을 찾을 경우 경우, 서브 루틴의 "추상적"이고, 재사용 가능한 버전의 서브 루틴을 찾아내고 글로벌 라이브러리에 저장할 것이다. 이러한 과정은 *추상화*를 위한 기능과 "극단적 일반화"를 달성하기 위해 필요한 콤포넌트를 구현할 것이다.(서로 다른 작업과 도메인에서 유용하다고 판명된 서브루틴은 문제 해결의 일부 측면을 "추상화"한다고 할 수 있다. "추상화"의 정의는 소프트웨어 개발에서의 추상화 개념과 유사하다. 이 서브루틴들은 기하학적이거나(사전에 학습된 결과의 딥러닝 모듈들) 또는 알고리즘(현대 소프트웨어 개발에서 다루어지는 라이브러리들과 가깝다)이 될 수 있다.

![img2]({{ site.url }}/assets/future-of-dl-02.png)

**그림:** *재사용가능한 프리미티브(알고리즘과 기하학)를 사용하여 작업별 모델을 빠르게 개발할 수 있는 메타-러너(meta-learner), 이렇게 하여 "극단적 일반화"를 달성한다.*

# 정리 : 장기적인 비전

요약하면, 여기에는 머신러닝에 대한 나의 장기적인 비전이 있다.

* 모델들은 더욱 더 프로그램 처럼 될 것이고, 우리가 현재 다루는 입력 데이터들에 대한 연속적인 기하학적 변환을 훨씬 능가하는 기능을 갖게 될 것이다. 이 프로그램들은 틀림없이 인간들이 그들의 환경과 스스로를 유지하려는 추상적인 마음의 모델들에 더욱 가까워 질 것이며, 그것들은 풍부한 알고리즘적 특성 때문에 더 강력한 일반화가 가능할 것이다.
* 특히, 모델들은 포멀한 추론, 검색, 추상화할 수 있는 기능을 제공하는 *알고리즘 모듈*과 인포멀한 직관, 패턴 인식 능력을 제공하는 *기하학적 모듈*과 함께 섞일 것이다. 알파고는 상징적 AI와 기하학적 AI 사이의 이러한 조화가 어떻게 생겼는지에 대해 이미 그 예를 보여줬다.
* 그것들은 재사용가능한 서브루틴의 글로벌 라이브러리에 저장된 모듈 일부를 사용하면 인간 엔지니어에 의한 것 보다는 자동으로 *발전*할 것이다(라이브러리는 수천개의 이전 작업과 데이터셋에서 고성능 모델의 학습으로 발전된다). 공통된 문제해결 패턴들은 메타-러닝 시스템에 의해 식별되어지고, 그것들은 재사용가능한 서브루틴(현대의 소프트웨어 개발에서의 함수와 객체와 매우 같은)으로 바뀌고 글로벌 라이브러리에 추가될 것이다. 이를 통해 *추상화* 기능을 구현한다.
* 이 글로벌 라이브러리, 연관된 모델 성장 시스템은 인간과 비슷한 형태의 "극단적 일반화"를 달성할 수 있을 것이다. 새로운 작업, 새로운 상황이 주어지면 이 시스템은 매우 적은 데이터를 사용하여 그 작업에 적합한 새로운 작업 모델을 모을 수 있을 것이다. 일반화가 잘된 풍부한 프로그램과 같은 프리미티브와 유사한 작업에 대한 광범위한 경험 덕분이다. 이전에 많은 게임 경험을 가지고 있어서 매우 적은 시간에 인간이 새로운 비디오 게임을 배울 수 있는 것과 같은 방식으로, 이전 경험으로 부터 파생된 모델들은 자극과 행동사이의 기본 매핑보다는 추상적이고 프로그램과 유사하기 때문이다.
* 이러한, 끊임없이 배우는 모델 성장 시스템은 AGI(Artificial General Intelligence)로 이해할 수 있다. 하지만 로봇이 세상의 파멸을 가져올 것이라는 어떠한 특이점주의도 기대하지 마라. 그것은 순전히 판타지이며, 정보와 기술에 대한 일련의 깊은 오해로 부터 비롯된 것이다. 이 비평은 여기에 포함하지 않는다.

*[@fchollet](https://twitter.com/fchollet), May 2017*
 