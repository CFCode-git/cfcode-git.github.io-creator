---
title: "Axios使用手册"
date: 2019-12-03T18:45:06+08:00
draft: false
---

参考文献：  
[axios中文网](http://www.axios-js.com/zh-cn/docs/)  
[axios英文原文](https://kapeli.com/cheat_sheets/Axios.docset/Contents/Resources/Documents/index) 

# GET 请求

~~~JavaScript
/*为给定 ID 的 User 创建请求*/
axios.get('/user?ID = 12345')
    .then(function(response){
        console.log(response);
    })
    .catch(function(error){
        console.log(error);
    })

/*可以通过对象传递 User 的 ID*/
axios.get('/user',{
    params:{
        ID:12345
    }
})
    .then(function(response){
        console.log(response);
    })
    .catch(function(error){
        console.log(error);
    })
~~~

# POST 请求

~~~JavaScript
/*执行post请求*/
axios.post('user',{
    firstName:'Fred',
    lastName:'Flintstone'
})
    .then(function(response){
        console.log(response);
    })
    .catch(function(error){
        console.log(error);
    })
~~~

# 执行多个并发请求

~~~JavaScript
function getUserAccount(){
    return axios.get('/user/12345');
}

function getUserPermissions(){
    return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(),getUserPermissions()])
    .then(axios.spread(function(acct,perms){
        // 两个请求现在都执行完成
    }));
~~~

# 创建请求

~~~JavaScript
/*发送POST请求*/
axios({
    method:'post',
    url:'/user/12345',
    data:{
        firstName:'Fred',
        lastName:'Flintstone'
    }
});


/*获取远端照片*/
axios({
    method:'get',
    url:'http://bit.ly/2mTM3nY',
    responseType:'stream'
})
    .then(function(response){
        response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
    });


~~~

# 创建实例

~~~JavaScript
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
~~~

# 响应

~~~JavaScript
axios.get('/user/12345')
  .then(function(response) {
    console.log(response.data); // 由服务器提供的响应
    console.log(response.status); // 来自服务器响应的HTTP状态码
    console.log(response.statusText); // 来自服务器响应的状态信息
    console.log(response.headers); // 服务器响应的头
    console.log(response.config); // 为请求提供的配置信息
  });
~~~

# 配置默认值

~~~JavaScript
/*全局的 axios 默认值*/
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';

/*自定义实例默认值*/
// 创建实例时设置默认值
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 创建实例后设置默认值。
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;


/*配置的优先顺序*/
// 配置会以一个优先顺序进行合并。这个顺序是：
// 在 lib/defaults.js 找到的库的默认值，
// 然后是实例的 defaults 属性，
// 最后是请求的 config 参数。

// 使用由库提供的配置的默认值来创建实例
// 此时超时配置的默认值是 `0`
var instance = axios.create();

// 覆写库的超时默认值
// 现在，在超时前，所有请求都会等待 2.5 秒
instance.defaults.timeout = 2500;

// 为已知需要花费很长时间的请求覆写超时设置
instance.get('/longRequest', {
  timeout: 5000
});
~~~

# 拦截器

在请求和响应被 then 或 catch 处理前拦截他们。

~~~JavaScript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });

// 移除拦截器
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);

// 为自定义 axios 实例添加拦截器
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
~~~

# 错误处理

~~~JavaScript
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 已经创建了一个请求，并且服务器返回了一个状态码
      // 状态码不是 2xx
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 创建了一个请求，但是服务器没有返回响应。
      // `error.request` is an instance of XMLHttpRequest in 
      // the browser and an instance of http.ClientRequest in node.js
      console.log(error.request);
    } else {
      // 在请求配置过程中发生了某些问题触发了错误
      console.log('Error', error.message);
    }
    console.log(error.config);
});
~~~

~~~JavaScript
// 使用 validateStatus 配置选项自定义 HTTP 状态码的错误范围。
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; 
    // Reject only if the status code is greater than or equal to 500
    // 只有当状态码大于等于 500 的时候才返回错误
  }
})
~~~

# 取消请求

使用 cancel token 取消请求

~~~JavaScript
/*使用 CancelToken.source 方法创建 cancel token*/
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
     // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
~~~

~~~JavaScript
/*可以通过传递一个 executor 函数到 CancelToken 的构造函数来创建 cancel token：*/
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// cancel the request
cancel();
~~~