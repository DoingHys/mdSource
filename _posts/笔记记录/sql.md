# 关于sql注入

1.什么是SQL注入

答：SQL注入是通过把SQL命令插入到web表单提交或通过页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL指令。

　　注入攻击的本质是把用户输入的数据当做代码执行。

　　举例如: 表单有两个用户需要填写的表单数据，用户名和密码，如果用户输入admin(用户名)，111(密码)，若数据库中存在此用户则登录成功。SQL大概是这样

　　　　　　SELECT * FROM XXX WHERE userName = admin and password = 111

　　　　　但若是遭到了SQL注入，输入的数据变为 admin or 1 =1 # 密码随便输入，这时候就直接登录了，SQL大概是这样

　　　　　　SELECT * FROM XXX WHERE userName = admin or 1 = 1 # and password = 111 ,因为 # 在sql语句中是注释，将后面密码的验证去掉了，而前面的条件中1 = 1始终成立，所以不管密码正确与否，都能登录成功。

2.mybatis中的#{} 为什么能防止sql注入，${}不能防止sql注入

答: #{}在mybatis中的底层是运用了PreparedStatement 预编译，传入的参数会以 ? 形式显示，因为sql的输入只有在sql编译的时候起作用，当sql预编译完后，传入的参数就仅仅是参数，不会参与sql语句的生成，而${}则没有使用预编译，传入的参数直接和sql进行拼接，由此会产生sql注入的漏洞。