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

# String

value 为字符串类型是最基本的类型，一个value不能超过512mb.

```js
127.0.0.1:6379> set key value [EX seconds|PX milliseconds|EXAT timestamp|PXAT milliseconds-timestamp|KEEPTTL] [NX|XX] [GET]
```

使用Set、Get设置和获取一个简单的key user:qtc:likes,value为a-b-c-d

```js
127.0.0.1:6379> set user:qtc:likes a-b-c-d
OK
```

```js
127.0.0.1:6379> get user:qtc:likes
"a-b-c-d"
```

Set命令有一些有趣的参数，比如 nx、xx。

nx代表如果当前key存在，那么我希望写入失败。

xx代表如果当前key存在，那么我希望覆盖写入成功。

默认情况下，Set命令执行的是xx，也就是覆盖。

```js
127.0.0.1:6379> get user:qtc:likes
"a-b-c-d"
127.0.0.1:6379> set user:qtc:likes e-f-g
OK
127.0.0.1:6379> set user:qtc:likes 123456 nx
(nil)
127.0.0.1:6379> set user:qtc:likes 123456 xx
OK

```

虽然string是最基本的类型，但是我们仍然可以执行一些有趣的操作。比如递增，递减。

incr、decr、decrby、incrby命令，都是原子性的，也就是多个客户端同时发出递增或者递减命令时，不会进入竞速

```js

127.0.0.1:6379> set user:qtc:likes 10 xx
OK
127.0.0.1:6379> incr user:qtc:likes
(integer) 11
127.0.0.1:6379> incrby user:qtc:likes 50
(integer) 61
127.0.0.1:6379> decr user:qtc:likes
(integer) 60

```



