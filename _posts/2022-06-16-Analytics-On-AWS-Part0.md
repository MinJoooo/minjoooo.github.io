---
layout: single
title: "[AWS, Data Infrastructure] AWS에서 분석 플랫폼 구축 실습 Part 0 (실습 목표 및 전제 조건)"
categories: AWS, Data-Infrastructure
tag: [AWS, Data Infrastructure]
toc: false
---

<br>
<br>
<br>

###### 2022.06.16.
###### AWS에서 분석 플랫폼 구축 실습 Part 0
###### (실습 목표 및 전제 조건)

<br>
<br>
<br>


# 실습 목표 및 전제 조건

<br>

- 실습 목표

  * Serverless data lake architecture 설계
  * Amazon S3 storage를 사용하여 data를 data lake로 수집하는 data 처리 pipeline 구축
  * 실시간 streaming data에 Amazon Kinesis 사용
  * AWS Glue를 사용하여 data set 자동 분류
  * AWS Glue 개발 end point에 연결된 Amazon SageMaker Jupyter notebook에서 대화형 ETL script 실행
  * EMR을 사용하여 Spark 변환 작업 실행
  * Glue에서 Amazon Redshift로 data 적재
  * Amazon Redshift 모범 설계 사례 소개
  * Amazon Athena를 사용하여 data를 query하고 Amazon QuickSight를 사용하여 visualization

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb372Kd%2FbtrEUh9bG19%2FlMkrkHsgJJc6BPwFKs2IHK%2Fimg.png" width=800><br>
  Architecture<br>
</p>

<br>

- 실습 전제 조건

  * AWS account에서 AdminstratorAccess에 대한 access 권한이 있어야 함.
  * 실습이 us-east-1 region에서 실행되어야 함.


<br>
<br>


참고: Analytics on AWS (https://catalog.us-east-1.prod.workshops.aws/workshops/44c91c21-a6a4-4b56-bd95-56bd443aa449/ko-KR)

이 워크샵을 추천해주시고 항상 많은 도움을 주시는 정도현 선배님께 감사합니다.
