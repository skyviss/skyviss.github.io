```
网络到分布式
    * 几条命令
    查看路由表： route -n
    arp协议查看MAC地址：arp -a
    查看本机IP地址：ipconfig
    查看网络连接：netstat -natp
    指令重定向到文件：echo 1 > arp_ignore  
```
# 计算机的四个维度
    * IP地址 如：192.168.1.13
    * 子网掩码 如：255.255.255.0
    * 网关 如：192.168.1.1
    * DNS 域名解析地址
```
    ps：IP地址和子网掩码的按位与运算得到网段地址
```

# 七层网络
    * 应用层
    * 表示层
    * 会话层
    * 传输协议层
    * 网络层
    * 链路层
    * 物理层
# 五层网络
    * 应用层
      http协议  协议：规范与标准
      ssh协议
    * 传输协议层
       三次握手
       数据传输     最小粒度
       四次挥手
    * 网络层
       查找下一跳     route -n
                    路由判定-按位与
                    MAC地址          IP地址的端到端的-MAC地址是节点间的
    * 链路层
    * 物理层
# 网络连接基石 - 三次握手、四次挥手


# 网络连接
 ## 内网连接
```
计算机01  
        交换机01
计算机02
```

 ## 外网连接
```
计算机01                         计算机11
        交换机01  路由器  交换机02
计算机02                         计算机12
```

 ## rs隐藏vip
  ### 修改协议
    cd /proc/sys/net/ipv4/conf/eth0
    echo 1 > arp_ignore
    echo 2 > arp_announce
    cd /proc/sys/net/ipv4/conf/all
    echo 1 > arp_ignore
    echo 2 > arp_announce
  ### 新增环形IP lo
    ifconfig lo:3 192.168.110.80 netmask 255.255.255.255
## lvs暴露的vip
  ### 新增以太网IP eth0
    ifconfig eth0:1 192.168.110.90 netmask 255.255.255.0
    或者
    ifconfig eth0:1 192.168.110.90/24
  ### 删除ip
    ip addr del 192.168.110.90 dev eth0
    
 ## 调度算法
  ### 静态
    rr 轮询
    wrr 加权轮询
    dh 目标地址散列调度
    sh 源地址散列调度
  ### 动态
    lc 最少连接
    wic 加权最少连接
    sed 最短期望延迟
    nq never queue
    dblc 基于本地最少连接
    DH 目标地址散列调度
    LBLCR 基于本地的带复制功能的最少连接


 ## 通讯模型推导 LVS
 ### DR模型 
    (cip-vip)                 (隐藏vip) (vip-cip)
        ((cip-vip)@rip的MAC地址) 
    (cip-vip)                 (隐藏vip) (vip-cip)
    
    优点
        基于二层MAC地址欺骗 速度快，成本低
        lvs不会成为瓶颈
    缺点
        rs和负载均衡服务器必须在同一网段（同一局域网）
    要求
        后端真实服务器rs直接响应客服端，不安全
        
 ### TUN模型 - 隧道模型
    (cip-vip)                 (隐藏vip) (vip-cip)
        (vip-rip(cip-vip)) 
    (cip-vip)                 (隐藏vip) (vip-cip)
    
    优点 
        速度快 lvs不会成为瓶颈
    缺点
        不安全 不能抵抗dos攻击
    要求
        负载均衡服务器lvs 和后端真实服务器rs都有vip
        请求报文不能太大
 ### NAT模型  cip-vip-dip-rip-dip-vip-cip
    (cip-vip)                         (rip-dip)
        (cip-vip) (vip-dip) (dip-rip) 
    (cip-vip)                         (rip-dip)
    
    优点 
        安全
        可以实现不同网段的数据请求
    缺点
        带宽成为瓶颈，消耗算力，非对称
        单点故障
    要求
        rs的gw指向负载均衡服务器
        lvs有不同的网段
        后端真实服务器rs的网关gw必须设置lvs的IP地址

