---
title: "[Prometheus/프로메테우스]"
date:  2020-03-16
tags: [Something,Prometheus]
toc: true
---

## 1. 프로메테우스(Prometheus)란?

- 오픈소스 모니터링/알림 솔루션
- 시계열 기반의 다차원 데이터 모델 제공
- HTTP기반 pulling으로 시계열 데이터 수집
- 중간 gateway를 통한 시계열 push 지원

------

프로메테우스에 데이터가 저장이 되며, 이 데이터를 기반으로 룰을 실행하여 새로운 시계열 데이터를 기록하거나 집계하고 alert을 생성한다. 그라파나는 이렇게 수집된 데이터를 시각화해준다.



## 2. 프로메테우스  

![프로메테우스 구조](../assets/images/prome.png)

Components

- 메인 프로메테우스 서버(시계열 데이터를 가져와서 적재함)
- client 라이브러리들(application code 실행)
- push하는 gateway(단시간 작업)
- 특정 작업을 해주는 exporter들
- alertmanager
- 다양한 툴...



## 3. 프로메테우스 vs InfluxDB

저번에 다뤄봤던 influxdb랑 비교한 문서가 있어서 참고했다..

- 프로메테우스가 더 나은점
  - doing metrics
  - 더 강력한 쿼리언어, alert기능
- InfluxDB가 더 나은점
  - event logging
  - 장기간 data storage



## 4. Metric Types

1. Counter

   - value가 증가 or 0으로 리셋 후 재시작

   - value가 감소되지 않음

2. Gauge

   - value가 증가 혹은 감소

3. Histogram

   - 특정기간동안 측정된 values의 sum 나타냄

4. Summary

   - 히스토그램과 유사한데 
     While it also provides a total count of observations and a sum of all observed values, it calculates configurable quantiles over a sliding time window.
     이라고 써있는데 사실 차이점은 잘 모르겠다



## 5. Prometheus JVM Client

### Assets

```xml
<!-- The client -->
<dependency>
  <groupId>io.prometheus</groupId>
  <artifactId>simpleclient</artifactId>
  <version>0.8.1</version>
</dependency>
<!-- Hotspot JVM metrics-->
<dependency>
  <groupId>io.prometheus</groupId>
  <artifactId>simpleclient_hotspot</artifactId>
  <version>0.8.1</version>
</dependency>
<!-- Exposition HTTPServer-->
<dependency>
  <groupId>io.prometheus</groupId>
  <artifactId>simpleclient_httpserver</artifactId>
  <version>0.8.1</version>
</dependency>
<!-- Pushgateway exposition-->
<dependency>
  <groupId>io.prometheus</groupId>
  <artifactId>simpleclient_pushgateway</artifactId>
  <version>0.8.1</version>
</dependency>
```

해당 dependency추가



### Counter

```java
import io.prometheus.client.Counter;
class YourClass {
  static final Counter requests = Counter.build()
     .name("requests_total").help("Total requests.").register();
  //Counter 생성 
  void processRequest() {
    requests.inc();
    // Your code here.
  }
}
```



### Gauge

```java
class YourClass {
  static final Gauge inprogressRequests = Gauge.build()
     .name("inprogress_requests").help("Inprogress requests.").register();

  void processRequest() {
    inprogressRequests.inc();
    // Your code here.
    inprogressRequests.dec();
  }
}
```

.inc()과 .dec()으로 증가 및 감소



### Histogram

```java
lass YourClass {
  static final Histogram requestLatency = Histogram.build()
     .name("requests_latency_seconds").help("Request latency in seconds.").register();

  void processRequest(Request req) {
    Histogram.Timer requestTimer = requestLatency.startTimer();
    try {
      // Your code here.
    } finally {
      requestTimer.observeDuration();
    }
  }
}
```



### Labels

모든 metric은 label을 가질 수 있으며, group화가 가능하다

```java
class YourClass {
  static final Counter requests = Counter.build()
     .name("my_library_requests_total").help("Total requests.")
     .labelNames("method").register();
 //method로 label name을 줌
  void processGetRequest() {
    requests.labels("get").inc();
    // Your code here.
  }
}
```



### Registering Metrics

metric을 등록하는 가장 좋은 방법은 **static final** 을 이용하는 것임.

```java
static final Counter requests = Counter.build()
   .name("my_library_requests_total").help("Total requests.").labelNames("path").register();
```



아래 예시는 label없이 metric을 등록하는 방법

```java
class YourClass {
  static final Gauge activeTransactions = Gauge.build()
     .name("my_library_transactions_active")
     .help("Active transactions.")
     .register();
 //Gauge등록. 
  void processThatCalculates(String key) {
    activeTransactions.inc();
    try {
        // Perform work.
    } finally{
        activeTransactions.dec();
    }
  }
}
```

builder와 함께 labelNames()을 사용하면, label이 붙은 timeseries생성가능. 

```java
class YourClass {
  static final Counter calculationsCounter = Counter.build()
     .name("my_library_calculations_total").help("Total calls.")
     .labelNames("key").register();

  void processThatCalculates(String key) {
    calculationsCounter.labels(key).inc();
    // Run calculations.
  }
}
```



### PushGateway

```java
void executeBatchJob() throws Exception {
     CollectorRegistry registry = new CollectorRegistry();
    //registry생성. 정보를 담고있는 DB라고 생각하면 될듯.
     Gauge duration = Gauge.build()
         .name("my_batch_job_duration_seconds").help("Duration of my batch job in seconds.").register(registry);
     Gauge.Timer durationTimer = duration.startTimer();
     try {
       // Your code here.

       // This is only added to the registry after success,
       // so that a previous success in the Pushgateway isn't overwritten on failure.
       Gauge lastSuccess = Gauge.build()
           .name("my_batch_job_last_success").help("Last time my batch job succeeded, in unixtime.").register(registry);
       lastSuccess.setToCurrentTime();
     } finally {
       durationTimer.setDuration();
       PushGateway pg = new PushGateway("127.0.0.1:9091"); //pushgateway open
       pg.pushAdd(registry, "my_batch_job"); //push해줌
     }
   }
```

