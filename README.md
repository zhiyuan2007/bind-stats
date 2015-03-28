# bind-stats
Bind(DNS) has some statistics, but seemly no realtime data , such as qps. so this project modify bind source code, provide qps, success rate,  search type and result by command rndc, it is useful sometimes.


BIND本身提供非常丰富的统计信息，其中几个信息比如当前qps，成功率，或者查询类型是什么，没有统计，或者通过rndc 命令是无法获取的，有的时候这些信息对于查看当前服务器的状态有很大的帮助，所以hack了bind的源码，增加了这几个统计项。

统计项目： 每秒查询量（qps），解析成功率， 解析类型，解析结果。

BIND原版本：bind-9.10.0-p2 

编译要求： BIND 需要编译成多线程版本，即使用--enable-threads 选项。

使用方法：BIND启动后，即可通过rndc查询。查询命令格式如下

rndc -c rndc.conf key [view] [zone] 

key 主要有 qps, success_rate, rcode, rtype 

view 如果指定则表示查询此VIEW下的对应key信息。

zone 如果指定则表示查询在视图view 下的区对应的key 信息，目前版本只实现了对于zone的qps计算，并且需要在BIND的配置文件中指定zone-statistics yes; 

返回值 是字符串

用法举例

1 查询当前总的qps 

   rndc -c rndc.conf qps 

  返回值  qps 5000.000

2 查询当前default视图下的qps

  rndc -c rndc.conf qps default

  返回值 5000.00

3 查询区123.com在视图default下的qps

  rndc -c rndc.conf qps default 123.com

  返回值 qps 4000.00
