---
title: Vue3旅途中的一些小知识
author: Ned
tags:
  - Vue
abbrlink: 809095630
date: 2021-10-17 22:42:19
---

## 前言

最近在学习Vue3跟用node koa写后端，此篇文章是记录一些平时学习中的小知识点，未经整理，以后或许会整理一下。

> 很乱，但有很多都是当时最直接的想法
>
> 整理后也会在此挂上新链接的
>
> 还会一直更新

## 路由跳转的三种方式

### router-link

```vue
<router-link to"/login">去登陆</router-link>
```

### 传统跳转（options API）

```Vue
<template>
	<el-button @click="goLogin">去登陆</el-button>
</template>
<script>
	export default{
        name:'home',
        methods:{
            goLogin(){
				this.$router.push('/login')
            }
        }
    }
</script>
```

### Composition API跳转

```Vue
<script setup>
import { useRouter } from 'vue-router'
let router = userRouter()
const goLogin = ()=>{
    router.push('/login')
}
</script>
```

**注意：setup是钩子函数，如果像第三个例子写的话，里面的函数都属于钩子范畴，可以像第二种方式那样，setup类似于methods，完后将它return出去应该也是可以的。**

### 下面为上面注意中，方法的实现代码

**大体上还是options API的写法，只是setup的思想属于composition API范畴，应该属于框架迁移过程中的写法吧**

```Vue
<template>
  <el-button @click="goLogin">去登陆</el-button>
</template>

<script>
import { useRouter } from 'vue-router'
export default{
  setup(){
    let router = useRouter()
    function goLogin(){
      router.push('./login')
    }
    return { goLogin }
  }
}
</script>
```

## 封装两种习惯使用request

### this.$request({ obj })

将参数用一个对象的方式进行解构。

```javascript
this.$request({
      methods: 'get',
      url: '/login',
      data:{
        name:'Ned'
      }
    }).then((res)=>{
      console.log(res)
    })
```

**封装如下**

```javascript
/**
 * axios二次封装
 */
import axios from 'axios'
import config from './../config'
import router from './../router';
import { ElMessage } from 'element-plus'

const TOKEN_INVALID = 'Token认证失败，请从新登陆'
const NETWORK_ERROR = '网络请求异常，请稍后重试'

// 创建axios实例对象，添加全局配置
const service = axios.create({
     baseURL:config.baseApi,
     timeout:8000
})

// 请求拦截器
service.interceptors.request.use((req) => {
    // to do
    const headers = req.headers
    if(!headers.Authorization) headers.Authorization = 'Bear token'
    return req
})

// 响应拦截器
service.interceptors.response.use((res) => {
    // to do
    /**
     * 注意状态码一共有两个
     * 一为http状态码
     * 二为接口返回状态码
     */
    const { code, data, msg } = res.data
    if(code === 200){
        return data
    }else if(code === 40001){
        ElMessage.error(TOKEN_INVALID)
        setTimeout(() => {
            router.push('/login')
        }, 15000);
        return Promise.reject(TOKEN_INVALID)
    }else{
        ElMessage.error(msg || NETWORK_ERROR)
        return Promise.reject(msg || NETWORK_ERROR)
    }
})

/**
 * 请求核心函数
 * @param {*} options  请求配置
 */
function request(options){
    // 判断get/post
    options.method = options.method || 'get'
    // 防止有时候写了GET
    if(options.method.toLowerCase() === 'get'){
        // 如果是get就将data直接赋值给params
        // 类型转换
        options.params = options.data;
    }
    if(config.env === 'prod'){
        service.defaults.baseURL = config.baseApi
    }else{
        service.defaults.baseURL = config.mock ? config.mockApi:config.baseApi
    }
    return service(options)
}

export default request
```



### this.$request.get/post('/api',{ obj })

将get/post用.的方式取出，一个参数是接口路径，另一个参数是数据对象。

```javascript
this.$request.get('/login',{name:Ned}).then((res)=>{
      console.log(res)
    })
```

