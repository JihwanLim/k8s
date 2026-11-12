## **Pod 기본 개념**

K8S를 사용하는 궁극적인 목적은 컨테이너 형태의 Application을 서버에 쉽게 띄우기 위함이다.

서버(또는 machine이라고 하자)는 worker node이다.

K8S는 컨테이너를 노드에 직접 배포하지 않는다.

Pod라는 object로 감싸서(encapsulate) 컨테이너를 배포한다.

A pod is a single instance of an application.
A pod is the smallest object that you can create in Kubernetes.

![](https://blog.kakaocdn.net/dna/Fqa8Y/dJMcaip3wfe/AAAAAAAAAAAAAAAAAAAAAKgl7YDMVhLPunLXjFJIEaQtQwxG_RwJIHkvB8ahGy39/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=2bcOuB88Cv0ucF1mpExaOqRwe%2FA%3D)

Pod가 생성 가능한 가장 작은 단위의 Object이기 때문에 Scale In/Out이 Pod 단위로 이루어진다.

User가 많아져서 Applicaton Instance를 늘리려면(Scale Out) 같은 Pod에 컨테이너를 더 늘리는게 아니라 동일한 이미지의 컨테이너가 담긴 Pod를 늘린다.

Node에 더이상 Pod를 띄울 공간이 없다면 새로운 Node에 Pod를 늘린다.

이건 일반적인 1 Pod - 1 Container를 Scale In/Out 할 때에 대한 예시고 1 Pod - N Containers로 띄우는 것도 가능하다.

![](https://blog.kakaocdn.net/dna/bl8zPk/dJMcac4qwk8/AAAAAAAAAAAAAAAAAAAAANOaku75Su_RCDxzUmfxinFHJ-842kSwA36BkKWKAv_r/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=hGb9tTNFOvXkHLQVO2m%2BykjguFE%3D)

## **Multi-Container Pod**

반드시 한개의 컨테이너에 한개의 Pod만 띄우도록 제한돼있지는 않다.

한개의 컨테이너에 여러개의 Pod를 띄울 수도 있다.

주 역할을 할 컨테이너를 도와줄 Helper Container를 같이 띄우는 것인데 
Helper Container가 수행하는 역할의 예시는 다음과 같다.
- Processing a user entered data
- Processing a file uploaded by the user
- Leave a log from the main container: **Side Car**

Helper Contaier는 Main Container와 생애주기를 함께한다. 둘은 함께 Create되고 함께 Delete 된다.

그리고 둘은 같은 network space를 공유하기 때문에 localhost 이름으로 서로를 참조하고 통신할 수 있다.

![](https://blog.kakaocdn.net/dna/QwiLn/dJMcahdDrTN/AAAAAAAAAAAAAAAAAAAAANeHcSaWeJT2D9YBjrp92gVZjOnkz_DQsBONUWha3fQu/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=evOWcj9uPJs%2BD5F%2FGBrwTiYIgqI%3D)

Multi-Container는 왜 존재할까?

K8S 가장 작은 배포 단위 Object인 Pod와 배포 대상인 Container를 1대1로 하면 좀더 편하지 않았을까?

Multi-Container가 rare use case이긴 하지만 장점이 확실히 있다.

아래와 같은 사례에서 Multi-Container가 매우 효율적이다.
- python-app을 1개 띄워서 서비스 한다
- 사용자가 많아지고 load가 늘어난다.
- Load 분산을 위해 python-app을 더 띄운다.(그림에선 4개까지) 여기까진 아무 문제가 없다.
- 나중에 관리 목적으로 더 필요한 기능이 생겨 heper라는 app을 만들고 python-app에 붙여주게 되었다.

이런 시나리오에서 수반되는 문제점들

- 어떤 python-app이 어떤 helper와 연결됐는지 맵핑하고 있는 정보를 관리할 수밖에 없다.
- python-app별로 helper를 각각 link 해주는 방식으로 두 컨테이너간의 connection을 관리해줘야 한다.
- 두 컨테이너 간에 공유될 volume을 관리해야 한다.
- 만약 python-app이 죽으면 helper는 필요 없어지기 때문에 수동으로 kill 해줘야 한다.

![](https://blog.kakaocdn.net/dna/okhVw/dJMcabqXklO/AAAAAAAAAAAAAAAAAAAAAJ5wwJt6Hv9DDybMmscD_yeP76Y3QW5L0CCjC60Y7EKD/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=KOoCgKuOuhxRNibhkbZc354Wic0%3D)

멀티 컨테이너를 사용함으로써 위 문제들이 모두 해결된다.

Containers in a pod by default will
- have access to the same storage
- have the same network namespace
- have same fate as in they will be created together and destroyed together.

![](https://blog.kakaocdn.net/dna/cZwfuS/dJMcabYN8Ql/AAAAAAAAAAAAAAAAAAAAACoFMxDXfuum-AafUPUeYm4dV-oGjU11bml8SQo2_lNz/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=UDyU3lTariwaTeRv8Lm0GqzxDXY%3D)

## **Pod 주요 명령어**

```
# pod 생성하기. 예시는 nginx라는 이름의 pod를 nginx 이미지로 생성하기
kubectl run nginx --image=nginx

# pod 리스트 보기
kubectl get pods
kubectl get pod
kubectl get po

# pod 상세 내용 확인
kubectl desribe pod [podname]

# 떠있는 pod 설정 수정하기 (running 아니라 error상태라도 가능)
k edit pod [podname]

# pod 생성할 때 container port 같이 지정
kubectl run custom-nginx --image=nginx --port=8080
```

[pod 실행할 때 옵션]
-it : interactive + tty (터미널 실행)
-- 이후는 컨테이너 안에서 실행할 실제 명령
TTY는 TeleTYpewriter(텔레타이프라이터)의 약자로, 역사적으로는 전기식 타자기를, 현대에는 주로 Unix/Linux 시스템에서 터미널(console)을 지칭하며, 전화 통신에서는 청각/언어 장애인용 문자 전화(Text Telephone) 모드를 의미합니다. 컴퓨팅에서는 사용자가 텍스트 기반으로 컴퓨터와 소통하는 인터페이스(물리적 콘솔, 가상 콘솔, 터미널 에뮬레이터)를 가리키며, tty 명령어는 현재 사용 중인 터미널 이름을 보여줍니다.

Pod

. . .
