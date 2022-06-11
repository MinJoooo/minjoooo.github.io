---
layout: single
title: "[AWS] AWS 알아보기 Part 6 - EBS, Snapshot, AMI, AMI 실습"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.10.
###### AWS 알아보기 Part 6
###### (EBS, Snapshot, AMI, AMI 실습)

<br>
<br>
<br>


### EBS(Elastic Block Storage)

- 정의

  * AWS cloud의 EC2 instance에 사용할 영구 block storage volume을 제공함.
  * 각 EBS volume은 가용 영역 내에 자동으로 복제되어 구성요소 장애로부터 보호해주고, 고가용성 및 내구성을 제공함.

<br>

- 소개

  * 가상의 hard drive
  * EC2 instance와 EBS는 서로 분리되어 network로 연결되어 있음 => instance가 종료되어도 EBS는 계속 유지 가능함, instance를 변경하고 싶을 때 EBS는 그대로 두고 instance만 변경 후 network를 변경하면 되서 간편함, 하나의 instance에 여러 개의 EBS를 연결할 수 있음
  * instance 정지 후 재 기동이 가능함.
  * root volume으로 사용시 EC2가 종료되면 같이 삭제되는 것이 default임. 그러나 설정을 통해 EBS만 따로 존속도 가능함.
  * EC2와 EBS는 같은 가용 영역에 존재함. 만약 EC2와 EBS가 다른 가용 영역에 존재한다면 연결할 수가 없음.
  * 5가지 type을 제공: General Purpose(범용), Provisioned IOPS, Throughput Optimized HDD, Cold Hdd, Magnetic 

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBia3X%2FbtrEntKHMwZ%2F3XKs74cM9BRww88Ldr1R21%2Fimg.png" width=400><br>
  instance와 EBS는 network로 연결됨<br>
</p>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FV8aA5%2FbtrEtMJEIy9%2F7JawsgFCVgtT5sMW6mmSZ0%2Fimg.png" width=600><br>
  EBS의 5가지 type<br>
</p>

<br>

- Snapshot

  * 특정 시간의 EBS 상태의 저장본 => 필요시 Snapshot을 통해 특정 시간의 EBS를 복구 가능함
  * EBS에 사진을 찍어둔 개념으로, EBS를 저장하는 효율적인 방법 
  * S3에 보관하고, 증분식으로 저장(변화된 부분만 저장)함 

<p align="center">
  <img src="" width=300><br>
  다음과 같이 변화될 때마다 state를 저장하면 memory가 많이 필요함<br>
</p>

<p align="center">
  <img src="" width=300><br>
  증분식 저장: 다음과 같이 추가/삭제되는 action만 저장하는 것<br>
</p>


<br>


### AMI(Amazon Machine Image)

- 소개

  * EC2 instance를 실행하기 위해 필요한 정보를 모은 단위 (OS, architecture type(32bit or 64bit), 저장공간 용량 등에 대한 정보)
  * AMI를 사용하여 EC2를 복제하거나 다른 region에서 계정으로 전달이 가능함.
  * Snapshot을 기반으로 AMI 구성이 가능함. AMI를 통해 현재 상태의 EC2를 정확하게 복제하여 다른 사람에게 넘겨줄 수 있음.

<br>

- 구성

  * 1개 이상의 EBS Snapshot
  * Instance store 기반 AMI의 경우, instance의 root volume에 대한 template (ex. OS, application server, application)
  * 사용 권한 (어떤 AWS account가 사용할 수 있는지)
  * Block device mapping (EC2 instance를 위한 volume 정보 = EBS가 무슨 용량으로 몇 개 붙는지)

<br>

