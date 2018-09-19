### Linux Kafka配合zookeeper启动并整合SpringBoot
1. 下载kafka，我下载的是kafka_2.12-1.0.0.tgz，所以pom添加依赖的时候也要添加对应的jar包

2. 下载zookeeper

3. 解压，首先启动zookeeper，修改zookeeper/conf下面的zoo_sample.cfg, 修改里面的`dataDir=/opt/zkdata`(举例)，将这个文件改名为zoo.cfg或者复制出一个名为zoo.cfg的文件。

4. 启动zookeeper 
  `./bin/zkServer.sh start`

5. 修改kafka/config 的server.properties文件，修改
  `log.dirs=/opt/kadata`
  `advertised.listeners=PLAINTEXT://192.168.113.136:9092`(这一步很重要，暴露出地址外网能访问，主机才能访问虚拟机)

6. 启动kafka  

   `./bin/kafka-server-start.sh  -daemon config/server.properties`  一定要加上 -daemon，加上意味着程序在后台运行a

7. 创建一个名为test 的topic 

   `./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test`

8. 现在添加pom依赖，一定要根据自己下的kafka下载相应版本

   ```java
   <dependency>
       <groupId>org.apache.kafka</groupId>
       <artifactId>kafka_2.12</artifactId>
       <version>1.0.0</version>
   </dependency>
   <dependency>
       <groupId>org.springframework.kafka</groupId>
       <artifactId>spring-kafka</artifactId>
       <version>2.1.9.RELEASE</version>
   </dependency>
   ```

9. 在application.properties添加一些配置

   ```java
   spring.kafka.bootstrap-servers=192.168.113.136:9092
   spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
   spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
   spring.kafka.producer.batch-size=4096
   spring.kafka.producer.buffer-memory=40960
   spring.kafka.consumer.group-id=test
   spring.kafka.consumer.enable-auto-commit=true
   spring.kafka.consumer.auto-commit-interval=100
   spring.kafka.consumer.key-serializer=org.apache.kafka.common.serialization.StringDeserializer
   spring.kafka.consumer.value-serializer=org.apache.kafka.common.serialization.StringDeserializer
   spring.kafka.consumer.bootstrap-servers=192.168.113.136:9092
   spring.kafka.consumer.auto-offset-reset=earliest
   ```

10. 编写一个ProducerController

    ```java
    package com.niu.springboot.formal;
    
    import com.niu.springboot.kafka.Response;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.kafka.core.KafkaTemplate;
    import org.springframework.web.bind.annotation.*;
    
    /**
     * @Auth justinniu
     * @Date 2018/9/3
     * @Desc
     */
    
    @RestController
    @RequestMapping("/kafka")
    public class ProducerController {
        protected final Logger logger = LoggerFactory.getLogger(this.getClass());
        @Autowired
        private KafkaTemplate<String, String> kafkaTemplate;
    
        @RequestMapping(value = "/send", method = RequestMethod.GET)
        public String sendKafka(@RequestParam("message") String message) {
            try {
                logger.info("kafka的消息={}", message);
                kafkaTemplate.send("test", "key", message);
                logger.info("发送kafka成功.");
                return "successs";
            } catch (Exception e) {
                logger.error("发送kafka失败", e);
                return "failure";
            }
        }
    
    }
    ```

11. 编写一个ConsumerController

    ```java
    package com.niu.springboot.formal;
    
    import org.apache.kafka.clients.consumer.ConsumerRecord;
    import org.springframework.kafka.annotation.KafkaListener;
    import org.springframework.stereotype.Component;
    
    /**
     * @Auth justinniu
     * @Date 2018/9/3
     * @Desc
     */
    @Component
    public class TestConsumer {
        @KafkaListener(topics = "test")
        public void listen (ConsumerRecord<?, ?> record) throws Exception {
            System.out.printf("topic = %s, offset = %s, value = %s \n", record.topic(), record.key(), record.value());
        }
    }
    ```

12. 测试结果

    ![Snipaste_2018-09-04_10-28-05](C:\Users\11291\Desktop\Snipaste_2018-09-04_10-28-05.png)

    ![Snipaste_2018-09-04_10-29-24](C:\Users\11291\Desktop\Snipaste_2018-09-04_10-29-24.png)

![1536028236606](C:\Users\11291\AppData\Local\Temp\1536028236606.png)

![1536029401134](C:\Users\11291\AppData\Local\Temp\1536029401134.png)