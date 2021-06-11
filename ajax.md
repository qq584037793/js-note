# Ajax

## ajax 使い

```js
const xhr = new XMLHttpRequest()
xhr.open('GET', '/data/test.json', true)
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if (xhr.readyState === 200) {
      console.log(JSON.parse(xhr.responseText))
      alert(xhr.responseText)
    } else if (xhr.status === 404) {
      console.log('404 not found')
    }
  }
}
```

open('设置类型','url'，'是否异步')
send() //发送请求

## ajax 応用

```js
function ajax(url) {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url, true)
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.readyState === 200) {
          resolve(JSON.parse(xhr.responseText))
        } else if (xhr.readyState === 404 || xhr.status === 500) {
          reject(new Error('404 not found '))
        }
      }
    }
    xhr.send(null)
  })
  return p
}

const url = './data/text.json'
ajax(url)
  .then((res) => console.log(res))
  .catch((err) => console.log(err))
```

## Json

```json
abc({ "name": "xxx" })
```

JSON.parse() //将 JSON 转换为 javascript 对象
JSON.stringify() // javascript 对象转换为字符串
