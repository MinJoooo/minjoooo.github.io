---
layout: single
title: "[AWS] AWS 알아보기 Part 9 - ELB 소개 및 실습"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

<p align="center">
  ###### 2022.06.14.
  ###### AWS 알아보기 Part 9
  ###### (ELB 소개 및 실습)
</p>
  
<br>
<br>
<br>


# ELB(Elastic Load Balancer)

<br>

- 기본 개념 - Load Balancing

  * auto scaling group을 이용하면 다수의 instance를 효율적으로 활용해서 안정적인 service를 제공할 수 있다는 장점이 있음. 그러나 이를 사용하는 user 입장에서는 모든 instance의 IP 주소를 각각 알고 있어야 각각에 접근할 수 있음. => 각 instance를 모두 관리할 수 없으니, 부하를 분산해주는 load balancing service 없이는 활용이 불가능함. => ELB가 생겨남 

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKqmn8%2FbtrEI8LHrZg%2FI5QllWQJZA60P3Rrg7hiD0%2Fimg.png" width=800><br>
  ELB 없이는 user가 각 instance의 IP를 모두 알고 있어야 함<br>
</p>

<br>

- 정의

  * 들어오는 application traffic을 EC2 instance, container, IP address, Lambda 함수와 같은 여러 대상에 자동으로 분산시킴.
  * 단일 또는 여러 가용 영역에서 다양한 application 부하를 처리할 수 있음. 

<br>

- 소개

  * load balancer가 다수의 instance를 하나로 묶어서, traffic을 하나의 경로로 받아 분산해 줌 => 다수의 service에 traffic을 분산해주는 service
  * user 입장에서는 각각의 instance의 IP에 접근하는 것이 아니라 하나의 address로 접근하기만 해도 load balancer가 알아서 통신 traffic을 분산해 줌.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6HSIU%2FbtrEHWkZJwQ%2FGXJTQp6Q38enFvHF268ok1%2Fimg.png" width=800><br>
  ELB가 모든 instance를 관리해줌<br>
</p>

<br>

- 기능 소개

  * **Health check**: 직접 traffic을 발생시켜 instance가 살아있는지 check
  * Autoscalinng과 연동 가능함.
  * 여러 가용 영역에 분산 가능함.
  * 지속적으로 IP address가 바뀌며 IP 고정이 불가능함 => 항상 domain 기반으로 사용해야 함


<br>
<br>


# ELB 종류

<br>

- Application Load Balancer

  * 똑똑한 녀석
  * traffic을 monitoring하여 routing이 가능함.
  * 예를 들어, image.sample.com은 image server로, web.sample.com은 web server로 traffic을 분산함 => user가 어떤 address로 접속을 했는지에 따라 다른 경로로 routing이 가능함 => address를 읽을 수 있는 똑똑한 load balancer임

<br>

- Network Load Balancer

  * 빠른 녀석
  * TCP 기반의 빠른 traffic을 분산함.
  * Elastic IP(IP를 고정시켜주는 service) 할당이 가능함.

<br>

- Classic Load Balancer

  * 옛날 녀석
  * 예전에 사용되던 type으로 현재는 잘 사용하지 않음.

<br>

- Gateway Load Balancer

  * 먼저 traffic을 check하는 녀석
  * 가상 appliance 배포/확장 관리를 위한 service
  * 예를 들어, client가 gateway load balancer를 통해 EC2 등의 service에 접속할 때, traffic을 먼저 network appliance에 전달시켜 traffic을 이용해 무언가를 하게 함. EC2에 도달하기 전에 먼저 traffic을 검사/분석/인증/logging하는 등의 작업을 수행할 수 있도록 도와주는 service.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSkbaW%2FbtrELU0qIKC%2Fm8Rny8zfkvrD8Jz6w9XPjK%2Fimg.png" width=700><br>
</p>


<br>
<br>


# Application Load Balancer(ALB)

<br>

- Target Group

  * **정의**: ALB가 routing할 대상의 집합. 분산할 때 어디로 분산할 것인지를 모은 group임.
  * Instance, IP address(private), Lambda, ALB(다른 ALB와 연결시키는 것이 가능함)
  * Protocol (ex. HTTP, HTTPS, gRPC 등)
  * 기타 설정 (traffic 분산 algorithm, 고정 session 등)