在上一步的封装上，支持这种习惯

**增加如下：**

```js
['get','post','put','delete','patch'].forEach((item)=>{
    request[item] = (url,data,options) =>{
        return request({
            url,
            data,
            method:item,
            ...options
        })
    }
})
```

 ## 环境配置

### 什么时候用到

开发环境：mock地址

测试环境：测试接口的地址

部署环境：线上接口的地址



### mock的重要性

前后端分离的时候，mock可以帮助前端利用接口文档测试接口。

帮助前端完成开发

### 配置config

默认是生产环境 prod，如果有值就赋值到env变量上，当做当前的环境变量。

最后将EnvConfig以解构的方式导出，方便根据当前环境直接调取两种Api

```js
const env = import.meta.env.MODE || 'prod'
const EnvConfig = {
    dev:{
        baseApi:'',
        mockApi:''
    },
    test:{
        baseApi:'',
        mockApi:''
    },
    prod:{
        baseApi:'',
        mockApi:''
    }
}
export default {
    env:env,
    mock:true,
    ...EnvConfig[env]
}
```

## 关于Storage使用场景

### jwt token

在登陆时候，有一个状态，以及用token做一些权限、由访问权限等

### 跨组件数据共享

> 跨组件数据共享：
>
> ​	Vue中使用Vuex
>
> ​	React中使用Redux

存在问题：页面刷新后数据丢失

原因：Vuex存储数据在js内存中，刷新会销毁内存。

解决方案：Vuex+storage结合去做

### storage存储

storage存储量：4M

cookie存储量：2K-4K

###  封装storage

- setItem()
- getItem()
- clearItem()
- clearAll()

```js

import config from './../config'
export default {
    setItem(key,val){
        let storage = this.getStorage()
        storage[key] = val
        window.localStorage.setItem(config.namespace,JSON.stringify(storage))
    },
    getItem(key){
        return this.getStorage()[key]
    },
    getStorage(){
       return  JSON.parse(window.localStorage.getItem(config.namespace)) || "{}"
    },
    clearItem(key){
        let storage = this.getStorage()
        delete storage[key]
        window.localStorage.setItem(config.namespace,JSON.stringify(storage))
    },
    clearAll(){
        window.localStorage.clear()
    }
}
```

## 使用koa初始化项目

###  koa-generator快速生成koa服务的脚手架工具

> 1.1 全局安装脚手架工具

```shell
cnpm install -g koa-generator 
# or 
yarn global add koa-generator 
```

> 1.2 进入到项目文件夹目录,执行生成命令

```shell
# koa2+项目名
koa2 manager-server
```

> 如果无法使用koa2命令，说明需要配置环境变量，window用户，需要找到koa-generator的安装目录，找到里面bin下面的koa2命令文件，然后配置到环境变量中。mac用户可直接创建软连接，指向到/usr/local/bin中，比如：ln -s /Users/Jack/.config/yarn/global/node_modules/koa-generator/bin/koa2 /usr/local/bin/koa2

> 1.3 安装依赖

```shell
npm install 
# or
cnpm install
# or
yarn
```

## 使用pm2部署Koa项目并实现启动、关闭、自动重启

### **1. 全局安装**

```
npm install -g pm2
```

### **2. 启动项目**

> 进入项目目录，然后使用pm2启动项目。这里要特别注意：启动**单文件**时用（app.js是项目文件名）

```
pm2 start app.js       #启动单文件
```

> 但是在koa2中需要这样启动：

```
pm2 start ./bin/www #启动koa2项目
```

### **3. pm2自动重启**

> 把pm2的服务先停下,然后起来的时候带上–watch就可以了

```
pm2 start ./bin/www --watch
```

### **4. 启动完成，可以访问了**

![pm2启动成功](https://segmentfault.com/img/bVbGHD3)

### **5. pm2相关命令(www是项目名)**

```shell
pm2 list           #查看所用已启动项目
pm2 start          #启动
pm2 restart www    #重启
pm2 stop www       #停止
pm2 delete www     #删除
```

