---
layout: single
title: "[Data Engineering] Hadoop, Spark 알아보기"
categories: Data-Engineering
tag: [Data Engineering]
toc: false
---

<br>
<br>
<br>

###### 2022.05.31.
###### Hadoop, Spark 알아보기

<br>
<br>
<br>


<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZH4Xs%2FbtrDE0Ht3Gb%2FrVwcqkdjIZ1jJJ3DBteqfK%2Fimg.png" width=800>
</p>


<br>


### Hadoop

- HDFS(Hadoop Distributed File System)와 MapReduce로 구성됨. HDFS는 분산 저장이 가능한 data 저장소이고, MapReduce는 분산 처리 기술임.

- 여러 대의 server에 data를 저장하고, 저장된 각 server에서 동시에 data를 처리하는 방식

- 특징

  * Distributed: 수십만 대의 컴퓨터에 자료 분산 저장 및 처리
  * Scalable: 용량이 증대되는대로 컴퓨터 추가
  * Fault-tolerant: 하나 이상의 컴퓨터가 고장나는 경우에도 system이 정상 동작
  * Open source


<br>


### Hadoop을 사용하는 이유

- hard disk drive 용량이 엄청나게 증가한데 반해 access 속도는 그에 훨씬 미치지 못하여 drive를 읽는 데 시간이 굉장히 오래 걸렸음 => 이 시간을 줄이기 위해 여러 disk로부터 data를 한 번에 읽는 distributed programming을 하는 Hadoop을 사용함

- distributed computing 방식으로 구축 비용이 저렴하며 그 비용 대비 data 처리가 굉장히 빠름.

- 장애를 대비하여 매번 운영한 이후의 결과들을 disk에 기록함 => 문제가 발생했을 때 기록된 결과들을 활용하여 문제를 해결하기 쉽다는 장점이 있음


<br>


### RDBMS 대신 Hadoop을 사용하는 이유

- Oracle, MySQL과 같은 기존의 RDBMS는 무결성이 높은 대신 비쌈. 이에 반해 Hadoop은 무결성이 낮은 대신 open source이고 저렴함.

- transaction이나 무결성을 보장해야 하는 data 처리에는 RDBMS가 적합함. 하지만 배치성으로 data를 저장하고 처리하는 데는 Hadoop이 더 적합함 => Hadoop은 기존의 RDBMS를 대치하는 것이 아님

- ex. 쇼핑몰에서 회원가입이나 결제 진행 등은 transaction이나 무결성을 보장해야 하는 작업임. 이런 것들은 Hadoop으로 처리하면 안 되고 RDBMS로 처리해야 함. 대신 회원이 관심있게 보는 제품과 같은 data는 Hadoop으로 처리하는 게 적합함. 이런 것들을 매번 비용이 비싼 RDBMS에 저장하면 낭비이기 때문임. 따라서 Hadoop은 RDBMS와 경쟁하는 것이 아닌 RDBMS와 협력하는 것이라 볼 수 있음.


<br>


### Hadoop의 문제점과 Spark의 등장 배경

- big data를 다룰 때 가장 일반적으로 쓰이는 기술이 Hadoop의 MapReduce 기술과 그 연관기술인 Hive임. MapReduce는 슈퍼컴퓨터 없이 server를 여러 대 연결해 big data 분석을 가능하게 함.

- 하지만 시간이 지나면서 여러 단점들이 보이기 시작함 => 그래서 대안으로 나온 것이 Spark임. Spark는 MapReduce와 비슷한 목적의 업무를 수행하는데 메모리를 활용한 굉장히 빠른 데이터 처리를 할 수 있다는 특징이 있음.


||Hadoop MapReduce|Spark|
|:---:|:---:|:---:|
|Speed||MapReduce보다 100배 더 빠름|
|Written In|Java|Scala|
|Data Processing|Batch processing|Batch / Real-time / Iterative / Interactive / Graph|
|Ease of Use||Hadoop보다 간단하고 쉬움|
|Caching|data caching을 지원하지 않음|In-Memory Cache라 더 빠름|


<br>


### Apache Spark

- big data workload에 주로 사용되는 분산 처리 open source framework

- 기존의 Hadoop을 통해 끌어오는 data들은 시간 소요가 크기 때문에 실시간으로 분석해야 하는 업무에서는 어려운 부분이 있어 새롭게 개발됨.

- 원래 Hadoop용으로 설계되어 개발되었기 때문에 Hadoop과 Spark를 함께 사용할 때 최상의 효과를 낼 수 있음. 하지만 Hadoop의 HDFS 외에 다른 cloud 기반 data platform과도 융합될 수 있어 Hadoop이 반드시 필요한 건 아님.

- 빠른 성능을 위해 In-Memory Cache와 최적화된 실행을 사용하고 일반적인 batch 처리, streaming 분석, ML, graph DB 등을 지원함.

- 함수형 programming이 가능한 언어 Scala를 사용해 간단한 코드로 Interactive shell을 사용할 수 있음.

- Spark 2.0부터는 Untyped API일 때는 DataFrame, Typed API일 때는DataSet으로 지원함. => DataFrame과 DataSet을 다 지원하는 구조가 됨.


