1. 作为配置中心的文件，文件名最好不要用关键字，比如config
2. 关键信息要加密，使用jdk 8 中的 jce_policy-8.zip 下的加密算法
3. 启动应用使用crul进行加密：curl -X POST http://localhost:7000/encrypt -d root
# spring cloud 自动化更新配置中心
1. 之前我们已经把配置中心搭建好了，之后创建的微服务都从配置中心拉取配置，这样如果配置发生变化，我们只需要在一个地方修改即可。
2. 那么还有一个问题，就是，当我们修改配置后，我们需要挨个重启服务，如果有多个服务的话，那依然会很麻烦
3. rabbitMq可以帮我们解决这个问题（自行百度安装rabbitMq）
4. 添加依赖
```xml
        <!-- 引入bus依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
```
5. 配置mq信息
```yml
rabbitmq:
    virtual-host: /
    host: 127.0.0.1
    username: guest
    password: guest
```
6. 启动程序，当我们修改配置后，需要使用暴露出来的接口进行更新拉取，就不用在重启服务了
 > curl -X POST http://localhost:7000/actuator/bus-refresh
7. 我发现，其实不用拉取，直接通过应用访问文件，也会更新配置
8. 当微服务有使用配置属性的需要使用@RefreshScope
