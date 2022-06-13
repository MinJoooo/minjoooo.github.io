---
layout: single
title: "[AWS] AWS 알아보기 Part 8 - Auto Scaling"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.13.
###### AWS 알아보기 Part 8
###### (Auto Scaling)

<br>
<br>
<br>


### Auto Scaling

-정의

  * application을 monitoring하고 용량을 자동으로 조정하여, 최대한 저렴한 비용으로 안정적이고 예측 가능한 성능을 유지함.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDB3cn%2FbtrEu8zSDgb%2FqcRK1mNX78cBxHSrc2TJa0%2Fimg.png" width=400><br>
  AWS에서 지원하는 Auto Scaling<br>
</p>

<br>

- 목표

  * 정확한 수의 EC2 instance를 보유하도록 보장함. group의 최소와 최대 instance 개수 사이를 유지하도록 instance를 추가하고 삭제함. 다양한 scaling 정책을 적용하여 CPU 부하에 따라 instance를 추가하고 삭제함.
  * 가용 영역에 instance가 골고루 분산될 수 있도록 분배함. 하나의 가용 영역에 문제가 생기면 service에 심각한 장애가 발생하기 때문에 이를 방지하기 위함임.

<br>

- 기본 개념 - Scaling Type

  * **Vertical Scale (Scale Up)**: 성능의 증가와 비용의 증가가 비례하지 않음. 기술 문제 때문에 성능을 무한대로 늘릴 수 없음.
  * **Horizontal Scale (Scale Out)**: 성능의 증가와 비용의 증가가 비례함. 성능을 무한대로 늘릴 수도 있음. 대신 여러 instance를 사용할 수 있는 program이나 architecture가 잘 준비되어야 함.
  * cloud 환경에서는 저렴한 것을 많이 사용하고 그것을 줄였다 늘였다 할 수 있는 Horizontal Scale이 더 합리적임.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb8dgUx%2FbtrExGPH2dJ%2FGd5g64SKk6XukwTpUVheqK%2Fimg.png" width=300> <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvY6Xp%2FbtrEuWeeoBg%2FLwUgupTtBOmjWZtiSKkP5K%2Fimg.png" width=500><br>
</p>


<br>


### Auto Scaling 구성

- Launch configurations(시작 구성) / Launch template(시작 템플릿)

  * '무엇을 실행시킬 것인가?'에 관함.
  * 우리가 올리고 싶은 EC2의 기본 정보들을 담음. auto scaling을 통해 instance를 늘리고 싶을 때 어떤 instance를 늘릴 것인지에 대한 내용임.
  * EC2 type과 size, AMI, Security Group, Key, IAM, User data 등

<br>

- Monitoring

  * '언제 실행시킬 것인가? + 상태 확인'에 관함.
  * auto scaling이 어떤 조건에 실행될 것인지에 대한 내용임. 어떤 상황에서 auto scaling을 실행시킬 것인지를 알기 위해 monitoring을 함.
  * CloudWatch (and/or) ELB와 연계됨.
  * 상황 예시: CPU 점유율이 일정 %을 넘어섰을 때, 2개 이상이 필요한 스택에서 EC2 하나가 죽었을 때

<br>

- 설정

  * '얼마나 어떻게 실행시킬 것인가?'에 관함.
  * auto scaling에 적용할 설정 내용
  * 최대/최소/원하는 instance 개수, ELB와 연동 등

<br>

- 간소화한 동작 방식

  * EC2 instance cluster에 8개의 instance가 필요하다고 가정 => EC2 1개가 죽음 => auto scaling이 감지함 => launch template에 맞는 instance를 생성해 auto scaling cluster에 다시 넣어줌

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbbZt8B%2FbtrEHCmqTrA%2Fsm8MPyochMnBpqk9hgjZJ0%2Fimg.png" width=600><br>
</p>


<br>


### Auto Scaling 실습

**실습 과정 (1) - Launch template 만들기**

* 'EC2' 검색 후 왼쪽 메뉴에서 '시작 템플릿' 선택 => 오른쪽에 '시작 템플릿 생성' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fuqfh4%2FbtrEF80qDo9%2FVphOoX67V35R4gRaQdVcOK%2Fimg.png" width=600><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrRXzJ%2FbtrEHCfJU3n%2FPwGFg0Ii5QWRdGHacRcAZk%2Fimg.png" width=600><br>
</p>

<br>

* '시작 템플릿 이름 및 설명'에서 '시작 템플릿 이름'에 'MyTemplate' 입력 => 'EC2 Auto Scaling에 사용할 수 있는 템플릿을 설정하는 데 도움이 되는 지침 제공'에 체크

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb1KIN0%2FbtrEEV7vsBC%2FI0XURff94bcmEBAIRW2vKk%2Fimg.png" width=500><br>
</p>

<br>

