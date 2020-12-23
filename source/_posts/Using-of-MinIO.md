---
title: MinIO 的使用
date: 
mathjax: false
tags: [MinIO]
categories: tools
---

![image-20201223221349337](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201223221349337.png)

<!--more-->

## 概览

[TOC]

## MinIO Docker

### Docker-compose 部署

目录：

```shell
$ cd /work/minio
```

**docker-compose.yml**

```yaml
services:
  minio-trainning:
    image: minio/minio
    container_name: minio-trainning
    restart: always
    volumes:
      - ./common/minio/data:/data
      - ./common/minio/config:/root/.minio
      - /etc/localtime:/etc/localtime
    #expose:
    #  - "9000"
    ports:
      - 9000:9000
    environment:
      MINIO_ACCESS_KEY: USERAKIAIOSFODNN7EXAMPLE
      MINIO_SECRET_KEY: PASSwJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    command: server /data
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 30s
      timeout: 20s
      retries: 3
```

新建文件夹：

```shell
$ mkdir -p common/minio/data || mkdir -p common/minio/config
```

拉取镜像：

```shell
$ docker-compose pull
Pulling minio-trainning ... done
```

启动服务：

```shell
$ docker-compose up -d
Creating network "minio_default" with the default driver
Creating minio-trainning ... done
```

通过浏览器查看：

