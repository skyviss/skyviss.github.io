```
浅谈 redis
```
# 1 二进制安全
``` 
当前服务只处理二进制字节流数据，字符集由上游服务控制
```
# 2 字符集
## 2.1 ascii码
``` 
最基础的计算机编码
```
## 2.1 扩展字符集-（UTF-8）
``` 
在ascii码基础上扩展来的

如：
    读取一个字节，
    1 如果第一个二进制码为0，直接到ascii码找对应的值
    2 如果前三个二进制码为1，则再读取两个字节，共三个字节，
      意思是三个1代表三个字节，再找相关字符集对应的值
```

# 3 磁盘  内存
## 3.1 描述磁盘两个维度
```
寻址：毫秒级别
带宽：同一时间内网络传输多少字节
```
## 3.2 描述内存两个维度
```
寻址：纳秒级别
宽带：很大
```
## 3.3 两者区别
```  
秒>>毫秒>>微秒>>纳秒
```
# 4 磁盘io
## 4.1 磁道、扇区、
``` 
```
## 4.2 
``` 
```
# 5 io模型
## 5.1 BIO模型
``` 
```
## 5.2 NIO模型
``` 
```
## 5.3 多路复用NIO模型
``` 
```
## 5.4 多路复用NIO模型（伪AIO）
``` 
```
## 5.5 AIO模型
``` 
```




# 6 docker redis
```

第一步: Launch RedisBloom with Docker 
    # docker run -p 6380:6380 --name redis-redisbloom redislabs/rebloom:latest
第二步: Use RedisBloom withredis-cli 
    # docker exec -it redis-redisbloom bash 
    # redis-cli 
    # 127.0.0.1:6379>
第三步: Start a new bloom filter by adding a new item 
    # 127.0.0.1:6379> BF.ADD newFilter foo 
    (integer) 1
第四步: Checking if an item exists in the filter 
    # 127.0.0.1:6379> BF.EXISTS newFilter foo 
    (integer) 1
第五步: 配置密码或其它相关设置 ...... 
    # 127.0.0.1:6379> config set requirepass xxxxx 
    OK 
    # 127.0.0.1:6379> config set notify-keyspace-events xE 
    (error) NOAUTH Authentication required. 
    # 127.0.0.1:6379> auth xxxxx 
    OK 
    # 127.0.0.1:6379> config set notify-keyspace-events xE 
    OK 
    # 127.0.0.1:6379>
```

