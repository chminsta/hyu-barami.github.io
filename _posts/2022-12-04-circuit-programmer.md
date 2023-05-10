---
title: Circuit Programmer
author: Park HyungJoo
date: 2022-12-04 03:30:00 +0900
categories: [Exhibition,2022년]
tags: [post,hyungjoo,dongseon,changjo,choihyunseo]     # TAG names should always be lowercase, 띄어쓰기도 금지 
---

------------------------------------------
# Ciruit Programmer 

학과: 융합전자공학부  

지도교수: 한재덕  

팀원: 박형주, 최창조, 김동선, 최현서  

---

## 어쩌다 시작하게 됐는가
박형주 학생이 한재덕 교수님 연구실에서 인턴을 하게 되면서 시작된 프로젝트로 뒤이어 김동선, 최창조 학생의 합류하면서 규모가 커졌고 마지막으로 2022년 바라미 회장인 최현서 학생까지 합류하면서 랩실 인턴 프로젝트 겸 바라미 작품 프로젝트가 됐다.

---

## 이게 뭔가
결론부터 말하면 **오픈소스 반도체 회로 설계 툴체인**이다. 기존에 오픈소스로 나온 회로설계 프로그램들이 여럿 있었지만 Synopsys사의 Design-Compiler나 Cadence사의 Virtuoso처럼 하나의 프로그램이 통합적으로 모든 설계 과정을 지원하지는 않았다. 이 프로젝트에서 이 문제를 해결하기 위해, 회로도부터 Post Simulation까지 모든 반도체 설계과정을 지원할 수 있는 툴체인을 구현했다.

---

## 왜 필요한가
무료툴로 반도체 회로를 설계하기 위해서는 여러가지 프로그램이 사용법을 모두 알아야 했다. 또한 무료툴이기 때문에 사용자 인터페이스가 불친절하다는 단점 때문에 이론상의 성능은 Synopsys, Cadence사의 EDA에 크게 뒤쳐지지 않지만 산업에서는 물론이고 학교에서도 이들을 사용하지 않았다. 그래서 이 프로젝트의 목표를 돈이 부족한 스타트업이나 학교에서 기존의 비싼 EDA대신 사용할만한 성능까지 구현하는 것으로 잡았다. 따라서 최적화 대신 사용의 편의성에 집중했고, 그 결과, 기존의 회로설계와 파이썬에 대한 지식만 있다면 충분히 사용할 수 있는 수준까지 편의성을 끌어올리는 데 성공했다.

---

## TOOL은 그렇다 치고 PDK는?

