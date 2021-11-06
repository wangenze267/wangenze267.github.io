---
title: 恭喜你，Node后端学习经验值+1
author: Ned
tag:
  - Node.js
abbrlink: 3366338322
---
## 前言
这次整理了一些写node中遇到的知识点呀，查缺补漏，才能让自己成长的更稳哦！
>最近有在参加蓝桥杯web方向的模拟赛哦，来抓一波也在参加的小伙伴~
## 感想

遇到很多都是模块化的写法，之前自己写的时候确实这方面不是很注重，因为自己写自己看，当然怎么舒服怎么来了，它提醒我要**优雅一点~**，但是确实，这么写之后香的一批~

## 一些工具知识
### Koa初始化

> 1.1 全局安装脚手架工具

```
cnpm install -g koa-generator 
# or 
yarn global add koa-generator 
```

> 1.2 进入到项目文件夹目录,执行生成命令

```
# koa2+项目名
koa2 manager-server
```


> 1.3 安装依赖

```
npm install 
# or
cnpm install
# or
yarn
```
### 使用pm2部署Koa项目并实现启动、关闭、自动重启

**1. 全局安装**

```
npm install -g pm2
```

**2. 启动项目**

> 进入项目目录，然后使用pm2启动项目。这里要特别注意：启动**单文件**时用（app.js是项目文件名）

```
pm2 start app.js       #启动单文件
```

> 但是在koa2中需要这样启动：

```
pm2 start ./bin/www #启动koa2项目
```

**3. pm2自动重启**

> 把pm2的服务先停下,然后起来的时候带上–watch就可以了

```
pm2 start ./bin/www --watch
```

**4. pm2相关命令(www是项目名)**

```
pm2 list           #查看所用已启动项目
pm2 start          #启动
pm2 restart www    #重启
pm2 stop www       #停止
pm2 delete www     #删除
```

## 以下为封装的两个工具函数
### 利用log4js封装日志输出
首先在`utils`文件夹中建好你的`log4j.js`文件，引入**log4js**
```
    const log4js = require('log4js')
```
下面是定义的level等级，截止到目前只用到了三种，但是教程里定义的确实是很多，可能是我没用到。
```
const levels = {
    'trace':log4js.levels.TRACE,
    'debug':log4js.levels.DEBUG,
    'info':log4js.levels.INFO,
    'warn':log4js.levels.WARN,
    'error':log4js.levels.ERROR,
    'fatal':log4js.levels.FATAL,
}
```
接下来就是要用**log4js.configure**去添加一些配置。
> appenders: 追加器
```
    appenders:{
        console:{ type: 'console' },
        info:{
            type:'file',
            filename:'logs/all-logs.log'
        },
        error:{
            type: 'dateFile',
            filename:'logs/log',
            pattern:'yyyy-MM-dd.log',
            alwaysIncludePattern:true // 设置文件名称是filename+pattern
        }
    },
```
**可以看到我们在追加器上，让info和error的日志输出都存到文件里，方便我们查阅和记录，其中error的日志我们用当天的年月日来作为记录，info的日志我们用一个文件来存储就好**
> categories: 输出种类
```
    categories:{
        default: { appenders: ['console'], level: 'debug' },
        info:{
            appenders: ['info','console'],
            level: 'info'
        },
        error:{
            appenders: ['error','console'],
            level: 'error'
        }
    }
```
**default默认，这个单词都很熟悉了，默认输出level就是debug，我们自己用来打印东西用的一个输出等级**

**我们要注意一下，此时appenders里的我们只能添加上面appenders里定义好的东西。**

下面是三个函数，分别为debug、error、info，将他们导出出去方便使用：
```
exports.debug = (content) =>{
    let logger = log4js.getLogger()
    logger.level = levels.debug
    logger.debug(content)
}

exports.error = (content) =>{
    let logger = log4js.getLogger('error')
    logger.level = levels.error
    logger.error(content)
}

 exports.info = (content) =>{
    let logger = log4js.getLogger('info')
    logger.level = levels.info
    logger.info(content)
}
```
随后我们去`app.js`里将原本的日志输出换成我们写好的。位置大约一个在logger那，一个在error那。分别换上我们的info和error就行啦。
> 记得先引入log4js！
### 封装工具函数
**工具函数目前具有的功能：**
- 成功或者失败返回状态码
- 初步的分页结构
先来定义我们的状态码：
```
const CODE = {
    SUCCESS:200,
    PARAM_ERROR:10001, //参数错误
    USER_ACCOUNT_ERROR:20001, //账号或者密码错误
    USER_LOGIN_ERROR:30001, //用户未登录
    BUSINESS_ERROR:40001, //业务请求失败
    AUTH_ERROR:50001, //认证失败或者TOKEN过期
}
```
> 下一个肯定是60001，别猜，我说哒！
> 下面函数均用`module.exports`导出:
```
   success(data='',msg='',code=CODE.SUCCESS){
        log4js.debug(data);
        return{
            code,data,msg
        }
    },
    fail(msg='',code=CODE.ERROR){
        log4js.debug(msg);
        return{
            code,data,msg
        }
    }
```
我这里特意将之前日志输出的函数引入过来，在此打印方便我们调试，如果成功的话，应该重点看返回的数据，如果失败的话，我们肯定不返回数据，重点要看当时的**msg**，查明失败原因。
```
    pager({pageNum=1,pageSize=10}){
        pageNum*=1;
        pageSize*=1;
        const skipIndex = (pageNum-1)*pageSize;
        return{
            page:{
                pageNum,
                pageSize
            },
            skipIndex
        }
    },
```
这个函数是分页结构的函数，默认一页是十条数据，利用算数将pageNum和pageSize转换成number类型。

**skipIndex是我们用来查索引的，这里来解释一下：一页十条数据，假如我们现在是第三页，(3-1) * 10=20,所以我们下一个索引就从20开始查起就好了。**

## 写在最后
>1024马上就到了呀，祝愿天下的程序员最近的bug都少一点，头发都多一点！
