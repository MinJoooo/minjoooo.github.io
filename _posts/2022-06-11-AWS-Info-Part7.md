---
layout: single
title: "[AWS] AWS 알아보기 Part 7 - EC2의 생명주기"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.11.
###### AWS 알아보기 Part 7
###### (EC2의 생명주기)

<br>
<br>
<br>


### EC2의 생명주기

- 정의
  * AMI로부터 EC2가 실행이 된 후부터 종료될 때까지 EC2가 거치는 과정

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb7igw9%2FbtrExGhthJZ%2FzbOKEM1JuVeGz72zTNAVMk%2Fimg.png" width=600><br>
</p>

<br>

- 소개

  * **pending state**: 준비 state. EC2를 가동하기 위해 virtual machine에 올라간다거나 ENI나 EBS 등이 준비된 state.
  * **running state**: EC2를 사용할 수 있는 state.
  * **rebooting mode**: rebooting 시에는 public IP의 변동이 없음.
  * **중지 mode**: 중지 중에는 instance 요금이 청구되지 않음. 그러나 EBS 요금과 다른 구성요소(Elastic IP 등)의 요금은 청구됨. 중지 후 재시작 시 public IP가 변경됨. (재시작 후 PuTTY 등을 이용해 재접속 시 public IP가 변경되어 접속이 되지 않는 문제가 생김) EBS를 사용하는 instance만 중지가 가능함. instance store 기반인 instance의 경우 중지가 불가능함.
  * **최대 절전 mode**: memory 내용을 보존해서 재시작 시 중단 지점에서 시작할 수 있는 정지 mode.
  * **종료 mode**: instance를 종료시킴. 설정에 따라 EBS도 같이 종료시킬 수도 있음.
  * 중지 mode와 최대 절전 mode의 차이: 우리가 program을 실행하면 data가 HDD에서 memory로 옮겨진 후에 실행됨. 그런데 memory는 휘발성이라 전원이 공급되지 않으면 data가 날아가버림. 그래서 중지 mode를 하면 memory의 data가 날아가버리는 문제가 생김. 하지만 최대 절전 mode에서는 memory의 data를 HDD에 복사해놓고 종료하기 때문에 data가 날아가지 않음.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FuRlEv%2FbtrEw8TdEiL%2FtgrbkOJnC1IWA9MpWYzHHk%2Fimg.png" width=400><br>
  program을 실행하면 data가 HDD에서 memory로 옮겨진 후에 실행됨<br>
</p>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdjyFcP%2FbtrEvfkiv9d%2Fj9aYQMmn5FD6xMCCce1t21%2Fimg.png" width=500><br>
  instance 요금 청구 기준<br>
</p>


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
