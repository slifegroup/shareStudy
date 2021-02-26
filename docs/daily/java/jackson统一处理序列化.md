### 在消息转换器设置 `jackson`序列化，统一处理

创建`Jackson2ObjectMapperBuilderCustomizer` 对象，设置自定义的序列化标准    代码如下：

```java
@Bean
public Jackson2ObjectMapperBuilderCustomizer customJackson() {
        return jacksonObjectMapperBuilder -> {
            //若POJO对象的属性值为null，序列化时不进行显示
            jacksonObjectMapperBuilder.serializationInclusion(JsonInclude.Include.NON_NULL);
            jacksonObjectMapperBuilder.featuresToDisable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
            jacksonObjectMapperBuilder.featuresToDisable(DeserializationFeature.ADJUST_DATES_TO_CONTEXT_TIME_ZONE);
            //针对于Date类型，文本格式化
            jacksonObjectMapperBuilder.simpleDateFormat(DatePattern.NORM_DATETIME_PATTERN);
            // jdk8日期序列化和反序列化设置
            JavaTimeModule javaTimeModule = new JavaTimeModule();
            javaTimeModule.addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(DatePattern.NORM_DATETIME_PATTERN)));
            javaTimeModule.addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern(DatePattern.NORM_DATETIME_PATTERN)));
            javaTimeModule.addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern(DatePattern.NORM_DATE_PATTERN)));
            javaTimeModule.addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern(DatePattern.NORM_DATE_PATTERN)));
            javaTimeModule.addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern(DatePattern.NORM_TIME_PATTERN)));
            javaTimeModule.addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern(DatePattern.NORM_TIME_PATTERN)));
            SimpleModule simpleModule = new SimpleModule();
            // Long类型序列化成字符串，避免Long精度丢失
            simpleModule.addSerializer(Long.class, ToStringSerializer.instance);
            simpleModule.addSerializer(Long.TYPE, ToStringSerializer.instance);
            jacksonObjectMapperBuilder.modules(javaTimeModule,simpleModule);
        };
    }
```

注意：

```
 return jacksonObjectMapperBuilder -> {}
 等于
 return new Jackson2ObjectMapperBuilderCustomizer() {
 	@Override
 	public void customize(Jackson2ObjectMapperBuilder jacksonObjectMapperBuilder) {}
}
```

上述主要是自定义统一的`jackson` 处理，去对象统一序列化和反序列化

