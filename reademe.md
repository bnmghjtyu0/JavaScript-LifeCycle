## JavaScript 生命週期相關文章

https://github.com/fi3ework/blog/issues/3

### 設置 loadScript

```js
var loadScriptAsync = function(uri) {
  return new Promise((resolve, reject) => {
    var tag = document.createElement('script')
    tag.src = uri
    tag.async = true
    tag.onload = () => {
      resolve()
    }
    var firstScriptTag = document.getElementsByTagName('script')[0]
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)
  })
}
```

### index.html 中使用 async await 載入 function and script

```js
<script type="text/babel">
      ;(async () => {
        const res = await axios.get('https://gank.io/api/random/data/%E7%A6%8F%E5%88%A9/1')
        const setLocalStroage = await localStorage.setItem('data', JSON.stringify(res.data.results))
        const filter = await loadScriptAsync('./filter.js')
        const render = await renderReact()
      })()
</script>
```
