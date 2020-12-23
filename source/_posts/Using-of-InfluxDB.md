---
title: InfluxDB 的使用
date: 
mathjax: false
tags: [InfluxDB, Database]
categories: tools
---

![pexels-buenosia-carol-707582](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/pexels-buenosia-carol-707582.jpg)

InfluxDB是一个时间序列数据库，SQL数据库可以提供时序的功能，但严格说时序不是其目的。简而言之，InfluxDB用于存储大量的时间序列数据，并对这些数据进行快速的实时分析。

<!--more-->

概念：[什么是InfluxDB？跟其他数据库比有哪些优势？ -- 韩健 腾讯 2020](https://www.yinxiang.com/everhub/note/d134fecc-b51a-4a6b-a3d2-cff6f903bb7d)

InfluxDB是一个时间序列数据库，SQL数据库可以提供时序的功能，但严格说时序不是其目的。简而言之，InfluxDB用于存储大量的时间序列数据，并对这些数据进行快速的实时分析。

## 时序数据

> 时序数据以时间作为主要的查询纬度，通常会将连续的多个时序数据绘制成线，制作基于时间的多纬度报表，用于揭示数据背后的趋势、规律、异常，进行实时在线预测和预警，**时序数据普遍存在于IT基础设施、运维监控系统和物联网中。**

时序数据主要有如下**3个特点：**

- 抵达的数据几乎总是作为新条目被记录，无更新操作。
- 数据通常按照时间顺序抵达。
- 时间是一个主坐标轴。

### 特点

- 无系统环境依赖，部署方便。
- 无模式（schema-less）的数据模型，灵活强大。
- 原生HTTP管理接口，免插件配置和免第三方依赖。
- 强大的类SQL查询语句，学习成本低，上手快。
- 丰富的权限管理功能：精细到“表”级别。
- 丰富的时效管理功能：自动删除过期数据，自定义删除指标数据。
- 低成本存储，采样时序数据，压缩存储。
- 丰富的聚合函数，支持AVG、SUM、MAX、MIN等聚合函数。

### 名词

- 数据分片 -- [数据库分片（Database Sharding)详解 -- 知乎 2019](https://zhuanlan.zhihu.com/p/57185574)
  - 数据量达到一定程度，“垂直扩展（vertical scaling）”性能受到限制，考虑“水平扩展（horizontal scaling）”，将表按行分区的方式。
- 聚合查询 -- 聚合函数 COUNT、SUM、AVG、MIN、MAX
- 无模式数据存储 -- 无结构定义

### 与 SQL 的区别

- **时序数据库：**InfluxDB是一个时间序列数据库，SQL数据库可以提供时序的功能，但严格说时序不是其目的。简而言之，InfluxDB用于存储大量的时间序列数据，并对这些数据进行快速的实时分析。
- **时间为主键：**在InfluxDB中，timestamp标识了在任何给定数据series中的单个点。这就像一个SQL数据库表，其中主键是由系统预先设置的，并且始终是时间。
- **tag、field** 区分是否有索引
- **series** 
- 数据通常一次写入，**很少更新**。由于优先考虑create和read数据的性能而不是update和delete，InfluxDB不是一个完整的CRUD数据库，更像是一个CR-ud。

### Schema 设计

[schema设计 -- 文档](https://jasper-zhang1.gitbooks.io/influxdb/content/Concepts/schema_and_data_layout.html)

### 关键概念

|     **database**     |  **field key**  | **field set** |
| :------------------: | :-------------: | :-----------: |
|   **field value**    | **measurement** |   **point**   |
| **retention policy** |   **series**    |  **tag key**  |
|     **tag set**      |  **tag value**  | **timestamp** |

measurement 相当于 sql 数据库的表

tag 相当于带有索引的列

field 相当于不带有索引的列

retention policy = 数据留存时间（DURATION）+ 副本数量（REPLICATION）

**series = same（measurement、retention policy、tag set）**

不同时序 即不同 series，以 measurement、retention policy、tag set 区分。

| 任意series编号 | retention policy | measurement | tag set                               |
| -------------- | ---------------- | ----------- | ------------------------------------- |
| series 1       | `autogen`        | `census`    | `location = 1,scientist = langstroth` |
| series 2       | `autogen`        | `census`    | `location = 2,scientist = langstroth` |
| series 3       | `autogen`        | `census`    | `location = 1,scientist = perpetua`   |
| series 4       | `autogen`        | `census`    | `location = 2,scientist = perpetua`   |

## influxdb、grafana 安装和脚本使用

vi restart_granfana.sh   重启 改密 数据持久化

> *注意缩进和空格*

```shell
#!/bin/bash
basedir=$(cd `dirname $0`;pwd)

mkdir -p data # creates a folder for your data
ID=$(id -u) # saves your user id in the ID variable

docker stop grafana
docker rm grafana
docker run \
		-d --name grafana  -p 3000:3000 \
		-e "GF_SERVER_ROOT_URL=http://grafana.server.name" \
		-e "GF_SECURITY_ADMIN_PASSWORD=newpwd" \
		--user $ID --volume "$PWD/data:/var/lib/grafana" \
		grafana/grafana grafana
```

vi restart_influxdb.sh

```shell
#!/bin/bash
basedir=$(cd `dirname $0`;pwd)
 
docker stop influxdb
docker rm influxdb
docker run -d --name influxdb -p 8086:8086 -v $basedir/influxdb:/var/lib/influxdb influxdb
```

查询语句中加入 tz('America/Chicago') 可以显示所设置的时区格式

### InfluxDB

开启服务

```shell
./restart_influxdb.sh
```

进入 influxdb shell

```shell
(base) ➜  ~ docker exec -it influxdb bash
root@905cc9094021:/# influx
Connected to http://localhost:8086 version 1.8.2
InfluxDB shell version: 1.8.2
```

#### InfluxDB使用HTTP的API编写数据

```shell
# 新建数据库
curl -i -XPOST http://localhost:8086/query --data-urlencode "q=CREATE DATABASE testdb"

# 使用HTTP的API写入数据
curl -i -XPOST 'http://localhost:8086/write?db=testdb' --data-binary 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'

# 使用HTTP的API请求写入多个点的数据
curl -i -XPOST 'http://localhost:8086/write?db=testdb' --data-binary 'cpu_load_short,host=server02 value=0.67
cpu_load_short,host=server02,region=us-west value=0.55 1422568543702900257
cpu_load_short,direction=in,host=server01,region=us-west value=2.0 1422568543702900257'

# 读取文件，然后使用HTTP的API来写入数据
curl -i -XPOST 'http://localhost:8086/write?db=testdb' --data-binary @cpu_data.txt
```

> 注意：附加`pretty=true`到URL可以启用漂亮的JSON输出。虽然这对于调试或直接使用类似工具查询很有用curl，但不建议将其用于生产，因为它会消耗不必要的网络带宽。

> 可以从上面的返回结果看出，两个查询语句的结果都会合并到result数组中返回。

>该[`max-row-limit`配置选项](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.influxdata.com%2Finfluxdb%2Fv1.7%2Fadministration%2Fconfig%23max-row-limit-0)允许用户限制返回结果的最大数量，以防止InfluxDB运行内存溢出。默认情况下，`max-row-limit`配置选项设置为`0`。该默认设置允许每个请求返回无限数量的行。



```shell
# API查询语句
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=testdb" --data-urlencode "q=SELECT \"value\" FROM \"cpu_load_short\" WHERE \"region\"='us-west'"

# API进行多个查询语句
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=testdb" --data-urlencode "q=SELECT \"value\" FROM \"cpu_load_short\" WHERE \"region\"='us-west';SELECT count(\"value\") FROM \"cpu_load_short\" WHERE \"region\"='us-west'"

# 设置时间戳格式
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=testdb" --data-urlencode "epoch=s" --data-urlencode "q=SELECT \"value\" FROM \"cpu_load_short\" WHERE \"region\"='us-west'"

# 最大行限制
# 该max-row-limit配置选项允许用户限制返回结果的最大数量，以防止InfluxDB运行内存溢出。默认情况下，max-row-limit配置选项设置为0。该默认设置允许每个请求返回无限数量的行。
# 最大行限制仅适用于非分块查询。分块查询可以返回无限数量的点。

# 分块
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=testdb" --data-urlencode "epoch=ms" --data-urlencode "chunked=true" --data-urlencode "chunk_size=1" --data-urlencode "q=SELECT \"value\" FROM \"cpu_load_short\""
```

### telegraf

#### 安装

添加库

```shell
cat <<EOF | sudo tee /etc/apt/sources.list.d/influxdata.list
deb https://repos.influxdata.com/ubuntu bionic stable
EOF
```

导入 apt key

```shell
sudo curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -
```

更新 apt

```shell
sudo apt-get update
```

安装 telegraf

```shell
apt-get -y install telegraf
```

启动 telegraf 服务并设置开机自启

```shell
$ sudo systemctl enable --now telegraf
$ sudo systemctl is-enabled telegraf
enabled
```

查看 telegraf 状态

```shell
systemctl status telegraf
```

看到 telegraf 配置文件在 `/etc/telegraf/telegraf.conf` （可以修改telegraf 导出的 influxdb 端口和数据库）：

```shell
(base) ➜  ~ systemctl status telegraf
● telegraf.service - The plugin-driven server agent for reporting metrics into InfluxDB
   Loaded: loaded (/lib/systemd/system/telegraf.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2020-09-06 10:43:57 CST; 3s ago
     Docs: https://github.com/influxdata/telegraf
 Main PID: 24128 (telegraf)
    Tasks: 6 (limit: 2122)
   CGroup: /system.slice/telegraf.service
           └─24128 /usr/bin/telegraf -config /etc/telegraf/telegraf.conf -config-directory /etc/telegraf/telegraf.d

Sep 06 10:43:57 ccogito.xyz systemd[1]: Stopped The plugin-driven server agent for reporting metrics into InfluxDB.
Sep 06 10:43:57 ccogito.xyz systemd[1]: Started The plugin-driven server agent for reporting metrics into InfluxDB.
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! Starting Telegraf 1.15.2
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! Loaded inputs: processes swap system cpu disk diskio kernel mem
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! Loaded aggregators:
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! Loaded processors:
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! Loaded outputs: influxdb
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! Tags enabled: host=ccogito.xyz
Sep 06 10:43:57 ccogito.xyz telegraf[24128]: 2020-09-06T02:43:57Z I! [agent] Config: Interval:10s, Quiet:false, Hostname:"ccogito.xyz", Flush Interval:10s
(base) ➜  ~
```

遇到报错就修改 telegraf 配置文件（找到 outputs.influxdb，修改 influxdb 端口和数据库）并重启 telegraf 服务

```shell
vi /etc/telegraf/telegraf.conf
systemctl restart telegraf
```

#### telegraf 添加网络监控

```zsh
(base) ➜  ~ vi /etc/telegraf/telegraf.conf
(base) ➜  ~ systemctl restart telegraf
```

![image-20200906120046550](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906120046550.png)

查看数据库下的 measurements

```shell
curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=telegraf" --data-urlencode "q=show measurements"
```

返回面板确认数据的展示效果

具体怎么添加 telegraf 插件，参考 [Telegraf+InfluxDB+Grafana 增加input配置项说明 -- 简书 海洋的渔夫 2019](https://www.jianshu.com/p/fbaec3bf119b)

### grafana 添加 influxdb 数据源

防火墙放行8086端口

```shell
ufw allow 8086
```

> url需配置成正确的宿主机ip和端口（防火墙需放行8086），若不想暴露数据库端口，可换成influxdb容器的ip地址（需自行进入容器查看，容器重启后可能会发生变化）避免数据库暴露至公网。

添加数据源

![img](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/13423234-755cf0eb7ea2daf3.png)

![image-20200906113243628](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906113243628.png)

导入模板

官方看板模板库：[https://grafana.com/dashboards](https://links.jianshu.com/go?to=https%3A%2F%2Fgrafana.com%2Fdashboards)

![image-20200906114050341](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114050341.png)

查看模板id

![image-20200906114146607](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114146607.png)

按照 id 导入到自己的 grafana 模板库

![image-20200906114429141](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114429141.png)

![image-20200906114541322](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114541322.png)

![image-20200906114633870](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114633870.png)

#### 模板展示

模板 [https://grafana.com/grafana/dashboards/10581](https://links.jianshu.com/go?to=https%3A%2F%2Fgrafana.com%2Fgrafana%2Fdashboards%2F10581)

![image-20200906114211189](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114211189.png)

![image-20200906114235823](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114235823.png)

![image-20200906114255270](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906114255270.png)

模板 [https://grafana.com/grafana/dashboards/10581](https://links.jianshu.com/go?to=https%3A%2F%2Fgrafana.com%2Fgrafana%2Fdashboards%2F10581)

![image-20200906115311295](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906115311295.png)



### nmon2influxdb

`json: cannot unmarshal number into Go value of type grafanaclient.DataSourcePlugin`

可能要改一下 `/root/.nmon2influxdb.cfg` 再重试几次。

![image-20200906145913648](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200906145913648.png)

### influxdb 常用操作

[influxdb 常用操作 -- 简书 海洋的渔夫 2019](https://www.jianshu.com/p/b499347ea73a)

### 数据保留策略

[InfluxDB 设置数据保留策略，验证保留的数据存储大小 -- 简书 海洋的渔夫 2019](https://www.jianshu.com/p/cc8f0167ec6b)

### 状态码

![image-20200907202136674](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200907202136674.png)

![image-20200907203436778](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200907203436778.png)

### Downsample and retain data

influxdb 可以在一秒内处理成千上万个数据点，长期保存和使用这么大的数据，久而久之会有存储上的忧虑。一个很自然的应对方法是对数据做采样（downsample），即**仅在一段有限的时间内保持数据的高精度，而在更长的时间上保存低精度和概括性的数据**。接下来将介绍如何用 influxQL 自动化地实现数据采样和删除过期数据。

### 名词定义

- Continuous query（CQ）是数据库的一个自动化和定期执行任务的 InfluxQL query，它必须包含一个至少有一个函数的 select 从句，和一个 GROUP BY time() 从句。
- Retention Policy（RP）



## InfluxDB-Python

[API Documentation -- readthedocs](https://influxdb-python.readthedocs.io/en/latest/api-documentation.html)

[InfluxDB Python Examples](https://influxdb-python.readthedocs.io/en/latest/examples.html)

[入门：将数据写入InfluxDB](https://www.influxdata.com/blog/getting-started-writing-data-to-influxdb/)

[时序数据探索（3）——数据导入InfluxDB方法之直接导入 CSDN 2018](https://blog.csdn.net/qq_41059320/article/details/84566635)

### InfluxDBClient

> *class* `influxdb.InfluxDBClient`(*host=u'localhost'***,** *port=8086***,** *username=u'root'***,** *password=u'root'***,** *database=None***,** *ssl=False***,** *verify_ssl=False***,** *timeout=None***,** *retries=3***,** *use_udp=False***,** *udp_port=4444***,** *proxies=None***,** *pool_size=10***,** *path=u''***,** *cert=None***,** *gzip=False***,** *session=None***,** *headers=None***)**



- 热备冷备
- csv json
- influxDB-python

### 向 influxdb 导入数据

#### 方法

1. 使用 pandas 和 influxdb 行协议导入

2. 使用 influxdb v2 的 CLI 命令导入

   ```bash
   influx write \
     -b bucketName \
     -o orgName \
     -p ns \
     --format=lp
     -f /path/to/myLineProtocolData.txt
   ```

3. 使用Telegraf文件输入插件从CSV中写入数据

4. 

```python
import pandas as pd
#convert csv to line protocol;

#convert sample data to line protocol (with nanosecond precision)
df = pd.read_csv("data/BTC_sm_ns.csv")
lines = ["price"
         + ",type=BTC"
         + " "
         + "close=" + str(df["close"][d]) + ","
         + "high=" + str(df["high"][d]) + ","
         + "low=" + str(df["low"][d]) + ","
         + "open=" + str(df["open"][d]) + ","
         + "volume=" + str(df["volume"][d])
         + " " + str(df["time"][d]) for d in range(len(df))]
thefile = open('data/chronograf.txt', 'w')
for item in lines:
    thefile.write("%s\n" % item)
```

```python
# DDL
CREATE DATABASE import

# DML
# CONTEXT-DATABASE: import
# CONTENT-RETENTION-POLICY: autogen

```

```
influx -import -path=yourfilepath -precision=ns
```



![image-20200907154556312](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200907154556312.png)

![image-20200907154755900](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200907154755900.png)

### json 法

```python
body = [
    {
        "measurement": "students",
        "time": current_time,
        "tags": {
            "class": 1
        },
        "fields": {
            "name": "Hyc",
            "age": 3
        },
    }，
    {
        "measurement": "students",
        "time": current_time,# 只能是rfc3339格式，不能是时间戳
        "tags": {
            "class": 2
        },
        "fields": {
            "name": "Ncb",
            "age": 21
        },
    }，
]
res = client.write_points(body)
```

client.write_points()



报错：

```shell
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb1 in position 0: invalid start byte
```

```python
df = pd.read_csv("便签__com.oneplus.note.csv", encoding='GBK')
```

![image-20200908190326621](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200908190326621.png)

C:\Windows\System32\wsl.exe

```
btc = {
    'file_path': 'bitmex_xbtusd_1m_2016-12-31_2018-06-17.csv',
    'measurement': 'price',
    'tag_keys': [
    ],
    'excp': ['date', 'time', ],
}

proc_bianqian = {
    'file_path': '便签__com.oneplus.note.csv',
    'measurement': 'process_status',
    'tag_keys': [
        'processname',
        'pkgname',
        'label',
        'pid',
        'status',
        'foreground',
    ],
    'excp': ['date', 'time', ],
}
```





生成json 格式文件，写入前要先把json格式转成字符串

读取json文件时再把字符串转成json格式



json.loads json.dumps





类型冲突



## InfulxDB 导入数据的方法

数据量 `pkg_status` 50290 条； `process_status` 56724 条。

1. 通过 **influx CLI** 导入存有 line protocol 格式数据的文件
2. 读取 csv 文件，通过 **InfluxDBClient.write_points()** 以 line protocol 格式导入
3. 读取 json 文件，通过 **InfluxDBClient.write_points()** 以 json 格式导入
4. requests 读取

### 具体实现

```python
file(line)	-> line	-> influxdb		requests		10s				# 实现

file(line)	-> line	-> influxdb		InfluxDBClient					# 暂未实现

file(csv)	-> line -> influxdb		InfluxDBClient	56s				# 实现

file(json)	-> json -> influxdb		InfluxDBClient	66s				# 实现

file(line) 	-> CLI  -> influxdb		CLI				直接传文件 快		# 实现
```

#### 小工具

csv2json()：csv 转 json（influxdb 可用格式）

clock()：程序执行时间计算

### 结果

![image-20200909192622779](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200909192622779.png)

![image-20200909192636784](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200909192636784.png)

![image-20200909194210615](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200909194210615.png)

![image-20200909200652040](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200909200652040.png)

![image-20200910101226108](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910101226108.png)



![image-20200909200929758](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200909200929758.png)



存在一个大表里，用CLI读取

requests 读取



![image-20200910122537551](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910122537551.png)

![image-20200910123053809](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910123053809.png)

```python
file(line)	-> line	-> influxdb		requests		10s

file(line)	-> line	-> influxdb		InfluxDBClient

file(csv)	-> line -> influxdb		InfluxDBClient	56s

file(json)	-> json -> influxdb		InfluxDBClient	66s

file(line) 	-> CLI  -> influxdb		CLI				直接传文件 快

```

![image-20200910130711598](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910130711598.png)

![image-20200910162136262](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910162136262.png)



崩了

![image-20200910163516308](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910163516308.png)

![image-20200910163536994](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910163536994.png)

![image-20200910173345314](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20200910173345314.png)







计算不可靠，消耗性能且服务容易奔溃。
