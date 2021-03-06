# Hadoop

### 빅데이터

* 기존의 데이터베이스 관리도구의 능력을 넘어서는 대량(수십 테라바이트)정형 또는 비정형 데이터 집합에서 유의미한 정보를 도출하고 처리하는 기술입니다.
* 인공지능, 자율주행 등의 최첨단 기술들의 출발점입니다.
* 데이터의 양이 커질수록 이를 처리하는 컴퓨터의 성능도 좋아져야 합니다.
* 하지만 데이터 양의 증가에 비해 단일 컴퓨터 성능의 향상은 한계가 존재합니다.



### 분산 처리 시스템

* 하나의 컴퓨터 또는 서버에서 처리하는 방식을 넘어 네트워크에서 원격 컴퓨터와 통신하면서 하나의 목적을 위해 여러 서버에서 연산을 처리하도록 만든 시스템입니다.

* 분산 처리를 위해선 다수의 컴퓨터와 네트워크가 필요하며 이를 클러스터라고 합니다.

  * 클러스터 = 컴퓨터 + LAN + 클러스터를 구현할 수 있는 소프트웨어
  * 클러스터는 저렴한 비용과 상대적으로 저성능인 컴퓨터들로 슈퍼컴퓨터 같은 컴퓨팅 파워를 얻을 수 있습니다.

* Hadoop은 빅데이터 분석이 일반화 될 수 있도록 클러스터를 쉽게 활용할 수 있는 프레임워크입니다. 

  

### Hadoop

* 빅데이터 인프라 기술 중 하나로 분산처리를 통해 수많은 데이터를 저장하고 처리하는 자바 기반의 오픈소스 프레임워크입니다.
* 데이터를 나눠 클러스터로 보내고 그 결과를 수집하는 일련의 과정을 사용자에게 제공합니다.
* 데이터를 분산처리 하는 수를 줄이거나 늘림으로서 저렴한 비용으로 원하는 크기의 저장소를 가질 수 있습니다.

##### Hadoop을 사용하는 이유

* RDBMS보다 비용이 저렴합니다.
  * RDBMS는 대용량인 빅데이터를 감당하기엔 비용이 큽니다.
* 분산처리 방식을 이용해서 저렴한 구축 비용과 비용대비 빠른 데이터 처리, 장애를 대비한 특성을 갖추고 있습니다.

##### Hadoop의 특징

* Distributed: 자료 분산 저장 및 처리
* Scaliable: 컴퓨터 추가로 용량 확장 가능
* Fault-tolerant: 일부 컴퓨터가 고장나도 시스템이 동작
* Open Source: 아파치 재단에서 오픈 소스입니다.

##### Hadoop과 빅데이터의 관계

* Hadoop의 대량으로 확장이 가능하기 때문에 빅데이터 워크로드를 처리하는데 주로 사용됩니다.
  * 만약 Hadoop의 클러스터의 처리 성능을 향상시키려면 CPU 및 메모리 리소스가 있는 서버를 추가하면 됩니다.
* Hadoop의 높은 수준의 가용성, 확장성, 내구성은 빅데이터 워크로드에 적합합니다.

##### HDFS

* HDFS (Hadoop Distrubuted File System)은 대용량의 파일을 분산된 서버에 저장하고 빠르게 처리할 수 있게 하는 파일 시스템입니다.
* 저사양 서버를 이용해서 스토리지를 구성할 수 있습니다.
* 블록 구조 파일 시스템입니다.
  * 파일을 특정 크기의 블록으로 나눠서 분산시킵니다.
  * 초기 블록 크기는 64MB, Hadoop 2.0 이후엔 128MB

##### Hadoop Echo System

* 에코 시스템 (echo system)이란 중앙시스템에 여러가지 소프트웨어가 추가된 시스템을 의미합니다.
* 즉, Hadoop이라는 중앙시스템을 중심으로 다양한 소프트웨어가 추가된 버전들을 의미합니다.
* Hadoop을 사용하기 위해 필요한 언어인 JAVA, Scala를 모를 경우, 에코 시스템을 통해서 Hadoop의 사용성을 높일 수 있습니다.



### Spark

##### Spark란?

* 아파치 재단에서 만든 빅데이터 처리를 위한 오픈소스 분산 처리 플랫폼 (빅데이터 분산 처리 엔진)

##### Hadoop과의 차이점

* Hadoop의  HDFS는 분산형 파일 시스템을 기반으로 만들어졌습니다.
* HDFS는 DISK I/O를 기반으로 동작하기 때문에 늘어난 실시간성 데이터들을 처리하기엔 부적한 상황이 발생하게 됩니다.
* Spark는 하드웨어의 발전에 따라 저렴해진 고용량 메모리를 활용하기 때문에 반복적인 처리가 필요한 작업에서 Hadoop보다 1000배 이상 빠릅니다.
* 하지만 데이터 운영 및 리포팅 요구 대부분은 정적이기 때문에 Spark가 필요한 상황은 특정되어 있습니다.
* 최근에는 Hadoop과 Spark가 연계되는 방식으로 아키텍쳐를 구성합니다.
  * Hadoop의 YARN 위에 Spark를 얹고, 실시간성이 필요한 데이터를 Spark로 처리하는 방식으로 동작합니다.
  * YARN은 Hadoop의 분산자원을 관리를 담당합니다.



