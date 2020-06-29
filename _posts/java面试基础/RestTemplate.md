```
SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder]
19:57:03.373 [main] DEBUG org.springframework.web.client.RestTemplate - HTTP POST http://apis.17wo.cn:8088/share-center-rights/trades/querynumroute
19:57:03.394 [main] DEBUG org.springframework.web.client.RestTemplate - Accept=[text/plain, application/xml, text/xml, application/json, application/cbor, application/*+xml, application/*+json, */*]
19:57:03.468 [main] DEBUG org.springframework.web.client.RestTemplate - Writing [{"data":{"csc":"1001","rtime":"2020-06-19 19:57:03","params":{"serialNumber":"18620433472","cityCode":"0020","provinceCode":"51","channelType":"1030100","operId":"HLW00003","channelId":"51b2aub","eparchyCode":"510"}},"sign":"ECAF47A8AD975883D0EB2DE8C03940A5","time":"2020-06-19 19:57:03","channelId":"SSC201909251447034829514"}] with org.springframework.http.converter.xml.MappingJackson2XmlHttpMessageConverter
19:57:03.604 [main] DEBUG org.springframework.web.client.RestTemplate - Response 200 OK
19:57:03.608 [main] DEBUG org.springframework.web.client.RestTemplate - Reading to [java.lang.String] as "application/json;charset=UTF-8"
{"code":"1000","time":1592567823672,"msg":"系统异常，请稍后重试！","data":null,"randomCode":null,"srs":null}

Process finished with exit code 0
```

