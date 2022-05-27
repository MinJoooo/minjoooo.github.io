---
layout: single
title: "[Data Infrastructure] 이해하기 Part 4 - Batch Query Engine, Event Streaming, Stream processing"
categories: Data-Infrastructure
tag: [Data Infrastructure]
toc: false
---

<br>
<br>
<br>

###### 2022.05.27.
###### Data Infrastructure 이해하기 Part 4
###### (Batch Query Engine, Event Streaming, Stream processing)

<br>
<br>
<br>

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6QWME%2FbtrC0QGjg3M%2FKFQOjcIcMC6PixmxyoLBF1%2Fimg.jpg" width=800><br>
  출처: Emerging Architectures for Modern Data Infrastructure 2020
</p>


<br>


### Ingestion and Transformation: data를 가져와서 transform


<br>


---


<br>


### Ingestion and Transformation과 관련한 용어 설명 (2)


<br>


### Batch Query Engine

- tool

  * Hive: Hadoop의 HDFS에 있는 data의 query를 처리하기 위한 engine. Hadoop에서 동작하는 data warehouse infrastructure 구조로서 data의 query 처리와 분석 기능 등을 제공함. 원래 Hadoop을 통해 query를 처리하기 위해서는 MapReduce 작업을 작성해야 하는데, Hive는 SQL 작업을 MapReduce 작업으로 transform하여 실행시켜 주어서 SQL만 작성하면 Hadoop에 있는 data를 읽어와서 처리하도록 도와줌.
  * Spark platform과 Hive의 관계: Spark에는 SparkSQL이 있는데 왜 Spark platform과 Hive가 연결되어 있는지 궁금할 수 있음. Hive는 meta store를 통해 자신이 연결되어 있는 DB의 meta data를 가지고 있기 때문에 Hive를 이용하면 이 meta store를 Spark가 그대로 이용할 수 있음. 또한 Hive는 Thrift, JDBC, ODBC와도 연결되어 다양한 DB와도 연결되어 있어서 이를 Spark를 이용해서 쓰면 Hive를 실행하는 engine으로 Spark를 쓸 수 있음. => 더 빠르게 Hive query를 Spark를 이용해서 실행할 수 있음.
  * Spark와 Hive의 관계: Spark와 Hive는 서로 보완하는 요소가 있어서 Spark가 Hive를 대체한다고 하기보다는 Spark를 설치하고 기존의 HDFS에 있는 Hive로 연결된 것들을 같이 사용한다고 보면 됨.


<br>


### Event Streaming

- streaming: data가 생성되는 즉시 실시간으로 동시에 처리함.

- data infrastructure에서 streaming이 중요한 이유: data availability(가용성) 때문임. 예전에는 data의 load 시점에 data에 대한 접근이 가능했기 때문에 실시간으로 data를 처리할 수 없었음. 하지만 streaming이 되는 현재는 data의 ingestion 시점에 data에 대한 접근이 가능하여 실시간으로 data 처리가 가능함.

- Spark와 streaming: Spark는 micro-batch 기반으로 data를 모아두었다가 500ms마다 실행시킴. batch이긴 하지만 1초에 2번씩 실행시키기 때문에 거의 실시간처럼 느껴져 streaming을 지원한다고 말함. 하지만 진정한 native streaming은 data가 들어오는대로 하나씩 처리하기 때문에 Spark는 native streaming은 아님.

