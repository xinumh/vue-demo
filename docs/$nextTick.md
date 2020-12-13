### $nextTck

`作用`：$nextTick 是用来知道什么时候DOM更新完成

`异步更新原理`：

Vue 在观察到数据变化时斌不是直接去更新DOM，而是开启一个队列，并缓冲在同一事件循环中所发生的所有数据改变。在缓冲时会去除重复数据，从而避免不必要的计算和DOM操作。然后在下一个事件循环 tick 中，Vue 刷新队列并执行实际（以去重）的工作。

> Vue 会根据当前浏览器环境优先使用原生的Promise.then 和 MutationObserver，如果都不支持，就会采用setTimeout 代替

```vue
<body>
  <div id="app">
    <div id="div" v-if="showDiv">这是一段文本</div>
    <button @click="getText">获取div内容</button>
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        showDiv: false
      },
      methods: {
        getText() {
          this.showDiv = true
          var text = document.getElementById('div').innerHTML
          console.log('text', text) // [Vue warn]: Error in v-on handler: "TypeError: Cannot read property 'innerHTML' of null"
          this.$nextTick(function() {
            var text = document.getElementById('div').innerHTML
            console.log('text', text)
          })
        }
      }
    })
  </script>
</body>
```

