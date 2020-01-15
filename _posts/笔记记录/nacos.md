## 什么是nacos？

Nacos 支持基于 DNS 和基于 RPC 的服务发现（可以作为springcloud的注册中心）、动态配置服务（可以做配置中心）、动态 DNS 服务。

官方介绍是这样的：

> Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您实现动态服务发现、服务配置管理、服务及流量管理。
>  Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构(例如微服务范式、云原生范式)的服务基础设施。

官方网址：http://nacos.io



# 参考博客

1. https://www.jianshu.com/p/16ff6d6db0cf



# 集成springBoot的配置

描述：基于yml 的配置,须配置bootstrap.yml ,bootstrap-xxx.yml

1. 配置bootstrap.yml

   ```
   spring:
     application:
       name: ${项目名字(英文)}
     profiles:
       active: dev
   logging:
     config: classpath:logback-spring.xml
   ```

2. 配置bootstrap-dev.yml

   ```
   server:
     port: 8082
     servlet:
       context-path: /${项目名字(英文)}
       #（nacos配置主要这块）
   spring:
     cloud:
       nacos:
         server-addr: ${nacos集群ip}
         #命名空间
         namespace: d158b3bc-17a5-4914-98a3-0c21d6325fdf
         discovery:
           register-enabled: true
           heart-beat-timeout: 15000
           heart-beat-interval: 5000
           namespace: ${spring.cloud.nacos.namespace}
         config:
           config-long-poll-timeout: 30000
           file-extension: yml
           enabled: true
           namespace: ${spring.cloud.nacos.namespace}
           shared-dataids: share-center-dev-base.yml,share-center-dev-database.yml,share-center-dev-redis.yml
           refreshable-dataids: share-center-dev-base.yml,share-center-dev-database.yml,share-center-dev-redis.yml
     main:
       allow-bean-definition-overriding: true
   
   dubbo:
     application:
       qos-port: 2334
     consumer:
       timeout: 60000
       #本地开发修改版本号和分组,避免注册和别人混淆
       version: 1.0.0
       group: dev_living
     protocol:
       name: dubbo
       port: 20834
     provider:
       #本地开发修改版本号和分组,避免注册和别人混淆
       version: 1.0.0
       group: dev_living
     # 三大中心配置
     registry:
       address: zookeeper://10.123.174.112:2181
   #      address: nacos://10.123.141.13:8848
     metadata-report:
       address: zookeeper://10.123.174.112:2181
   #    address: nacos://10.123.141.13:8848
     config-center:
       address: zookeeper://10.123.174.112:2181
   #    address: nacos://10.123.141.13:8848
   log:
     file: /home/app/log/share-center-rights/dev
   ```

3. 配置参数解析

   + 通过 `spring.cloud.nacos.config.shared-dataids` 来支持多个共享 Data Id 的配置，多个之间用逗号隔开（多个共享配置间的一个优先级的关系我们约定：按照配置出现的先后顺序，即后面的优先级要高于前面，Data Id 必须带文件扩展名，文件扩展名既可支持 properties，也可以支持 yaml/yml）
   + 通过 spring.cloud.nacos.config.refreshable-dataids 来支持哪些共享配置的 Data Id 在配置变化时，应用中是否可动态刷新， 感知到最新的配置值，多个 Data Id 之间用逗号隔开。如果没有明确配置，默认情况下所有共享配置的 Data Id 都不支持动态刷新

4. 需要引入的jar包

   ```
    <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
               <version>2.1.1.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
               <version>2.1.1.RELEASE</version>
               <exclusions>
                   <exclusion>
                       <groupId>org.bouncycastle</groupId>
                       <artifactId>bcpkix-jdk15on</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
   ```

   

