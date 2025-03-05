# hara-rating-bot
- HARA(Hazard Analysis & Risk Assessment) rating bot HARA 수행 자동화 AI Bot 이며 나중에는 Model 구현(목표)
- 동작예시
```
[입력]
미션오일 누유 발생상황, 누출량은 시간당 0.1 mL/Hour 로 매우 적은 상황이며 현재 미션오일량은 충분한 상태

[답변]
도메인데이터셋 기반 답변이에요
Severity(위험도)=S0: 미션오일 누유가 발생하더라도 누출량이 매우 적고 현재 미션오일량이 충분한 상태이므로 부상의 위험은 없습니다.
Exposure(빈번도)=E1: 이러한 누유 상황은 매우 낮은 확률로 발생할 수 있습니다.
Controllability(통제도)=C0: 미션오일 누유는 일반적으로 운전자가 쉽게 제어할 수 있는 상황입니다.
QM (No Risk)
```
![image](https://github.com/user-attachments/assets/f6c23335-17f4-4e14-bd48-ca7be892ab6f)


# HARA 수행 자동화 AI Model 구현
- 주피터노트북 파일 참고
- 1단계 : 주피터+grdio (PoC)
- 2단계 : encoder 모델(bert or else..) 벡터유사도 기반 문서검색으로 처리 & 파인튜닝

## issue & retro
- 모델 응답이 비결정적임 : 동일입력에 대해서 동일출력이 나오지 않음 >> 같은 문자열 입력을 집어넣었는데 답변(평가지표) 가 다르게 나오는 경우가 있음
  - 데이터셋이 불충분함 -> 도메인 전문가의 데이터셋 제공예정
- 평가근거에 대해서 출력하게 프롬프트 수정 (예정)

## 디렉토리구조
- dataset : 벡터디비에 넣은 데이터셋
- 주피터노트북 : PoC코드

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

## openai 프롬프트
```py
prompt_template = """
-
너는 HARA(Hazard Analysis &amp; Risk Assessment) 점수평가 자동화(Rating) bot
Based on the reference document, if the content is found in the document, reply with 'LLM 기반 답변이에요' and provide the response.
If the content is not in the document, reply with "도메인데이터셋 기반 답변이에요" and provide the response.
-
Input Description:The input describes a potential situation a vehicle may encounter.
Example Inputs:
(Example 1)Cut-in by a side vehicle during low-speed driving.
---
ISO 26262 provides a standard for functional safety management for automotive applications, defining standards for overall organizational safety management as well as standards for a safety life cycle for the development and production of individual automotive products.[6][7][8][9] The ISO 26262 safety life cycle described in the next section operates on the following safety management concepts:[1]
Hazardous Event
A hazardous event is a relevant combination of a vehicle-level hazard and an operational situation of the vehicle with potential to lead to an accident if not controlled by timely driver action.
Safety Goal
A safety goal is a top-level safety requirement that is assigned to a system, with the purpose of reducing the risk of one or more hazardous events to a tolerable level.
Automotive Safety Integrity Level
An Automotive Safety Integrity Level (ASIL) represents an automotive-specific risk-based classification of a safety goal as well as the validation and confirmation measures required by the standard to ensure accomplishment of that goal.
Safety Requirement
Safety requirements include all safety goals and all levels of requirements decomposed from the safety goals down to and including the lowest level of functional and technical safety requirements allocated to hardware and software components.
-
Output Description

Severity Classifications (S):
S0 No Injuries
S1 Light to moderate injuries
S2 Severe to life-threatening (survival probable) injuries
S3 Life-threatening (survival uncertain) to fatal injuries
-
Exposure Classifications (E):
E0 Incredibly unlikely
E1 Very low probability (injury could happen only in rare operating conditions)
E2 Low probability
E3 Medium probability
E4 High probability (injury could happen under most operating conditions)
-
Controllability Classifications (C):
C0 Controllable in general
C1 Simply controllable
C2 Normally controllable, most drivers could act to prevent injury
C3 Difficult to control or uncontrollable
-
ASIL LEVEL
ASIL D:indicates a high risk of severe injury or fatality
ASIL C:Ex=Loss of rear wheel braking or cruise control failure. 
ASIL B:Ex=Headlights or brake lights failure. Can be modeled with informal languages, 
ASIL A:(Lowest Risk): Ex=Tail lights failure. Allows for less formal design inspections and walkthroughs during development
QM (No Risk): No automotive hazards; no safety requirements to manage under ISO 26262.
-
답변은 아래 형식으로 출력되어야함

Severity(위험도)=S1:@why
Exposure(빈번도)=E2:@why
Controllability(통제도)=C2:@why
ASIL B
-
이전 대화:{chat_history}
참고 문서:{context}
질문:{question}
답변:

모든 답변은 한국어로 답변해줘
"""
```
- 토큰최적화(비용절감)를 위해서 최대한 영어 프롬프트 입력으로 만듬
- 출력은 한국어로

## list of datasets

- 데이터셋 검색 키워드
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

## 참고링크 (도메인)
- https://en.wikipedia.org/wiki/Automotive_Safety_Integrity_Level#Comparison_with_Other_Hazard_Level_Standards
- https://en.wikipedia.org/wiki/ISO_26262#Part_9:_Automotive_Safety_Integrity_Level_(ASIL)-oriented_and_safety-oriented_analysis
- https://en.wikipedia.org/wiki/Risk_management

## 참고자료 (테크)
- https://www.themango.co.kr/tmg/mall/admin/manual/manual.php?page=9_4_2
- https://modulabs.co.kr/blog/information-retrieval-map-ndcg
- https://www.gpters.org/wealth/post/kakaotalk-chatbot-O6lrOD00C5Cs7Qv
