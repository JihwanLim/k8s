## **Replica 들어가기 전에**

Replication Controller와 Replica Set 목적은 동일하다. 하지만 동일한 Object는 아니다.

Replication Controller가 더 예전 개념이고 Relica Set에 의해 대체되었다.

Replica Set이 더 권고되는 방식이다.

CKA 시험에도 Replication Controller는 안나오는데 그래도 개념 집고 넘어가자

## **Replication Controller**

Replication Controller는 Controller의 한 종류이다.

Controller는 K8S에서 Brain 역할을 한다.

Controller는 K8S Object를 모니터링하며 적절한 조치를 취한다.

Replication Controller의 역할

1. High Availability

만약 Single Pod에 문제가 생기면 App이 동작안한다.

이럴 때를 대비해 App이 여러개의 Pod에서 동작하도록 만들고
문제가 생기면 App을 다른 Pod로 이동시키는게 가능해야 한다.

이 역할을 Replicaton Controller가 한다.

그렇다면 Single Pod에서는 Replication Controller 사용이 불가능한가? No. 사용 가능하다.

Single Pod라도 fail 나면 Replication Controller가 바로 새로운 Pod 가져다 쓰는게 가능하다.

The replication controller ensures that the specified number of pods are running at all times,even if it's just 1 or 100

