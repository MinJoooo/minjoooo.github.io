---
layout: single
title: "[AWS] AWS 알아보기 Part 2 - AWS 계정, Root user, IAM user, MFA 설정"
categories: AWS
tag: [AWS]
toc: false
---

<br>
<br>
<br>

###### 2022.06.04.
###### AWS 알아보기 Part 2
###### (AWS 계정, Root user, IAM user, MFA 설정)

<br>
<br>
<br>


### AWS 계정

- 소개

  * 처음 계정을 생성할 때 본인 명의 신용카드가 필요함.
  * 계정을 처음 생성하면 root user와 기본 resource(기본 VPC) 등이 생성됨.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHrmyw%2FbtrDSyslB6D%2FwSI0UwjjiCxRsWkZJsKMzK%2Fimg.png" width=600><br>
  AWS 계정을 처음 만들었을 때 제공되는 것들
</p>

<br>

- Root user

  * 계정 생성시 만든 e-mail 주소로 log-in
  * 생성한 계정의 모든 권한을 자동으로 가지고 있음.
  * 탈취당했을 때 복구가 매우 힘듦 => 사용을 자제하고 MFA 설정이 필요함
  * root user는 관리용으로만 이용하는 것이 좋음 (ex. 계정 설정 변경, billing)
  * AWS API 호출 불가 => AccessKey, Secret AccessKey 부여가 불가함

<br>

- IAM(Identity and Access Management) user

  * 만들 때 주어진 ID로 log-in
  * 기본 권한이 없음 => 따로 권한을 부여해야 함 (ex. 개발자 IAM, 디자이너 IAM, 회계팀 IAM)
  * AWS API 호출 가능 => AccessKey = ID 개념, Secret AccessKey = PW 개념
  * AWS 관리를 제외한 모든 작업은 관리용 IAM user를 만들어 사용함.
  * 권한 부여시 root user와 같이 모든 권한을 가질 수 있지만 billing 관련 권한은 root user가 허용해야 함.


<br>


### AWS 계정 생성하기

- AWS 계정 생성 후 root user로 log-in

  * 아직 IAM user를 만들지 않았기 때문에 root user로 log-in

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk1PFd%2FbtrDUI8b4mL%2FEt638zVPujz9V1OED2Ld3k%2Fimg.png" width=300>
</p>

<br>

- region 변경

  * 오른쪽 상단에서 region을 서울로 변경

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdFSrQL%2FbtrDVn3jjI1%2FZQHSY4rre9a3rhLzd4QKq1%2Fimg.png" width=400>
</p>

<br>

- MFA 설정

  * 오른쪽 상단에서 '보안 자격 증명' 선택 => '멀티 팩터 인증(MFA)' 선택 => 'MFA 활성화' 선택
  * '가상 MFA 디바이스' 선택 => QR code의 사진을 저장
  * 휴대폰에 '구글 OTP' application download => QR code scan 후 MFA code 입력

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0j8XR%2FbtrDUHBQDIY%2FcgYFeCavw0vKFXp6s7xgGK%2Fimg.png" width=600>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjMzn7%2FbtrDU4CW3Xd%2FKCxQ5QXkxffy7lC6VnM2I1%2Fimg.png" width=500>
</p>

<br>

- IAM user 생성

  * 왼쪽에 '대시보드' 선택 => 오른쪽 'AWS 계정'에서 '계정 별칭' 설정. IAM 계정 log-in시 계정 별칭으로 log-in 가능함.
  * 왼쪽에 '사용자' 선택 => 오른쪽 위에 '사용자 추가' 선택 => '액세스 키', '암호' 선택 => '사용자 지정 비밀번호' 설정 => 나만 사용할 것이니 '비밀번호 재설정 필요' 미선택. 만약 선택한다면, 다른 사람한테 넘겨주었을 때 그 사람이 log-in할 때 비밀번호를 재설정해서 사용할 수 있음. => '권한 설정'에서 '기존 정책 직접 연결' 선택 => 'AdministratorAccess' 선택. billing을 제외한 AWS의 모든 권한을 부여하는 것임. => 사용자 생성 완료하기

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcqqNgN%2FbtrDU1TSQcP%2FVMPpvhuTEkMKD0ZEIlYH20%2Fimg.png" width=600>
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxUHTA%2FbtrDUuCDki5%2FVaBCvvppAwqpqJaB7Zzfa0%2Fimg.png" width=600>
</p>

<br>

- IAM user에 MFA 설정

  * 왼쪽에 '사용자' 선택 => MFA를 설정할 user 선택 => '보안 자격 증명' 선택 => '할당된 MFA 디바이스'에 '관리' 선택

<br>

- root user log-out 후 IAM user log-in

  * root user로 할 일이 모두 끝남 => 앞으로는 IAM user로 log-in

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FuLYbS%2FbtrDUUgBOqi%2FBZI6QJqB2G31bkyW89kRc0%2Fimg.png" width=300>
</p>


<br>


참고: AWS 강의실(https://www.youtube.com/c/AAAWS)
