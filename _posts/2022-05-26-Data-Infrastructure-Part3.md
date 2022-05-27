---
layout: single
title: "[Data Infrastructure] 이해하기 Part 3 - Connector, Data Modeling, Workflow Manager, Spark Platform, Python Library"
categories: Data-Infrastructure
tag: [Data Infrastructure]
toc: false
---

<br>
<br>
<br>

###### 2022.05.26.
###### Data Infrastructure 이해하기 Part 3
###### Connector, Data Modeling, Workflow Manager, Spark Platform, Python Library

<br>
<br>
<br>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6QWME%2FbtrC0QGjg3M%2FKFQOjcIcMC6PixmxyoLBF1%2Fimg.jpg" width=800><br>
  출처: Emerging Architectures for Modern Data Infrastructure 2020
</p>


<br>


### Ingestion and Transformation: data를 가져와서 transform


<br>


---


<br>


### Ingestion and Transformation과 관련한 용어 설명 (1)


<br>


### Connector

- source에서 발생한 data를 연결해서 data warehouse나 data lake로 전달하는 기능

- ETL/ELT tool인 Fivetran, Stitch, Matillion 등이 있음. Matillion은 extract와 load 기능이 무료임.


<br>


### Data Modeling

- tool

  * dbt: Data Analyst에게 transformed data를 SQL로 제공해주는 data transformation tool. Data Analyst가 SW Developer처럼 일하도록 개발환경을 제공해줌.


<br>


### Workflow Manager

- data source에서 data를 가져와서 data warehouse로 옮길 때 workflow 단위로 관리하는 tool

- tool

  * Airflow: task scheduling, distributed execution, dependency management(의존성 관리) 기능 수행. dependency management는 앞의 workflow가 끝나기 전에 그 다음 workflow가 시작되지 않게 관리하는 것임.
  * Dagster: DAG(Directed Acyclic Graph)으로 구성된 python code로 data를 transform하는 모든 것들을 하나의 application처럼 만든 tool. DAG은 한 방향으로 이어지지만 순환은 하지 않는 그래프를 말함.
  * Prefect: Airflow랑 비슷한데 좀 더 최신 버전
  * Airflow와 Dagster의 다른 점: Airflow는 task-driven으로 task 중심이고, Dagster는 data-driven으로 data 중심임.


<br>


### Workflow Manager와 Spark Platform/Python Library/Batch Query Engine의 관계

- Workflow Manager가 task를 수행할 때 big data를 다루는 작업이 있는데, 이때 big data를 다루기 위해서는 많은 기기에서의 분산 작업이 필요함. 이때 Workflow Manager가 Spark에게 big data의 분산 처리 작업을 시킴.


<br>


### Spark Platform

- Apache Spark: big data를 처리할 때 작업을 여러 대의 server에 나누어서 처리해야 하는데, 이를 도와주는 framework

- Spark platform은 data를 가져와서 처리하는데, 이것을 대규모로 처리 가능하게 해주는 게 Spark engine

- tool

  * Databricks: Spark 기반의 data analytics platform. Spark에 dashboard, storage, BI tool과의 연계 등 다양한 service를 지원함.
  * EMR: Amazon의 cloud big data platform. 기능은 Databricks와 비슷함. 
  * Databricks와 EMR의 다른 점: Databricks는 Spark만 지원하는 반면 EMR은 Spark, Hive 등을 다 지원함. 비용은 작은 instance에서는 Databriks가 더 싸고, 큰 instance에서는 EMR이 더 쌈.


<br>


### Python Library

- Pandas를 이용하여 data를 분석하고, Dask나 Ray를 이용하여 이를 여러 대의 server에서 수행함.

- library

  * Pandas: data를 table 형태의 DataFrame 구조로 읽어서 분석 가능하게 도와줌.
  * Boto: python에서 Amazon resource를 사용하게 도와주는 API. AWS에 접속해서 Amazon S3나 EC2에 접근할 수 있게 도와줌.
  * Dask: python code를 분산 computing 할 수 있게 도와줌. 대부분의 python library는 local에서 혼자 돌아가도록 만들어졌는데, 이를 병렬로 여러 대의 server에서 동시에 실행하여 큰 작업을 할 수 있게 도와줌.
  * Ray: Dask와 같이 python code를 분산 computing 할 수 있게 도와줌.
  * Dask와 Ray의 다른 점: Dask는 centralized scheduler가 있어 중앙에서 모든 것을 scheduling함. Ray는 distributed bottom-up scheduling을 해서 local scheduler들에게 task를 주면 local scheduler가 자기가 가진 worker들에게 task를 다시 분산함. 그래서 Ray가 Dask보다 조금 더 빠름. 보통 Dask는 data frame으로 분산 처리할 때 쓰고, Ray는 여러 대의 server에서 ML을 돌릴 때 씀.


<br>


참고: GeekNews in Youtube
