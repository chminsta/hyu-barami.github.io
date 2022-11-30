---
title: Card recorder
author: Dongbeom Son
date: 2022-11-30 15:00:00 +0900
categories: [Exhibition,2022년]
tags: [post,dongbeomson]     # TAG names should always be lowercase, 띄어쓰기도 금지
---

------------------------------------------
# Card Recorder

### 제작동기


평소 취미생활로 수집형 카드 게임을 즐기고 있습니다. 수집형 카드 게임은 트럼프 카드 게임과는 달리 방대한 종류의 카드 중 본인이 원하는 일부만을 사용하여 게임을 진행하게 됩니다.
그러다보니 카드를 얻게 되면 당장은 사용하지 않더라도 훗날 사용게되는 경우가 종종 있습니다. 이를 대비하여 자신이 얻은 카드 목록을 기록하여 관리합니다. 
그런데, 한번에 얻는 카드가 100장, 200장 수준으로 많고 이 많은 카드를 일일이 검색하고 손으로 분류해가며 관리하는 것이 굉장히 번거롭다 느껴졌습니다. 이러한 부분을 자동화하면 편리할 것 같아 작품을 제작하게 되었습니다.


---

### 작품소개


카드가 담긴 사진에서 카드를 추출, 카드에 적힌 일련번호를 식별하여 공식 database에서 검색해 인식된 카드가 어떤 카드인지 식별하고, excel 파일로 기록하는 python 프로그램입니다.


---

### 작품구현 


1. 주어진 이미지에서 카드를 인식하기 위해 Canny algorithm 을 통한 edge detction을 수행합니다. 이미지 처리 관련 기능은 openCV를 이용하였습니다.
2. 검출된 Edge 중 사각형을 판별해 imutils에서 제공하는 four_point_transform을 통하여 카드 영역만 crop합니다.
3. Crop된 사진에서 카드 일련번호를 검출하기 위해 대략적인 ROI를 설정합니다.
4. morphology 연산을 통해 일련번호가 적혀 있는 부분만을 ROI로 정확히 설정합니다.
5. TEXT의 인식률을 높이기 위해 gaussian blur를 통해 노이즈를 제거하고, adaptiveThreshold를 이용해 이미지를 이진화 합니다.
6. Tesseract OCR을 이용하여 이미지로부터 text를 추출하고, 추출된 text를 후처리한 후 웹페이지서 카드를 검색합니다.
7. Crawling을 통해 필요한 정보만을 얻어낸 후, 파일로 저장합니다.


---

### 작품 동작 결과


<img src="/assets/img/post/2022-11-30-cardrecorder/1.png" width="40%">

<img src="/assets/img/post/2022-11-30-cardrecorder/2.png" width="60%">

<img src="/assets/img/post/2022-11-30-cardrecorder/3.png" width="40%">




---

### 결론 및 작품 후기

기존에 구상했던 것에 비해 실제 작품을 진행해보니 어려움이 많아 작품 구상을 대폭 축소할 수밖에 없어 아쉬움이 남는 작품입니다.
생각했던 것만큼 카드 일련번호의 인식이 잘 이루워지지 않았고, 높은 화질의 이미지를 요구하여 많은 양의 카드를 인식하는 것에 어려움이 있었습니다.
카드마다 일련번호를 인식하기 위한 threshold값이 달랐고, 이로 인해 전처리를 반복적으로 수행하다보니 인식 속도가 느려지는 단점이 있었습니다.
카드 일련번호에만 의존한 분류가 아닌 이미지 분류 기능까지 추가하여 구현한다면 더 높은 정확도와 추가 기능의 구현(특수 처리 여부 구분 등)이 가능할 것이라 기대됩니다.

---

### 소스 코드
Source Code : https://github.com/DongbeomSon/CardRecorder