![](https://blog.kakaocdn.net/dna/o1GCb/dJMcacXqDz8/AAAAAAAAAAAAAAAAAAAAAEVqPC0T9bvA9q_CSDACSDhaqM7MgnplO9QxPVDB7FtS/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=wUrgfV5l2PQEU0oPk8cJ0H4us5o%3D)

2. Load Balancing and Scaling

Multiple Pod가 Load를 Share하도록 하는 것도 Replication Controller의 역할이다

사용자 요청이 많아지면 Pod를 늘리며 확장한다.

Replication Controller는 같은 Cluster 내 다른 Node로 확장도 가능하다.

![](https://blog.kakaocdn.net/dna/I844u/dJMcagFzM3g/AAAAAAAAAAAAAAAAAAAAANEQPIy7sepVD1M7yLDqI1fYK-0OKThYZOLEGAYq8sIz/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=NjAWTm%2B1svZOxmkwFh8zqfasz40%3D)

ReplicationController yaml 파일 작성 방법을 쉽게 기억하는 방법

template 아래 부분은 pod yaml 중 apiVersion, kind 제외한 모든 것이 들어간다고 생각하면 된다.

![](https://blog.kakaocdn.net/dna/mPdCe/dJMcaaFjUds/AAAAAAAAAAAAAAAAAAAAAE9mWYm4oLbtkXW7GORLECaKS0FykBAqV751SGMguEjZ/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=CxtKVJllGba1sb%2FFJz%2FlZuAbz2Q%3D)

![](https://blog.kakaocdn.net/dna/U4Ah9/dJMcahdlvIp/AAAAAAAAAAAAAAAAAAAAAOoczrWqkQ8EZuufM3-nIERCeuZVK5ompjnFSVhgKsEk/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=L0u%2BdXd8sL%2BF6Qx0RADr4C2K1f8%3D)

그리고 마지막에 원하는 replica 개수를 적어준다.

![](https://blog.kakaocdn.net/dna/qKG6u/dJMcadPt2n0/AAAAAAAAAAAAAAAAAAAAAN_SC-H7S2nGpZBa2NDKnZ8VipZLzkmURRu195LA9I2M/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=8nP%2BzBbIRfvDNWZOv7Ebua2G4Hc%3D)

create은 pod와 동일하게 kubectl create -f [파일명].yml

get해보면 DESIRED, CURRENT, READY가 보인다

kubectl get pod 해보면 replication controller에 의해 자동으로 생성된걸 알수 있게

pod name 앞에 prefix로 replication controller의 name이 붙는다.

![](https://blog.kakaocdn.net/dna/cI1lDA/dJMcaichEoO/AAAAAAAAAAAAAAAAAAAAAGAkdNz5u_Igz9P14oSpU9iuAaiemCibbDWYnUfztDSA/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=u7KK%2FomYkyS0gbaIceak7xQhDZI%3D)

## **Replica Set**

Repliacation Controller와는 apiVersion이 다르다. apps/v1이다. 그냥 v1 하면 에러남

![](https://blog.kakaocdn.net/dna/bZG0Jh/dJMcaffzgfX/AAAAAAAAAAAAAAAAAAAAAEpXx4t0OG0-CxoRzvNs-GYQbkmBNjZfPGP1WJss1C2J/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=8NE5ClQS1aAeBovkrilmJk9yIvI%3D)

Replication Controller와 동일하게 template 아래에 pod를 적어준다.

![](https://blog.kakaocdn.net/dna/cyqpAc/dJMcacb1X0h/AAAAAAAAAAAAAAAAAAAAABiMbZ54Sj-WsgiklRUT82GpMdH3YmWTr0egEWWsl-PF/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=LlMTFy8J6Ep%2BdU%2FmkThAxkQSewo%3D)

![](https://blog.kakaocdn.net/dna/IlYah/dJMcahLdT0Q/AAAAAAAAAAAAAAAAAAAAAAK8AZBLEQogHsISyDTTUdh_ParRRT0ztwynBZ-Rt2ma/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=MoukYoOM5zvDY2Ppm3ZV4zgzneg%3D)

Relication Controller와는 다르게 selector라는 요소가 있다.

selector는 이 replica set에 의해 생성될 pod가 뭔지 정해준다.

근데 selector가 왜 필요할까? 이미 spec.template에 pod definition file 형태로 들어가는데?

이유는 replica set은 자신과 같이 생성된 pod 외에 다른 pod도 manage할 수 있기 때문이다.

예를 들어 replica set이 만들어지기 전에 생성된 pod라도 selector와 match 되면 manage한다

그럼 예전에 생성된 pod만 replica set으로 관리하려면 template 섹션은 필요 없을까? No

만약 pod가 종료되면 replica set이 template 기반으로 다시 생성해야하기 때문에 필요하다.

selector는 Replication Controller와 Relica Set의 가장 큰 차이점이다.

Replication Controller도 selector를 사용할 수 있지만 필수는 아님. Replica Set에서는 필수.

selector에는 matchLabels 외에도 다양한게 있다

Replica Set도 get으로 DESIRED, CURRENT, READY 볼수있고

같이 생성된 pod name 앞에 prefix로 replica set 이름이 붙는다.

![](https://blog.kakaocdn.net/dna/lml9q/dJMcai4mhU8/AAAAAAAAAAAAAAAAAAAAADDyoF7iJufo_BYUiJR56Ko451tbvljhWgAF7k7iln7B/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=DCLYrcvcuSXKsSFIOHT9EGr40ac%3D)

## Labels and Selectors

k8s에서 label과 selector를 사용하는 이유는?

**Replica Set = pod를 모니터하는 프로세스** 이다.

그런데 수많은 pod 중에서 자신이 monitor할 pod를 어떻게 인식할까? labels와 selector가 필요하다!

labels와 selector는 replica set 뿐만 아니라 k8s 다른 영역에서도 같은 역할로 많이 사용된다.

![](https://blog.kakaocdn.net/dna/bB0P7C/dJMcafzStos/AAAAAAAAAAAAAAAAAAAAAPcH0dUofphkXtvivqFhQP9PgPgDEd09sCEkF1nWKk2G/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=AaBTjkIt7MwGnNWzoKByC1446ek%3D)

## **Replica Scale out 하는 방법**

1. yml에서 replica 개수 변경하고 kubectl replace 명령어 실행 (화면에는 create이라고 했는데 잘못된듯)

![](https://blog.kakaocdn.net/dna/GAcwU/dJMcabqIvrQ/AAAAAAAAAAAAAAAAAAAAAAy-i9Vjrp9KXbRdXnPsAvriIaYhJrkCyQmyYoedarLt/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=TeIs0IULho3b0a52zXKda2IjtVo%3D)

2. scale 명령어 사용

yml 파일을 사용할수도 있고, replicaset type, name을 명시하여 사용할 수도 있다.

이 경우 원본 yml 파일의 replicas 수는 변경안되니까 주의!

![](https://blog.kakaocdn.net/dna/qaFLI/dJMcadBW9b6/AAAAAAAAAAAAAAAAAAAAAP8rz08ioOgxIQf08V9X8EjIVFSgMNZJahLwYx1A3ja0/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=Tu8amAdhzYI6A7hTQm3F8qsVOF8%3D)

3. Load에 따라서 자동으로 scale out 하는 option도 있지만 이건 나중에 살펴봄 (HPA)

## **Replica Set 명령어**

![](https://blog.kakaocdn.net/dna/bjcs0F/dJMcaii2IYa/AAAAAAAAAAAAAAAAAAAAAAslJSaf1OBwfBeyqS7_zXAqi8G-9rZOTKOYb-wdsLwG/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1796050799&allow_ip=&allow_referer=&signature=VyATQXswlwwjGR4iuv%2FY6b89d2Y%3D)