[http://ccogito.xyz:9000/minio/](http://ccogito.xyz:9000/minio/)

![image-20201206124401408](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/image-20201206124401408.png)

## Minio Client

### 安装

```shell
$ cd /work/minio
$ wget https://dl.min.io/client/mc/release/linux-amd64/mc
$ chmod +x mc
$ cp mc /usr/local/bin/		# 作为全局命令
```

### 验证

`mc`预先配置了云存储服务URL：[https://play.min.io，别名“play”。它是一个用于研发和测试的MinIO服务。如果想测试Amazon](https://play.min.xn--io%2Cplay-y36cna8429jrle.xn--minio-fg1hyjr1au8zy4c1ucg90b0w5adtcyt9aey2af3ev4hlp9h.xn--amazon-hh4kt50co9n0mnd15f/) S3,你可以将“play”替换为“s3”。

*示例:*

列出[https://play.min.io上的所有存储桶。](https://play.min.xn--io-rv2c26h33tc7j9mir1cc61c./)

```shell
$ mc ls play
[2020-12-06 15:52:04 CST]     0B 2063b651-92a3-4a20-a4a5-03a96e7c5a89/
[2020-12-06 15:48:00 CST]     0B card-reader-requests/
[2020-12-06 16:33:19 CST]     0B e2d6024483ab4b04/
[2020-12-06 16:51:15 CST]     0B feyo/
[2020-12-06 15:48:38 CST]     0B minio-js-test-1056248f-23c2-4582-8931-0ece0ba676a2/
[2020-12-06 16:40:29 CST]     0B myimg/
[2020-12-06 16:28:38 CST]     0B parla/
[2020-12-06 15:42:36 CST]     0B ps-mpay/
[2020-11-07 00:37:03 CST]     0B rkbucket/
[2020-12-06 03:32:18 CST]     0B test/
[2020-11-06 14:02:33 CST]     0B test-retention-alex/
[2020-12-06 16:37:27 CST]     0B upload/
```

### 添加一个云存储服务

添加一个或多个S3兼容的服务，请参考下面说明。`mc`将所有的配置信息都存储在`~/.mc/config.json`文件中。

```
mc config host add <ALIAS> <YOUR-S3-ENDPOINT> <YOUR-ACCESS-KEY> <YOUR-SECRET-KEY> [--api API-SIGNATURE]
```

别名就是给你的云存储服务起了一个短点的外号。S3 endpoint,access key和secret key是你的云存储服务提供的。API签名是可选参数，默认情况下，它被设置为"S3v4"。

**示例-MinIO云存储**

从MinIO服务获得URL、access key和secret key。

```
mc config host add minio http://localhost:9000 USERAKIAIOSFODNN7EXAMPLE PASSwJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY --api s3v4
```

**示例-Amazon S3云存储**

参考[AWS Credentials指南](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html)获取你的AccessKeyID和SecretAccessKey。

```
mc config host add s3 https://s3.amazonaws.com BKIKJAA5BMMU2RHO6IBB V7f1CwQqAcwo80UEIJEjc5gVQUSSx5ohQ9GSrr12 --api s3v4
```

**示例-Google云存储**

参考[Google Credentials Guide](https://cloud.google.com/storage/docs/migrating?hl=en#keys)获取你的AccessKeyID和SecretAccessKey。

```
mc config host add gcs  https://storage.googleapis.com BKIKJAA5BMMU2RHO6IBB V8f1CwQqAcwo80UEIJEjc5gVQUSSx5ohQ9GSrr12 --api s3v2
```

注意：Google云存储只支持旧版签名版本V2，所以你需要选择S3v2。

### 使用

**命令列表**

|                                                              |                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [**ls** - 列出存储桶和对象](https://docs.min.io/cn/minio-client-complete-guide.html#ls) | [**mb** - 创建存储桶](https://docs.min.io/cn/minio-client-complete-guide.html#mb) | [**cat** - 合并对象](https://docs.min.io/cn/minio-client-complete-guide.html#cat) |
| [**cp** - 拷贝对象](https://docs.min.io/cn/minio-client-complete-guide.html#cp) | [**rm** - 删除对象](https://docs.min.io/cn/minio-client-complete-guide.html#rm) | [**pipe** - Pipe到一个对象](https://docs.min.io/cn/minio-client-complete-guide.html#pipe) |
| [**share** - 共享](https://docs.min.io/cn/minio-client-complete-guide.html#share) | [**mirror** - 存储桶镜像](https://docs.min.io/cn/minio-client-complete-guide.html#mirror) | [**find** - 查找文件和对象](https://docs.min.io/cn/minio-client-complete-guide.html#find) |
| [**diff** - 比较存储桶差异](https://docs.min.io/cn/minio-client-complete-guide.html#diff) | [**policy** - 给存储桶或前缀设置访问策略](https://docs.min.io/cn/minio-client-complete-guide.html#policy) |                                                              |
| [**config** - 管理配置文件](https://docs.min.io/cn/minio-client-complete-guide.html#config) | [**watch** - 事件监听](https://docs.min.io/cn/minio-client-complete-guide.html#watch) | [**events** - 管理存储桶事件](https://docs.min.io/cn/minio-client-complete-guide.html#events) |
| [**update** - 管理软件更新](https://docs.min.io/cn/minio-client-complete-guide.html#update) | [**version** - 显示版本信息](https://docs.min.io/cn/minio-client-complete-guide.html#version) |                                                              |

**全局参数**

> 参数 [--debug]
>
> Debug参数开启控制台输出debug信息。
>
> *示例：输出`ls`命令的详细debug信息。*
>
> ```
> Copymc --debug ls play
> mc: <DEBUG> GET / HTTP/1.1
> Host: play.min.io
> User-Agent: MinIO (darwin; amd64) minio-go/1.0.1 mc/2016-04-01T00:22:11Z
> Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20160408/us-east-1/s3/aws4_request, SignedHeaders=expect;host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
> Expect: 100-continue
> X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
> X-Amz-Date: 20160408T145236Z
> Accept-Encoding: gzip
> 
> mc: <DEBUG> HTTP/1.1 200 OK
> Transfer-Encoding: chunked
> Accept-Ranges: bytes
> Content-Type: text/xml; charset=utf-8
> Date: Fri, 08 Apr 2016 14:54:55 GMT
> Server: MinIO/DEVELOPMENT.2016-04-07T18-53-27Z (linux; amd64)
> Vary: Origin
> X-Amz-Request-Id: HP30I0W2U49BDBIO
> 
> mc: <DEBUG> Response Time:  1.220112837s
> 
> [...]
> 
> [2016-04-08 03:56:14 IST]     0B albums/
> [2016-04-04 16:11:45 IST]     0B backup/
> [2016-04-01 20:10:53 IST]     0B deebucket/
> [2016-03-28 21:53:49 IST]     0B guestbucket/
> ```
>
> 参数 [--json]
>
> JSON参数启用JSON格式的输出。
>
> *示例：列出MinIO play服务的所有存储桶。*
>
> ```
> Copymc --json ls play
> {"status":"success","type":"folder","lastModified":"2016-04-08T03:56:14.577+05:30","size":0,"key":"albums/"}
> {"status":"success","type":"folder","lastModified":"2016-04-04T16:11:45.349+05:30","size":0,"key":"backup/"}
> {"status":"success","type":"folder","lastModified":"2016-04-01T20:10:53.941+05:30","size":0,"key":"deebucket/"}
> {"status":"success","type":"folder","lastModified":"2016-03-28T21:53:49.217+05:30","size":0,"key":"guestbucket/"}
> ```
>
> 参数 [--no-color]
>
> 这个参数禁用颜色主题。对于一些比较老的终端有用。
>
> 参数 [--quiet]
>
> 这个参数关闭控制台日志输出。
>
> 参数 [--config-dir]
>
> 这个参数参数自定义的配置文件路径。
>
> 参数 [ --insecure]
>
> 跳过SSL证书验证。

## MinIO SDK - Python Client

### pip 安装

```
$ pip install minio	
```

### 初始化 MinIO Client

MinIO client需要以下4个参数来连接MinIO对象存储服务。

| 参数       | 描述                                   |
| :--------- | :------------------------------------- |
| endpoint   | 对象存储服务的URL。                    |
| access_key | Access key是唯一标识你的账户的用户ID。 |
| secret_key | Secret key是你账户的密码。             |
| secure     | true代表使用HTTPS。                    |

```python
from minio import Minio
from minio.error import ResponseError

minioClient = Minio('localhost:9000',
                  access_key='USERAKIAIOSFODNN7EXAMPLE',
                  secret_key='PASSwJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY',
                  secure=False)
```

### 示例-上传文件

本示例连接到一个MinIO对象存储服务，创建一个存储桶并上传一个文件到存储桶中。

我们在本示例中使用运行在 [https://play.min.io](https://play.min.io/) 上的MinIO服务，你可以用这个服务来开发和测试。示例中的访问凭据是公开的。

#### file-uploader.py

```python
# 引入MinIO包。
from minio import Minio
from minio.error import (ResponseError, BucketAlreadyOwnedByYou,
                         BucketAlreadyExists)

# 使用endpoint、access key和secret key来初始化minioClient对象。
minioClient = Minio('localhost:9000',
                    access_key='USERAKIAIOSFODNN7EXAMPLE',
                    secret_key='PASSwJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY',
                    secure=False)

# 调用make_bucket来创建一个存储桶。
try:
       minioClient.make_bucket("maylogs", location="us-east-1")
except BucketAlreadyOwnedByYou as err:
       pass
except BucketAlreadyExists as err:
       pass
except ResponseError as err:
       raise
else:
        try:
               minioClient.fput_object('maylogs', 'pumaserver_debug.log', './1.log')
        except ResponseError as err:
               print(err)
```

#### Run file-uploader

```shell
$ python file_uploader.py

$ mc ls play/maylogs/
[2020-12-06 16:47:54 CST]     0B pumaserver_debug.log
```

### [Python Client API](https://docs.min.io/cn/python-client-api-reference.html)

| 操作存储桶                                                   | 操作对象                                                     | Presigned操作                                                | 存储桶策略/通知                                              |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`make_bucket`](https://docs.min.io/cn/python-client-api-reference.html#make_bucket) | [`get_object`](https://docs.min.io/cn/python-client-api-reference.html#get_object) | [`presigned_get_object`](https://docs.min.io/cn/python-client-api-reference.html#presigned_get_object) | [`get_bucket_policy`](https://docs.min.io/cn/python-client-api-reference.html#get_bucket_policy) |
| [`list_buckets`](https://docs.min.io/cn/python-client-api-reference.html#list_buckets) | [`put_object`](https://docs.min.io/cn/python-client-api-reference.html#put_object) | [`presigned_put_object`](https://docs.min.io/cn/python-client-api-reference.html#presigned_put_object) | [`set_bucket_policy`](https://docs.min.io/cn/python-client-api-reference.html#set_bucket_policy) |
| [`bucket_exists`](https://docs.min.io/cn/python-client-api-reference.html#bucket_exists) | [`copy_object`](https://docs.min.io/cn/python-client-api-reference.html#copy_object) | [`presigned_post_policy`](https://docs.min.io/cn/python-client-api-reference.html#presigned_post_policy) | [`get_bucket_notification`](https://docs.min.io/cn/python-client-api-reference.html#get_bucket_notification) |
| [`remove_bucket`](https://docs.min.io/cn/python-client-api-reference.html#remove_bucket) | [`stat_object`](https://docs.min.io/cn/python-client-api-reference.html#stat_object) |                                                              | [`set_bucket_notification`](https://docs.min.io/cn/python-client-api-reference.html#set_bucket_notification) |
| [`list_objects`](https://docs.min.io/cn/python-client-api-reference.html#list_objects) | [`remove_object`](https://docs.min.io/cn/python-client-api-reference.html#remove_object) |                                                              | [`remove_all_bucket_notification`](https://docs.min.io/cn/python-client-api-reference.html#remove_all_bucket_notification) |
| [`list_objects_v2`](https://docs.min.io/cn/python-client-api-reference.html#list_objects_v2) | [`remove_objects`](https://docs.min.io/cn/python-client-api-reference.html#remove_objects) |                                                              | [`listen_bucket_notification`](https://docs.min.io/cn/python-client-api-reference.html#listen_bucket_notification) |
| [`list_incomplete_uploads`](https://docs.min.io/cn/python-client-api-reference.html#list_incomplete_uploads) | [`remove_incomplete_upload`](https://docs.min.io/cn/python-client-api-reference.html#remove_incomplete_upload) |                                                              |                                                              |
|                                                              | [`fput_object`](https://docs.min.io/cn/python-client-api-reference.html#fput_object) |                                                              |                                                              |
|                                                              | [`fget_object`](https://docs.min.io/cn/python-client-api-reference.html#fget_object) |                                                              |                                                              |
|                                                              | [`get_partial_object`](https://docs.min.io/cn/python-client-api-reference.html#get_partial_object) |                                                              |                                                              |

#### 构造函数

Minio(endpoint, access_key=None, secret_key=None, secure=True, region=None, http_client=None)

初使化一个新的client对象。

参数

| 参数          | 类型                              | 描述                                                         |
| :------------ | :-------------------------------- | :----------------------------------------------------------- |
| `endpoint`    | *string*                          | S3兼容对象存储服务endpoint。                                 |
| `access_key`  | *string*                          | 对象存储的Access key。（如果是匿名访问则可以为空）。         |
| `secret_key`  | *string*                          | 对象存储的Secret key。（如果是匿名访问则可以为空）。         |
| `secure`      | *bool*                            | 设为`True`代表启用HTTPS。 (默认是`True`)。                   |
| `region`      | *string*                          | 设置该值以覆盖自动发现存储桶region。 （可选，默认值是`None`）。 |
| `http_client` | *urllib3.poolmanager.PoolManager* | 设置该值以使用自定义的http client，而不是默认的http client。（可选，默认值是`None`）。 |

### 操作桶

#### make_bucket(bucket_name, location='us-east-1')

创建一个存储桶。

**示例**

```
try:
    minioClient.make_bucket("mybucket", location="us-east-1")
except ResponseError as err:
    print(err)
```

#### list_buckets()

列出所有的存储桶。

**示例**

```
buckets = minioClient.list_buckets()
for bucket in buckets:
    print(bucket.name, bucket.creation_date)
```

#### bucket_exists(bucket_name)

检查存储桶是否存在。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |

**示例**

```
Copytry:
    print(minioClient.bucket_exists("mybucket"))
except ResponseError as err:
    print(err)
```



#### remove_bucket(bucket_name)

删除存储桶。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |

**示例**

```
Copytry:
    minioClient.remove_bucket("mybucket")
except ResponseError as err:
    print(err)
```



#### list_objects(bucket_name, prefix=None, recursive=False)

列出存储桶中所有对象。

参数

| 参数          | 类型     | 描述                                                         |
| :------------ | :------- | :----------------------------------------------------------- |
| `bucket_name` | *string* | 存储桶名称。                                                 |
| `prefix`      | *string* | 用于过滤的对象名称前缀。可选项，默认为None。                 |
| `recursive`   | *bool*   | `True`代表递归查找，`False`代表类似文件夹查找，以'/'分隔，不查子文件夹。（可选，默认值是`False`）。 |

**返回值**

| 参数     | 类型     | 描述                                           |
| :------- | :------- | :--------------------------------------------- |
| `object` | *Object* | 该存储桶中所有对象的Iterator，对象的格式如下： |

| 参数                   | 类型                | 描述                                                         |
| :--------------------- | :------------------ | :----------------------------------------------------------- |
| `object.bucket_name`   | *string*            | 对象所在存储桶的名称。                                       |
| `object.object_name`   | *string*            | 对象的名称。                                                 |
| `object.is_dir`        | *bool*              | `True`代表列举的对象是文件夹（对象前缀）， `False`与之相反。 |
| `object.size`          | *int*               | 对象的大小。                                                 |
| `object.etag`          | *string*            | 对象的etag值。                                               |
| `object.last_modified` | *datetime.datetime* | 最后修改时间。                                               |
| `object.content_type`  | *string*            | 对象的content-type。                                         |
| `object.metadata`      | *dict*              | 对象的其它元数据。                                           |

**示例**

```
Copy# List all object paths in bucket that begin with my-prefixname.
objects = minioClient.list_objects('mybucket', prefix='my-prefixname',
                              recursive=True)
for obj in objects:
    print(obj.bucket_name, obj.object_name.encode('utf-8'), obj.last_modified,
          obj.etag, obj.size, obj.content_type)
```



#### list_objects_v2(bucket_name, prefix=None, recursive=False)

使用V2版本API列出一个存储桶中的对象。

参数

| 参数          | 类型     | 描述                                                         |
| :------------ | :------- | :----------------------------------------------------------- |
| `bucket_name` | *string* | 存储桶名称。                                                 |
| `prefix`      | *string* | 用于过滤的对象名称前缀。可选项，默认为None。                 |
| `recursive`   | *bool*   | `True`代表递归查找，`False`代表类似文件夹查找，以'/'分隔，不查子文件夹。（可选，默认值是`False`）。 |

**返回值**

| 参数     | 类型     | 描述                                           |
| :------- | :------- | :--------------------------------------------- |
| `object` | *Object* | 该存储桶中所有对象的Iterator，对象的格式如下： |

| 参数                   | 类型                | 描述                                                         |
| :--------------------- | :------------------ | :----------------------------------------------------------- |
| `object.bucket_name`   | *string*            | 对象所在存储桶的名称。                                       |
| `object.object_name`   | *string*            | 对象的名称。                                                 |
| `object.is_dir`        | *bool*              | `True`代表列举的对象是文件夹（对象前缀）， `False`与之相反。 |
| `object.size`          | *int*               | 对象的大小。                                                 |
| `object.etag`          | *string*            | 对象的etag值。                                               |
| `object.last_modified` | *datetime.datetime* | 最后修改时间。                                               |
| `object.content_type`  | *string*            | 对象的content-type。                                         |
| `object.metadata`      | *dict*              | 对象的其它元数据。                                           |

**示例**

```
Copy# List all object paths in bucket that begin with my-prefixname.
objects = minioClient.list_objects_v2('mybucket', prefix='my-prefixname',
                              recursive=True)
for obj in objects:
    print(obj.bucket_name, obj.object_name.encode('utf-8'), obj.last_modified,
          obj.etag, obj.size, obj.content_type)
```



#### list_incomplete_uploads(bucket_name, prefix, recursive=False)

列出存储桶中未完整上传的对象。

参数

| 参数          | 类型     | 描述                                                         |
| :------------ | :------- | :----------------------------------------------------------- |
| `bucket_name` | *string* | 存储桶名称。                                                 |
| `prefix`      | *string* | 用于过滤的对象名称前缀。                                     |
| `recursive`   | *bool*   | `True`代表递归查找，`False`代表类似文件夹查找，以'/'分隔，不查子文件夹。（可选，默认值是`False`）。 |

**返回值**

| 参数            | 类型     | 描述                                |
| :-------------- | :------- | :---------------------------------- |
| `multipart_obj` | *Object* | multipart对象的Iterator，格式如下： |

| 参数                        | 类型     | 描述                       |
| :-------------------------- | :------- | :------------------------- |
| `multipart_obj.object_name` | *string* | 未完整上传的对象的名称。   |
| `multipart_obj.upload_id`   | *string* | 未完整上传的对象的上传ID。 |
| `multipart_obj.size`        | *int*    | 未完整上传的对象的大小。   |

**示例**

```
Copy# List all object paths in bucket that begin with my-prefixname.
uploads = minioClient.list_incomplete_uploads('mybucket',
                                         prefix='my-prefixname',
                                         recursive=True)
for obj in uploads:
    print(obj.bucket_name, obj.object_name, obj.upload_id, obj.size)
```



#### get_bucket3_policy(bucket_name, prefix)

获取存储桶的当前策略。

参数

| 参数          | 类型     | 描述             |
| :------------ | :------- | :--------------- |
| `bucket_name` | *string* | 存储桶名称。     |
| `prefix`      | *string* | 对象的名称前缀。 |

**返回值**

| 参数     | 类型                  | 描述                                                         |
| :------- | :-------------------- | :----------------------------------------------------------- |
| `Policy` | *minio.policy.Policy* | Policy枚举：Policy.READ_ONLY，Policy.WRITE_ONLY，Policy.READ_WRITE或 Policy.NONE。 |

**示例**

```
Copy# Get current policy of all object paths in bucket that begin with my-prefixname.
policy = minioClient.get_bucket_policy('mybucket',
                                       'my-prefixname')
print(policy)
```



#### set_bucket_policy(bucket_name, prefix, policy)

给指定的存储桶设置存储桶策略。如果`prefix`不为空，则该存储桶策略仅对匹配这个指定前缀的对象生效。

参数

| 参数          | 类型                  | 描述                                                         |
| :------------ | :-------------------- | :----------------------------------------------------------- |
| `bucket_name` | *string*              | 存储桶名称。                                                 |
| `prefix`      | *string*              | 对象的名称前缀。                                             |
| `Policy`      | *minio.policy.Policy* | Policy枚举：Policy.READ_ONLY，Policy.WRITE_ONLY，Policy.READ_WRITE或 Policy.NONE。 |

**示例**

```
Copy# Set policy Policy.READ_ONLY to all object paths in bucket that begin with my-prefixname.
minioClient.set_bucket_policy('mybucket',
                              'my-prefixname',
                              Policy.READ_ONLY)
```



#### get_bucket_notification(bucket_name)

获取存储桶上的通知配置。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |

**返回值**

| 参数           | 类型   | 描述                                                         |
| :------------- | :----- | :----------------------------------------------------------- |
| `notification` | *dict* | 如果没有通知配置，则返回一个空的dictionary，否则就和set_bucket_notification的参数结构一样。 |

**示例**

```
Copy# Get the notifications configuration for a bucket.
notification = minioClient.get_bucket_notification('mybucket')
# If no notification is present on the bucket:
# notification == {}
```



#### set_bucket_notification(bucket_name, notification)

给存储桶设置通知配置。

参数

| 参数           | 类型     | 描述                               |
| :------------- | :------- | :--------------------------------- |
| `bucket_name`  | *string* | 存储桶名称。                       |
| `notification` | *dict*   | 非空dictionary，内部结构格式如下： |

`notification`参数格式如下：

- (dict) --
  - **TopicConfigurations** (list) -- 服务配置项目的可选列表，指定了AWS SNS Topics做为通知的目标。
  - **QueueConfigurations** (list) -- 服务配置项目的可选列表，指定了AWS SQS Queues做为通知的目标。
  - **CloudFunctionconfigurations** (list) -- 服务配置项目的可选列表，指定了AWS Lambda Cloud functions做为通知的目标。

以上项目中至少有一项需要在`notification`参数中指定。

上面提到的“服务配置项目”具有以下结构：

- (dict) --

  - **Id** (string) -- 配置项的可选ID，如果不指定，服务器自动生成。

  - **Arn** (string) -- 指定特定的Topic/Queue/Cloud Function identifier。

  - **Events** (list) -- 一个含有事件类型字符串的非空列表，事件类型取值如下： *'s3:ReducedRedundancyLostObject'*, *'s3:ObjectCreated:\*'*, *'s3:ObjectCreated:Put'*, *'s3:ObjectCreated:Post'*, *'s3:ObjectCreated:Copy'*, *'s3:ObjectCreated:CompleteMultipartUpload'*, *'s3:ObjectRemoved:\*'*, *'s3:ObjectRemoved:Delete'*, *'s3:ObjectRemoved:DeleteMarkerCreated'*

  - Filter

     

    (dict) -- 一个可选的dictionary容器，里面含有基于键名称过滤的规则的对象。

    - Key

       

      (dict) -- dictionary容器，里面含有基于键名称前缀和后缀过滤的规则的对象。

      - FilterRules

         

        (list) -- 指定过滤规则标准的容器列表。

        - (dict) -- 键值对的dictionary容器，指定单个的过滤规则。
          - **Name** (string) -- 对象的键名称，值为“前缀”或“后缀”。
          - **Value** (string) -- 指定规则适用的值。

没有返回值。如果目标服务报错，会抛出`ResponseError`。如果有验证错误，会抛出`InvalidArgumentError`或者`TypeError`。输入参数的configuration不能为空 - 为了删除存储桶上的通知配置，参考`remove_all_bucket_notification()` API。

**示例**

```
Copynotification = {
    'QueueConfigurations': [
        {
            'Id': '1',
            'Arn': 'arn1',
            'Events': ['s3:ObjectCreated:*'],
            'Filter': {
                'Key': {
                    'FilterRules': [
                        {
                            'Name': 'prefix',
                            'Value': 'abc'
                        }
                    ]
                }
            }
        }
    ],
    'TopicConfigurations': [
        {
            'Arn': 'arn2',
            'Events': ['s3:ObjectCreated:*'],
            'Filter': {
                'Key': {
                    'FilterRules': [
                        {
                            'Name': 'suffix',
                            'Value': '.jpg'
                        }
                    ]
                }
            }
        }
    ],
    'CloudFunctionConfigurations': [
        {
            'Arn': 'arn3',
            'Events': ['s3:ObjectRemoved:*'],
            'Filter': {
                'Key': {
                    'FilterRules': [
                        {
                            'Name': 'suffix',
                            'Value': '.jpg'
                        }
                    ]
                }
            }
        }
    ]
}


try:
    minioClient.set_bucket_notification('mybucket', notification)
except ResponseError as err:
    # handle error response from service.
    print(err)
except (ArgumentError, TypeError) as err:
    # should happen only during development. Fix the notification argument
    print(err)
```



#### remove_all_bucket_notification(bucket_name)

删除存储桶上配置的所有通知。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |

没有返回值，如果操作失败会抛出 `ResponseError` 异常。

**示例**

```
Copy# Remove all the notifications config for a bucket.
minioClient.remove_all_bucket_notification('mybucket')
```



#### listen_bucket_notification(bucket_name, prefix, suffix, events)

监听存储桶上的通知，可以额外提供前缀、后缀和时间类型来进行过滤。使用该API前不需要先设置存储桶通知。这是一个MinIO的扩展API，MinIO Server会基于过来的请求使用唯一标识符自动注册或者注销。

当通知发生时，产生事件，调用者需要遍历读取这些事件。

参数

| 参数          | 类型     | 描述                       |
| :------------ | :------- | :------------------------- |
| `bucket_name` | *string* | 监听事件通知的存储桶名称。 |
| `prefix`      | *string* | 过滤通知的对象名称前缀。   |
| `suffix`      | *string* | 过滤通知的对象名称后缀。   |
| `events`      | *list*   | 启用特定事件类型的通知。   |

完整示例请看 [这里](https://raw.githubusercontent.com/minio/minio-py/master/examples/listen_notification.py)。

```
Copy# Put a file with default content-type.
events = minioClient.listen_bucket_notification('my-bucket', 'my-prefix/',
                                                '.my-suffix',
                                                ['s3:ObjectCreated:*',
                                                 's3:ObjectRemoved:*',
                                                 's3:ObjectAccessed:*'])
for event in events:
    print event
```

### 操作对象

#### get_object(bucket_name, object_name, request_headers=None)

下载一个对象。

参数

| 参数              | 类型     | 描述                                    |
| :---------------- | :------- | :-------------------------------------- |
| `bucket_name`     | *string* | 存储桶名称。                            |
| `object_name`     | *string* | 对象名称。                              |
| `request_headers` | *dict*   | 额外的请求头信息 （可选，默认为None）。 |

**返回值**

| 参数     | 类型                            | 描述                    |
| :------- | :------------------------------ | :---------------------- |
| `object` | *urllib3.response.HTTPResponse* | http streaming reader。 |

**示例**

```
Copy# Get a full object.
try:
    data = minioClient.get_object('mybucket', 'myobject')
    with open('my-testfile', 'wb') as file_data:
        for d in data.stream(32*1024):
            file_data.write(d)
except ResponseError as err:
    print(err)
```



#### get_partial_object(bucket_name, object_name, offset=0, length=0, request_headers=None)

下载一个对象的指定区间的字节数组。

参数

| 参数              | 类型     | 描述                                                        |
| :---------------- | :------- | :---------------------------------------------------------- |
| `bucket_name`     | *string* | 存储桶名称。                                                |
| `object_name`     | *string* | 对象名称。                                                  |
| `offset`          | *int*    | `offset` 是起始字节的位置                                   |
| `length`          | *int*    | `length`是要读取的长度 (可选，如果无值则代表读到文件结尾)。 |
| `request_headers` | *dict*   | 额外的请求头信息 （可选，默认为None）。                     |

**返回值**

| 参数     | 类型                            | 描述                    |
| :------- | :------------------------------ | :---------------------- |
| `object` | *urllib3.response.HTTPResponse* | http streaming reader。 |

**示例**

```
Copy# Offset the download by 2 bytes and retrieve a total of 4 bytes.
try:
    data = minioClient.get_partial_object('mybucket', 'myobject', 2, 4)
    with open('my-testfile', 'wb') as file_data:
        for d in data:
            file_data.write(d)
except ResponseError as err:
    print(err)
```



#### fget_object(bucket_name, object_name, file_path, request_headers=None)

下载并将文件保存到本地。

参数

| 参数              | 类型     | 描述                                    |
| :---------------- | :------- | :-------------------------------------- |
| `bucket_name`     | *string* | 存储桶名称。                            |
| `object_name`     | *string* | 对象名称。                              |
| `file_path`       | *dict*   | 对象数据要写入的本地文件路径。          |
| `request_headers` | *dict*   | 额外的请求头信息 （可选，默认为None）。 |

**返回值**

| 参数  | 类型     | 描述                       |
| :---- | :------- | :------------------------- |
| `obj` | *Object* | 对象的统计信息，格式如下： |

| 参数                | 类型        | 描述                 |
| :------------------ | :---------- | :------------------- |
| `obj.size`          | *int*       | 对象的大小。         |
| `obj.etag`          | *string*    | 对象的etag值。       |
| `obj.content_type`  | *string*    | 对象的Content-Type。 |
| `obj.last_modified` | *time.time* | 最后修改时间。       |
| `obj.metadata`      | *dict*      | 对象的其它元数据。   |

**示例**

```
Copy# Get a full object and prints the original object stat information.
try:
    print(minioClient.fget_object('mybucket', 'myobject', '/tmp/myobject'))
except ResponseError as err:
    print(err)
```



#### copy_object(bucket_name, object_name, object_source, copy_conditions=None, metadata=None)

拷贝对象存储服务上的源对象到一个新对象。

注意：本API支持的最大文件大小是5GB。

参数

| 参数              | 类型             | 描述                                             |
| :---------------- | :--------------- | :----------------------------------------------- |
| `bucket_name`     | *string*         | 新对象的存储桶名称。                             |
| `object_name`     | *string*         | 新对象的名称。                                   |
| `object_source`   | *string*         | 要拷贝的源对象的存储桶名称+对象名称。            |
| `copy_conditions` | *CopyConditions* | 拷贝操作需要满足的一些条件（可选，默认为None）。 |

**示例**

以下所有条件都是允许的，并且可以组合使用。

```
Copyimport time
from datetime import datetime
from minio import CopyConditions

copy_conditions = CopyConditions()
# Set modified condition, copy object modified since 2014 April.
t = (2014, 4, 0, 0, 0, 0, 0, 0, 0)
mod_since = datetime.utcfromtimestamp(time.mktime(t))
copy_conditions.set_modified_since(mod_since)

# Set unmodified condition, copy object unmodified since 2014 April.
copy_conditions.set_unmodified_since(mod_since)

# Set matching ETag condition, copy object which matches the following ETag.
copy_conditions.set_match_etag("31624deb84149d2f8ef9c385918b653a")

# Set matching ETag except condition, copy object which does not match the following ETag.
copy_conditions.set_match_etag_except("31624deb84149d2f8ef9c385918b653a")

# Set metadata
metadata = {"test-key": "test-data"}

try:
    copy_result = minioClient.copy_object("mybucket", "myobject",
                                          "/my-sourcebucketname/my-sourceobjectname",
                                          copy_conditions,metadata=metadata)
    print(copy_result)
except ResponseError as err:
    print(err)
```



#### put_object(bucket_name, object_name, data, length, content_type='application/octet-stream', metadata=None)

添加一个新的对象到对象存储服务。

注意：本API支持的最大文件大小是5TB。

参数

| 参数           | 类型           | 描述                                                         |
| :------------- | :------------- | :----------------------------------------------------------- |
| `bucket_name`  | *string*       | 存储桶名称。                                                 |
| `object_name`  | *string*       | 对象名称。                                                   |
| `data`         | *io.RawIOBase* | 任何实现了io.RawIOBase的python对象。                         |
| `length`       | *int*          | 对象的总长度。                                               |
| `content_type` | *string*       | 对象的Content type。（可选，默认是“application/octet-stream”）。 |
| `metadata`     | *dict*         | 其它元数据。（可选，默认是None）。                           |

**返回值**

| 参数   | 类型     | 描述           |
| :----- | :------- | :------------- |
| `etag` | *string* | 对象的etag值。 |

**示例**

单个对象的最大大小限制在5TB。put_object在对象大于5MiB时，自动使用multiple parts方式上传。这样，当上传失败时，客户端只需要上传未成功的部分即可（类似断点上传）。上传的对象使用MD5SUM签名进行完整性验证。

```
import os
# Put a file with default content-type, upon success prints the etag identifier computed by server.
try:
    with open('my-testfile', 'rb') as file_data:
        file_stat = os.stat('my-testfile')
        print(minioClient.put_object('mybucket', 'myobject',
                               file_data, file_stat.st_size))
except ResponseError as err:
    print(err)

# Put a file with 'application/csv'.
try:
    with open('my-testfile.csv', 'rb') as file_data:
        file_stat = os.stat('my-testfile.csv')
        minioClient.put_object('mybucket', 'myobject.csv', file_data,
                    file_stat.st_size, content_type='application/csv')
except ResponseError as err:
    print(err)
```



#### fput_object(bucket_name, object_name, file_path, content_type='application/octet-stream', metadata=None)

通过文件上传到对象中。

参数

| 参数           | 类型     | 描述                                                         |
| :------------- | :------- | :----------------------------------------------------------- |
| `bucket_name`  | *string* | 存储桶名称。                                                 |
| `object_name`  | *string* | 对象名称。                                                   |
| `file_path`    | *string* | 本地文件的路径，会将该文件的内容上传到对象存储服务上。       |
| `content_type` | *string* | 对象的Content type（可选，默认是“application/octet-stream”）。 |
| `metadata`     | *dict*   | 其它元数据（可选，默认是None）。                             |

**返回值**

| 参数   | 类型     | 描述           |
| :----- | :------- | :------------- |
| `etag` | *string* | 对象的etag值。 |

**示例**

单个对象的最大大小限制在5TB。fput_object在对象大于5MiB时，自动使用multiple parts方式上传。这样，当上传失败时，客户端只需要上传未成功的部分即可（类似断点上传）。上传的对象使用MD5SUM签名进行完整性验证。

```
Copy# Put an object 'myobject' with contents from '/tmp/otherobject', upon success prints the etag identifier computed by server.
try:
    print(minioClient.fput_object('mybucket', 'myobject', '/tmp/otherobject'))
except ResponseError as err:
    print(err)

# Put on object 'myobject.csv' with contents from
# '/tmp/otherobject.csv' as 'application/csv'.
try:
    print(minioClient.fput_object('mybucket', 'myobject.csv',
                             '/tmp/otherobject.csv',
                             content_type='application/csv'))
except ResponseError as err:
    print(err)
```



#### stat_object(bucket_name, object_name)

获取对象的元数据。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |
| `object_name` | *string* | 名称名称。   |

**返回值**

| 参数  | 类型     | 描述                       |
| :---- | :------- | :------------------------- |
| `obj` | *Object* | 对象的统计信息，格式如下： |

| 参数                | 类型        | 描述                    |
| :------------------ | :---------- | :---------------------- |
| `obj.size`          | *int*       | 对象的大小。            |
| `obj.etag`          | *string*    | 对象的etag值。          |
| `obj.content_type`  | *string*    | 对象的Content-Type。    |
| `obj.last_modified` | *time.time* | UTC格式的最后修改时间。 |
| `obj.metadata`      | *dict*      | 对象的其它元数据信息。  |

**示例**

```
Copy# Fetch stats on your object.
try:
    print(minioClient.stat_object('mybucket', 'myobject'))
except ResponseError as err:
    print(err)
```



#### remove_object(bucket_name, object_name)

删除一个对象。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |
| `object_name` | *string* | 对象名称。   |

**示例**

```
Copy# Remove an object.
try:
    minioClient.remove_object('mybucket', 'myobject')
except ResponseError as err:
    print(err)
```



#### remove_objects(bucket_name, objects_iter)

删除存储桶中的多个对象。

参数

| 参数           | 类型                           | 描述                     |
| :------------- | :----------------------------- | :----------------------- |
| `bucket_name`  | *string*                       | 存储桶名称。             |
| `objects_iter` | *list* , *tuple* or *iterator* | 多个对象名称的列表数据。 |

**返回值**

| 参数                    | 类型                                       | 描述                                  |
| :---------------------- | :----------------------------------------- | :------------------------------------ |
| `delete_error_iterator` | *iterator* of *MultiDeleteError* instances | 删除失败的错误信息iterator,格式如下： |

*注意*

1. 由于上面的方法是延迟计算（lazy evaluation），默认是不计算的，所以上面返回的iterator必须被evaluated（比如：使用循环）。
2. 该iterator只有在执行删除操作出现错误时才不为空，每一项都包含删除报错的对象的错误信息。

该iterator产生的每一个删除错误信息都有如下结构：

| 参数                             | 类型     | 描述                 |
| :------------------------------- | :------- | :------------------- |
| `MultiDeleteError.object_name`   | *string* | 删除报错的对象名称。 |
| `MultiDeleteError.error_code`    | *string* | 错误码。             |
| `MultiDeleteError.error_message` | *string* | 错误信息。           |

**示例**

```
Copy# Remove multiple objects in a single library call.
try:
    objects_to_delete = ['myobject-1', 'myobject-2', 'myobject-3']
    # force evaluation of the remove_objects() call by iterating over
    # the returned value.
    for del_err in minioClient.remove_objects('mybucket', objects_to_delete):
        print("Deletion Error: {}".format(del_err))
except ResponseError as err:
    print(err)
```



#### remove_incomplete_upload(bucket_name, object_name)

删除一个未完整上传的对象。

参数

| 参数          | 类型     | 描述         |
| :------------ | :------- | :----------- |
| `bucket_name` | *string* | 存储桶名称。 |
| `object_name` | *string* | 对象名称。   |

**示例**

```
Copy# Remove an partially uploaded object.
try:
    minioClient.remove_incomplete_upload('mybucket', 'myobject')
except ResponseError as err:
    print(err)
```

### Presigned操作



#### presigned_get_object(bucket_name, object_name, expiry=timedelta(days=7))

生成一个用于HTTP GET操作的presigned URL。浏览器/移动客户端可以在即使存储桶为私有的情况下也可以通过这个URL进行下载。这个presigned URL可以有一个过期时间，默认是7天。

参数

| 参数               | 类型                | 描述                                                         |
| :----------------- | :------------------ | :----------------------------------------------------------- |
| `bucket_name`      | *string*            | 存储桶名称。                                                 |
| `object_name`      | *string*            | 对象名称。                                                   |
| `expiry`           | *datetime.datetime* | 过期时间，单位是秒，默认是7天。                              |
| `response_headers` | *dictionary*        | 额外的响应头 （比如：`response-content-type`、`response-content-disposition`）。 |

**示例**

```
Copyfrom datetime import timedelta

# presigned get object URL for object name, expires in 2 days.
try:
    print(minioClient.presigned_get_object('mybucket', 'myobject', expires=timedelta(days=2)))
# Response error is still possible since internally presigned does get bucket location.
except ResponseError as err:
    print(err)
```



#### presigned_put_object(bucket_name, object_name, expires=timedelta(days=7))

生成一个用于HTTP PUT操作的presigned URL。浏览器/移动客户端可以在即使存储桶为私有的情况下也可以通过这个URL进行上传。这个presigned URL可以有一个过期时间，默认是7天。

注意：你可以通过只指定对象名称上传到S3。

参数

| 参数          | 类型                | 描述                            |
| :------------ | :------------------ | :------------------------------ |
| `bucket_name` | *string*            | 存储桶名称。                    |
| `object_name` | *string*            | 对象名称。                      |
| `expiry`      | *datetime.datetime* | 过期时间，单位是秒，默认是7天。 |

**示例**

```
Copyfrom datetime import timedelta

# presigned Put object URL for an object name, expires in 3 days.
try:
    print(minioClient.presigned_put_object('mybucket',
                                      'myobject',
                                      expires=timedelta(days=3)))
# Response error is still possible since internally presigned does get
# bucket location.
except ResponseError as err:
    print(err)
```



#### presigned_post_policy(PostPolicy)

允许给POST操作的presigned URL设置策略条件。这些策略包括比如，接收对象上传的存储桶名称，名称前缀，过期策略。

创建policy：

```
Copyfrom datetime import datetime, timedelta

from minio import PostPolicy
post_policy = PostPolicy()

# Apply upload policy restrictions:

# set bucket name location for uploads.
post_policy.set_bucket_name('mybucket')
# set key prefix for all incoming uploads.
post_policy.set_key_startswith('myobject')
# set content length for incoming uploads.
post_policy.set_content_length_range(10, 1024)
# set content-type to allow only text
post_policy.set_content_type('text/plain')

# set expiry 10 days into future.
expires_date = datetime.utcnow()+timedelta(days=10)
post_policy.set_expires(expires_date)
```

获得POST表单的键值对形式的对象：

```
Copytry:
    signed_form_data = minioClient.presigned_post_policy(post_policy)
except ResponseError as err:
    print(err)
```

使用`curl`POST你的数据：

```
Copycurl_str = 'curl -X POST {0}'.format(signed_form_data[0])
curl_cmd = [curl_str]
for field in signed_form_data[1]:
    curl_cmd.append('-F {0}={1}'.format(field, signed_form_data[1][field]))

# print curl command to upload files.
curl_cmd.append('-F file=@<FILE>')
print(' '.join(curl_cmd))
```

## 引用

### MinIO Client 命令

> **`ls`命令 - 列出对象**
>
> `ls`命令列出文件、对象和存储桶。使用`--incomplete` flag可列出未完整拷贝的内容。
>
> ```
> Copy用法：
> mc ls [FLAGS] TARGET [TARGET ...]
> 
> FLAGS:
> --help, -h                       显示帮助。
> --recursive, -r          递归。
> --incomplete, -I         列出未完整上传的对象。
> ```
>
> *示例： 列出所有[https://play.min.io上的存储桶。](https://play.min.xn--io-rv2c26h33ty9sspw./)*
>
> ```
> Copymc ls play
> [2016-04-08 03:56:14 IST]     0B albums/
> [2016-04-04 16:11:45 IST]     0B backup/
> [2016-04-01 20:10:53 IST]     0B deebucket/
> [2016-03-28 21:53:49 IST]     0B guestbucket/
> [2016-04-08 20:58:18 IST]     0B mybucket/
> ```
>
> 
>
> **`mb`命令 - 创建存储桶**
>
> `mb`命令在对象存储上创建一个新的存储桶。在文件系统，它就和`mkdir -p`命令是一样的。存储桶相当于文件系统中的磁盘或挂载点，不应视为文件夹。MinIO对每个用户创建的存储桶数量没有限制。
> 在Amazon S3上，每个帐户被限制为100个存储桶。有关更多信息，请参阅[S3上的存储桶限制和限制](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) 。
>
> ```
> Copy用法：
> mc mb [FLAGS] TARGET [TARGET...]
> 
> FLAGS:
> --help, -h                       显示帮助。
> --region "us-east-1"         指定存储桶的region，默认是‘us-east-1’.
> ```
>
> *示例：在[https://play.min.io上创建一个名叫"mybucket"的存储桶。](https://play.min.xn--io"mybucket"-wl9yef8xg31bemhzvklwax41l07sim0dskxe./)*
>
> ```
> Copymc mb play/mybucket
> Bucket created successfully ‘play/mybucket’.
> ```
>
> 
>
> **`cat`命令 - 合并对象**
>
> `cat`命令将一个文件或者对象的内容合并到另一个上。你也可以用它将对象的内容输出到stdout。
>
> ```
> Copy用法：
> mc cat [FLAGS] SOURCE [SOURCE...]
> 
> FLAGS:
> --help, -h                       显示帮助。
> ```
>
> *示例： 显示`myobject.txt`文件的内容*
>
> ```
> Copymc cat play/mybucket/myobject.txt
> Hello MinIO!!
> ```
>
> 
>
> **`pipe`命令 - Pipe到对象**
>
> `pipe`命令拷贝stdin里的内容到目标输出，如果没有指定目标输出，则输出到stdout。
>
> ```
> Copy用法：
> mc pipe [FLAGS] [TARGET]
> 
> FLAGS:
> --help, -h                    显示帮助。
> ```
>
> *示例： 将MySQL数据库dump文件输出到Amazon S3。*
>
> ```
> Copymysqldump -u root -p ******* accountsdb | mc pipe s3/sql-backups/backups/accountsdb-oct-9-2015.sql
> ```
>
> 
>
> **`cp`命令 - 拷贝对象**
>
> `cp`命令拷贝一个或多个源文件目标输出。所有到对象存储的拷贝操作都进行了MD4SUM checkSUM校验。可以从故障点恢复中断或失败的复制操作。
>
> ```
> Copy用法：
> mc cp [FLAGS] SOURCE [SOURCE...] TARGET
> 
> FLAGS:
> --help, -h                       显示帮助。
> --recursive, -r          递归拷贝。
> ```
>
> *示例： 拷贝一个文本文件到对象存储。*
>
> ```
> Copymc cp myobject.txt play/mybucket
> myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
> ```
>
> 
>
> **`rm`命令 - 删除存储桶和对象。**
>
> 使用`rm`命令删除文件对象或者存储桶。
>
> ```
> Copy用法：
> mc rm [FLAGS] TARGET [TARGET ...]
> 
> FLAGS:
> --help, -h                       显示帮助。
> --recursive, -r              递归删除。
> --force              强制执行删除操作。
> --prefix             删除批配这个前缀的对象。
> --incomplete, -I      删除未完整上传的对象。
> --fake               模拟一个假的删除操作。
> --stdin              从STDIN中读对象列表。
> --older-than value               删除N天前的对象（默认是0天）。
> ```
>
> *示例： 删除一个对象。*
>
> ```
> Copymc rm play/mybucket/myobject.txt
> Removed ‘play/mybucket/myobject.txt’.
> ```
>
> *示例：删除一个存储桶并递归删除里面所有的内容。由于这个操作太危险了，你必须传`--force`参数指定强制删除。*
>
> ```
> Copymc rm --recursive --force play/myobject
> Removed ‘play/myobject/newfile.txt’.
> Removed 'play/myobject/otherobject.txt’.
> ```
>
> *示例： 从`mybucket`里删除所有未完整上传的对象。*
>
> ```
> Copymc rm  --incomplete --recursive --force play/mybucket
> Removed ‘play/mybucket/mydvd.iso’.
> Removed 'play/mybucket/backup.tgz’.
> ```
>
> *示例： 删除一天前的对象。*
>
> ```
> Copymc rm --force --older-than=1 play/mybucket/oldsongs
> ```
>
> 
>
> **`share`命令 - 共享**
>
> `share`命令安全地授予上传或下载的权限。此访问只是临时的，与远程用户和应用程序共享也是安全的。如果你想授予永久访问权限，你可以看看`mc policy`命令。
>
> 生成的网址中含有编码后的访问认证信息，任何企图篡改URL的行为都会使访问无效。想了解这种机制是如何工作的，请参考[Pre-Signed URL](http://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html)技术。
>
> ```
> Copy用法：
> mc share [FLAGS] COMMAND
> 
> FLAGS:
> --help, -h                       显示帮助。
> 
> COMMANDS:
> download   生成有下载权限的URL。
> upload     生成有上传权限的URL。
> list       列出先前共享的对象和文件夹。
> ```
>
> **子命令`share download` - 共享下载**
>
> `share download`命令生成不需要access key和secret key即可下载的URL，过期参数设置成最大有效期（不大于7天），过期之后权限自动回收。
>
> ```
> Copy用法：
> mc share download [FLAGS] TARGET [TARGET...]
> 
> FLAGS:
> --help, -h                       显示帮助。
> --recursive, -r          递归共享所有对象。
> --expire, -E "168h"          设置过期时限，NN[h|m|s]。
> ```
>
> *示例： 生成一个对一个对象有4小时访问权限的URL。*
>
> ```
> Copy
> mc share download --expire 4h play/mybucket/myobject.txt
> URL: https://play.min.io/mybucket/myobject.txt
> Expire: 0 days 4 hours 0 minutes 0 seconds
> Share: https://play.min.io/mybucket/myobject.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=Q3AM3UQ867SPQQA43P2F%2F20160408%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20160408T182008Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=1527fc8f21a3a7e39ce3c456907a10b389125047adc552bcd86630b9d459b634
> ```
>
> **子命令`share upload` - 共享上传**
>
> `share upload`命令生成不需要access key和secret key即可上传的URL。过期参数设置成最大有效期（不大于7天），过期之后权限自动回收。
> Content-type参数限制只允许上传指定类型的文件。
>
> ```
> Copy用法：
> mc share upload [FLAGS] TARGET [TARGET...]
> 
> FLAGS:
> --help, -h                       显示帮助。
> --recursive, -r              递归共享所有对象。
> --expire, -E "168h"          设置过期时限，NN[h|m|s].
> ```
>
> *示例： 生成一个`curl`命令，赋予上传到`play/mybucket/myotherobject.txt`的权限。*
>
> ```
> Copymc share upload play/mybucket/myotherobject.txt
> URL: https://play.min.io/mybucket/myotherobject.txt
> Expire: 7 days 0 hours 0 minutes 0 seconds
> Share: curl https://play.min.io/mybucket -F x-amz-date=20160408T182356Z -F x-amz-signature=de343934bd0ba38bda0903813b5738f23dde67b4065ea2ec2e4e52f6389e51e1 -F bucket=mybucket -F policy=eyJleHBpcmF0aW9uIjoiMjAxNi0wNC0xNVQxODoyMzo1NS4wMDdaIiwiY29uZGl0aW9ucyI6W1siZXEiLCIkYnVja2V0IiwibXlidWNrZXQiXSxbImVxIiwiJGtleSIsIm15b3RoZXJvYmplY3QudHh0Il0sWyJlcSIsIiR4LWFtei1kYXRlIiwiMjAxNjA0MDhUMTgyMzU2WiJdLFsiZXEiLCIkeC1hbXotYWxnb3JpdGhtIiwiQVdTNC1ITUFDLVNIQTI1NiJdLFsiZXEiLCIkeC1hbXotY3JlZGVudGlhbCIsIlEzQU0zVVE4NjdTUFFRQTQzUDJGLzIwMTYwNDA4L3VzLWVhc3QtMS9zMy9hd3M0X3JlcXVlc3QiXV19 -F x-amz-algorithm=AWS4-HMAC-SHA256 -F x-amz-credential=Q3AM3UQ867SPQQA43P2F/20160408/us-east-1/s3/aws4_request -F key=myotherobject.txt -F file=@<FILE>
> ```
>
> **子命令`share list` - 列出之前的共享**
>
> `share list`列出没未过期的共享URL。
>
> ```
> Copy用法：
> mc share list COMMAND
> 
> COMMAND:
> upload:   列出先前共享的有上传权限的URL。
> download: 列出先前共享的有下载权限的URL。
> ```
>
> 
>
> **`mirror`命令 - 存储桶镜像**
>
> `mirror`命令和`rsync`类似，只不过它是在文件系统和对象存储之间做同步。
>
> ```
> Copy用法：
> mc mirror [FLAGS] SOURCE TARGET
> 
> FLAGS:
> --help, -h                       显示帮助。
> --force              强制覆盖已经存在的目标。
> --fake               模拟一个假的操作。
> --watch, -w                      监听改变并执行镜像操作。
> --remove             删除目标上的外部的文件。
> ```
>
> *示例： 将一个本地文件夹镜像到[https://play.min.io上的'mybucket'存储桶。](https://play.min.xn--io'mybucket'-rq9y885ai94c522clx5c./)*
>
> ```
> Copymc mirror localdir/ play/mybucket
> localdir/b.txt:  40 B / 40 B  ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃  100.00 % 73 B/s 0
> ```
>
> *示例： 持续监听本地文件夹修改并镜像到[https://play.min.io上的'mybucket'存储桶。](https://play.min.xn--io'mybucket'-rq9y885ai94c522clx5c./)*
>
> ```
> Copymc mirror -w localdir play/mybucket
> localdir/new.txt:  10 MB / 10 MB  ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃  100.00 % 1 MB/s 15s
> ```
>
> 
>
> **`find`命令 - 查找文件和对象**
>
> `find`命令通过指定参数查找文件，它只列出满足条件的数据。
>
> ```
> Copy用法：
> mc find PATH [FLAGS]
> 
> FLAGS:
> --help, -h                       显示帮助。
> --exec value                     为每个匹配对象生成一个外部进程（请参阅FORMAT）
> --name value                     查找匹配通配符模式的对象。
> ...
> ...
> ```
>
> *示例： 持续从s3存储桶中查找所有jpeg图像，并复制到minio "play/bucket"存储桶*
>
> ```
> Copymc find s3/bucket --name "*.jpg" --watch --exec "mc cp {} play/bucket"
> ```
>
> 
>
> **`diff`命令 - 显示差异**
>
> `diff`命令计算两个目录之间的差异。它只列出缺少的或者大小不同的内容。
>
> 它*不*比较内容，所以可能的是，名称相同，大小相同但内容不同的对象没有被检测到。这样，它可以在不同站点或者大量数据的情况下快速比较。
>
> ```
> Copy用法：
> mc diff [FLAGS] FIRST SECOND
> 
> FLAGS:
> --help, -h                       显示帮助。
> ```
>
> *示例： 比较一个本地文件夹和一个远程对象存储服务*
>
> ```
> Copy mc diff localdir play/mybucket
> ‘localdir/notes.txt’ and ‘https://play.min.io/mybucket/notes.txt’ - only in first.
> ```
>
> 
>
> **`watch`命令 - 监听文件和对象存储事件。**
>
> `watch`命令提供了一种方便监听对象存储和文件系统上不同类型事件的方式。
>
> ```
> Copy用法：
> mc watch [FLAGS] PATH
> 
> FLAGS:
> --events value                   过滤不同类型的事件，默认是所有类型的事件 (默认： "put,delete,get")
> --prefix value                   基于前缀过滤事件。
> --suffix value                   基于后缀过滤事件。
> --recursive                      递归方式监听事件。
> --help, -h                       显示帮助。
> ```
>
> *示例： 监听对象存储的所有事件*
>
> ```
> Copymc watch play/testbucket
> [2016-08-18T00:51:29.735Z] 2.7KiB ObjectCreated https://play.min.io/testbucket/CONTRIBUTING.md
> [2016-08-18T00:51:29.780Z]  1009B ObjectCreated https://play.min.io/testbucket/MAINTAINERS.md
> [2016-08-18T00:51:29.839Z] 6.9KiB ObjectCreated https://play.min.io/testbucket/README.md
> ```
>
> *示例： 监听本地文件夹的所有事件*
>
> ```
> Copymc watch ~/Photos
> [2016-08-17T17:54:19.565Z] 3.7MiB ObjectCreated /home/minio/Downloads/tmp/5467026530_a8611b53f9_o.jpg
> [2016-08-17T17:54:19.565Z] 3.7MiB ObjectCreated /home/minio/Downloads/tmp/5467026530_a8611b53f9_o.jpg
> ...
> [2016-08-17T17:54:19.565Z] 7.5MiB ObjectCreated /home/minio/Downloads/tmp/8771468997_89b762d104_o.jpg
> ```
>
> 
>
> **`events`命令 - 管理存储桶事件通知。**
>
> `events`提供了一种方便的配置存储桶的各种类型事件通知的方式。MinIO事件通知可以配置成使用 AMQP，Redis，ElasticSearch，NATS和PostgreSQL服务。MinIO configuration提供了如何配置的更多细节。
>
> ```
> Copy用法：
> mc events COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]
> 
> COMMANDS:
> add     添加一个新的存储桶通知。
> remove  删除一个存储桶通知。使用'--force'可以删除所有存储桶通知。
> list    列出存储桶通知。
> 
> FLAGS:
> --help, -h                       显示帮助。
> ```
>
> *示例： 列出所有存储桶通知。*
>
> ```
> Copymc events list play/andoria
> MyTopic        arn:minio:sns:us-east-1:1:TestTopic    s3:ObjectCreated:*,s3:ObjectRemoved:*   suffix:.jpg
> ```
>
> *示例： 添加一个新的'sqs'通知，仅接收ObjectCreated事件。*
>
> ```
> Copymc events add play/andoria arn:minio:sqs:us-east-1:1:your-queue --events put
> ```
>
> *示例： 添加一个带有过滤器的'sqs'通知。*
>
> 给`sqs`通知添加`prefix`和`suffix`过滤规则。
>
> ```
> Copymc events add play/andoria arn:minio:sqs:us-east-1:1:your-queue --prefix photos/ --suffix .jpg
> ```
>
> *示例： 删除一个'sqs'通知*
>
> ```
> Copymc events remove play/andoria arn:minio:sqs:us-east-1:1:your-queue
> ```
>
> 
>
> **`policy`命令 - 管理存储桶策略**
>
> 管理匿名访问存储桶和其内部内容的策略。
>
> ```
> Copy用法：
> mc policy [FLAGS] PERMISSION TARGET
> mc policy [FLAGS] TARGET
> mc policy list [FLAGS] TARGET
> 
> PERMISSION:
> Allowed policies are: [none, download, upload, public].
> 
> FLAGS:
> --help, -h                       显示帮助。
> ```
>
> *示例： 显示当前匿名存储桶策略*
>
> 显示当前`mybucket/myphotos/2020/`子文件夹的匿名策略。
>
> ```
> Copymc policy play/mybucket/myphotos/2020/
> Access permission for ‘play/mybucket/myphotos/2020/’ is ‘none’
> ```
>
> *示例：设置可下载的匿名存储桶策略。*
>
> 设置`mybucket/myphotos/2020/`子文件夹可匿名下载的策略。现在，这个文件夹下的对象可被公开访问。比如：`mybucket/myphotos/2020/yourobjectname`可通过这个URL https://play.min.io/mybucket/myphotos/2020/yourobjectname访问。
>
> ```
> Copymc policy set download play/mybucket/myphotos/2020/
> Access permission for ‘play/mybucket/myphotos/2020/’ is set to 'download'
> ```
>
> *示例：删除当前的匿名存储桶策略*
>
> 删除所有*mybucket/myphotos/2020/*这个子文件夹下的匿名存储桶策略。
>
> ```
> Copymc policy set none play/mybucket/myphotos/2020/
> Access permission for ‘play/mybucket/myphotos/2020/’ is set to 'none'
> ```
>
> 
>
> **`config`命令 - 管理配置文件**
>
> `config host`命令提供了一个方便地管理`~/.mc/config.json`配置文件中的主机信息的方式，你也可以用文本编辑器手动修改这个配置文件。
>
> ```
> Copy用法：
> mc config host COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]
> 
> COMMANDS:
> add, a      添加一个新的主机到配置文件。
> remove, rm  从配置文件中删除一个主机。
> list, ls    列出配置文件中的主机。
> 
> FLAGS:
> --help, -h                       显示帮助。
> ```
>
> *示例： 管理配置文件*
>
> 添加MinIO服务的access和secret key到配置文件，注意，shell的history特性可能会记录这些信息，从而带来安全隐患。在`bash` shell,使用`set -o`和`set +o`来关闭和开启history特性。
>
> ```
> Copyset +o history
> mc config host add myminio http://localhost:9000 OMQAGGOL63D7UNVQFY8X GcY5RHNmnEWvD/1QxD3spEIGj+Vt9L7eHaAaBTkJ
> set -o history
> ```
>
> 
>
> **`update`命令 - 软件更新**
>
> 从[https://dl.min.io](https://dl.min.io/)检查软件更新。Experimental标志会检查unstable实验性的版本，通常用作测试用途。
>
> ```
> Copy用法：
> mc update [FLAGS]
> 
> FLAGS:
> --quiet, -q  关闭控制台输出。
> --json       使用JSON格式输出。
> --help, -h   显示帮助。
> ```
>
> *示例： 检查更新*
>
> ```
> Copymc update
> You are already running the most recent version of ‘mc’.
> ```
>
> 
>
> **`version`命令 - 显示版本信息**
>
> 显示当前安装的`mc`版本。
>
> ```
> Copy用法：
> mc version [FLAGS]
> 
> FLAGS:
> --quiet, -q  关闭控制台输出。
> --json       使用JSON格式输出。
> --help, -h   显示帮助。
> ```
>
> *示例： 输出mc版本。*
>
> ```
> Copymc version
> Version: 2016-04-01T00:22:11Z
> Release-tag: RELEASE.2016-04-01T00-22-11Z
> Commit-id: 12adf3be326f5b6610cdd1438f72dfd861597fce
> ```

## 单例类


