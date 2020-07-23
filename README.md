# iproxy
HTTP/HTTPS proxy server by golang [high performance version]

## 快速上手

```bash
$ ./iproxy_darwin_amd64 -t 1024
2020/07/23 13:42:07 Server running: 0.0.0.0:8080 , task_id: 1024
```

## 编辑 config.json
```json

// host 代理监听 IP
// port 代理监听 端口
// database   数据库连接信息
// datatable  数据存储的表

{
    "host": "0.0.0.0", 
    "port": "8080",    
    "database": "root:123456@tcp(localhost:3306)/test", 
    "datatable": "capture",
}
```

## 启动服务

-t 可以为每一个扫描任务指定一个id，来区分不同的任务

```bash
./iproxy_darwin_amd64 -t 1024
```

```
./iproxy_darwin_amd64 -h
Usage of ./iproxy_darwin_amd64:
  -t int
    	inspector task (task_id)
  -v	should every proxy request be logged to stdout
```

## 安装mysql环境

```bash
docker pull mysql
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

## 初始化数据库

```bash
mysql -h127.0.0.1 -uroot -p
mysql> CREATE DATABASE IF NOT EXISTS test;
mysql> use test;
mysql> source capture.sql
```

如果不想使用source初始化数据表，可以直接复制SQL在mysql里面执行；

```sql
CREATE TABLE IF NOT EXISTS `capture` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `task_id` int DEFAULT '0',
  `method` varchar(8) DEFAULT 'GET',
  `scheme` varchar(16) DEFAULT 'http',
  `host` varchar(220) DEFAULT NULL,
  `uri` text,
  `path` text,
  `status_code` int DEFAULT '0',
  `content_type` varchar(128) DEFAULT NULL,
  `header` text,
  `content` mediumblob,
  `request_header` text,
  `request_body` mediumblob,
  `created_at` datetime DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=0;
```




