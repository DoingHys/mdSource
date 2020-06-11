# @requestPart

译文： 
1.@RequestPart这个注解用在multipart/form-data表单提交请求的方法上。 
2.支持的请求方法的方式MultipartFile，属于Spring的MultipartResolver类。这个请求是通过http协议传输的。 
3.@RequestParam也同样支持multipart/form-data请求。 
4.他们最大的不同是，当请求方法的请求参数类型不再是String类型的时候。 
5.@RequestParam适用于name-valueString类型的请求域，@RequestPart适用于复杂的请求域（像JSON，XML）。
————————————————
版权声明：本文为CSDN博主「向小凯同学学习」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/wd2014610/java/article/details/79727061



# feign传输文件

openFeign不支持文件传输，但是其内部集成的feign-form 支持以@requestPart方式上传文件

