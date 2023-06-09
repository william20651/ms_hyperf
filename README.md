# microservice
a devolepment environment of microservice base on hyperf。  
基于hyperf搭建的微服务架构DEMO，用于了解如何用hyperf搭建微服务。
## 项目中涉及技术关键字
    1. docker-compose 单机模拟集群环境搭建
    2. 雪花id生成方法
    3. nacos 服务发现 & 服务注册
    4. naces 配置中心使用 ( yaml 格式)
    5. 链路追踪 基于zipkin
    6. 协程非阻塞程序设计
    7. 单元测试
    8. json_rpc 通信协议

# quick start
## 运行环境：
    1. Linux
    2. 文档中出现的mytest.net 需要解析到docker所在宿主服务器的ip


## 所需存储空间：
    服务器上，要建立以下目录，作为持久化所使用的存储空间。并且以下目录处docker中的进行，其他进程尽量不要访问。
    1. 源码存储空间 ：/opt/workspace 【将github上下载的代码目录 microservice 放置在这里】
    2. 其他持久化空间 ： /var/container_data

## 启动环境：
 1. 数据库初始化脚本 : env/service/mysql/init.sql
                    env/service/nacos/nacos_shcema.sql
 2. change dir to : cd /opt/workspace/microservice/env
 3. start environment : docker-compose up
 4. start rpc server : 
    1. go into rpc server container : docker exec -it rpc-server-01 /bin/sh
    2. change dir to rpc server workspace : cd /opt/workspace/microservice/rpc-server/
    3. run : composer update
    4. start rpc server : php bin/hyperf.php start
 5. start rpc client : 
    1. go into rpc client container : docker exec -it rpc-client-01 /bin/sh
    2. change dir to rpc client workspace : cd /opt/workspace/microservice/rpc-client/
    3. run : composer update
    4. start rpc client : php bin/hyperf.php start
 
## 看一下完整链路是否联通
    curl "http://mytest.net:9504/v1/api/test/test"
## unit test commonds
 1. go into rpc server container : docker exec -it rpc-server-01 /bin/sh
 2. change dir to rpc server workspace : cd /opt/workspace/microservice/rpc-server/
 3. commonds : 
    1. run all unit tests : composer test 
    2. run unit test by method : composer test -- --filter=test_create
    
## DEMO
### DEMO_1:创建用户
curl --no-check-certificate --quiet \
--method POST \
--timeout=0 \
--header 'Content-Type: application/x-www-form-urlencoded' \
--body-data 'name=test22222&gender=2' \
'http://mytest.net:9504/v1/api/user/create'
### DEMO_2:获取用户信息
curl --no-check-certificate --quiet \
--method GET \
--timeout=0 \
--header '' \
'http://mytest.net:9504/v1/api/user/info?snow_id={用户创建完成获取的雪花id}'
## some service address:
 1. zipkin : http://mytest.net:9411/zipkin
 2. nacos : http://mytest.net:8848/


