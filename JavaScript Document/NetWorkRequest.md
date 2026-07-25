# NetworkRequester

### XMLHttpRequest

实际上，axios的底层实现也是XMLHttpRequest（XHR）实现的

###### 1.创建一个XHR的实例

``` typescript
const xhr = new XMLHttpRequest()
```

###### 2.如需使用GET方法在Params中传递参数

``` typescript
const paramsObj = {
  key: value
}

const requestParams = new URLSearchParams(parmasObj)

// 在URL后拼接即可
xhr.open(method: string,url: string?requestParmas | URL)
```

###### 3.向xhr实例对象中传入对应参数

``` typescript
xhr.open(method: string,url: string | URL)
```

###### 4.为xhr实例对象增加事件监听以便获取到响应结果

``` typescript
xhr.addEventListener('loadend',() => {
  console.log(xhr.response)
})
```

###### 5.如请求体(body)内携带参数，需声明请求体的格式

``` typescript
xhr.setRequestHeader('Content-type', 'application/json')
```

###### 6.声明请求体内容

```typescript
const requestBody = {
  key: value
}

const requestBodyString = JSON.stringify()
```

###### 7.将请求体传入该次请求并发出

```typescript
xhr.send(requestBodyString)
```

###### 8.使用Promise管理XHR请求的响应与失败

```typescript
const xhrReseponse = new Promise((response,reject) => {
  const xhr = new XMLHttpRequest()
  xhr.open(method,url)
  xhr.addEventListener('loadend',() => {
    if(xhr.status >= 200 && xhr.status <300){
      response(xhr.response)
    } else {
      reject(new Error(xhr.response))
    }
  })
  xhr.send()
})

xhrResponse.then(result => {
  console.log(result)
}).catch(error => {
  console.log(error)
}).finally(() => {
  
})
```

#### 自行封装一个简易的Axios

```typescript
function myAxios(config){
  return new Promise((response,reject) => {
    const xhr = new XMLHttpRequest()
    const params
    if(config.params){
      params = new URLSearchParams(config.params)
    }
   	const url = config.params ? config.url+ '?' +params.toString() : config.url
    xhr.open(config.method || 'GET',url)
    xhr.addEventListener('loadend',() => {
      if(xhr.status >= 200 && xhr.status < 300){
        response(xhr.response)
      } else {
        reject(new Error(xhr.response))
      }
    })
    
    if(connfig.data){
      const body = JSON.stringify(confige.data)
      xhr.setRequestHeader('Content-Type','application/json')
      xhr.send(body)
    } else {
      xhr.send()
    }
  })
}

myAxios({
	url: value
}).then((result) => {
  console.log(result)
}).catch((error) => {
  console.log(error)
}).finally(() => {
  
})
```





### Fetch

Fetch作为一个更加现代的资源获取接口，他是一个比XMLHttpRequest更加强大、更加灵活的替代

> [!WARNING]
>
> 但在需要**获取文件上传进度**的情况FetchAPI无法做到

###### 1.使用fetch()

```typescript
// fetch返回的是一个Promise
const response = await fetch(url: string,option: object)

// 我们可以通过.then()
response.then((response) => {
	console.log(response)
})
```

###### 2.配置fetch()

```typescript
// 携带params参数同XHR一样在url后拼接Params参数
async function postData(url: string,date: object){
	const response = await fetch(url,{
		method:'',
		headers: {
	  	"Content-Type": "application/json",
		},
	  body: JSON.stringify(date),
	})
	
  return response.json()
}
```

### Axios

###### 1.导入Axios

``` typescript
import axios from "axios"
```

###### 2.发起请求

```typescript
const response = await axios.get(url,{
  baseURL: '',
  timeout: value,
  headers: {
    "Content-Type" : 'application/json',
    Authorization: `Bearer ${getToken()}`
  },
  params: {
    key: value
  },
  date: {
    key: value
  }
})
```

###### 3.拿到response

``` typescript
response.then((res) => {
  console.log'(res)
}).catch((error) => {
  console.log(error)
}).finally(() => {

})
```

#### 如要使用请求拦截器

###### 封装复用的axios

``` typescript
const request = axios.create({
  baseURL:'value',
})

// 请求拦截器
request.interceptors.request.use{
  (config) => {
    if(getToken()){
       config.headers.Authorization = `Bearer ${getToken}`
    }
  },
  (error) => {
    return Promise.reject(error)
  }
}

// 响应拦截器
request.interceptors.response.use({
  (response) => {
  
	},
  (error) => {
    
  }
})
```





