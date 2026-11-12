## **Deployment 기본 개념**

K8S Document를 보면 Deployment에 대한 설명을 참 대충 해놨다.

"Deployment는 ~~~ 이다" 라는 정의도 안해줌. 그래서 딱히 읽어볼 필요는 없다.

> 디플로이먼트는 애플리케이션 워크로드를 실행하기 위해 여러 파드 집합을 관리하며, 보통 스테이트리스(stateless) 애플리케이션을 대상으로 한다.
>
> 디플로이먼트(Deployment) 는 파드와 레플리카셋(ReplicaSet)에 대한 선언적 업데이트를 제공한다.
>
> 디플로이먼트에서 의도하는 상태 를 설명하고, 디플로이먼트 컨트롤러 (원문, Controller)는 현재 상태에서 의도하는 상태로 비율을 조정하며 변경한다. 새 레플리카셋을 생성하는 디플로이먼트를 정의하거나 기존 디플로이먼트를 제거하고, 모든 리소스를 새 디플로이먼트에 적용할 수 있다.

그냥 Deployment가 언제 필요한 지를 보면서 개념을 잡는게 좋다.
- Python으로 웹서버 하나 만들고 Container로 배포해서 서비스를 시작했다.
- Python 웹서버 사용량이 많아져서 여러개의 인스턴스를 더 많이 배포해야 한다. (이거까지는 Replica Set으로 할 수 있다.)
- Python 웹서버 변경이 필요해서 v2를 출시했고, Docker Registry에 v2 이미지를 올렸다.
- v2로 업그레이드할 때 한꺼번에 하면 위험하다. v2에 문제가 있을 경우 모든 웹서버 인스턴스에 접속이 불가능해진다.
그래서 한개씩 한개씩 v2로 업그레이드 하는게 좋다: **Rolling Update**
- v2로 업그레이드 하다가 예상치 못한 에러가 발생하면 바로 직전으로 **Roll Back**할 수 있어야 한다.
- 여러개의 변경사항을 동시에 적용해야할 때가 있다. (웹 서버 v2로 업그레이드 + scale out + 리소스 할당 변경)
하지만 각각의 변경 사항을 명령어 하나씩 실행한 직후에 적용되는 방식으로 하면 위험하다.
모든 변경 사항이 한꺼번에 적용되는 것이 유리하다.

위와 같은 작업을 가능하게 하는 것이 Deployment다.

The deployment provides us with the capability to upgrade the underlying instances seamlessly using
rolling updates, undo changes, and pause and resume changes as required.

Deployment는 Pod와 ReplicaSet의 상위 Hierarchy에 있는 개념으로 보면 된다.

![](https://blog.kakaocdn.net/dna/BwPRY/dJMcaiczSz6/AAAAAAAAAAAAAAAAAAAAAAVXnEtGtko9AmQD3vLRU7NAKthHDSzDLcm_3T2q2JmT/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=8B%2F070%2BAGlOeUjmeUm0tzAptFEQ%3D)

Deployment 생성 방법

ReplicaSet과 거의 동일한데, kind만 Deployment로 써주면 된다.

![](https://blog.kakaocdn.net/dna/bq6Qga/dJMcag6PY6n/AAAAAAAAAAAAAAAAAAAAAGCDdvwNP3y-tdnWPa-c3bhI4__NWvQ7PVYr6F0_d4QJ/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=oYBXHkyRvNExbVGGNLEhNKiwUYo%3D)

생성해보면 Deployment가 생성되고

Deployment에 의해 ReplicaSet이 생성되고

ReplicaSet에 의해 Pod가 생성된다.

결과적으로 ReplicaSet과 유사한데 Deployment라는 object가 k8s cluster에 하나 더 생기는 셈이다.

![](https://blog.kakaocdn.net/dna/bP04aX/dJMcadh0BvB/AAAAAAAAAAAAAAAAAAAAAHGBeTgM06Ad9oVWYwjqqKubhbLM4Imz_tcAF1DqGZh4/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=7uxxex%2B51ujF1wn%2F9jWeKU04xjg%3D)

