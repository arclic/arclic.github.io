---
title: "[TIL]모두의 네트워크 3장"
excerpt: "[모두의 네트워크] 3장 요약 및 정리"

categories:
  - TIL
tags:
  - TIL
  - CS
  - Network

toc: true
---

## TIL

[모두의 네트워크](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791160505030)를 읽고 요약 및 정리한 글 입니다.

## 3장: 물리 계층 - 데이터를 전기 신호로 변환하기

### 09 물리 계층의 역할과 랜 카드의 구조

컴퓨터는 0과 1만 이해 가능하다. 하지만 네트워크를 통해 데이터를 주고 받을 때는 0과 1의 비트열을 **전기 신호**로 변환해야 한다. 0과 1만으로 이루어진 비트열을 전기 신호로 변환하려면 OSI 계층 중 맨 아래 계층인 **물리 계층**의 기술이 필요하다.

**물리 계층**은 컴퓨터와 네트워크 장비를 연결하고 컴퓨터와 네트워크 장비 간에 전송되는 데이터를 전기 신호로 변환하는 계층이다.

컴퓨터는 네트워크를 통해 데이터를 송·수신할 수 있도록 **랜 카드**가 메인 보드에 포함되어 있는 **내장형 랜 카드**나 별도의 랜 카드를 가지고 있다. 즉, 0과 1의 정보가 컴퓨터 내부에 있는 랜 카드로 전송되고 랜 카드는 0과 1을 전기 신호로 변환하는 것이다.

### 10 케이블의 종류와 구조

**전송 매체**는 **데이터가 흐르는 물리적인 선로**로 종류가 크게 유선과 무선으로 나눠진다. 유선에는 <u>트위스트 페어 케이블, 광케이블</u> 등이 있고, 무선에는 <u>라디오파, 마이크로파, 적외선</u> 등이 있다.

가장 많이 사용되는 **트위스트 페어 케이블(twisted pair cable)**의 종류에는 **UTP 케이블**과 **STP 케이블**이 있다.

**UTP(Unshielded Twist Pair) 케이블**은 구리 선 여덟 개를 두 개씩 꼬아 만든 네 쌍의 전선으로 **실드(shield)**로 보호되어 있지 않은 케이블이다. 실드는 <u>금속 호일이나 금속의 매듭과 같은 것</u>으로 외부에서 발생하는 노이즈(noise)를 막는 역할을 한다. UTP 케이블은 실드로 보호되어 있지 않아서 노이즈의 영향을 받기 쉽지만 저렴하기 때문에 일반적으로 많이 사용되는 케이블이다.

**STP(Shielded Twist Pair) 케이블**은 두 개씩 꼬아 만든 선을 실드로 보호한 케이블이다. UTP 케이블과 달리 노이즈의 영향을 매우 적게 받지만 비싸기 때문에 보편적으로 사용하지는 않는다.

트위스트 페어 케이블은 일반적으로 **랜 케이블(LAN cable, 랜 선)**이라고 하고, 보통 랜 케이블 이라는 용어로 더 많이 불린다. 랜 케이블 양쪽 끝에는 **RJ-45**라고 부르는 커넥터가 붙어 있다. 이 커넥터를 컴퓨터의 랜 포트나 네트워크 기기에 연결한다.

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/78/Uncrimped_rj-45_connector_close-up.jpg/440px-Uncrimped_rj-45_connector_close-up.jpg">
<figcaption>RJ-45</figcaption></center>

---

랜 케이블의 종류에는 **다이렉트 케이블**과 **크로스 케이블**이 있다. 다이렉트 케이블은 구리 선 여덟 개를 같은 순서로 커넥터에 연결한 케이블이고, 크로스 케이블은 구리 선 여덟 개 중 <u>한쪽 커넥터의 1번 2번에 연결되는 구리 선을 다른 쪽 커넥터의 3번과 6번에 연결한 케이블이다.</u>

