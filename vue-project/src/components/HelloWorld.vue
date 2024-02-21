<script setup>
import { nextTick, ref } from "vue";

defineProps({
  msg: {
    type: String,
    required: true
  }
})

// 一般响应
const count = ref(0);

console.log(count.value) // 0 


// 深度响应
const obj = ref({
    nested: {count: 0},
    arr :['foo', 'bar']
})

console.log(obj.value.nested.count +1) //1
console.log(obj.value.arr.push('ccc'));
console.log(obj.value.arr); // Proxy(Array) {0: 'foo', 1: 'bar', 2: 'ccc'}

// 点击按钮时增加 count
// const incrementCount = () => {
//   count.value++;
// };

// DOM update to complete after a state change
async function increment() {
  count.value++
  await nextTick()
  // Now the DOM is updated
}


// ref 实现思路 
const myRef = {
    _value:0,
    get value(){
        track()
        return this._value;
    }
    
    set value(newValue){
        this._value = newValue;
        trigger()
    }
}

// ref reactive 实现过程 
  // 创建包含 value 属性的对象
  function ref(initialValue) {
  // 创建包含 value 属性的对象
  const refObject = {
    value: initialValue
  };

  const handlers = {
    get(target, key, receiver) {
      // 记录引用的依赖关系
      track(refObject, 'value');
      const result = Reflect.get(target, key, receiver);
      return result;
    },
    set(target, key, value, receiver) {
      const oldValue = target.value;
      const result = Reflect.set(target, key, value, receiver);
      // 触发更新，通知依赖引用值发生变化
      if (oldValue !== value) {
        trigger(refObject, 'value');
      }
      return result;
    },
    // 其他操作的处理函数...
  };

  // 返回包含 value 属性的对象的代理
  return new Proxy(refObject, handlers);
}

  // 返回包含 value 属性的对象的代理
  return new Proxy(refObject, handlers);
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <div>
        <button @click="increment">{{ count }}</button>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
