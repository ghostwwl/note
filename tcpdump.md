**tcpdump**

``` tcpdump tcp -i p2p1 -t -s 0 -c 100 and dst port ! 22 and src net 192.168.2.0/24 -w /data0/target.cap``` 


>(1)tcp: ip icmp arp rarp 和 tcp、udp、icmp这些选项等都要放到第一个参数的位置，用来过滤数据报的类型  

>(2)-i p2p1 : 只抓经过网卡接口 p2p1 的包  

>(3)-t : 不显示时间戳  

>(4)-s 0 : 抓取数据包时默认抓取长度为68字节。加上-S 0 后可以抓到完整的数据包  

>(5)-c 100 : 只抓取100个数据包  

>(6)dst port ! 22 : 不抓取目标端口是22的数据包  

>(7)src net 192.168.2.0/24 : 数据包的源网络地址为192.168.2.0/24  

>(8)-w /data0/target.cap : 保存成cap文件，方便用ethereal(即wireshark)分析  
