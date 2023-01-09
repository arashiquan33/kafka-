# Keys

redis key有几个原则：
* 不能太长，超过1024bytes不是好的实践，如果非要，可以只要哈希，比如sha1
* 不能太短，比如“u1000flw”,可以使用"user:1000:followers",可读性更高
* 最大允许的key是512mb

# select

select命令可以切换redis数据库，redis单实例默认16个数据库，索引从0-15，当Cluster集群模式下，不能使用select命令指定数据库

```js
127.0.0.1:6379> select 1
```

# Strings

value 为字符串类型是最基本的类型，一个value不能超过512mb.

```js
127.0.0.1:6379> set key value [EX seconds|PX milliseconds|EXAT timestamp|PXAT milliseconds-timestamp|KEEPTTL] [NX|XX] [GET]
```
设置一个简单的key user:qtc:likes,value为a-b-c-d

```js
127.0.0.1:6379> set user:qtc:likes a-b-c-d
OK
```