<br>


### Spark와 RDD(Resilient Distributed Datasets)

- Spark는 RDD + Interface라고도 할 수 있을 정도로, Spark의 핵심은 RDD임.

- MapReduce가 big data 분석을 쉽게 만들어주긴 함. 그런데 ML이나 graph와 같이 복잡하고 multi-stage한 처리나 interactive하고 ad-hoc한 query의 처리를 잘 하지 못함 => 이를 해결하기 위해 pregel 같은 특수 목적 분산 framework들이 나왔지만 더 근본적인 접근은 없는지에 대해 생각해봄 => MapReduce가 iteration에서 힘든 이유는 각 iteration을 돌 때마다 stage간 자료 공유가 HDFS를 거치지 않기 때문이라 생각함. 그래서 이를 저렴한 RAM을 이용해 해결하기로 함. => 효율적인 data sharing 도구의 탄생

- 즉, HDFS에서 read와 write를 반복하면 느리니까 RAM에 저장해서 사용하기로 함. query를 처리할 때마다 처음부터 HDFS에서 읽어오기보단 RAM에 한 번 올려놓고 그 다음에 query들을 처리함. => 그런데 RAM은 빠르지만 꺼지면 정보가 날아간다는 문제가 생김. 그러면 어떻게 fault-tolerant하게 만들지를 고민함 => RDD의 탄생. RAM을 read-only로 사용하는 것임. Resilient는 '쉽게 복원이 되는'이라는 뜻으로, RDD는 data가 한 번 수정이 되면 바뀌지 않는 immutable, partitioned collections of records 특성을 가짐. => immutable는 'read-only'이기 때문에 만들어진 이래 고쳐진 적이 없다는 뜻임. 그렇다면 어떻게 만들어졌는지만 기록해두면 또 만들 수 있는 것임. 부모로부터 어떻게 만들어졌는지 lineage(계보)만 기록해도 falut-tolerant함. => coding하는 것은 실제로 계산하는 작업이 아니고, lineage를 DAG로 디자인하는 작업임. 자료가 어떻게 변해갈지를 그리는 transformation 과정에서 실제 계산은 일어나지 않고, lazy-execution함.

- lazy-execution: 실제로 요청하는 작업인 action 명령이 불리기 전까지는 실행되지 않고, action 명령이 불리면 그제서야 쌓였던 것들을 실행함 => lineage를 다 그려놓은 상태에서, 즉 대강의 execution plan이 다 만들어진 상태에서 실행하므로 자원이 배치될 상황을 미리 고려해서 최적의 course로 실행할 수 있음.

- 결론: Spark가 빠른 이유는 RAM을 ROM처럼 써서 fault-tolerant하고 efficient한 RAM storage를 만들었기 때문임.


<br>


### MapReduce와 Spark의 data 처리 단계 비교

- MapReduce의 data 처리 단계

1. cluster에서 data 읽기
2. 동작 실행
3. cluster에 결과 기록
4. update된 내용 읽기
5. 다음 동작 실행
6. cluster에 결과 기록

- Spark의 data 처리 단계

1. cluster에서 data 읽기
2. analytics 운영 수행 및 결과값 cluster 입력 동작과 같은 전 과정이 동시 진행

- Spark는 Hadoop과 달리 2단계로 처리가 되므로 일반적 상황에서 Spark의 data 처리 속도가 Hadoop에 비해 월등히 빠름. batch processing의 경우에는 Spark가 Hadoop의 MapReduce에 비해 10배 더 빠르고, In-Memory analytics의 경우에는 100배 더 빠른 수행 속도를 낸다고 함.


<br>


### Hadoop과 Spark 비교

- 장점

  * Hadoop: 매번 운영한 결과를 disk에 기록하기 때문에 system에 사고나 고장이 나면 그 결과를 활용할 수 있어서 유용함. data 일괄 처리를 최선으로 하며, PB(Petabyte)급의 data를 저렴한 비용으로 저장·처리할 수 있음.
  * Spark: 탄력적 분산형 dataset을 활용하여 data object들을 cluster 전반에 분산하기 때문에 사고가 나면 완벽하게 복구할 수 있음. streaming data로의 전환을 편리하게 할 수 있음.
둘 다 어떤 식으로든 고장에서 회복시킬 수 있지만 그 방법만 다를 뿐임. 따라서 Hadoop과 Spark 중 어떤 framework를 쓰는 게 더 나은지 고민된다면 data를 어떻게 다루고 싶은지에 대해 생각해 본 후 결정하는 게 좋음.

- 사용하는 상황 비교

  * Hadoop을 사용하는 상황: reporting 요구들이 대체로 정적이거나 batch mode processing을 기다릴 수 있다면 MapReduce만으로 문제 없이 처리할 수 있음.
  * Spark를 사용하는 상황: streaming data 처리와 ML 알고리즘처럼 application과의 복합적 운영이 필요할 때 적합함. 예를 들어 실시간 캠페인과 상품 추천 그리고 cyber security 분석과 같은 application 영역에서 data 처리가 용이함.


<br>


참고: https://eehoeskrap.tistory.com/220 [Enough is not enough], https://swalloow.github.io/map-reduce/