- Type

  * **EBS 기반**: 일반적인 경우로, EC2와 EBS가 network로 연결됨.
  * **Instance store 기반**: EC2 안에 EBS가 들어가있음.
  * network로 연결된 EBS 기반보다 물리적으로 연결된 Instance store 기반이 속도가 더 빠름. 대신 Instance store 기반은 EC2 instance가 삭제되면 무조건 같이 삭제된다는 단점이 있음. 보통 영구적이지 않은 data(cache data), 복구가 가능한 data, 속도가 굉장히 중요한데 꼭 저장할 필요가 없는 data를 Instance store 기반에 저장함.

<p align="center">
  <img src="" width=300><br>
  왼쪽은 EBS 기반, 오른쪽은 Instance store 기반<br>
</p>

<br>

- Type에 따른 AMI의 생성 방법

  * EBS: Snapshot을 기반으로 root device 생성
  * Instance store: S3에 저장된 template를 기반으로 생성

<br>

- AMI를 만드는 과정

  * EBS의 Snapshot을 찍은 후 S3에 저장 => 이를 기반으로 AMI를 만듦 => 이를 이용하여 EC2를 실행하거나 복제한 AMI를 공유하는 작업을 함

<p align="center">
  <img src="" width=300><br>
</p>


<br>


### EC2 실습

- 'Storage 구성' 항목 추가 설명

  * **Volume 유형**: 'magnetic'을 선택하면 더 저렴함
  * **종료 시 삭제**: 'Yes'를 선택하면 Instance 종료 시 EBS도 같이 삭제됨
  * **암호화됨**: 'No'를 선택해도 됨. 여기서 암호화는, data를 암호화하는 것이 아니라 hard disk를 암호화하는 것임. 누가 AWS에 들어와서 EBS를 빼간다고 해도 우리의 data는 안전하다는 뜻임. 실제로 물리적으로 쓰일 때 암호화시키는 것이기 때문에 우리는 암호화의 차이를 직접적으로 느끼지 못할 것임.

<p align="center">
  <img src="" width=300><br>
</p>

<br>

- 실습 과정 (1) - Instance 생성 후 EC2 connector

  * 이전 글을 참고하여 Instance 생성
  * 이전 글을 참고하여 EC2 connector 접속 후 index.html file까지 만듦
  * '인스턴스' 화면 => Instance 선택 => '퍼블릭 IPv4 DNS' 주소로 들어가도 index.html 화면이 나옴

<p align="center">
  <img src="" width=300><br>
  <img src="" width=300><br>
</p>

<br>

- 실습 과정 (2) - AMI 복제

  * 복제하고자 하는 instance를 선택 후 오른쪽 버튼 클릭 => '이미지 및 템플릿' 선택 후 '이미지 생성' 선택

<p align="center">
  <img src="" width=300><br>
</p>

  * '이미지 이름'에 'MyEC2Clone' 입력 => EBS는 기존 instance의 'volume 유형'인 'gp2'를 그대로 따라감

<p align="center">
  <img src="" width=300><br>
</p>

  * 왼쪽 메뉴에서 'AMI' 선택 => 상태가 '사용 가능'으로 바뀔 때까지 기다림






- 복제 instance를 만들기 위해 다시 왼쪽 메뉴에서 '인스턴스' 선택 => 오른쪽 상단의 '인스턴스 시작' 선택 => 'AMI'에서 '내 AMI' 선택 => 복제할 AMI 선택






- '이름 및 태그'에서 '키'에 Name, '값'에 'MyWebServer-2' 입력 => 나머지는 기존과 그대로 한 후 '인스턴스 생성' 선택






- 생성된 'MyWebServer-2' 선택 후 연결 => '사용자 이름'에 'ec2-user'를 입력 => EC2 connector 접속








- 'sudo -s'와 'service httpd start'만 입력하여 web server를 실행 => 하단의 '퍼블릭 IP' 값을 url 창에 입력 => 전과 같이 test page가 생성된 것을 확인














실습 과정 (3) - EC2 종료
- 이전 글을 참고하여 Instance 종료











참고: AWS 강의실(https://www.youtube.com/c/AAAWS)

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
