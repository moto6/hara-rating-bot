# hara-rating-bot
HARA(Hazard Analysis &amp; Risk Assessment) rating bot

# HARA 수행 자동화 AI Model 구현

## 컨셉 및 목표

## 용어집

## ISO26262

- [ISO26262](https://en.wikipedia.org/wiki/ISO_26262) 는 도로주행 차량의 기능 안전(Road vehicles Functional Safety) 을 다루는 국제 표준으로, 자동차 전기·전자 시스템의 안전성을 확보하기 위해 제정되었습니다.
- 국제 표준화 기구(ISO) 에서 정의한 표준으로, 자동차 산업에서 필수적으로 고려해야 할 **안전 수명 주기(Safety Lifecycle)** 및 자동차 안전 무결성 수준(ASIL, Automotive Safety Integrity Level)을 규정하고 있습니다.
- 차량 전자·전기 시스템이 고장 났을 때 발생할 수 있는 위험을 평가하고, 이를 제어하기 위한 위험 기반 접근 방식(Risk-based Approach)의 관점을 제공합니다.
- **ASIL(A, B, C, D)** 등급을 통해 시스템의 위험도를 평가하고 이에 따라 적절한 안전 조치를 요구합니다.
- **ISO 26262의 목표**는 자동차 전기·전자 시스템의 기능 안전을 보장하기 위해 다음과 같은 목표를 설정합니다.
    - **자동차 안전 수명 주기 전체를 관리**
    - **기능적 안전성을 위한 프로세스 정의** (요구 사항, 설계, 검증, 확인 등)
    - **위험 기반 접근법 제공** (ASIL 등급을 통한 안전 평가)
    - **허용 가능한 잔여 위험 수준 설정**
    - **검증 및 확인(V&V) 절차를 통해 안전성 확보**
    

```jsx

나만의 데이터로 챗봇 만들기

- PDF 파일을 읽는다던가, txt 파일을 읽어서 청킹
- 청킹 전, 후에 필요없는 문서들. 답변에 필요없는 문서들 제거.
- 검색 대상인 문서가 불필요하게 HTML이라던가 테이블 형태라서 임베딩 할 때 방해할 수 있는 요소가 있다면 제거
- 멀티턴 적용 (가점)
- 크로스 인코더 적용 (가점)
- 앙상블 리트리버(BM25, 임베딩 모델을 섞어서 쓰는 경우), 하이브리드 리트리버(임베딩, 검색기 섞어서 쓰는 또 다른 방법)
- 챗봇

----------------------------------------
내가 원하는 도메인의 데이터를 가지고 챗봇을 만들 때 이 과정에서 실제로 도움되는 것들을 썼는가
데이터가 일반적으로 문맥이 긴 편이라서 자를 때 500으로 자르지 않고 1000으로 자르고 OVERLAP은 200으로 했다던가
```

### **자동차 안전 무결성 수준(ASIL)**

- ISO 26262는 자동차의 전기·전자 시스템이 고장 났을 때의 **위험도**를 평가하기 위해 **ASIL(A, B, C, D)** 등급을 정의합니다.

| **ASIL 등급** | **위험 수준** | **예제** |
| --- | --- | --- |
| ASIL D | 매우 위험함 | 브레이크 고장, 조향 시스템 고장 |
| ASIL C | 상당히 위험함 | 엔진 제어 시스템 오작동 |
| ASIL B | 보통 위험함 | 전조등 시스템 오류 |
| ASIL A | 낮은 위험 | 라디오, 에어컨 시스템 오류 |
- **ASIL D가 가장 높은 위험도를 의미하며, 더 엄격한 안전 요구 사항이 필요합니다.**

### HARA : Hazard Analysis & Risk Assessment

- 제품이 제공하는 기능의 오동작 상황을 고려하여 오동작 시의 영향을 차량의 운전자 입장에
서 평가하는 것으로Severity (S), Exposure (E) 그리고 Controllability (C) 세 가지 영역으
로 구분하여 평가를 진행하고, 그 결과에 따라 총 4단계로 구분되어진다 (ASIL A/B/C/D,
“Automotive Safety Integrity Level”).
- HARA분석은 Item Definition에서 정의되어진 기능을 바탕으로 평가를 진행
- HARA를 통해 개발하는 제품 기능의 안정성을 증명하기 위한 최종 목표 (Safety Goal)
가 해당 문서를 통해 정의
- 위에서 언급한 Severity (S), Exposure (E) 그리고 Controllability (C)를 통해 결정되어진 ASIL 결과를 공유하게 된다.여기서 언급한 Safety Goal 또한 개발하는 제품이 최종적으로 증명해야 하는 요구사항이며,
이런 인식의 출발이야 말로 ISO 26262를 올바른 방향으로 적용하는 시작점이라 할 수 있다.

## 참고데이터

-


## list of datasets
- 검색키워드 :
### 검색키워드
```
NHTSA filetype:pdf
교통사고 통계 / traffic accident statistics
car breakdown accident Automotive Road Injuries Collisions involving vehicles
breakdown risk

```

### NHTSA 데이터셋 
- pdf or ppt 로 주어짐 >> PDF 로 처리
  - [Hazard Analysis of Concept Heavy Truck Platooning Systems](https://www.nhtsa.gov/sites/nhtsa.gov/files/2021-06/Hazard%20Analysis%20of%20Concept%20Heavy%20Truck%20Platooning%20Systems-%20A.Svenson%20final_0.pdf)
  - [Functional Safety Assessment of an Automated Lane Centering System](https://www.nhtsa.gov/sites/nhtsa.gov/files/documents/13498a_812_573_alcsystemreport.pdf)
  - [Assessment of Safety Standards for Automotive Electronic Control Systems](https://www.nhtsa.gov/sites/nhtsa.gov/files/812285_electronicsreliabilityreport.pdf)
  - [SAFETY ANALYSIS APPROACHES FOR AUTOMOTIVE ELECTRONIC CONTROL SYSTEMS](https://www.nhtsa.gov/sites/nhtsa.gov/files/2015sae-hommes-safetyanalysisapproaches.pdf)
  - [Functional Safety Assessment of a Generic Steer-by-Wire Steering System With Active Steering and Four-Wheel Steering Features](https://www.nhtsa.gov/sites/nhtsa.gov/files/documents/13502_812576_steerbywire.pdf)
  - [Functional Safety Assessment of a Generic, Conventional, Hydraulic Braking System With Antilock Brakes, Traction Control, and Electronic Stability Control](https://www.nhtsa.gov/sites/nhtsa.gov/files/documents/13497a_812574_hydraulicbrakingsystem.pdf)

### 정부 데이터셋 : https://catalog.data.gov/ 
- csv
  - [https://catalog.data.gov/dataset/crash-data](https://data.townofcary.org/api/v2/catalog/datasets/cpd-crash-incidents/exports/csv)
  - [https://catalog.data.gov/dataset/crash-reporting-non-motorists-data](https://data.montgomerycountymd.gov/api/views/n7fk-dce5/rows.csv?accessType=DOWNLOAD)
  - [https://catalog.data.gov/dataset/crash-reporting-drivers-data](https://data.tempe.gov/api/download/v1/items/0c333bd164d64d62aa0ee6f99b1ccf82/csv?layers=0)
  - [https://catalog.data.gov/dataset/allegheny-county-crash-data](https://data.wprdc.org/datastore/dump/2c13021f-74a9-4289-a1e5-fe0472c89881)
  - [https://catalog.data.gov/dataset/recalls-data](https://data.transportation.gov/api/views/6axg-epim/rows.csv?accessType=DOWNLOAD)
  - [https://catalog.data.gov/dataset/crash-reporting-drivers-data](https://data.montgomerycountymd.gov/api/views/mmzv-x632/rows.csv?accessType=DOWNLOAD)
  - [https://catalog.data.gov/dataset/collisions-involving-vehicles-managed-by-department-of-health-and-mental-hygiene-dohmh](https://data.cityofnewyork.us/api/views/knr6-vurn/rows.csv?accessType=DOWNLOAD)
  - [https://catalog.data.gov/dataset/crash-reporting-non-motorists-data](https://data.montgomerycountymd.gov/api/views/n7fk-dce5/rows.csv?accessType=DOWNLOAD)
- 이외 형식
  - zip : https://catalog.data.gov/dataset/road-traffic-injuries-0935b >> 풀어서 excel 데이터셋만
  - json : [https://catalog.data.gov/dataset/crash-reporting-incidents-data](https://data.montgomerycountymd.gov/api/views/bhju-22kf/rows.json?accessType=DOWNLOAD)

### 이외잡 데이터셋
- [자율주행차상용화확산을 위한 국제표준가이드](https://avstandard.or.kr/uploads/file_content/5515f802-a282-4e61-8404-5d10cb275400/ISO_%EA%B5%AD%EC%A0%9C%ED%91%9C%EC%A4%80_116%EC%84%A0_%EA%B0%80%EC%9D%B4%EB%93%9C.pdf)

- []()

### (중복) pdf 링크만

```txt
```


## 참고자료
- https://www.themango.co.kr/tmg/mall/admin/manual/manual.php?page=9_4_2
- https://modulabs.co.kr/blog/information-retrieval-map-ndcg
- https://www.gpters.org/wealth/post/kakaotalk-chatbot-O6lrOD00C5Cs7Qv
