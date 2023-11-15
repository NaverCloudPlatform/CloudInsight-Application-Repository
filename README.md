# Cloud Insight Application Monitoring: Spring Boot
Micrometer Registry for Cloud Insight

## Index
[1. Supported Environments](#1.-Supported-Environments)  
[2. Usage](#2.-Usage)  
[3. Appendix](#3.-Appendix)  
[4. More Information](#4.-More-Information)  
[5. Release Notes](#5.-Release-Notes)  

## 1. Supported Environments<a id="1.-Supported-Environments"></a>
- **JDK 8 &uarr;**
- **Spring Boot 2.0.4 &uarr;**

## 2. Usage<a id="2.-Usage"></a>

#### :heavy_check_mark: Add a repository in your spring boot application
- `pom.xml`
    - env: **local**
      ```xml
      <repository>
        <id>ncp-cloudinsight-application-release</id>
        <url>https://raw.githubusercontent.com/NaverCloudPlatform/CloudInsight-Application-Repository/main/releases/</url>
      </repository>
      ```
    - env: **ncp server**
      ```xml
      <repository>
        <id>ncp-cloudinsight-application-release</id>
        <url>https://nsight.ncloud.com/application/releases/</url>
      </repository>
      ```
- `build gradle`
    - env: **local**
      ```gradle
      maven { url "https://raw.githubusercontent.com/NaverCloudPlatform/CloudInsight-Application-Repository/main/releases/" }
      ```
    - env: **ncp server**
      ```gradle
      maven { url "https://nsight.ncloud.com/application/releases/" }
      ```
#### :heavy_check_mark: Import a dependency in your spring boot application
- `pom.xml`
```xml
<dependency>
  <groupId>com.ncp.cloudinsight</groupId>
  <artifactId>spring-boot-metrics-cloudinsight-autoconfigure</artifactId>
  <version>1.0.0</version>
</dependency>
```
- `build.gradle`
```gradle
implementation 'com.ncp.cloudinsight:spring-boot-metrics-cloudinsight-autoconfigure:1.0.0'
```

#### :heavy_check_mark: Add some properties in your application config
- `.properties`
```properties
# Cloud Insight's Collector URI
management.metrics.export.cloudinsight.uri=http://cnx-collector.nsight.ncloud.com:9973

# The application key issued from Cloud Insight
management.metrics.export.cloudinsight.id=${application key} 

# true if you want to publish metrics to Cloud Insight. Default is true.
management.metrics.export.cloudinsight.enabled=true
```
- `.yml`
```yaml
management:
  metrics:
    export:
      cloudinsight:
        uri: http://cnx-collector.nsight.ncloud.com:9973
        id: ${application key}
        enabled: true
```

## 3. Appendix<a id="3.-Appendix"></a>
- **add custom metrics**
  ```java
  Counter counter = Counter.builder("custom.counter.metric")
      .tag("K1", "V1")
      .description("custom counter metric description")
      .register(registry);
  counter.increment(10);
  ```
- **filter metrics**
  ```java
  registry.config()
      .meterFilter(MeterFilter.ignoreTags("too.much.information"))
      .meterFilter(MeterFilter.denyNameStartsWith("jvm"));
  ```
- **add common tags to every metric**
    - `.properties`
      ```properties
      # add common tags to every metric reported to the Cloud Insight
      management.metrics.export.cloudinsight.tags.common-tag-key1=common-tag-val1
      management.metrics.export.cloudinsight.tags.common-tag-key2=common-tag-val2
      ```
    - `.java`
      ```java
      // add common tags to every metric reported to the Cloud Insight
      registry.config().commonTags(Arrays.asList(
             Tag.of("common-tag-key1", "common-tag-val1"), 
             Tag.of("common-tag-key2", "common-tag-val2")
      ));
      ```
## 4. More Information<a id="4.-More-Information"></a>
- [NAVER CLOUD PLATFORM > Cloud Insight](https://guide.ncloud-docs.com/docs/cloudinsight-cloudinsightoverview)
- [Micrometer Docs](https://micrometer.io/docs/concepts)

## 5. Release Notes<a id="5.-Release-Notes"></a>
version | date | changes
--- | --- | ---
v 1.0.0 | 2023.11.23 | first release

```
spring-boot-metrics-cloudinsight-autoconfigure
Copyright (c) 2023-present NAVER Cloud Corp.
Apache-2.0
```
