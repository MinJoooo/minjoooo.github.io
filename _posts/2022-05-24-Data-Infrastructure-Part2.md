---
layout: single
title: "[Data Infrastructure] 이해하기 Part 2 - Data Source, OLTP, OLAP, CDC, ERP, Event collector, 3rd party"
tag: [Data Infrastructure]
toc: false
---

<br>
<br>
<br>

###### 2022.05.24.
###### Data Infrastructure 이해하기 Part 2
###### (Data Source, OLTP, OLAP, CDC, ERP, Event collector, 3rd party)

<br>
<br>
<br>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6QWME%2FbtrC0QGjg3M%2FKFQOjcIcMC6PixmxyoLBF1%2Fimg.jpg" width=800><br>
  출처: Emerging Architectures for Modern Data Infrastructure 2020
</p>


<br>


### Data Source: data가 만들어지는 곳


<br>


---


<br>


### Data Source와 관련한 용어 설명


<br>


### OLTP와 OLAP

- OLTP (Online transaction processing)

* 1개의 요청 작업을 처리하는 여러 단계의 과정
* 예를 들면, 돈을 입금하는 과정에서 '입금 요청 - 입금 진행 - 결과 출력'과 같이 중간에 그만두면 안 되는 작업을 이어서 모두 처리하는 것.
* 주 transaction 형태는 SELECT, INSERT, UPDATE, DELETE
* 일반적으로 빨리 처리해야 하는 작업이고, normalized(정규화된) data 구조이고, table의 수가 많음.

- OLAP (Online analytical processing)

* data warehouse에 저장되어 있는 data를 분석하고 이를 통해 사용자에게 유의미한 정보를 제공해주는 처리 방법
* 예를 들면, 거래 내역을 바탕으로 1년 동안의 지출 내역을 정리하는 것.
* 주 transaction 형태는 SELECT
* 일반적으로 denomalized(정규화되지 않은) data 구조이고, table의 수가 적음.

- OLTP와 OLAP의 차이점

* OLTP는 data의 저장, 삭제, 수정 등과 같이 주로 data를 update하는 작업을 의미함. 따라서 현재의 data 처리가 얼마나 정확하고 무결한지가 중요함.
* OLAP는 이미 저장된 data를 바탕으로 어떤 정보를 제공하는지를 의미함. 따라서 정상적인 OLTP 작업 하에, 정확한 data를 이용하여 사용자가 원하는 정보를 어떻게 표현하고 제공하는지가 중요함.


<br>


### CDC (Change Data Capture)

- OLTP에서 data가 update되면, update된 change event만 가져다 다른 external DB로 복사하는 기술

- OLTP DB도 있지만 OLTP DB는 transaction을 기록하는 데만 집중되어 있어 transaction data를 다른 데에 쓰지 못함. 그래서 CDC가 OLTP에서 update된 부분만 뽑아내 다른 DB로 보내줌.


<br>


### ERP (Enterprise Resource Planning)

- 회사에서 일어나는 모든 data를 collect하는 system


<br>


### Event collector

- mobile, web 등에서 사용자가 만들어내는 data를 collect하는 도구

- Google analytics, Facebook pixel, Braze 등에 collect된 data => customer data platform인 segment, snowplow 등을 이용하여 data를 모아 간편하게 사용가능


<br>


### 3rd party

- 제3자 기업. 회사에서 사용하는 외부 service.

<br>

참고: GeekNews in Youtube
