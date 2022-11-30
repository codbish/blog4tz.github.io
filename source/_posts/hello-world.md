---
title: 在前端页面调用api时的问题
---
1.请求路径利用.env.development文件去实现url的统一化。
代码
# .env.development
VITE_APP_BASE_API ='http://localhost:3000'

2.在request文件夹内，新建index.js文件，实现拦截器，用于封装所有请求的通用功能（get,post,patch,put..）
import axios from 'axios'

axios.defaults.baseURL = import.meta.env.VITE_APP_BASE_API
axios.defaults.timeout = 3000
/**
 * http request 拦截器
 */
 axios.interceptors.request.use(
    (config) => {
      config.data = JSON.stringify(config.data)
      config.headers = {
        'Content-Type': 'application/json',
      }
      return config
    },
    (error) => {
      return Promise.reject(error)
    }
  )
  
  /**
   * http response 拦截器
   */
  axios.interceptors.response.use(
    (response) => {
      if (response.data.errCode === 2) {
        console.log('过期')
      }
      return response
    },
    (error) => {
      console.log('请求出错：', error)
    }
  )
  
  /**
   * 封装get方法
   * @param url  请求url
   * @param params  请求参数
   * @returns {Promise}
   */
  export function get(url, params = {}) {
    return new Promise((resolve, reject) => {
      axios
        .get(url, {
          params: params,
        })
        .then((response) => {
          landing(url, params, response.data)
          resolve(response.data)
        })
        .catch((error) => {
          reject(error)
        })
    })
  }
  
  /**
   * 封装post请求
   * @param url
   * @param data
   * @returns {Promise}
   */
  
  export function post(url, data) {
    return new Promise((resolve, reject) => {
      axios.post(url, data).then(
        (response) => {
          //关闭进度条
          resolve(response.data)
        },
        (err) => {
          reject(err)
        }
      )
    })
  }
  
  /**
   * 封装patch请求
   * @param url
   * @param data
   * @returns {Promise}
   */
  export function patch(url, data = {}) {
    return new Promise((resolve, reject) => {
      axios.patch(url, data).then(
        (response) => {
          resolve(response.data)
        },
        (err) => {
          msag(err)
          reject(err)
        }
      )
    })
  }
  
  /**
   * 封装put请求
   * @param url
   * @param data
   * @returns {Promise}
   */
  
  export function put(url, data = {}) {
    return new Promise((resolve, reject) => {
      axios.put(url, data).then(
        (response) => {
          resolve(response.data)
        },
        (err) => {
          msag(err)
          reject(err)
        }
      )
    })
  }
  
  //统一接口处理，返回数据
  export default function (fecth, url, param) {
    let _data = ''
    return new Promise((resolve, reject) => {
      switch (fecth) {
        case 'get':
          // console.log('begin a get request,and url:', url)
          get(url, param)
            .then(function (response) {
              resolve(response)
            })
            .catch(function (error) {
              // console.log('get request GET failed.', error)
              reject(error)
            })
          break
        case 'post':
          post(url, param)
            .then(function (response) {
              resolve(response)
            })
            .catch(function (error) {
              // console.log('get request POST failed.', error)
              reject(error)
            })
          break
        default:
          break
      }
    })
  }
  /**
 * 查看返回的数据
 * @param url
 * @param params
 * @param data
 */
function landing(url, params, data) {
    if (data.code === -1) {
    }
  }
3.对应事件写对应请求
import http from './index'
export default {
  // 请求示例
  /* getBanner() {
    return new Promise((resolve, reject) => {
      http('get', '/banner').then(
        (res) => {
          resolve(res)
        },
        (error) => {
          reject(error)
        }
      )
    })
  }, */

  // 获取所有公告
  getNotice() {
    return new Promise((resolve, reject) => {
      http('get', '/gg_notice').then(
        (res) => {
          resolve(res)
        },
        (error) => {
          reject(error)
        }
      )
    })
  },
  // 添加公告 data: {content,title,username}
  addNotice(data) {
    return new Promise((resolve, reject) => {
      http('post', '/gg_notice', data).then(
        (res) => {
          resolve(res)
        },
        (error) => {
          reject(error)
        }
      )
    })
  },
}