<br>

- 동작 과정

  * user가 ALB를 통해 통신을 함 => ALB는 똑똑해서 address와 port를 읽어낼 수 있음 => dashboard.mydomain.com으로 가면 dashboard target group으로, lambda.mydomain.com:6060으로 가면 lambda 함수 target group으로 traffic을 전송함

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FelzK55%2FbtrEI77vp1X%2FXvsZCiOOz3UkkbJAZXgXC1%2Fimg.png" width=800><br>
</p>

<br>

- Architecture

  * **일반적인 architecture 구조**: AWS Cloud 안에 VPC가 있음 => 다양한 Availability Zone 안에 Auto Scaling을 통해 EC2를 분산해놓음 => 고가용성과 장애내구성을 가진 EC2 구성 service가 됨

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblATkp%2FbtrEH35Gpn1%2FIDH3HHKBqJfe21kZ5WJKZ0%2Fimg.png" width=800><br>
</p>


<br>
<br>


# Application Load Balancer 실습

<br>

### 실습 과정 (1) - Launch template 수정하기

* 'EC2' 검색 후 왼쪽 메뉴에서 '시작 템플릿' 선택 => 만들어놓은 시작 템플릿 선택 => '작업' 선택 후 '템플릿 수정(새 버전 생성)' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdIjD4H%2FbtrEKUT41SR%2FnMnOO5waTpzWU70bh2dDh0%2Fimg.png" width=800><br>
</p>

<br>

* '시작 템플릿 이름 및 버전 설명'에서 '템플릿 버전 설명'에 '웹서버 띄우기' 입력 => 'EC2 Auto Scaling에 사용할 수 있는 템플릿을 설정하는 데 도움이 되는 지침 제공'에 체크

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsPUQc%2FbtrELU7ozMo%2FWuJuB5cMk7P3FDj8TkHvU1%2Fimg.png" width=600><br>
</p>

<br>

* 'AMI'에서 'Amazon Linux 2 AMI' 선택, '인스턴스 유형'에서 't2.micro' 선택, '키 페어'에서 'MyWebServerKP' 선택, '네트워크 설정'에서 '기존 보안 그룹 선택' 후 'MyWebServerSG' 선택

* '고급 세부 정보'에서 '사용자 데이터'에 아래의 script를 입력 => EC2 instance가 올라갈 때 자동으로 script를 실행시키도록 정의함

* **script 내용**: instance ID를 가져옴 (instance는 각각 자신의 ID/meta data를 불러올 수 있음) => web server를 download 받아 install 함 => index.html에 방금 받아온 instance ID를 넣음 => web server를 실행시킴


```
#!/bin/bash
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
yum  install httpd -y
echo ""$INSTANCE_ID"" >> /var/www/html/index.html
service httpd start
```

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbnIeFL%2FbtrEJwe7Bnn%2FQoK7haaK8AAa26V8cNpCDk%2Fimg.png" width=600><br>
</p>

<br>

* '템플릿 버전 생성' 선택


<br>
<br>


### 실습 과정 (2) - Auto Scaling Group 수정하기

* 'EC2' 검색 후 왼쪽 메뉴에서 'Auto Scaling 그룹' 선택 => 만들어놓은 auto scaling group 선택 => '세부 정보'의 '시작 템플릿'에서 '편집' 선택 => '버전'에서 'Latest' 선택 => '업데이트' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FonxH7%2FbtrELVeCpNq%2FJPvYt53ikKytDBg6vfDk20%2Fimg.png" width=700><br>
</p>

<br>

* '세부 정보'의 '그룹 세부 정보'에서 '편집' 선택 => '원하는 용량'과 '최소 용량'에 2를 입력 (다른 숫자를 입력해도 됨)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsXTek%2FbtrEJ4ws5Qz%2FHOmruoYaRYCNXuJFzN2YV1%2Fimg.png" width=600><br>
</p>


<br>
<br>


### 실습 과정 (3) - Instance 확인하기

* 'EC2' 검색 후 왼쪽 메뉴에서 '인스턴스' 선택 => 실행 중인 인스턴스의 '퍼블릭 IPv4 주소' 값을 url 창에 입력

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2V8lJ%2FbtrEIsSrCeg%2FlWOKShqT9Ajx5MzXAfw24k%2Fimg.png" width=800><br>
</p>

