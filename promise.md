# Promise

- 3 つの状態
- 状態和 then catch
- 他の方法

Promise の基本的な使用法

```js
// 画像を読み込む
function loadImg(src) {
  const p = new Promise((resolve, reject) => {
    const img = document.createElement('img')
    img.onload = () => {
      resolve(img)
    }
    img.onerror = () => {
      const err = new Error(`読み込み失敗 ${src}`)
      reject(err)
    }
    img.src = src
  })
  return p
}
const url = 'https://xx.jpg'
loadImg(url)
  .then((img) => {
    console.log(img.width)
    return img
  })
  .then((img) => {
    console.log(img.height)
  })
  .catch((ex) => console.error(ex))
```

## 3 つの状態

3 つの状態 pending resolved rejected

（変換の関係を示すために絵を描くと、変換は元に戻せません）

```js
// 定義したばかりの場合、デフォルトは pending
const p1 = new Promise((resolve, reject) => {})

// 実行 resolve() 後，状態は resolved
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve()
  })
})

// 実行 reject() 後，状態は rejected
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject()
  })
})
```

```js
// resolved 状態に直接戻る
Promise.resolve(100)
// rejected 状態に直接戻る
Promise.reject('some error')
```

## 状態和 then catch

状態变化会触发 then catch ——

- pending 不会触发任何 then catch 回调
- 状態变为 resolved 会触发后续的 then 回调
- 状態变为 rejected 会触发后续的 catch 回调

---

then catch 会继续返回 Promise ，**此时可能会发生状态变化！！！**

```js
// then() 一般正常返回 resolved 状态的 promise
Promise.resolve().then(() => {
  return 100
})

// then() 里抛出错误，会返回 rejected 状态的 promise
Promise.resolve().then(() => {
  throw new Error('err')
})

// catch() 不抛出错误，会返回 resolved 状态的 promise
Promise.reject().catch(() => {
  console.error('catch some error')
})

// catch() 抛出错误，会返回 rejected 状态的 promise
Promise.reject().catch(() => {
  console.error('catch some error')
  throw new Error('err')
})
```

問題

```js
// 問題1
Promise.resolve()
  .then(() => {
    console.log(1)
  })
  .catch(() => {
    console.log(2)
  })
  .then(() => {
    console.log(3)
  })
//1 3

// 問題2
Promise.resolve()
  .then(() => {
    // 返回 rejected 状态的 promise
    console.log(1)
    throw new Error('erro1')
  })
  .catch(() => {
    // 返回 resolved 状态的 promise
    console.log(2)
  })
  .then(() => {
    console.log(3)
  })
//1 2 3

// 問題3
Promise.resolve()
  .then(() => {
    // 返回 rejected 状态的 promise
    console.log(1)
    throw new Error('erro1')
  })
  .catch(() => {
    // 返回 resolved 状态的 promise
    console.log(2)
  })
  .catch(() => {
    console.log(3)
  })
//1 2
```

## 他の方法

//.finally() //当 Promise 状态发生变化时，不论如何变化都会执行，不变化不执行 （关闭数据库）本质是 then()

//Promise.all([p1,p2,p3])

传入多个 Promise 实例，包装成一个新的 Promise 实例返回

Promise.all()的状态所有状态都为 resolved,才是 resolved

Promisr.race()

状态取决于第一个，如果成功就是成功

Promise.Settled()

永远都是成功，只会忠实记录下各个 Promise 的表现

Promise.any() 只要有一个成功就是成功
