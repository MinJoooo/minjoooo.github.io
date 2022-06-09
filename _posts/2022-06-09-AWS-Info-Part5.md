---
layout: single
title: "[AWS] AWS 알아보기 Part 5 - EC2 가격과 유형"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.09.
###### AWS 알아보기 Part 5
###### (EC2 가격과 유형)

<br>
<br>
<br>


### EC2 가격 정책

- 소개

  * **가격 순서**: Spot Instance < Reserved Instance < On-demand < Dedicated
  * EC2 price model은 EBS와는 별도임. EBS는 사용한 만큼 지불함.
  * 기타 data 통신 등의 비용은 별도로 청구됨. AWS는 AWS 바깥으로 나가는 traffic에 대해서만 요금을 부과함.

<br>

- On-demand

  * 실행하는 instance에 따라 시간 또는 초당 computing power로 측정된 가격을 지불함.
  * 약정은 필요 없음.
  * 주로 장기적인 수요 예측이 힘들거나 유연하게 EC2를 사용하고 싶은 경우, 혹은 한 번 써보고 싶은 경우 사용함.

<br>

- Spot Instance

  * 경매 형식으로 시장에 남는 instance를 저렴하게 구매해서 쓰는 방식 => 수요에 따라 Spot Instance 가격이 계속 변동되기 때문에 내가 지정한 가격보다 낮다면 사용하고 내가 지정한 가격보다 높다면 반환함
  * 최대 90% 정도 저렴함.
  * 반환 시간 예측이 불가능하다는 단점이 있음 => 언제 도로 내주어야 할지 모르기 때문에 instance가 확보되고 종료되는 것을 반복해도 문제 없는 분산 architecture가 필요함
  * 주로 시작과 종료가 자유롭거나 추가적인 computing power가 필요한 경우 사용함 (ex. big data 처리, ML 등 많은 instance가 필요한 작업) => Hadoop과 같은 cluster system의 경우 manager가 instance의 task를 관리하기 때문에 여러 instance가 사용과 반납을 반복해도 문제가 없음. 오히려 싼 가격에 많은 instance를 사용하면 더 좋음.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcoUytz%2FbtrEnDzr8er%2FrxgGAGfUqOIPy0gSyHDY5k%2Fimg.png" width=450><br>
  Spot Instance 가격이 변동됨에 따라 사용을 시작하고 반납을 함
</p>

<br>

- Reserved Instance (RI)

  * 미리 일정기간(1년 또는 3년)을 약정해서 쓰는 방식
  * 최대 75% 정도 저렴함.
  * 주로 수요 예측이 확실할 때 사용하고, 총 비용을 절감하기 위해 어느 정도 기간의 약정이 가능한 사람들이 사용함.

<br>

- Dedicated (전용 host)

  * 가상화된 server에서 EC2를 빌리는 것이 아닌 지정된 실제 물리적인 server에서 EC2를 대여하는 방식
  * 주로 License issue나(windows server 등) Performance issue나(CPU steal 등) 규정에 따른 이유로 사용함.


<br>


### EC2 유형과 Size

- Instance 유형과 size 읽는 법

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdWoFyd%2FbtrEnCAw12P%2FqS3XMtAyKuOgX9gfZ4wKs0%2Fimg.png" width=300>
</p>

<br>

- Instance 유형

  * 각 instance 별로 사용 목적에 따라 최적화 (ex. memory 위주, CPU 위주, graphic card 위주)
  * type 별로 이름 부여 (ex. t type, m type, inf type)
  * type 별 세대별로 숫자 부여 (ex. m5 = m instance의 5번째 세대)
  * architecture 및 사용 기술에 따라 접두사 (ex. t4g = t4 instance 중 g(AWS Graviton processor)를 사용)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc2uk5W%2FbtrEoKq6XeX%2Fa7kzx9xYYXypJwkROJSNQ0%2Fimg.png" width=600>
</p>

<br>

- Instance Size

  * instance의 CPU 갯수, memory size, 성능 등으로 size 결정
  * 크기가 클수록 => 더 많은 memory, 더 많은 CPU, 더 많은 network 대역폭, EBS와의 통신 가능한 대역폭


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