* 'AMI'에서 'Amazon Linux 2 AMI' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FA8pZI%2FbtrEIt3u1MK%2Fsf7BskuY2uelug8ZKpWkg0%2Fimg.png" width=500><br>
</p>

<br>

* '인스턴스 유형'에서 't2.micro' 선택 => '키 페어'에서 '시작 템플릿에 포함하지 않음' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDzkBn%2FbtrEF8M3Juc%2F5BUZLvNESHk2KvAmCSKwl1%2Fimg.png" width=600><br>
</p>

<br>

* '네트워크 설정'에서 '보안 그룹'에 'default' 선택 (VPC에서 security group을 설정하지 않은 이유는 network interface에서 security group을 설정해주기 때문임)

* '어드밴스드 네트워크 구성'에서 '네트워크 인터페이스 추가' 선택 => '네트워크 인터페이스'에 '새 인터페이스' 선택, '퍼블릭 IP 자동 할당'에 '활성화' 선택, '보안 그룹'에 'default' 선택 => '종료 시 삭제'에 '예' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLTV0G%2FbtrEItPZd2y%2Fzj3a9qBXpNWvSgkX1b2VMk%2Fimg.png" width=600><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbtuhhM%2FbtrEI7TtwvM%2Fu85WQl5MKvr39Uk0ckK9M0%2Fimg.png" width=600><br>
</p>

<br>

* '시작 템플릿 생성' 선택

<br>

**실습 과정 (2) - Auto Scaling Group 만들기**

* '다음 단계'의 '템플릿에서 Auto Scaling 그룹 생성'에서 'Auto Scaling 그룹 생성' 선택 (또는 'EC2' 검색 후 왼쪽 메뉴에서 'Auto Scaling 그룹' 선택 후 'Auto Scaling 그룹 생성'을 선택해도 같음)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fzdf41%2FbtrEItWNGNl%2FXkozpOQ5tqREDDWPVB69Yk%2Fimg.png" width=800><br>
</p>

<br>

* '이름'의 'Auto Scaling 그룹 이름'에 'MyASG' 입력 => '시작 템플릿'에 'MyTemplate' 선택

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7WwrB%2FbtrECvad8TJ%2FKaUNOMW2bsr1S6C9GNx1K0%2Fimg.png" width=600><br>
</p>

<br>

* '네트워크'의 '가용 영역 및 서브넷'에 모든 가용 영역 선택 (선택한 가용 영역에만 instance가 올라감. 바꿔 말해 선택하지 않은 가용 영역엔 올라가지 않는데, 굳이 선택하지 않을 이유가 없으니 다 선택함.)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdIOAaN%2FbtrEJJkg7Tk%2FurGhkpalyeLqwUR8UdyiF0%2Fimg.png" width=600><br>
</p>

<br>

* '그룹 크기'의 '원하는 용량'과 '최소 용량'과 '최대 용량'에 모두 2를 입력 (다른 숫자를 입력해도 됨)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTXMpI%2FbtrEHVlVzLK%2FiOKwzZmg5HKByBuKMUKdkK%2Fimg.png" width=600><br>
</p>

<br>

* '태그'에서 '태그 추가' 선택 => '키'에 'MyTag' 입력, '값'에 'tagvalue' 입력 (tag를 추가하면 auto scaling group의 모든 instance에 같은 tag가 적용된다는 장점이 있음)

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fm4VDH%2FbtrEHT2vCP7%2FkpnATudNQGbrzjtFCii5jk%2Fimg.png" width=600><br>
</p>

<br>

* 'Auto Scaling 그룹 생성' 선택

<br>

**실습 과정 (3) - Auto Scaling Group 확인**

* 'EC2' 검색 후 왼쪽 메뉴에서 'Auto Scaling 그룹' 선택 => 확인하고자 하는 auto scaling group 선택

* '활동'에서 group의 설정에 따라 instance가 생성되고 삭제되는 작업 내용을 확인 가능 => '인스턴스 관리'에서 group에 속한 instance 확인 가능

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FB0T7l%2FbtrEHVGhCpx%2FXExhjZGGjKsgedaOmKI4C1%2Fimg.png" width=600><br>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbtjYsW%2FbtrEHUNPL3g%2FzZcMBphwKOkkKKaUkuaMX1%2Fimg.png" width=600><br>
</p>

<br>

* 임의로 instance 하나를 삭제하면, auto scaling group에서 감지 후 instance를 새로 생성하는 것을 확인할 수 있음.

* 실습을 종료하고 싶으면 선택한 auto scaling group의 '세부 정보'에서 '편집'을 선택한 후 '원하는 용량'과 '최소 용량'에 0을 입력

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc6Uzn0%2FbtrEHXjM434%2FCduIsi5QiHzJgfBsyPkxl1%2Fimg.png" width=300><br>
</p>


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
