# 분산 시스템(Distributed Systems)?
컴퓨터 시스템의 변화는 압도적으로 진행되고 있다.

1945년 모던 컴퓨터 시대가 시작된 이후부터 1985년까지 컴퓨터는 매우 크고 비쌌다. 더욱이 다른 컴퓨터와 연결할 방법이 없었기 때문에 이 시대의 컴퓨터들은 다른 컴퓨터들과 독립적으로 동작했다.

1980년대 중반부터 이러한 상황을 바꾸는 2개의 기술 발전이 있었다.
1. 강력한 마이크로프로세서의 개발<br><br>
8bit 에서 시작한 프로세스가 이제 16,32 bit를 거쳐 64bit까지 발전했다. 또한 멀티코어 CPU가 등장하므로써 우리는 이제 병렬성을 고려하여 프로그램을 개발해야 한다.

<br>

2. 고속 네트워크의 발전<br><br>
LAN(Local Area Networks)을 통해 수천 대의 컴퓨터를 연결할 수 있고, 수 마이크로초 이내에 적은 양의 정보를 전달할 수 있다. 많은 양의 정보의 경우 몇십억 bps(bits per second)의 속도로 전달할 수 있다.<br><br>
WAN(Wide Area Networks)을 통해서는 지구 상의 수십억 대의 컴퓨터를 적게는 10에서 많게는 몇백억 bps 정도의 속도로 전달할 수 있다.

<br>

* **LAN**: 기계 간에 빠른 시간 내로 약간의 정보를 전송할 수 있도록 몇몇 기계 간의 연결을 도와주는 네트워크(근거리 통신망)
* **WAN**: 광역 통신망으로써 근거리, 중거리 통신망을 하나로 묶어주는 네트워크<br>
  *도시, 나라, 넓은 지역에 설치된 컴퓨터들 간 자원, 정보 공유를 원활하게 함

<br>

이러한 기술의 발전은 대규모 네트워크 컴퓨터 시스템을 쉽게 구성할 수 있게 했다. 이러한 컴퓨터들은 대부분 지리적으로 분산되어 있기 때문에, **분산 시스템(Distributed System)** 이라고 불리운다.

<br>

* **참고**<br>
  ```
  분산 시스템: 수많은 컴퓨터를 연결한 네트워크로 구성된 컴퓨터 시스템 <-> Centralized System(중앙 집중 처리 시스템)
  
  미들웨어(Middleware): 다양한 기계들을 이어지게 만들어 각각 application을 같은 상호작용을 제공한다.
  * 미들웨어 덕분에 하나의 시스템으로 구성된다.
  ```



<br><br>

## 분산 시스템의 정의
> 분산 시스템은 하나의 시스템 처럼 보이는 독립된 컴퓨터들의 집합이다.

위의 정의를 두 가지 주요한 관점으로 바라 볼 수 있다. 
* **하드웨어** - 독립되어 자율적으로 돌아가는 하드웨어 머신
* **소프트웨어** - 유저 관점에서 보았을 때 마치 하나의 시스템으로 여겨지는 시스템

위 두 가지 사항은 분산 시스템을 구성하는 핵심적인 요소이다.

### 분산 시스템 예시
분산 시스템은 독립적인(independent) 여러 대의 컴퓨터가 모여있고, 사용자 입장에서는 그 컴퓨터 집합이 하나 처럼 인식되는 것이다.

대표적인 예는 웹(WWW)이다. 그 다음으로 요새 많이 쓰이는 예는 클라우드 서비스이다.

드랍박스를 예로 들어보면, 사용자는 스토리지를 사용하고 싶어하고 그 스토리지는 사용자의 로컬 컴퓨터에서 제공되는 것이 아니고 어딘가에 자료를 저장해주는 서버가 존재한다. 그 서버는 한 개가 아니고 여러 개의 집합체이지만 사용자는 데이터를 드랍박스의 폴더 안에 넣으면서 서버가 어디에 있고 몇 대가 있는지 궁금해하지 않는다. 그저 로컬 스토리지처럼 사용한다.

<br><br>

## 분산 시스템의 목적
그렇다면 왜 유저 관점에서 하나 처럼 보이지만 뒤에서는 여러 다양한 컴퓨터들로 구성되어 있는 복잡한 분산 시스템이라는 것을 만들어야 하는 것일까?

### 1. 자원의 부족
여기서 말하는 '자원'이라는 것에는 컴퓨터의 CPU 파워, 메모리, 하드 디스크 스토리지 등등 우리가 사용하는 모든 컴퓨터의 자원을 의미한다. 요즘 같은 서버-클라이언트 구조의 서비스들이 점점 더 발달할 수록 더 많은 자원을 요구하게 되고, 머신 한 대로 처리 할 수 있는 양에는 한계가 있다. 그래서 우리는 분산 시스템을 도입하여 위의 문제를 해결한다.

