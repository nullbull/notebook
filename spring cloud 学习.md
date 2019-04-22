1. spring cloud 的 配置文件最好是bootstrap.yml

2. 要使spring config 在该项目下失效，请配置

```xml
spring:
  cloud:
    config:
      enabled: false
```

3. Hystrix class 类要加上@Compant 配置
4. @RefreshScope spring cloud bus 刷新配置，使用时要加上这个注解，值才会被刷新