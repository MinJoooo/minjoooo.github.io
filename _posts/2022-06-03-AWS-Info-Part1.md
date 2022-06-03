---
layout: single
title: "[AWS] AWS 알아보기 Part 1 - Cloud Computing, Global Service, Region, Availability Zone"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.03.
###### AWS 알아보기 Part 1
###### (Cloud Computing, Global Service, Region, Availability Zone)

<br>
<br>
<br>


### Cloud Computing

- 정의

  * IT resource를 internet을 통해 on-demand(수요에 따라)로 제공하고 사용한 만큼만 비용을 지불하는 것

<br>

- Cloud의 장점

  * 초기 비용이 적고, 대규모로 server를 운영하기 때문에 운영 비용이 저렴함.
  * 가변적으로 용량을 사용할 수 있어 용량 추정이 필요 없음.
  * 유지보수가 쉬움.


<br>


### Cloud Computing 유형

- Application 구성

  * Application
  * OS (Windows/Linux)
  * Computing (CPU + RAM)
  * Storage (HDD/SSD)
  * Network (LAN card/LAN cable)
  * Infrastructure는 'Network + Storage + Computing'을 말함.

<br>

- Cloud Computing Model

  * IaaS(Infrastructure as a Service): Infrastructure만 제공함. OS를 직접 설치하고 필요한 SW를 개발해서 사용함. 가상의 computer를 하나 임대하는 것과 비슷함. (ex. AWS EC2)
  * PaaS(Platform as a Service): 'Infrastructure + OS + 기타 program 실행에 필요한 부분(run time)'을 제공함. code만 올리면 바로 돌릴 수 있도록 구성됨. (ex. Firebase, Google App Engine)
  * SaaS(Software as a Service): 'Infrastructure + OS + 필요한 SW(application)'를 제공함. Application 구성 전체를 제공하는 것으로 다른 setting 없이 service만 이용할 수 있음. (ex. Gmail, DropBox, Slack, Google Docs)

<br>

- Cloud Computing 배포 Model

  * Cloud (공개형): 모든 부분이 cloud에서 실행됨. 낮은 비용과 높은 확장성의 장점이 있음.
  * Hybrid (혼합형): Cloud와 On-Premise가 혼합됨. On-Premise에서 Cloud로 전환하는 과도기나 On-Premise의 back-up으로 주로 사용됨.
  * On-Premise (폐쇄형): 높은 수준의 customizing이 가능하고 보안 수준이 높다는 장점이 있음. 하지만 초기 비용이 비싸고 유지보수 비용이 비싸다는 단점이 있음.


<br>


### AWS(Amazon Web Service)

- 소개

  * 전 세계적으로 분포한 data center에서 200개가 넘는 기능의 service를 제공하는, 세계적으로 가장 포괄적이며 널리 채택되고 있는 cloud platform

<br>

- 구조

  * Global Service  >  Region  >  Availability Zone

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMUYRG%2FbtrDNVnGUgO%2FWVyaYMYkQOhQRksEVfPSk0%2Fimg.png" width=600>
</p>

<br>

- Global Service

  * AWS 안에 존재하지만 region에는 속하지 않음.
  * data 및 service를 전 세계 모든 infrastructure가 공유함.
  * ex. Cloud Front, IAM, Route53, WAF

<br>

- Region

  * region에는 속하지만 availability zone에는 속하지 않음.
  * 특정 region을 기반으로 data 및 service를 제공함 => region별로 가능한 service가 다름
  * AWS service가 제공되는 server의 물리적 위치
  * 전 세계에 흩어져 있으며 각 region에는 고유 code가 있음. (ex. 서울 region: ap-northeast-2)
  * 지연 속도, data나 service 제공 관련 법률, 사용 가능한 service 등을 고려하여 region을 선택함.
  * ex. 대부분의 service, VPC, S3

<br>

- Availability Zone(AZ, 가용영역)

  * 하나의 region은 반드시 2개 이상의 AZ로 구성됨.
  * 하나의 AZ는 하나 이상의 data center로 구성됨.
  * region간의 연결은 매우 빠른 전용 network로 연결됨.
  * 물리적으로 반드시 일정 거리 이상 떨어져 있는 동시에 모든 AZ는 서로 100km 이내 거리에 위치함. => 재해에 대한 대비
  * 각 user별로 AZ의 code와 실제 위치는 다름 => 보안 수준을 높이고, 하나의 AZ로의 몰림을 방지함
  * ex. RDS, EC2

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBXxjJ%2FbtrDRiICwQz%2FmoEIGurCWp82Dz54jUfqkK%2Fimg.png" width=500>
</p>

<br>

- Edge Location

  * AWS의 Cloud Front(CDN) 등 여러 service를 빠르게 제공하기 위한 거점. 주로 caching에 사용됨.
  * 전 세계에 흩어져 있음.

<br>

- ARN(Amazon Resource Name)

  * resource의 고유 ID로, AWS의 모든 resource에는 ARN이 부여됨.


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
