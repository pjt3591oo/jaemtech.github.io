# Kinesis + Firehose + Athena를 이용한 로그 수집기 만들기

## 목차 :

- Kinesis , Firehose, Athena 란?
- Kinesis Stream 생성 하기
- Firehose 생성하기
- Athena로 로그 데이터 쿼리 날리기

### 1. Kinesis, Firehose, Athena란?
  > **Kinesis** 란 AWS에서 제공하는 스트리밍 데이터 처리 소프트웨어 입니다. 저는 분산 메세지 큐인 RedditMQ나 Kafka와 같은 서비스를 하는 역할로 이해했습니다.
  Kinesis 구성시 고려 사항은 안정성과 사용 편의성, 사용 비용이 해당 될 것입니다. 자세한 내용은 아래 참조 사이트를 확인하시기 바랍니다.
  
  > Ref : [https://aws.amazon.com/ko/kinesis/streams/](https://aws.amazon.com/ko/kinesis/streams/)

  > Firehose 란 AWS에서 제공하는 Kinesis Stream에 있는 데이터를 쉽게 로드할 수 있는 장치 입니다. 스트림 데이터를 S3, Redshift , Elastic Search Service   로 적재 하기 쉽다는 점이 장점 입니다. 자세한 사항 역시 아마존 공식 홈페이지를 참조 하시기 바랍니다.
  
  > Ref : [https://aws.amazon.com/ko/kinesis/firehose/](https://aws.amazon.com/ko/kinesis/firehose/)

  > Athena 란 AWS에서 구글의 Big Query와 견주는 서버리스 대화식 쿼리 서비스 입니다. S3에 적재 되어있는 로그 데이터에 표준 SQL을 사용해 데이터를 뽑아냅니다.       1Query 당 1TB 기준으로 5$가 책정되며 데이터 용량이 적으면 금액도 적게 나옵니다. AWS Glue라는 서비스와 같이 ETL 로써 사용 가능한 제품 입니다 .
  
  > Ref : [https://aws.amazon.com/ko/athena/](https://aws.amazon.com/ko/athena/)

### 2. Kinesis Stream 생성 하기
  > Kinesis Stream은 생성 하고 Producer와 Consumer로 구현해야 합니다. Lambda를 이용하거나 기존 서버에 module 로 구현해 삽입해서 구현 하면 됩니다.  로그 수   집기에선 Consumer는 Firehose에서 하기 때문에 저는 Producer 모듈만 Plugin-backend 서버에 삽입했습니다.

  > Kinesis Stream은 스트림 데이터를 처리하는 기준으로 과금이 됩니다. 데이터 처리 단위는 Shard 단위로 처리됩니다. 샤드 1개는 초당 1MB의 데이터 입력 및 2MB의 데   이터 출력을 제공하며 초당 최대 1,000개의 레코드를 지원 합니다. 자세한 과금 내역은 아래를 참조하시면 됩니다.
  
  > Ref : [https://aws.amazon.com/ko/kinesis/streams/pricing/](https://aws.amazon.com/ko/kinesis/streams/pricing/)

  > Kinesis Stream의 생성은 아래 공식 홈페이지를 참조 하시면 됩니다.
  
  > Ref : [https://docs.aws.amazon.com/ko_kr/streams/latest/dev/getting-started.html](https://docs.aws.amazon.com/ko_kr/streams/latest/dev/getting-started.html)

  > Producer 와 Consumer는 말 그대로 데이터를 공급하는 자와 소비하는 자입니다. 로그 수집기에서 Producer는 로그 데이터를 수집, 공급을 위한 일을 합니다. 소비자는   Stream 에 저장된 데이터를 소비하는 일을 주로 합니다.
  Producer는 먼저 Kinesis Stream을 생성하고 데이터를 Kinesis Stream에 넣는 일을 주로 합니다. 로그 수집기에선 인메모리 캐시 서비스인 Redis 를 활용해 지정한     버퍼 사이즈 만큼 데이터가 모이면 데이터를 Kinesis Stream으로 전송합니다. 데이터는 JSON 객체 형식으로 전송 되며 Athena에서 Query를 날릴때 레코드 데이터를 개     행 문자로 구분합니다. 어쩔수 없이 데이터를 묶을때 개행 문자를 추가해 주었습니다. 자세한 코드는 아래 소스 코드들을 참고 하시기 바랍니다.
  
  > (Plugin Source) Ref : [https://bitbucket.org/jaemkor/jaem-plugin-backend/src/4ad0fe54a85ae469278374b4dcae93b0beea89df/routes/kinesis/plugin-producer.js?at=master&fileviewer=file-view-default](https://bitbucket.org/jaemkor/jaem-plugin-backend/src/4ad0fe54a85ae469278374b4dcae93b0beea89df/routes/kinesis/plugin-producer.js?at=master&fileviewer=file-view-default)
  
  > (Awslabs Sample Code)Ref : [https://github.com/JayStevency/amazon-kinesis-client-nodejs/tree/master/samples/click_stream_sample/producer](https://github.com/JayStevency/amazon-kinesis-client-nodejs/tree/master/samples/click_stream_sample/producer)

### 3. Firehose 생성하기
  > Firehose는 생성시에 Kinesis Stream을 추가 하고 적재할 S3를 추가해 주면 됩니다.
  기존에 생성한 Kinesis Stream을 추가 합니다.로그 수집을 위한 S3를 추가 합니다. Prefix는 옵션 입니다.버퍼 사이즈는 S3 적재 되기전 임시 데이터 저장 공간의 용     량이고 퍼버 인터벌은 버퍼를 비우는 주기의 시간을 나타냅니다. 압축은 S3의 사용 용량을 절감하므로 되도록 사용해 주는것이 좋습니다.


  > IAM는 Firehose를 사용가능하게 하는 권한 매핑으로써 접근권한을 부여합니다. 위의 모든 과정을 수행 하면 Firehose가 생성 됩니다. 테스트는 aws cli를 이용 해 데이터를 Kinesis Stream putRecord를 이용하면 지정한 버퍼 인터벌 시간이 지난후 S3에 적재되는것을 확인 하시면 됩니다.

### 4. Athena로 로그 데이터 쿼리 날리기

  > Athena는 서버리스 대화형 쿼리 서비스 입니다. 별도의 서버를 구성하지 않고 S3에 쿼리를 날려 원하는 데이터를 가져오는 것이 장점이지만 한번 쿼리당 1TB 당 5$의 비용을 지불해야 합니다. 기본적으로 JDBC를 지원하므로 Java를 쓰지 않는 다른 언어 프레임워크에선 JDBC를 로드하는 모듈을 이용하면 기존 데이터베이스 처럼 연결해 데이터를 주고 받을 수 있습니다. 또 Glue와 통합되어 데이터 카탈로그로 쓸수있어 ETL 기능도 가능합니다. 자세한 내용은 아래 참조를 참고 하세요.

  > Ref : [https://aws.amazon.com/ko/athena/?hp=tile&so-exp=below](https://aws.amazon.com/ko/athena/?hp=tile&so-exp=below)

  > Athena는 기본적으로 Presto DB가 기본이고 표준 SQL을 따릅니다. 그리고 Table 생성은 Hive DDL을 따르므로 JSON 형태는 Hive DDL에서 허용하는 형태로 적재되어야 원하는 스키마를 구성 할 수 있습니다.

  > Ref : [http://docs.aws.amazon.com/athena/latest/ug/json.html](http://docs.aws.amazon.com/athena/latest/ug/json.html) , [https://github.com/rcongiu/Hive-JSON-Serde](https://github.com/rcongiu/Hive-JSON-Serde)
