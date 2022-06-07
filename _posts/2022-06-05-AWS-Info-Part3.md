---
layout: single
title: "[AWS] AWS 알아보기 Part 3 - Virtualization, HVM"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.05.
###### AWS 알아보기 Part 3
###### (Virtualization, HVM)

<br>
<br>
<br>


### Virtualization(가상화)

- 정의

  * 단일 computer의 HW 요소를 일반적으로 virtual machine(VM)이라고 하는 다수의 가상 computer로 분할할 수 있도록 해주는 기술
  * 하나의 computer를 여러 computer로 분할해주는 것임. 아래 사진을 보면 virtualization 전에는 computer 3대를 써야 하지만, virtualization 후에는 computer 1대만 써도 되어서 resource를 효율적으로 manage할 수 있음.  

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fs0d6R%2FbtrDViWxcFx%2FCGBbdQrNl6A6E8KK5pLs5k%2Fimg.png" width=600>
</p>

<br>

- OS와 Virtualization이 등장하기 전

  * **OS**: system HW 자원과 SW 자원을 운영 관리하는 program (ex. Windows, Linux, MacOS, Android)
  * **Privileged Instruction(특권 명령)**: system 요소들과 소통할 수 있는 명령. OS만 가능함.
  * 하나의 HW system에서 privileged instruction이 필요없는 일반 program은 동시에 여러 개를 수행 가능하였지만, privileged instruction과 소통할 수 있는 OS는 하나만 실행이 가능하였음. => virtualization이 나타나기 전까지는 하나의 HW system당 하나의 OS만 실행이 가능하였음. 즉, 일반적인 computer처럼 Bare-Metal(직접 OS가 HW에 설치된 상태)로만 운영이 가능했었음.

<br>

- 1세대: Fully Emulated(완전 가상화)

  * CPU, hard disk, mother board 등의 모든 system 요소를 emulator(기능을 구현한 program)로 구현하여 OS와 연동함 => 엄청나게 느림

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxXW8K%2FbtrDWEq2EEn%2FT7MsBRFUxSEsc6c1KOjgj1%2Fimg.png" width=200>
</p>

<br>

- 2세대: Paravirtualization

  * Guest OS(main OS가 아닌 system 안의 OS)는 hypervisor(OS와 HW 사이에 존재하는 일종의 virtualization manager)와 통신함  =>  속도가 향상되었지만, 몇몇 요소의 경우 여전히 emulator가 필요해서 느림

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdug1WI%2FbtrDXoH1OH0%2FfYzY4TSdvUMR00JFKYZH3k%2Fimg.png" width=200>
</p>

<br>

- 3세대: HVM(Hardware Virtual Machine)

  * HW에서 직접 virtualization을 지원함 => 직접 Guest OS가 HW와 통신함 => 속도가 빨라짐(near bare-metal)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FutBk5%2FbtrDXnWF567%2FqlkdJm9E6GijA0FqU0ZPpk%2Fimg.png" width=200>
</p>

<br>

- AWS와 Virtualization

  * AWS cloud 환경에서 resource를 작은 단위로 빠르게 구성할 수 있는 원동력은 virtualization임. 즉, AWS에서 user마다 computer를 할당해주는 것이 아니라 이미 구축된 virtualization 가능한 server의 한 부분을 할당해주는 것임.
  * ex. AWS의 EC2는 computer를 빌려주는 service임. EC2를 통해 user가 computer를 빌렸을 때, AWS에서는 computer를 하나씩 구성해서 주는 것이 아니라 server에서 virtualization을 통해 미리 여러 computer를 구축한 후 그 중 하나를 할당해주는 것임.
  * 결국 virtualization 없이는 cloud 환경이 존재할 수 없음.


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
