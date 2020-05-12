---
title: vue封装axios+promise
date: 2020-05-11 10:21:27
tags:
---
## 1.安装axios、qs
``` bash
  npm install axios --save
  npm install qs --save
```
Promise有三种状态:
  1.pending: 等待中，或者进行中，表示还没有得到结果
  2.resolved: 已经完成，表示得到了我们想要的结果，可以继续往下执行
  3.rejected: 也表示得到结果，但是由于结果并非我们所愿，因此拒绝执(用catch捕获异常)

axios特点:
  1.从浏览器中创建 XMLHttpRequests
  2.从 node.js 创建 http 请求
  3.支持 Promise API
  4.拦截请求和响应 （就是有interceptor）
  5.转换请求数据和响应数据
  6.取消请求
  7.自动转换 JSON 数据
  8.客户端支持防御 XSRF

## 2.在src目录下创建api文件夹，然后在下一级创建api.js、http.js
 http.js
 ``` bash
  import axios from 'axios' // 引入axios
  import store from '../store/index' // 引入Vuex
  import router from '../router' // 引入vue-router
  import { Message } from 'element-ui' //局部引入UI框架组件
  if(process.env.NODE_ENV == 'development'){
      axios.defaults.baseURL = "" //开发环境
    }else if(process.env.NODE_ENV == 'debug'){
      axios.defaults.baseURL = "" //调试环境
    }else if(process.env.NODE_ENV == 'production'){
      axios.defaults.baseURL = "" //线上环境
    }

  axios.defaults.timeout = 100000; // 响应超时设置

  //添加请求拦截器
  axios.interceptors.request.use(
    config => {
      if (sessionStorage.getItem('Authorization')) {
        config.headers.Authorization = `Bearer` + " " + sessionStorage.getItem('Authorization'); //查看是否存在token
        return config;
      } else if (config.isUpload) {
        config.headers = { 'Content-Type': 'multipart/form-data'} // 根据参数是否启用form-data方式
        return config;
      } else {
        config.headers = { 'Content-Type': 'application/json' }
        return config;
      },
      error => {
        return Promise.reject(error);
      }
    );
  
  axios.interceptors.response.use(
    response => {
        // 有响应，说明接口请求成功，但查询可能有误
        if (response.status === 1000) {
          //状态码1000代表返回了正确结果
          return Promise.resolve(response);
        } else {
          // 服务器状态码不是1000的的情况
          // 与后台协商统一好的状态码
          /**
           * 用户不存在 = 1001,
              密码错误 = 1002,
              模型状态无效=1003,
              未登录=1004,
              没有数据=1005,
              消息过长=1006,
              发送到终端时失败=1007,
              已存在=1008,
              失败=1009
           */
        let errorMes = ''
        switch (response.status) {
            case 1001:
                errorMes = '用户不存在';
                Message.warning(errorMes);
                break;
            case 1002:
                errorMes = '密码错误';
                Message.warning(errorMes);
                break;
            case 1005:
                errorMes = '暂无数据';
                Message.info(errorMes);
                break;
            case 1004:
                errorMes = '未登录';
                Message.warning(errorMes);
                sessionStorage.clear(); //清除session
                router.push({path: '/', query: {info: ''}}); // 返回首页
                setTimeout(()=>{
                  window.location.reload();
                },500)
                break;
            case 1006:
                errorMes = '消息过长';
                Message.warning(errorMes);
                break;
            case 1008:
                errorMes = '已存在';
                if(response.data.Data){
                    errorMes=response.data.Data
                }
                Message.warning(errorMes);
                break;
            case 1009:
                errorMes = '失败';
                Message.warning(errorMes);
                break;
            case 1099:
                errorMes = '未知错误';
                Message.warning(errorMes);
                break;
            case 1010:
                errorMes = '账号已存在';
                Message.warning(errorMes);
                break;
            case 1012:
                errorMes = '登录已超时';
                Message.warning(errorMes);
                sessionStorage.clear();
                router.push({path: '/', query: {info: 'logout'}});
                setTimeout(()=>{
                  window.location.reload();
                },500)
                break;
            case 1013:
                errorMes = '已离线';
                Message.warning(errorMes);
                break;
            case 1014:
                errorMes = '账户名、身份证号不能重复';
                Message.warning(errorMes);
                break;
            case 1015:
                errorMes = '角色名已存在';
                Message.warning(errorMes);
                break;
            // 其他错误，直接抛出错误提示
            default:
                Message.warning(errorMes);
                break;
        }
        // Message.error(errorMes)
        //登陆出现错误需要重置
            return Promise.reject(errorMes);
        }
    },

  /**
    * @param res返回数据列表
    */
  function handleResults(res){
      return res;
  }
  /**
    * 请求接口url
    */
  function handleUrl(url){
    var url = BASE_URL + url;
    return url;
  }
  /**
    * @param data数据列表
    * @return
    */
  function handleParams(data){
    return data;
  }

  /**
  * get方法，对应get请求
  * @param {String} url [请求的url地址]
  * @param {Object} params [请求时携带的参数]
  * @param {Object} config [请求时url后直接加的参数，默认为空]
  */
  export function get(url, params,config = {add: '' }) {
    return new Promise((resolve, reject) => {
      axios.get(url,{
        params: params
      },config).then(res => {
        resolve(handleResults(res))
      }).catch(err => {
        reject(err.data)
      })
    })
  }
  /**
  * post方法，对应post请求
  * @param {String} url [请求的url地址]
  * @param {Object} params [请求时携带的参数]
  * @param {Object} config [是否启用multipart/form-data格式请求，默认为false]
  */
  export function post(url, params,config = {isUpload: false }) {
    return new Promise((resolve, reject) => {
      axios.post(url, params,config)
        .then(res => {
          resolve(res.data)
        })
        .catch(err => {
          reject(err.data)
        })
    })
  }
```
api.js
  ``` bash
  import {post,get,} from './http' 

  const baseURL = ''
  export default {
    admin: {
      apiGet1: (params) => get(baseURL + '', params),
      apiGet2: (params,q) => get(baseURL + '' + q.add, params), 
      apiPost: (params) => post(baseUrl+ '', params),
      apiPost2: (params) => post(baseUrl+ '', params)
    }
  }
  ```
main.js全局引用...
``` bash
import axios from 'axios';
import api from './api/api';
Vue.prototype.$axios = axios;
Vue.prototype.$api = api;
```

请求:
``` bash
// post
let params = {
  p1: 'xxx',
  p2: 'xxx'
}
this.$api.admin(apiPost,params).then(res =>{

}).catch(err => {

})
// post
let formData = new FormData()
formData.append("name1",value)
formData.append("name2",value)
this.$api.admin(apiPost2,formData,{isUpload: true}).then(res =>{

}).catch(err => {

})

// get-- 也可以不加参数
let params = {
  p1: 'xxx',
  p2: 'xxx'
}
this.$api.admin(apiGet1,params).then(res =>{

}).catch(err => {

})
// get
this.$api.admin(apiGet2,{add:''}).then(res =>{

}).catch(err => {

})
```


  