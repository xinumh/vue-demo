### 子到父通信

> React & Vue

React，习惯将`函数`从父组件传递给子组件，以便子组件调用函数向父组件传参

Vue，永远不要这么做，被认为是`反模式`



**通常-传入函数参数**

`Parent:`

```js
// Parent
<template>
  <ChildComponent :method="parentMethod" />
</template>
// Parent
export default {
  methods: {
    parentMethod(valueFromChild) {
      // Do something with the value
      console.log('From the child:', valueFromChild);
    }
  }
}
```

在子组件中调用传入的方法并将子组件的值作为方法的参数传入:

`Child:`

```js
export default {
  props: {
    method: { type: Function },
  },
  data() {
    return { value: 'I am the child.' };
  },
  mounted() {
    // Pass a value to the parent through the function
    this.method(this.value);
  }
}
```

**可行，但，，，这不是vue中的最佳方式**

**Vue子到父通信 —— 事件**

`Parent:`

```vue
<template>
  <ChildComponent @send-message="handleSendMessage" />
</template>
```

```js
export default {
  methods: {
    handleSendMessage(event, value) {
      // Our event handler gets the event, as well as any
      // arguments the child passes to the event
      console.log('From the child:', value);
    }
  }
}
```

`Child:`

```js
export default {
  props: {
    method: { type: Function },
  },
  data() {
    return { value: 'I am the child.' };
  },
  mounted() {
    // Instead of calling the method we emit an event
    this.$emit('send-message', this.value);
  }
}
```