- tool

  * Kafka: 실시간 streaming data를 처리하기 위한 목적으로 설계된 분산형 messaging platform. writing에 최적화된 system임. publisher(producer)가 data를 생성하면 publish/subscribe messaging system이 이를 모아 queue에 넣어놓은 후 n명의 consumer인 subscriber가 queue에서 한 번씩만 읽어서 처리함. Kafka cluster 안에는 여러 server들이 message를 모아놓은 topic들을 분산해서 가지고 있음. 분산해서 가지고 있는 이유는 하나의 server가 down되더라도 안전하게 하기 위함임. producer들이 topic을 push하고, consumer들이 topic을 pull함. Zookeeper cluster는 Kafka cluster의 server 상태와 push, pull에 대한 요청을 받아서 관리함. Kafka는 message를 writing하는 partition의 수를 늘릴 수 있어서 data를 write 하는 데에 대한 속도 저하가 없어 빠른 writing이 가능함. 대신 partition을 한 번 늘리면 다시 줄이는 것은 불가능함.
  * Kafka의 장점: global-scale(모든 event stream을 한 군데로 모을 수 있음), real-time(실시간), persistent storage(영구 저장), stream processing
  * Kafka의 단점: 사용하기 어려움
  * Confluent: Kafka 기반의 data streaming platform. Kafka에 dashboard, data flow audit 등 다양한 service를 지원함.
  * Pulsar: publish/subscribe messaging 기반의 streaming platform. Kafka와 비슷함.
  * Kafka와 Pulsar의 다른 점: Pulsar는 저장 단위를 더 작게 쪼개서 저장하며 BookKeeper를 이용해 저장함.
  * Kinesis: Amazon이 만든 streaming platform
  * Kafka와 Kinesis의 다른 점: Kafka가 사용하기 더 어렵지만 성능이 훨씬 좋음. Kinesis는 대신 partition을 줄이는 기능이 제공됨.
  * Amazon MSK: Kafka를 사용하기 편하게 만들어 놓은 platform


<br>


### Stream processing

- streaming platform에서 event들을 중간에서 processing(처리)하는 것을 의미함.

- 내부에서 stream으로 쏟아지는 data들을 엮어서 처리해야 할 때나 event stream으로 들어온 것들 중 다른 것들과 join해서 data를 처리해야 할 때 쓸 수 있는 engine.

- tool

  * Kafka: Kafka stream과 ksql을 통해 raw stream에 대한 processing을 함.
  * Kafka Streams: Kafka 위에 올라가는 event stream에 대한 처리를 하는 JVM client library. data가 Kafka에 전달되면 그것을 읽어서 Kafka Streams API를 이용해 stream에 대한 처리를 한 후 다시 Kafka에 저장함. JVM client library를 이용해서 Java로 coding할 수 있음. application은 실제로 Kafka cluster 밖에 있기 때문에, Kafka cluster에서 Kafka의 event stream이 올라오면 Streams API를 가져다 application으로 처리한 후 그것을 다시 저장하는 형태로 수행됨.
  * Kfaka Streams API가 지원하는 것들: stateless processing(기존의 state와 상관없이 처리할 수 있는 것을 처리), stateful processing(기존의 state를 알아야만 처리할 수 있는 것을 처리), windowing operations(얼마만큼의 기간 동안 처리를 할 것인지를 정해서 처리)
  * Kafka Streams와 KSQL와 ksqlDB: 원래 Kafka의 API는 Consumer, Producer를 통해 만들 수 있었음. => 좀 더 사용하기 쉽게 만든 게 Kafka Streams임. Java로 작성 가능하고, KStream과 KTable이 있음. stream과 table의 차이는, stream은 '5에 2를 더함'과 같이 record history의 과정들을 모두 보여주는 반면 table은 '현재 3'과 같이 represent state를 보여줌.=> 더 사용하기 쉽게 만든 게 KSQL임. SQL로 작성 가능함. => 좀 더 발전한 게 ksqlDB임. Kafka에 올라가는 모든 event stream들을 SQL로 처리해서 Kafka나 DB에 올릴 수 있게 도와줌. push query는 data가 변경된 내역을 모두 가져오는 것이고 pull query는 현재 data의 상태만 가져오는 것인데, 기존에는 push query만 가능했는데 ksqlDB를 이용하면서 pull query도 가능하게 되어 완전하게 DB처럼 사용할 수 있게 됨.
  * Flink: distributed streaming processing platform
  * streaming processing platform들의 비교: 대표적으로 Spark, Flink, Storm, Samza, Kafka Streams이 있음. 일반적으로 Kafka를 쓰면 Kafka stream processing을, Spark를 쓰면 Spark stream processing을 하고, 만약 더 전문적으로 stream processing을 하고 싶으면 Flink나 Storm이나 Samza를 이용함.


<br>


참고: GeekNews in Youtube
