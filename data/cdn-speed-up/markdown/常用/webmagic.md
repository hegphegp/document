## webmagic 介绍

### 1. webmagic注意点
#### 1.1 webmagic注意点
* 放弃了pipeline保存数据，直接在PageProcessor的process()保存数据，同时使用page.setSkip(true)忽略pipeline
* Site.setUserAgent() 方法，在程序运行后，实时改变setUserAgent的取值，发送请求的userAgent参数不会变
* Site.addCookie() 方法，在程序运行后，实时使用addCookie方法修改和添加cookie，发送请求的cookie参数不会变
* Site.addHeader() 方法，在程序运行后，实时使用addHeader方法修改和添加Header，发送请求的header参数会实时变化
* Spider.addUrl() 方法，一般用于首次爬取的url集合，会把添加的url都保存到scheduler里面
* 多次连续出现请求的httpStatus非200时，若scheduler里面的url非空，还会继续爬取所有的url
* 重写onDownloadSuccess的非200的httpStatus逻辑，重试请求，官方默认的方法有bug，非200的httpStatus不会重试请求
* POST,PUT,DELETE不是幂等操作，webmagic默认不去重，要自己继承DuplicateRemovedScheduler，重写push方法
* 服务返回的Cookie，webmagic会自动带上(可能是httpclient自动带上)，不需要手动写业务代码配置


#### 1.2 开发逻辑
* 在PageProcessor的process(Page page)方法保存数据库数据
* 在PageProcessor配置要爬取的URL的Pattern


#### 1.3 的redis注意事项
* jedis.hmset(key, new HashMap()); 初始化一个空的map到redis，redis会抛错 ERR wrong number of arguments for 'hmset' command
* 不管redis是否存在某个key的map，直接使用 jedis.hget("map的key", "item项的key") 不抛错误
* 不管redis是否存在某个key的map，直接给某个map的key赋值不抛错误，jedis.hset("map的key", "item的key", "value");


#### 1.4 hash数据类型增删查改，事先都不需要提前在redis创建map结构
* 增      jedis.hset(key, field, value)  给某个map结构增加一个key-value
* 删      jedis.hdel(key, fields...)     删除某个map结构的key
* 查      jedis.hget(key, field)         查map的单个key的value
* 查多个   jedis.hmget(key, fields...)    查map的多个key的value
* 查所有   jedis.hgetAll(key)             查map的所有key-value
* 是否存在 jedis.hexists(key, field)      判断map的field是否存在


#### 1.5 set数据结构类型增删查改，事先都不需要提前在redis创建set结构
* 增          jedis.sadd(key, members...)
* 查所有       jedis.smembers(key)
* 是否存在     jedis.sismember(key, member)
* 删除多个key  jedis.srem(key, items...)
* 查数量       jedis.scard(key)


#### 1.6 list列表数据结构类型增删查改，事先都不需要提前在redis创建list结构
* 在head新增   jedis.lpush(key, fields...)
* 在tail新增   jedis.rpush(key, fields...)
* 查指定下标   jedis.lrange(key, start, end)
* 删除多个key  jedis.lrem(key, count, field)
* 查所有      jedis.lrange(key, 0, -1);

