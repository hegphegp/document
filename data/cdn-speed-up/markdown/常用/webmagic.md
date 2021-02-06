## webmagic 介绍

### 1. webmagic注意点
#### 1.1 常用参数和方法的介绍
* Site.setUserAgent() 方法，在程序运行后，实时改变setUserAgent的取值，发送请求的userAgent参数不会变
* Site.addCookie() 方法，在程序运行后，实时使用addCookie方法修改和添加cookie，发送请求的cookie参数不会变
* Site.addHeader() 方法，在程序运行后，实时使用addHeader方法修改和添加Header，发送请求的header参数会实时变化
* Spider.addUrl() 方法，一般用于首次爬取的url集合，会把添加的url都保存到scheduler里面
* Spider.setCycleRetryTimes()和Spider.setRetrySleepTime()作用于接收html的内容失败时才会触发，如果httpStatus返回非200，但是html内容被正常接收，不会触发重新请求
* 少使用Spider.setAcceptStatCode(new HashSet())方法，每次接收的httpStatus非200时，若需要，手动重新官方Spider类的onDownloadSuccess()方法中关于非200状态的逻辑
* 多次连续出现请求的httpStatus非200时，若scheduler里面的url非空，还会继续爬取所有的url

#### 1.2 开发逻辑
* 重写PageProcessor的方法，并在process(Page page)方法保存数据库数据