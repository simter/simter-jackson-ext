# simter-jackson-ext

**This module was deprecated, please use [simter-jackson-javatime](https://github.com/simter/simter-jackson-javatime) instead.**

A brand new [Jackson Java 8 Date & Time] serialization and deserialization module with global localize config.

[Jackson standard JavaTimeModule] has a standard [ISO-8601] format default, and it can not be config to another 
default format globally. That's too bad for real projects. This module do these things good. But it's not a extension 
of [Jackson standard JavaTimeModule]. It's a brand new module for setting global data-time format.

It is config to localize date-time format like `yyyy-MM-dd HH:mm` now, accurate to minute and without zone and 
offset info (global use local zone and offset default). That's more useful in my real projects. it would be 
config to another format by customization in the future.

## 1. Installation

```xml
<dependency>
  <groupId>tech.simter</groupId>
  <artifactId>simter-jackson-ext</artifactId>
  <version>${VERSION}</version>
</dependency>
```

Exclude `jackson-datatype-jsr310` for spring-boot:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-json</artifactId>
  <exclusions>
    <exclusion>
      <groupId>com.fasterxml.jackson.datatype</groupId>
      <artifactId>jackson-datatype-jsr310</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

## 2. Global default format

| DateTime Class | Value-Format     | Serialize To |
|----------------|------------------|--------------|
| OffsetDateTime | yyyy-MM-dd HH:mm | String       |
| OffsetTime     | yyyy-MM-dd HH:mm | String       |
| ZonedDateTime  | yyyy-MM-dd HH:mm | String       |
| LocalDateTime  | yyyy-MM-dd HH:mm | String       |
| LocalDate      | yyyy-MM-dd       | String       |
| LocalTime      | HH:mm            | String       |
| Instant        | {EpochSecond}    | Number       |
| YearMonth      | yyyyMM           | Number       |
| Year           | yyyy             | Number       |
| Month          | MM               | Number       |
| MonthDay       | MM-dd            | String       |

## 3. Usage

### 3.1. Base usage

```
val mapper = ObjectMapper()
mapper.registerModule(tech.simter.jackson.ext.javatime.JavaTimeModule())

// Recommand to disabled and enabled some features:
mapper.setSerializationInclusion(NON_EMPTY)
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)
mapper.disable(DeserializationFeature.ADJUST_DATES_TO_CONTEXT_TIME_ZONE)
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)
mapper.configure(DeserializationFeature.ACCEPT_EMPTY_STRING_AS_NULL_OBJECT, true)
```

### 3.2. WebFlux usage

See [simter-reactive-web/.../WebFluxConfiguration.kt].


[Jackson Java 8 Date & Time]: https://github.com/FasterXML/jackson-modules-java8/tree/master/datetime
[Jackson standard JavaTimeModule]: https://github.com/FasterXML/jackson-modules-java8/blob/master/datetime/src/main/java/com/fasterxml/jackson/datatype/jsr310/JavaTimeModule.java
[ISO-8601]: http://en.wikipedia.org/wiki/ISO_8601
[simter-reactive-web/.../WebFluxConfiguration.kt]: https://github.com/simter/simter-reactive-web/blob/master/src/main/kotlin/tech/simter/reactive/web/webflux/WebFluxConfiguration.kt