다이렉트 케이블은 **컴퓨터와 스위치를 연결할 때** 사용하고, 크로스 케이블은 **컴퓨터 간에 직접 랜 케이블로 연결할 때** 사용한다. 컴퓨터 간에 직접 데이터를 보낼 때는 양쪽 컴퓨터 모두 1번 2번 선을 사용하게 된다. 따라서 다이렉트 케이블로 연결할 경우 데이터가 충돌하게 된다.

### 11 리피터와 허브의 구조

**리피터**는 전기 신호를 정형(일그러진 전기 신호를 복원)하고 증폭하는 기능을 가진 네트워크 중계 장비이다. 리피터는 멀리 있는 상대방과도 통신할 수 있도록 파형을 정상적으로 만드는 기능을 한다. 하지만 요즘은 다른 네트워크 장비가 리피터를 지원하기 때문에 굳이 리피터를 사용하지 않아도 된다.

허브는 **포트(실제로 통신하는 통로)**를 여러 개 가지고 있고 리피터 허브라고도 불린다. 리피터는 일대일 통신망 가능하지만 허브는 포트를 여러 개 가지고 있어서 컴퓨터 여러 대와도 통신 가능하다. 허브는 리피터와 마찬가지로 **전기 신호를 정형하고 증폭하는 기능**을 한다. 컴퓨터에서 보낸 전기 신호가 허브에 도착하는 동안 노이즈의 영향으로 파형이 변경될 때가 있다. 그럴 때 허브가 파형을 정상으로 되돌리는 기능을 한다.

허브는 <u>어떤 특정 포트로부터 데이터를 받는다면 해당 포트를 제외한 나머지 모든 포트로도 받은 데이터를 전송하는 특징이 있다.</u> 이처럼 허브는 스스로 판단하지 않고, 전기 신호를 모든 포트로 보내서 **더미 허브(dummy hub)**라는 이름으로 불리기도 한다.

### 용어 정리

- 물리 계층(physical layer): OSI 모델의 최하위 계층으로, 데이터를 전송하기 위해 시스템 간의 물리적인 연결을 하고 전기 신호의 변환 및 제어하는 역할을 담당한다. 또한 전송 매체를 통해 데이터를 통신할 수 있는 전기적인 신호로 바꾸어 전송하는 일을 한다.
- 전기 신호(electronic signal): 전기 신호는 전압이 일정 패턴으로 변하여 생기는 일련의 흐름으로 전압의 변화가 모여서 만들어진 신호다. 이런 전기 신호들을 주고받음으로써 네트워크에서 사진이나 문서 등을 주고받을 수 있다.
- 디지털 신호(digital signal): 아날로그 신호와 대비되는 신호 형태로 아날로그 신호를 전류의 유무나 극성, 위상의 동일이나 반대 등 물리적 현상을 이용하여 컴퓨터가 인식하는 0 또는 1의 2진수에 대응 시켜 나타내는 신호를 말한다.
- 랜 카드(LAN card): 컴퓨터의 네트워크 연결 및 데이터 전송을 담당하며, '네트워크 카드'또는 '네트워크 인터페이스 컨트롤러(NIC)'라고도 불린다.
- 케이블(cable): 전선을 뜻한다. 전달하는 신호나 사용되는 장소에 따라 여러 종류의 케이블이 있는데, 네트워크를 연결하는 케이블을 랜 케이블 또는 랜 선이라고 한다.
- 리피터(repeater): 전기 신호를 전송할 때 전송하는 거리가 멀어지면 신호가 감쇠하는 성질이 있는데, 이때 감쇠된 전송 신호를 새롭게 재생하여 다시 전달하는 신호 중계 장치를 리피터라고 한다.
- 허브(hub): 랜을 구성할 때 한 사무실이나 가까운 거리에 있는 장비들을 케이블을 사용하여 연결하는 장치다.
- 노이즈(noise, 잡음): 데이터의 왜곡이나 분해로 인해 전송 매체에서 생기는 전자 신호다.
