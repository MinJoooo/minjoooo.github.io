---
layout: single
title: "[AWS] AWS 알아보기 Part 4 - EC2 소개 및 실습 (1)"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.08.
###### AWS 알아보기 Part 4
###### (EC2 소개 및 실습)

<br>
<br>
<br>


### EC2(Elastic Compute Cloud)

- 정의

  * 안전하고 크기 조정이 가능한 computing power를 cloud에서 제공하는 web service
  * 개발자가 더 쉽게 web 규모의 cloud computing 작업을 할 수 있도록 설계되어, computing resource에 대한 포괄적인 제어권을 제공함.

<br>

- 사용 예시

  * server를 구축할 때 (ex. game server, web server, application server)
  * application을 사용하거나 hosting할 때 (ex. DB, ML, bitcoin 채굴, 연구용 program)
  * 기타 목적 (ex. graphic rendering, game)

<br>

- 특성

  * **초 단위 on-demand price model**: on-demand model에서는 price가 초 단위로 결정됨. service 요금을 미리 약정하거나 선입금할 필요가 없음.
  * **빠른 구축 속도와 확장성**: 몇 분이면 전 세계에 instance 수백여대를 구축 가능
  * **다양한 구성방법 지원**: ML, web server, game server, image 처리 등 다양한 용도에 최적화된 server 구성이 가능함. 다양한 과금 model 사용도 가능함.
  * **여러 AWS service와 연동**: auto scaling(EC2의 숫자를 자동으로 조정해줌), ELB(Elastic Load Balancer, 다수의 EC2의 traffic을 분산해줌), CloudWatch(EC2를 monitoring) 등의 service와 연동 가능함.

<br>

- 구성

  * **Instance**: cloud에서 사용하는 가상 server로 CPU, memory, graphic card 등 연산을 위한 HW를 담당
  * **EBS(Elastic Block Storage)**: cloud에서 사용하는 가상 hard disk
  * **AMI**: EC2 instance를 실행하기 위한 정보를 담고 있는 image
  * **Security Group**: 가상의 방화벽


<br>


### EC2 실습

- 목표 및 과정

  * 목표: EC2 한 대를 provision하여 web server 구성하기
  * 과정: EC2를 구성하기 위한 AMI 선택 => EC2 유형과 size 선택 => EBS 설정 => Security Group 설정 => EC2 생성 => EC2 접속 후 web server 설치 및 web server 실행 => web browser에서 접속 test

<br>

**실습 과정 (1) - Instance 생성**

* 'EC2' 검색 후 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcLXkN9%2FbtrEkqMlM7Z%2FGFzHuqMu0xaqCl1HvSe9VK%2Fimg.png" width=600>
</p>

<br>

* 왼쪽 메뉴에서 '인스턴스' 선택 후 오른쪽 상단의 '인스턴스 시작' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FncbC2%2FbtrElc1pfMc%2FkPwK7ipT16Kcib7F2kdsK1%2Fimg.png" width=600>
</p>

<br>

* '이름 및 태그'에서 '태그 추가' 선택 => '키'에는 'Name'을, '값'에는 'MyWebServer'라고 입력 ('Name'에서 'N'을 꼭 대문자로 해야함)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDj9Bg%2FbtrEkpGIP8d%2FfncmmgTihiXUqLnUH9KBP0%2Fimg.png" width=450>
</p>

<br>

* 나중에 web server에 접속할 때 key pair가 반드시 필요함 => '키 페어'에서 '기존 키 페어 선택'을 해도 되고 '새 키 페어 생성'을 해도 됨 => '새 키 페어 생성'을 선택 => 이름을 'MyWebServerKP'로 변경 (다른 이름도 상관없음)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFUjSp%2FbtrEjiBlD1D%2F2amQZlCU1n2zpYU8OB4Quk%2Fimg.png" width=400>
</p>

<br>

