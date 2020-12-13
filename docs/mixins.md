# vue-demo

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