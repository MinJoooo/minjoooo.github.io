---
layout: single
title: "[Data Infrastructure] GFS, MapReduce, Hadoop 알아보기"
categories: Data-Infrastructure
tag: [Data Infrastructure]
toc: false
---

<br>
<br>
<br>

###### 2022.05.30.
###### GFS, MapReduce, Hadoop 알아보기

<br>
<br>
<br>

### GFS(Google File System)

- 2003년 Google에서 발표됨.

- 이전에 Google에서 사용하던 file system은 Big File이었는데, data가 급격히 늘어남에 따라 핵심 data 저장소와 검색 engine을 위해 최적화된 file system이 필요하게 됨.

- 동작 방식

  * 하나의 master node와 여러 개의 slave node로 구성됨. 기능으로 보면 master, chunk server, client로 이루어져 있음. master는 GFS 전체를 관리하고 통제하는 중앙 server의 역할이고, chunk server는 물리적인 server로 실제 입출력을 처리하고, client는 파일 입출력을 요청하는 client application임.
  * client가 master에게 파일의 읽기·쓰기를 요청함 => master는 client와 가까운 chunk server의 정보를 client에게 전달함 => client는 전달받은 chunk server와 직접 통신하며 I/O 작업을 수행함

- 장점

  * GFS의 장점은 Failure Tolerance로, 물리적으로 server 중 하나가 고장나도 정지하지 않고 잘 돌아가도록 설계되었다는 것임. 예를 들어 chunk server 중 하나가 고장이 나면 master는 고장나지 않은 chunk server의 정보를 전달하고, master server가 고장이 나면 다른 server가 master를 대체하게 됨. 이러한 이유로 chunk server는 가격이 저렴한 범용 컴퓨터들로 구성할 수 있게 되었고, cluster 환경에서 잘 동작할 수 있게 됨.


<br>


### MapReduce

- 2004년 Google에서 발표됨.

- 대용량 분산 cluster에서 data를 간단히 처리하는 기술

- 어떤 일을 master가 쪼개서 여러 대의 server에 나눠주면 그 일을 처리해서 server로 보내서 다시 map하고 reduce함.

- 동작 방식

  * Map 함수는 data 처리를 하고, Reduce 함수는 원하는 결과값을 계산함. 자세히 설명하면, Map 함수는 key-value를 input으로 받아 filtering하거나 다른 값으로 변환시켜주고, Reduce 함수는 Map을 통해 출력된 list에 새로운 key를 기준으로 groupping하고 이를 aggregation한 결과를 출력함.

- 장점

  * MapReduce는 여러 대의 컴퓨터에서 data를 처리하는 경우 병렬처리를 하기 때문에 확장이 쉬움. Scheduler가 data를 분산 배치하면 worker에서 작업을 수행하고 각 중간 결과는 로컬 디스크에 저장되며 나중에 Reduce 연산을 할당받으면 중간 결과를 읽어와서 작업을 수행하고 마찬가지로 file system에 저장함. => master node에 모든 data를 받아 처리하던 옛날 방식과 비교했을 때 통신 처리가 확실히 줄어든 것을 알 수 있음.


<br>


<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZH4Xs%2FbtrDE0Ht3Gb%2FrVwcqkdjIZ1jJJ3DBteqfK%2Fimg.png" width=800>
</p>


<br>


### Hadoop

- 2006년 Apache에서 발표됨.

- 대용량 data를 분산 처리하는 Java 기반의 open source framework

- HDFS(Hadoop Distributed File System)와 MapReduce로 구성됨. HDFS는 분산 저장이 가능한 data 저장소이고, MapReduce는 분산 처리 기술임.

- 여러 대의 server에 data를 저장하고 저장된 각 server에서 동시에 data를 처리하는 방식

- 동작 방식

  * HDFS와 MapReduce 둘 다 master/slave 구조인데, HDFS에서는 이를 각각 name node와 data node라 부르고, MapReduce에서는 이를 각각 JobTracker와 TaskTracker라고 부름. HDFS에서 name node는 파일의 meta 정보를 관리하고 실제 data는 여러 대의 data node에 분산해서 저장함. 데이터는 일정 크기의 block 단위로 나뉘어 관리되며 이 block들을 여러 대의 data node에 분산 및 복제해서 저장함. 이는 일부 data node에 장애가 발생하더라도 전체 시스템에서 데이터를 읽고 쓰는 것의 문제가 없게 하기 위함임.
  * MapReduce는 이렇게 HDFS에 분산 저장된 data를 여러 대의 TaskTracker에서 병렬로 처리함으로써 대용량의 data를 빠르게 처리하고자 만들어진 system임. 특히 MapReduce는 JobTracker에서 TaskTracker의 상태 및 전체 작업의 진행 상황 등을 지속적으로 감시하며 일시적인 장애에 대해 자동으로 복구하는 기능을 제공하기 때문에 일부 TaskTracker 장비에 문제가 발생하더라도 전체 작업이 진행되는 데 문제가 없도록 설계되어 있음. 또한 JobTracker가 여러 대의 TaskTracker에게 자동으로 작업을 할당하고 결과를 통합해주기 때문에 사용자는 전체 작업 흐름 및 세부 사항에 크게 신경쓰지 않고 data 처리 logic에만 집중할 수 있음.

- name node의 핵심 기능

  * meta data 관리: file system을 유지하기 위한 meta data를 관리
  * data node monitoring: data node는 name node에게 3초마다 heartbeat를 전송함. name node는 이를 이용하여 data node의 실행 상태와 용량을 체크함. heartbeat를 전송하지 않는 data node는 장애 server로 판단함.
  * block 관리: 장애가 발생한 data node의 block을 새로운 data node에 복제함. 용량이 부족하다면 여유가 있는 data node에 block을 옮김.
  * client 요청 접수: client가 HDFS에 접근하려면 반드시 name node에 먼저 접속해야 함. HDFS에 file을 저장할 경우 기존 file의 저장 여부와 권한 확인 절차를 거쳐 저장을 승인함. data node는 client가 HDFS에 저장하는 file을 local disk에 유지함. 이때 file은 두 가지로 저장되는데 하나는 실제 저장되는 raw data이고 다른 하나는 checksum이나 file 생성일자 같은 meta data가 저장된 file임.


<br>


참고: https://eehoeskrap.tistory.com/220 [Enough is not enough], https://swalloow.github.io/map-reduce/