<br>

* page에 instance ID가 뜨는 것을 확인할 수 있음.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtVyMn%2FbtrENuOJ6gF%2F5xL5dfkDcFRaGwCyjRzgzk%2Fimg.png" width=500><br>
</p>


<br>
<br>


### 실습 과정 (4) - Load Balancer 생성하기

* 'EC2' 검색 후 왼쪽 메뉴에서 '대상 그룹' 선택 => 오른쪽 상단에 'Create target group' 선택

* 'Basic Configuration'에서 'Choose a target type'에 'Instances' 선택 => 'Target group name'에 'MyWebTargetGroup' 입력 => 'Next' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FowzO9%2FbtrEKWyi6tX%2FIYOOkinKaaYVHxiPiZTP8K%2Fimg.png" width=700><br>
</p>

<br>

* 'Available instances'에서 모든 instance 선택 => 'Include as pending below' 선택 => 'Review targets'에 선택한 instance가 뜨는 것을 확인 => 'Create target group' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKY6uL%2FbtrELT9cLIV%2FrBtHcjW7DjNeFXRP7VUqkK%2Fimg.png" width=800><br>
</p>

<br>

* 'EC2' 검색 후 왼쪽 메뉴에서 '로드밸런서' 선택 => '로드 밸런서 생성' 선택 => 'Application Load Balancer' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTDT0m%2FbtrELV0gu5c%2F0S7uSLrUAXCcESD1QGXVG0%2Fimg.png" width=700><br>
</p>

<br>

* 'Basic configuration'에서 'Load balancer name'에 'MyWebALB' 입력

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6pQYd%2FbtrEJ5bdRmO%2FgKDXnXIaQUEpE34IJfDxO0%2Fimg.png" width=700><br>
</p>

<br>

* 'Network mapping'에서 'Mappings'에 모든 Availability Zone을 선택

<p align="center">
  <img src="" width=800><br>
</p>

<br>

* 'Security groups'에 'MyWebServerSG' 선택 => 'Listeneres and routing'에서 'Default action'에 'MyWebTargetGroup' 선택 (다음 조건(HTTP)으로 통신하면 이 target group으로 연결시키겠다는 뜻임) => 'Create load balancer' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FW7Kwq%2FbtrELUHmm96%2FkTTRkK1xRnqjEUAHjMEUOk%2Fimg.png" width=700><br>
</p>

<br>

* 왼쪽 메뉴에서 '로드밸런서'에 들어가면 생성한 load balancer가 활성된 것을 확인 => 선택 후 '설명'을 보면 'IP 주소 유형'에 'ipv4'라고 적혀 있지만 실제로 IP address는 없고 DNS로만 작동함 => 'DNS 이름' 값을 url 창에 입력

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWNnS3%2FbtrENnWk7Yn%2FvHKI1L8U1nkiuEqyBKYXD0%2Fimg.png" width=800><br>
</p>

<br>

* 같은 url이지만 새로고침 할 때마다 내용이 바뀌는 것을 확인할 수 있음 => load balancer가 2개의 EC2에 번갈아가며 traffic을 분산해주기 때문임

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxQjaj%2FbtrEJxlPXpJ%2F4o6egqlWArSK4O1U9XhDu1%2Fimg.png" width=500><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmwUch%2FbtrEKU8FB5r%2FObQtw9vX8Pcxjv1OWMe0e0%2Fimg.png" width=500><br>
</p>


<br>
<br>


### 실습 과정 (5) - Load Balancer와 Auto Scaling 붙이기

* 'EC2' 검색 후 왼쪽 메뉴에서 'Auto Scaling 그룹' 선택 => 만들어놓은 auto scaling group 선택 => '세부 정보'의 '로드 밸런싱'에서 '편집' 선택 => '애플리케이션, 네트워크 또는 게이트웨이 로드 밸런서 대상 그룹'에 체크 후 대상 그룹에 만들어놓은 target group선택 -> '업데이트' 선택 => 이제부터 선택한 auto scaling group에서 생성되는 모든 instance는 선택한 target group에 속하게 됨

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdSoX1e%2FbtrEJw1Btxz%2FMg8CBmzh3Fa0tD0Ed6TFlk%2Fimg.png" width=700><br>
</p>

<br>

