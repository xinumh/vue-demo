# vue-demo
## 自定义指令

### 自动获取焦点

``` js
Vue.directive('focus', {
  inserted(el, binding) {
  	el.focus()
  }
})

new Vue({
  el: '#app',
  data: {}
})
```

``` html
<div id="app">
  // 自动获取焦点
  <input v-focus />
</div>
```

指令定义对象提供几个钩子函数：

- `bind`  只调用一次，第一次绑定到元素时调用
- `inserted`  插入父节点时调用
- `update`  所在组件`vnode`更新时调用
- `componentUpdated`
- `unbind`

钩子函数参数：

- `el`：所绑定的元素
- `binding`：对象
  - `name`：指令名
  - `value`：指令绑定值
  - `oldValue`：更新前的值； 仅`update`、`componentUpdated`可用
  - `expression`：
  - `arg`
  - modifiers
- `vnode`
- `oldVnode` 仅`update`、`componentUpdated`可用

### 按钮权限

```js
const role = 'admin'
Vue.directive('permission', {
  inserted(el, binding) {
    if(role !== binding.value) {
      // 若指定的用户角色和当前用户角色role不匹配则删除当前指令绑定的元素
      el.parentElement.removeChild(el)
    }
  }
})
```

```html
<div id="app">
  <button v-permission=“‘admin’”>
    提交
  </button>
   <button v-permission=“‘user’”>
    提交
  </button>
</div>
```



## mixins

`mixins` 选项接收一个混入对象的数组

分发 `Vue` 组件中的可复用功能，一个混入对象可以包含任意组件选项

```js
var mixin = {
  created: function () { console.log(1) }
}
var vm = new Vue({
  created: function () { console.log(2) },
  mixins: [mixin]
})
// => 1
// => 2
```

```js
// 可复用功能
var myMixin = {
  data(){
    return {
      msg2: 'msg2'
    }
  },
  created() {
    this.say(this.msg)
  },
  methods: {
    say(msg){
      console.log(`hello from mixin!${msg}`)
    }
  },
}
new Vue({
  el: '#app',
  data: {
    msg: 'msg1'
  },
  mounted() {
    console.log('msg2', this.msg2)
  },
  created() {
    this.say(this.msg2)
  },
  mixins: [myMixin]
})
```

