---
title: Axios踩坑日记
author: Ned
tags:
  - Axios
  - Vue
  - 踩坑日记
categories:
 - 踩坑日记
 - Vue
abbrlink: 2698809561
---

### 起因

自己写项目的时候，axios向后端发送post请求时，后端无法接收到数据，同样的请求在我使用postman测的时候是正常的，不信邪的我又用原生的form表单提交试了一试，也是正常的，想了想，也查了查百度，觉得可能是form表单与axios请求，有哪里不一样。

#### 去找了axios的介绍

![axios](https://img-blog.csdnimg.cn/20190907170037504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3plbXByb2dyYW0=,size_16,color_FFFFFF,t_70)

由图片可以看出，它大概是对请求和响应的数据进行了一个转换，而且会对json进行自动转换，我去github上找了一下源码：

```javascript
transformRequest: [function transformRequest(data, headers) {
        normalizeHeaderName(headers, 'Accept');
        normalizeHeaderName(headers, 'Content-Type');
        if (utils.isFormData(data) ||
          utils.isArrayBuffer(data) ||
          utils.isBuffer(data) ||
          utils.isStream(data) ||
          utils.isFile(data) ||
          utils.isBlob(data)
        ) {
          return data;
        }
        if (utils.isArrayBufferView(data)) {
          return data.buffer;
        }
        if (utils.isURLSearchParams(data)) {
          setContentTypeIfUnset(headers, 'application/x-www-form-urlencoded;charset=utf-8');
          return data.toString();
        }
        // 看这里------------------------------------------
        if (utils.isObject(data)) {
          setContentTypeIfUnset(headers, 'application/json;charset=utf-8');
          return JSON.stringify(data);
        }
        return data;
      }]
```

<!-- more -->

### 解决思路

当判断为对象时，会把headers设置为application/json;charset=utf-8，也就是Content-Type，而根据后端同学所说的，服务端要求的是Content-Type': 'application/x-www-form-urlencoded，我试着在发送请求前将headers设置为application/x-www-form-urlencoded，结果还是不行，大概是因为源码中对headers的修改在自己的设置之后实现的，但是这样写的话，要写出一串很长的字符串，感觉挺麻烦的，那就可以尝试下面的方法

```javascript
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'
 axios.interceptors.request.use((config) => {
 if (config.method === 'post') {
    config.data = qs.stringify(config.data)
  }
  return config
})
```

这样会在你post请求的时候，加上这个头，并调用了qs库，将你请求的数据转换为字符串，但是可能有问题，这段代码是要加在main.js中的，那么我项目中想使用其他的请求头，或者转换为字符串会导致我其他的请求会有bug，应该如何去解决呢？

```javascript
axios.interceptors.request.use((config) => {
  const isFormData = config.data instanceof FormData
  if (isFormData) {
    config.headers['content-type'] = 'multipart/form-data'
  } else if (config.method === 'post') {
    config.data = qs.stringify(config.data)
  }
  return config
})
```

这是我在项目中，有一处请求对象需要是formData对象的时候，伙伴对上边代码做出的修改，避免了所有的请求都会被强制转换成字符串的问题。

### 另外

我百度查阅了一下，csdn上有一博主是这样做的，他在请求的时候修改了axios源码中的transformRequest方法，把对data的处理变成了自己想要的样子，我觉得这样也很好，但是我当初没想到emm
附上该博主的链接：https://blog.csdn.net/zemprogram/article/details/100599613
附上本小白项目的链接，入门菜鸟，请多指教：https://gitee.com/wang-enze/rank