* 기존 instance를 삭제하고 새로 생기는 instance를 확인 => 왼쪽 메뉴에서 '대상 그룹'에 들어가 만들어놓은 target group 선택 => 다음과 같이 target group 안에 새로 만들어진 instance가 자동으로 속해있는 것을 확인  

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbrIBbu%2FbtrENuHZebQ%2FjCzohffFVyhavnnMiKlmc1%2Fimg.png" width=800><br>
</p>

<br>

* 이때 auto scaling group에서는 정상적인데 ELB에서는 비정상적인 경우가 생기면 어떻게 되는지 궁금해짐 => 예를 들어, EC2 instance는 올라가있는데 그 instance의 web server가 죽은 경우는 어떻게 되는지 궁금해짐. load balancer 입장에서는 web server가 죽으면 무언가 보여줄 게 없기 때문에 비정상인 상황이지만, auto scaling group 입장에서는 web server가 올라가있는지 내려가있는지는 관심이 없고 EC2가 정상적으로 올라가있으면 상관이 없음.

* 위의 상황 실험을 위해 하나의 instance의 EC2 connector에 접속 => 'sudo -s'와 'service httpd stop'을 입력

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZWiZZ%2FbtrENHmQJjd%2FKlyXQAwZ5aG72rfUgRvmBK%2Fimg.png" width=500><br>
</p>

<br>

* 정상적인 instance의 web server와 비정상적인 instance의 web server인 '502 Bad Gateway'가 반복해서 뜸. '502 Bad Gateway'는 traffic을 분산해주기 위해 traffic을 보냈는데 아무런 traffic 응답이 없을 때 나오는 화면임. (다시 실습해보니 '502 Bad Gateway'는 안 뜨고 정상적인 instance의 web server만 뜸.)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FW3zsR%2FbtrENfxBf8h%2FFFeIohsjapzgeko1grI8Kk%2Fimg.png" width=500><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlsLmx%2FbtrEOHGJskV%2F0WegjNhl8UCJvUeDZCakDK%2Fimg.png" width=500><br>
</p>

<br>

* 이를 해결하기 위해 왼쪽 메뉴에서 'Auto Scaling 그룹' 선택 => 만들어놓은 auto scaling group 선택 => '세부 정보'의 '상태 확인'에서 '편집' 선택 => '상태 확인 유형'에서 'ELB' 체크 => '업데이트' 선택

* 이제 EC2의 상태 확인뿐만 아니라 ELB의 상태 확인도 하게 됨 (instance의 web server가 끊겼을 때 ELB 입장에서는 이것을 이미 'unhealthy'라고 판단했음)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5VpGe%2FbtrEKVmmxWC%2FL1tiUAORgaxjgaDrOwuAHK%2Fimg.png" width=700><br>
</p>

<br>

* 왼쪽 메뉴에서 '대상 그룹' 선택 => 만들어놓은 target group 선택 => 'Health checks'를 보면 unhealthy로 판단하는 조건을 알 수 있음 (2번 연속 상태 검사 실패 시 unhealthy로 판단함)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdykDBd%2FbtrENEjqD0t%2FtKJREnk8kBxVKgm0xQLnq0%2Fimg.png" width=800><br>
</p>

<br>

* 선택한 auto scaling gruop의 '활동'에 들어가면 'Failed'가 뜨는 것을 확인할 수 있음 => '인스턴스 관리'에 들어가면 'Unhealthy'가 뜨는 것을 확인할 수 있음

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJkCqO%2FbtrELTWaJKs%2Fsz5dB9jZ6yDxhH92L9K79k%2Fimg.png" width=800><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FceRZpb%2FbtrENgXD3FJ%2FnbKr6bNL9JpNkxkQVuyXOK%2Fimg.png" width=800><br>
</p>

<br>

* auto scaling group은 unhealthy한 instance를 종료시키고 새로운 instance를 생성함 => 왼쪽 메뉴에서 '인스턴스'에 들어가면 확인할 수 있음

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FybE3V%2FbtrEObagmoN%2F1OgP53jQtwsJaSWc5OL9X0%2Fimg.png" width=800><br>
</p>

<br>

* 선택한 auto scaling group의 '세부 정보'에서 '편집'을 선택한 후 '원하는 용량'과 '최소 용량'에 0을 입력하여 실습 종료


<br>
<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