### 2. 장애시 대처
단일 머신에서 돌아가는 시스템이라면 해당 머신의 수명이 다하거나 어떠한 불의의 사고로 다운이 되게 된다면 모든 시스템이 다운되게 된다. 하지만 여러 대의 머신이 분산하여 연산을 처리하고 데이터를 저장하고 있다면 한대의 장애가 전체 시스템의 장애로 이어지지는 않는다. 이를 위해서도 분산 시스템은 필요하다.

### 3. 자원 접근의 편리성 제공
사용자는 많은 종류의 다양한 자원들에 접근을 요구한다. 이때 사용자에게 일관적이고 단일화된 방식으로 사용자가 원하는 자원에 쉽게 접근하게 할 수 있는 것이 바로 분산 시스템이다. 예를 들어서 특정 인터넷 페이지가 있다고 가정해 보자. 우리는 이 페이지를 저장하고 있는 서버가 어디에 있으며 아이피가 무엇인지 알지 못한다. 하지만 인터넷 브라우저에 url을 입력하면 우리가 원하는 사이트에 접속하고, 우리가 원하는 정보를 얻을 수 있다. 여기서 url은 단일화된 자원 접근 방법이고, 앞에서도 말했다시피 사용자는 이 자원이 실제 세계 어떤 서버에 있는지에 대한 고민은 전혀 하지 않아도 된다.

<br><br>

## 분산 시스템의 특징
### 투명성(Transparency)
사용자는 어디에 어떤 자원이 어떻게 저장되어 있는지, 몇 대의 시스템이 어떻게 구성되어 돌아가는지 알 필요가 없다.

분산 시스템은 기본적으로 사용자로 하여금 우리가 제공하는 서비스를 위해서 리소스가 다 흩어져있고 커뮤니케이션이 필요하다는 사실을 알지 못하게 한다. 사용자 입장에서는 모르는 것이 더 서비스를 편하게 이용할 수 있다.

이런 투명성은 아래와 같이 여덟 가지 항목에 적용해 볼 수 있다.
* Access(접근성) : 데이터 표현 방식, 자원에 접근 방법을 사용자에게 숨긴다.
* Location(위치) : 자원이 어디에 위치하고 있는지 사용자에게 숨긴다.
* Migration(이동) : 자원의 이동을 사용자에게 숨긴다(이동하더라도 동일한 방법으로 접근 할 수 있음을 의미한다)
* Relocation(재배치) : 사용자가 사용 중임에도 자원이 다른 위치로 배치될 수 있다. 하지만 이 사실을 사용자에게 숨긴다.
* Replication(복사) : 자원은 복사 될 수 있다. 하지만 사용자는 자기가 복사본에 접근하는지, 원본에 접근하는지 모른다.
* Concurrency(동시실행) : 자원이 다른 유저들과도 공유하며 동시에 실행된다는 것을 숨긴다.

### 개방성(Openness)
사용자 입장에서 분산 시스템에 서비스를 요청하고 제공받는 인터렉션을 할 때 일관적이고 획일적인 방법으로 이뤄지도록 한다.

상황에 따라 요청하고 받는 방법이 달라질 수 있다. 분산 시스템은 여러 대의 컴퓨터로 이뤄지므로 어느 한 쪽으로 연결될 때에는 A라는 방법을 쓰고 다른 한 쪽으로 연결될 때에는 B라는 방법을 사용해야 할 수도 있다.<br>
이 때마다 사용자에게 다른 방식으로 제공된다고 한다면 좋은 디자인이라 할 수 없다. 분산 시스템의 컴포넌트들은 헤테로지니어스(heterogeneous)한 컴포넌트로 이루어지더라도 안의 구성이 어떻든 사용자 입장에서는 동일한 방법으로 서비스를 요청하고 동일한 방법으로 서비스를 제공 받을 수 있어야 한다. 방법 뿐만 아니고 그러한 인터렉션이 어느 장소에서, 어느 때에 이뤄지든 동일하게 서비스를 요청하고 제공받을 수 있으면 좋은 시스템이라 말할 수 있다.

### 확장 용이성(Scalability)
개발 입장에서 분산 시스템은 확장이 용이해야한다. 이 분야에서 가장 큰 화두는 scalability를 어떻게 서포트할까이다. 분산 시스템을 어떻게 확장성 있는 시스템으로 구축을 할까와 동일한 말이다. 

*확장성있는 분산 시스템이다* 라는 의미는 필요한 경우에 스케일이 가능해야한다는 것이다. 리소스를 추가할 때 사용자의 서비스를 중단하지 않고 동시에 이뤄질 수 있어야 확장이 용이하다고 할 수 있다.

<br><br><br>


























---
# References
* https://medium.com/@leeyh0216/distributed-system-introduction-c50883fcd3a0
* https://m.blog.naver.com/nog1922/220851746554
* https://kukuta.tistory.com/156 [HardCore in Programming]
* https://yeonduing.tistory.com/2 [yeonduing]

<br><br><br>
