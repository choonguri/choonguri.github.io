---
layout: post
title: (발번역)Reactive Programming에 대해 알아야할 5가지
---

_원문 : [https://developers.redhat.com/blog/2017/06/30/5-things-to-know-about-reactive-programming/](https://developers.redhat.com/blog/2017/06/30/5-things-to-know-about-reactive-programming/)_
 
 
**Reactive**, 너무나 감당하기 어려운 단어이다. 근래에 많은 것들이 마법처럼 Reactive로 만들어져 나온다. 이글에서 **Reactive Programming**, 즉 비동기 데이터 스트림을 중심으로 구현한 개발모델에 대해 이야기하려 한다.
 
나는 여러분들이 첫 Reactive 어플리케이션을 빨리 만들고 싶다는 것을 알고 있지만, 그전에 알아야 할 것들이 있다. Reactive Programming은 코딩 방식을 변화시킨다. 기차에서 뛰어 내리기전에 당신이 어느쪽을 향하는지 알아야하는 것처럼 말이다.
 
이 글에서 우리는 Reactive Programming이 당신에게 어떤 변화를 주는지 알기위해 5가지를 언급한다.
 
# 1. Reactive Programming은 비동기 데이터 스트림을 사용한다.
 
Reactive Programming에서 데이터 스트림은 당신의 어플리케이션의 중요한 뼈대가 된다. 이벤트, 메시지, 호출, 심지어 오류까지 데이터 스트림에 의해 운반되어진다. Reactive Programming으로 당신은 데이터 스트림을 관찰하고 값을 내보낼때 반응한다. 그래서 코드안에서 어떤 데이터 스트림이나 어떤 것으로 부터 온 것들(클릭이벤트, http request, 수집된 메시지, 사용가능한 알림, 값의 변화, 캐시 이벤트, 센서에서 측정된 값 등 한마디로 변화하거나 생길 수 있는 모든 것들)을 데이터 스트림으로 만들것이다. 이것은 어플리케이션에 재미있는 영향을 주며 본질적으로 비동기식이 되어가고 있다는 뜻이다.

![img1]({{ site.url }}/assets/reactive-programming-01.png)
 
Reactive eXtension(이하 RX)은 '*관찰 가능한 시퀀스를 사용하여 비동기, 이벤트 기반의 프로그램들을 만든다.*'는 원리의 Reactive Programming을 구현한 것이다. RX를 사용하면 Observables라는 데이터 스트림을 생성하고 subscribe할 수 있다. Reactive Programming은 컨셉에 관련된 것이며 RX는 당신에게 놀라운 툴박스를 제공한다. *observer, iterator 패턴들*과 함수형 표현 등이 조합되어 RX는 당신에게 슈퍼파워를 선사한다. 데이터 스트림을 결합, 병합, 필터링, 변환, 생성할 수 있는 능력을 가지게 된다. 다음 그림은 자바에서 RX 사용법을 보여준다.([https://github.com/ReactiveX/RxJava](https://github.com/ReactiveX/RxJava)).

![img1]({{ site.url }}/assets/reactive-programming-02.png)

RX는 단지 오늘날 가장 많이 사용되는 Reactive Programming의 원리만을 구현한 것이 아니다. 이 글의 나머지 부분은 RxJava를 사용한다.
 
# 2. Observables은 cold 또는 hot이 될 수 있으며, 이것은 중요한 부분이다.
 
이 시점에서, 당신은 프로그램에서 처리할 다른 스트림들이 무엇인지 알려고 할것이다. 하지만 거기에는 두가지 종류(hot과 cold)의 스트림이 있다. 이 두개의 다른점을 이해하는 것이 Reactive Programming을 성공적으로 사용할 수 있게 해주는 중요한 키(key)이다.
 
cold observables는 게으르다. 누군가 그것에 대한 *관찰*(RX에서는 *subscribe*)을 시작하지 않는다면 아무것도 하지 않는다.
그것들은 단지 소비되어질때 실행된다. Cold 스트림은 비동기 작업을 나타내는 데 사용되어지는데 예를들면 누군가 그 결과에 관심가질 때까지는 실행되지 않는다. 파일 다운로드도 그 예가 될 수 있다. 그 데이터로 뭔가를 하지 않는다면 바이트를 읽지 않는다. cold 스트림이 만든 데이터는 구독자들(subscribers) 사이에 공유되지 않으며, 당신이 구독할때 모든 항목을 얻을 수 있다.
 
hot 스트림은 주식시세 혹은 센서나 사용자가 보낸 데이터 같은 것을 구독하기 전에 활성화된다. 이 데이터는 개별 구독자와 별개이다.
옵저버가 hot observable을 **구독한 이후**에 내보낸 스트림 안의 모든 값들을 얻게된다. 이 값들은 모든 구독자들과 공유가 된다.
예를들면, 어느 누구도 온도계를 지켜보지 않아도 현재 온도는 측정되고 표시된다. 구독자가 스트림에 등록하면 자동으로 다음 조취를 취한다.
 
당신의 스트림들이 hot인지 cold인지 이해하는게 왜 굉장히 중요한 걸까? 당신의 코드가 운반된 정보들을 소비하는 방식을 변화시키기 때문이다. hot observable을 구독하지 않는다면 당신은 데이터를 받지 않으며 이 데이터는 손실된다.
 
# 3. 잘못 사용된 비동기성
 
Reactive Programming *정의*에서 '비동기'라는 중요한 단어이 있다. 스트림에서 비동기적으로 데이터가 내보내질때 당신에게 알려진다는 의미는 주 프로그램 흐름과 독립적이라는 것이다. 데이터 스트림 중심으로 프로그램을 설계하면 비동기 코드를 작성하게 된다.(스트림이 새로운 정보를 내보낼때 호출 코드를 작성한다) 쓰레드, 블럭킹 코드, 사이드 이펙트들은 이 맥락에서 매우 중요한 문제다. 사이드 이펙트부터 시작해보자.
 
사이드 이펙트가 없는 함수들은 오로지 인자들과 리턴값에 의해 프로그램의 일부와 상호 작용한다. 사이드 이펙트들은 많은 경우에 매우 유용하면서도 피할수 없다. 그러나 그것들은 보이지 않는 위험성이 있다.
Reactive Programming을 할때 불필요한 사이드 이펙트들을 피해야하며 분명한 의도를 가져야 한다. 그래서 불변성을 수용하고 사이드 이펙트를 억제한다. 몇몇 사례들이 정당화되는 반면에 사이드 이펙트의 남용은 대혼란(Thread safety)으로 이어진다.
 
두번째 중요한 점은 '쓰레드'이다. 스트림을 관찰하고 관심이 있는 어떤  일이 일어났을때 알려주는데 매우 좋다. 하지만 당신을 부르는 사람을 잊어버리면 안된다. 또는 당신의 함수들을 실행시킨 쓰레드에서 더 신중해야 한다. 당신의 프로그램에서 너무 많은 쓰레드를 사용하지 말것을 강력히 추천한다. 여러개의 쓰레드에 의존하는 비동기 프로그램은 종종 교착 상태의 엔딩을 맞이하는, 풀기어려운 동기화 퍼즐이 된다.
 
세번재 포인트는 '막지말것'(never block)이다. 당신을 부르는 쓰레드는 당신의 것이 아니기 때문이다. 만약 다른 항목들을 내보내는 것을 것을 피할 수 있다면, 버퍼가 가득 찰 때까지 그 항목들은 버퍼링 된다. RX와 비동기 IO를 섞어 쓰면 non-blocking 코드를 넣어야만 한다. 더 많은 것을 알고 싶으면 반응성과 비동기성을 잘 사용하게 해주는 reactive 툴킷인 이클립스 Vert.x 찾아보면 된다. 예를 들면 다음은 Vert.x 웹클라이언트에서 서버에 있는 JSON 문서를 가져와서 *name*을 출력하는 RX API 코드이다.

```java
client.get("/api/people/4")
.rxSend()
.map(HttpResponse::bodyAsJsonObject)
.map(json -> json.getString("name"))
.subscribe(System.out::println, Throwable::printStackTrace);
``` 
 
# 4. 단순함을 유지해라
 
아시다시피 "*큰 힘은 큰 책임에서 나온다.*"라는 말이 있다. RX는 매우 많은 좋은 함수들을 제공하고, 어둠으로 쉽게 이끈다. *flapmap*, *retry*, *debounce*, *zip*을 연결하면 당신은 닌자처럼 느낄것이다. **그러나** 결코 잊으면 안되는 것은 좋은 코드는 누군가에게 읽기 쉬운 코드여야 한다는 것이다.

다음 코드를 보자.

```java
manager.getCampaignById(id)
  .flatMap(campaign ->
    manager.getCartsForCampaign(campaign)
      .flatMap(list -> {
        Single<List<Product>> products = manager.getProducts(campaign);
        Single<List<UserCommand>> carts = manager.getCarts(campaign);
        return products.zipWith(carts, 
            (p, c) -> new CampaignModel(campaign, p, c));
      })
     .flatMap(model -> template
        .rxRender(rc, "templates/fruits/campaign.thl.html")
        .map(Buffer::toString))
    )
    .subscribe(
      content -> rc.response().end(content),
     err -> {
      log.error("Unable to render campaign view", err);
      getAllCampaigns(rc);
    }
);
```
위와 같은 코드는 이해하기 힘들까? 여러 비동기 작업(*flatmap*)을 연결하고 다른 작업(*zip*) 셋에 포함시킨다. Reactive Programming 코드는 먼저 마인드 쉬프트(mind-shift)가 필요하다. 비동기 이벤트가 통지된다. 그러면 API를 이해하기 힘들수 있다([연산자 목록](https://github.com/ReactiveX/RxJava/wiki/Alphabetical-List-of-Observable-Operators) 참고). 남용하지 말고, 코멘트를 달거나, 설명하거나, 다이어그램을 그려라(당신이  asciiart 예술가라고 확신한다). RX는 강력하지만 그것을 오남용 하거나 설명하지 않는다면 동료들을 꽥꽥 거리게 만들것이다.
 
 
# 5. Reactive Programming != Reactive system
 
아마도 가장 혼란스러운 파트일거 같다. **Reactive Programming을 사용하는 것은 Reactive System을 구축하는 것이 아니다.** [Reactive manifesto](http://www.reactivemanifesto.org/)에 정의된 것처럼 Reactive System은 *반응형 분산 시스템을 만들기 위한* 설계 스타일이다. Reactive System은 분산 시스템으로 간주 될 수 있다. Reactive Programming은 4가지 속성으로 정의되어 진다.

- **Responsive** : 납득할만한 시간내에 요청을 처리해야 한다.
- **Resilient** : 오류 상태에서도 responsive 해야 한다.
- **Elastic** : 어떠한 로드 상황에서도 responsive 해야 한다.(스케일업, 스케일다운 등)
- **Message driven** : Reactive System 컴퍼넌트들은 비동기 메시지 전송으로 상호 작용한다.
 
Reactive System의 기본 원리들이 간단함에도 불구하고, 그중에 하나를 만드는 것은 어렵다. 일반적으로 각 노드는 비동기 Non-blocking 개발 모델, task-based 동시성 모델을 수용해야 하며
non-blocking I/O를 사용해야만 한다. 이점들을 처음에 생각하지 않는다면 빠르게 스파게티 접시가 될것이다.
 
Reactive Programming과 RX는 비동기 괴물을 길들이게 할 수있는 개발 모델을 제공한다. 이것을 폭넓게 사용한다면 당신의 코드는 readable, understandable이 유지될것이다. 그러나 Reactive Programming 사용은 당신의 시스템을 Reactive System으로 변경시키는 것은 아니다. Reactive System은 다음 단계이다.
 
# 결론
 
우리는 이 글의 마지막에 도달했다. 당신이 더 가길 원하거나 reactive에 관심이 있다면 나는 당신이 Eclipse Vert.x([반응형 분산 시스템을 만들 수 있는 툴킷](http://vertx.io/))와 [Reactive Microservices in Java minibook](https://developers.redhat.com/promotions/building-reactive-microservices-in-java/)을 찾아보라고 말하고 싶다.
Vert.x와 RX의 조합은 당신의 reactive 슈퍼파워를 촉발시킨다.
당신이 Reactive Programming을 사용할 수 있는 것 뿐만 아니라 Reactive System을 구축할 수 있고 스릴넘치고 성장하는 생태계에 다가갈 수 있다.