* 외부(web browser)에서 접속 test를 할 것이기 때문에 Security Group 생성이 필요함 => '네트워크 설정'에서 오른쪽의 '편집' 선택 => '보안 그룹 생성' 선택 => 이름을 'MyWebServerSG'로 변경 (다른 이름도 상관없음) => 'Add security group rule' 선택 => '유형'에서 'HTTP' 선택 (web server는 http protocol로 접속하기 때문임) => '원본'에서 '0.0.0.0/0'과 '::/0' 선택 => FTP를 사용할 때 key pair가 반드시 필요하기 때문에 download 받은 key pair file(pem 형식)은 잘 보관해야 됨

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcc85dy%2FbtrEjLJ1Fng%2FVTdhSrFQDhroK8lC3NDQuK%2Fimg.png" width=450><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeqipLD%2FbtrEiIz05TK%2FBC4EIYvNMI8SQMGqt9Ok0k%2Fimg.png" width=450>
</p>

<br>

* 나머지는 일단 모두 기본값으로 설정 => 'AMI(Amazon Machine Image)'에서 'Amazon Linux 2 AMI' 선택, 'Instance 유형'에서 't2.micro' 선택, 'Storage 구성'에서 '8', 'gp2' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5Palm%2FbtrEiNhfspO%2Fnq5gy5jEx4r7j2SWCB1Qr1%2Fimg.png" width=450>
</p>

<br>

* 오른쪽 하단의 '인스턴스 시작' 선택
* 조금 기다리면 '인스턴스' 화면에 뜸.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVJPBH%2FbtrEiaD2u3E%2FAcaHzXY3PMCNCXfdZQy8iK%2Fimg.png" width=800>
</p>

<br>

* web server를 설치할 것이기 때문에 오른쪽 상단의 '연결' 선택 => 그대로 둔 채 '연결' 선택 =>  EC2 connector(web browser에서 직접 EC2 instance로 접속하는 것)로 접속됨

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtpWy1%2FbtrEiMWWNCs%2FwLLLRkCHksmEEoBrnWzjBK%2Fimg.png" width=450>
</p>

<br>

**실습 과정 (2) - EC2 connector**

* EC2 connector 접속 => 'sudo -s'를 입력하여 권한 획득 => 'yum install httpd -y'를 입력하여 web server를 설치

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtRmvj%2FbtrEiNn3c0T%2FCWtBKuMKQHbvNhSrINBLT1%2Fimg.png" width=600>
</p>

<br>

* 'service httpd start'를 입력하여 web server를 실행 => 하단의 '퍼블릭 IP' 값을 url 창에 입력 => test page가 생성된 것을 확인

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcyj9WH%2FbtrEjUtbeNt%2FxKEWeoIwORbAzl22GFlQQ1%2Fimg.png" width=400><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4tC6e%2FbtrEjNgLuww%2FBTlvqcEoMkKXz9jxCashV1%2Fimg.png" width=600>
</p>

<br>

* test page가 허전하니 index.html file을 만들기로 함 (index.html file은 user가 처음으로 접속했을 때 보이는 page임) => 'nano /var/www/html/index.html'을 입력 => 원하는 내용 작성 후 저장을 위해 'Ctrl + X' 입력 => 저장할 것이냐는 물음에 'Y' 입력 => 'Enter' 입력하여 빠져나옴

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcleW76%2FbtrEhZpal96%2FVhGLXqmg5ufS0xbot2LBB0%2Fimg.png" width=600>
</p>

<br>

* 다시 하단의 '퍼블릭 IP' 값을 url 창에 입력하여 page에 들어가면 내용이 변한 것을 확인할 수 있음.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzbH6h%2FbtrEhq8yvKz%2FpDCkb8ILeiNknxCi5wdgT1%2Fimg.png" width=800>
</p>

<br>

**실습 과정 (3) - EC2 종료**

  * 인스턴스를 중지시키거나 종료시키지 않으면 돈이 계속 나감 => 왼쪽 메뉴에서 '인스턴스' 선택 후 종료할 인스턴스를 선택 => 마우스 오른쪽 버튼 클릭 후 '인스턴스 중지'나 '인스턴스 종료' 선택 ('인스턴스 중지'를 해도 EBS 요금은 계속 나가기 때문에 사용하지 않을 것이라면 '인스턴스 종료'를 해야 함)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzCV8m%2FbtrEgpVPuZ2%2F1B5x7suZMP8v3oGggE9zF1%2Fimg.png" width=800>
</p>


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
