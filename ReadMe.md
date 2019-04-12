## JavaScript 生命週期相關文章

- https://flaviocopes.com/javascript-async-defer/
- https://github.com/fi3ework/blog/issues/3

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
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div id="app"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.8.6/umd/react.production.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.8.6/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.18.0/axios.min.js"></script>
    <script src="loadScriptAsync.js"></script>

    <script type="text/babel">
      ;(async () => {
        const res = await axios.get('https://gank.io/api/random/data/%E7%A6%8F%E5%88%A9/1')
        const setLocalStroage = await localStorage.setItem('data', JSON.stringify(res.data.results))
        // const filter = await loadScriptAsync('./filter.js')
        const render = await renderReact()
      })()
    </script>

    <script type="text/babel">
      function renderReact() {
        const datas = JSON.parse(localStorage.getItem('data'))
        const App = () => {
          return (
            <ul>
              {datas.map(data => (
                <li className="layout">
                  <div className="left">
                    <h4>{data.who}</h4>
                    <a href={data.url}>{data.url}</a>
                  </div>
                  <div className="right">
                    <img src={data.url} style={{ width: '100%' }} />
                  </div>
                </li>
              ))}
            </ul>
          )
        }

        const MOUNT = document.querySelector('#app')

        ReactDOM.render(<App />, MOUNT)
      }
    </script>
  </body>
</html>

```