맞다. EDA를 무료로 만든다고 해서 무료로 설계를 바로 시작할 수 있는 것은 아니다. 반도체 설계에는 반도체 파운더리에서 지원하는 PDK라는 toolkit이 반드시 필요한데 이게 회사 기밀이기 때문에 일반인은 접근이 힘들다. 이 프로젝트를 시작할 수 있었던 것은 이 문제가 어느정도 해결됐기 때문이다. **Google**과 **Skywater**라는 파운더리가 합작하여 [skywater-130nm](https://skywater-pdk.readthedocs.io/en/main/)라는 Open-Source PDK를 배포했고 이를 이용해 회로 설계에 성공한 사례가 나왔다. 그래서 우리는 이 프로젝트에 skywater-130nm PDK를 무료로 사용할 수 있었다.

---

## 어떻게 동작하는가
이 툴체인을 사용한다면, 다음 design flow를 통해 회로를 설계할 수 있다.
- Xschem에서 기본 회로도 작성
- Python script로 templates을 구현(inverter, DFF, etc.)
- templates를 배치하고 배선(routing)하는 python script를 작성
- laygo2를 이용해 magic이 읽을 수 있는 layout file을 생성
- magic과 netgen에서 drc, pex, lvs작업 후, ngspice에서 시뮬레이션

Post simulation의 경우, 두가지 방법이 있다.
- magic에서 추출한 spice 파일을 Xschem에서 import한 다음, testbanch 회로를 짜고 simulation
- Xschem이 아닌 PySpice를 이용해 testbench를 python코드를 통해 짠 후 simulation

첫번째 방법은 기존에 Post Simulation 방법처럼 testbench를 회로도로 그릴 수 있다. 하지만 ngspice를 직접 사용해야 하기 때문에 시뮬레이션 후 결과 출력이 불친절 할 수 있다.

두번째 방법의 경우, PySpice사용법을 따로 익혀야 하지만 회로가 커지고 pin의 개수가 많아지거나 시뮬레이션 후 출력할 그래프가 다양해질수록 유용하다는 장점이 있어서 Post Simulation에 유용하다.

---

## 동작 예시
Python code를 통해 D-Flip-Flop을 구현하고 post simulation까지 돌릴 수 있었다. 현재 이를 바탕으로 clock buffer, 5tap, current-mode amplifier로 구성된 1Gbit transceiver 구현을 시도중이다. 또한 기존에 한재덕 교수님 연구실 선배들이 구현한 ip를 교수님과 선배들의 동의를 받아 일부 받아와서 오픈소스 공정에 포팅 후 공개할 예정이다. 포팅 작업 가능여부는 이미 테스트가 끝났다. 테스트 결과, 약간의 코드 수정이 필요하지만 기본적으로 기존 선배들의 코드를 그대로 사용해도 drc, lvs에러없이 구현 가능한 것으로 나왔다. 아래 그림은 D-Flip-Flop layout을 생성하는 파이썬 코드와 post simulation결과, 그리고 생성된 D-Flip-Flop layout이다

<img src="assets/img/post/2022-12-04-circuit-programmer/DFF_result.JPG" width="90%">

---

## 사용법

소스코드는 [깃헙](https://github.com/niftylab/laygo2_workspace_sky130)에 올라와있다.

설치 및 사용 예시는 [다음 링크](https://laygo2-sky130-docs.readthedocs.io/en/latest/)를 참고하면 된다. 링크에 나온 예제는 회로도와 layout을 생성법은 따로 설명하지 않았다. 회로도 그리는 법과 layout생성법은 아래 설명과 링크를 참고하면 된다.

회로도 schematic을 그리는 과정은 유튜브에 Xschem tutorial을 검색하면 step by step으로 나온다. 필자는 유튜브 'bminch' 채널을 추천한다.

<img src="assets/img/post/2022-12-04-circuit-programmer/bminch_youtube.JPG" width="90%">

Layout의 경우, 편의성 극대화를 위해 Laygo라는 python기반 Open-source layout generation Module을 사용했다. Open-source layout drawing program은 선택지가 거의 없어서 어쩔 수 없이 Magic이라는 이론상 성능은 좋지만 굉장히 다루기 힘든 프로그램을 사용했다. 그래서 layout을 손으로 직접 그리는 것이 어렵기 때문에 자유도를 약간 포기하는 대신 **python으로 layout을 코딩할 수 있는 방법**을 선택했다. Laygo는 python기반이기 때문에 예시를 몇가지만 익히면 쉽게 사용할 수 있다. Laygo의 사용법과 API에 대한 설명은 [다음 링크](https://laygo2.github.io/kor.html)를 참고하면 된다.

Post Simulation의 경우는 유튜브'bminch'채널에 위에서 말한 첫번째 방법이 나와있고 두번째 방법인 Pyspice사용법은 [여기](https://www.youtube.com/watch?v=62BOYx1UCfs&list=PL97KTNA1aBe1QXCcVIbZZ76B2f0Sx2Snh)를 참고하면 된다.

아직은 완전하지는 않지만 Collab을 사용하는 방법도 있다. [예시](https://colab.research.google.com/drive/1tpuUvqb6BujzZI6RBf2cFdAfMqBsxpep?usp=sharing)를 참고

---

## 추후 계획

우선 지금까지 개발한 툴체인으로 shift-register, LDO같은 간단한 회로를 설계하고 오픈소스 커뮤니티에 배포할 예정이다. 또 여유가 된다면 transmitter같은 복잡한 회로도 시도해볼 계획이다. 만약 transmitter같은 복잡한 회로가 실제로 동작한다면 간단한 USB에 사용할 칩은 이 툴체인으로 디자인이 가능하다는 것이 되기 때문에 의미 있는 일이라고 생각한다.

LVS의 경우 netgen이라는 프로그램이 지원중이지만, netgen은 O X 자체는 잘 판단해도 어디가 어떻게 틀렸는지는 제대로 알려주지 못한다. 이는 개발자 본인도 인정한 사실이다. 사실 이 툴체인뿐만 아니라 다른 open-source 프로그램 중에서도 현재 layout 디버깅을 지원하는 tool이 없다. 이를 해결하기 위해 laygo에서 자체적으로 LVS를 일부 지원하도록 모듈을 개발중이다. 이 모듈은 기존의 LVS방식이 아닌 컴파일러의 방식에 더 가까운 방식을 채택해 에러가 발생한 지점을 찾는 쪽에 강점이 있다. 현재 테스트 단계에 있으며 실제 layout 그림이 아닌 python코드 lavel에서 검사하는 방식이기 때문에 python으로 구현했음에도 virtuoso보다 더 빨리 검사를 끝냈고 출력된 결과도 어디에서 에러가 발생했는지 유의미하게 잘 짚어주는 것으로 나왔다. 개발이 끝나는 대로 업데이트 할 예정이고 이 작업이 끝나면 이 툴체인으로 최적화 이슈가 없는 회로는 대부분 설계할 수 있을 것으로 기대중이다.
