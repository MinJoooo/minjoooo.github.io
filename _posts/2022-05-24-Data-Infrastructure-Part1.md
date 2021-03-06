---
layout: single
title: "[Data Infrastructure] 이해하기 Part 1 - ETL, ELT, Data Engineer, Data Analyst, Data Scientist"
categories: Data-Infrastructure
tag: [Data Infrastructure]
toc: false
---

<br>
<br>
<br>

###### 2022.05.24.
###### Data Infrastructure 이해하기 Part 1
###### (ETL, ELT, Data Engineer, Data Analyst, Data Scientist)

<br>
<br>
<br>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6QWME%2FbtrC0QGjg3M%2FKFQOjcIcMC6PixmxyoLBF1%2Fimg.jpg" width=800><br>
  출처: Emerging Architectures for Modern Data Infrastructure 2020
</p>


<br>


### Data Infrastructure Architecture

- Source: data가 만들어지는 곳

- Ingestion and Transformation: data를 가져와서 transform

- Storage: 저장소

- Historical: 가져온 data로 과거를 분석

- Predictive: 분석한 내용으로 미래를 예측

- Output: 결과


<br>


### Data Infrastructure의 목적

- business leader의 의사결정을 도와주기

- service/product를 data의 도움을 받아 향상시키기


<br>


### Data Infrastructure의 기초

- Production System: ERP, CRM, Database(Oracle, MySQL)와 같이 회사 내에 생겨나는 data. normalized schema라고 하여 table을 작은 단위로 많이 나누어서 관리. (table 개수가 상대적으로 많음)

- Data Warehouse: 통합된 분석 보고서 작성을 위해 다양한 source로부터 data를 저장. dimensional schema라고 하여 table을 큰 단위로 간단하게 많이 나누지 않은 채로 관리. (table 개수가 상대적으로 적음)


<br>


### ETL과 ELT

- ETL (Extract, Transform, Load)

  * data를 extract(추출)하여, transform(변환) 후에, data warehouse에 load(적재)함. 일반적으로 production system에서 extract하여 dimensional schema로 transform 후에 data warehouse에 load함.

- ELT (Extract, Load, Transform)

  * data를 extract하여, load 후에, transform함.

- ETL → ELT로 넘어가는 이유

  * ETL에서는 extract와 transform 과정이 자동화 될 수 없고, transform 과정이 회사마다 다르다는 문제점이 있음. => ELT가 나옴. ELT에서는 extract와 load 과정을 자동화 할 수 있음.
  * transform 과정에서의 computing resource 비용보다 load 과정에서의 저장공간의 비용이 더 저렴해짐. => 저장공간이 저렴하니 load를 먼저 한 후 다양한 용도로 transform해볼 수 있는 ELT가 더 효율적임.


<br>


---


<br>


### Data Engineer

- big data를 처리할 수 있는 infrastructure과 architecture를 만드는 사람.

- 요구기술: programming, math, big data 관련 다양한 database 지식, ETL 및 BI 도구들에 대한 지식

- 주 사용 언어: Python, SQL, Shell script


<br>


### Data Analyst

- data를 해석해서 business 의사결정을 돕는 정보를 만드는 사람.

- 요구기술: statistics, math, communication, spreadsheet & database 사용, BI tool을 이용한 visualization

- 주 사용 언어: SQL, R, Python


<br>


### Data Scientist

- 수학자 + 과학자 + 도메인 전문가. big data를 잘 다루고 복잡한 문제를 해결하는 사람.

- 현업에서는 Data engineer + Data analyst를 의미하기도 하고, Data Analyst의 상위 직군을 의미하기도 함.

- 요구기술: math, statistics, ML, DL, 분산 computing, data modeling, story telling, visualization, domain 지식, communication

- 주 사용 언어: SQL, Python, R


<br>


참고: GeekNews in Youtube
