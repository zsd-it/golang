## Golang日志模块


### 日志字段

请求响应日志 + 报错日志
```
{
    "trace_id": "xjfojaosdjfkaojdf",            //链路追踪 - 暂时不要
    "request_id": "xfjoaiwji2ejfojweo"          //服务追踪
    "request_time": "2022-09-09 12:01:23.001",  //请求时间
    "request_time_unix": "1662695458000",
    "level":"info",     //日志级别
    "status_code":200,  // 状态码
    "remote_Ip":"127.0.0.1", //请求IP、 
    "method":"post", //请求方式
    "request_host": "baike.baidu.com", //请求地址
    "request_path": "/abc/xxx", //请求路径
    "request_body": "{\"name\":\"zhangsan\"}", //请求内容

    "response_time_unix": "1662695457000",
    "response_time": "2022-09-09 12:01:24.001",  //响应时间
    "response_body": "{\"name\":\"zhangsan\"}",  //响应内容

    //日志内容
    "business_key": "OP_ERR",
    "business_body": {
        "code": 90001,
        "message": "操作失败！",
        "params": "{}"
    }
}
```


#### 基于trace_id链路追踪

核心思想：将trace_id设置到请求头中透传给下游服务，下游服务按同样的方式再透传给下游服务。

Notice::暂不考虑，目前项目处于初创阶段，不需要完整的链路追踪

#### 基于request_id服务追踪

核心思想：
* 当请求达到Nginx时，Nginx中的拦截器会从请求头中获取request_id，如果为空，则生成一个request_id，并设置到请求头中。
* 服务内的所有日志都加上 request_id 。

