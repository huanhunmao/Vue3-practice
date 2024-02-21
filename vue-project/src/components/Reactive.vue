<script setup>
import { reactive } from "vue";

// 使用 不同于 ref 的另一种方式 实现数据响应
const state = reactive({count: 0})

// Reactive Proxy obj 和 Original obj 不一样
const raw = {}
const proxy = reactive(raw)
console.log(proxy === raw)
console.log('proxy',proxy); // Proxy(Object) {}
console.log('raw',raw); // {} 

console.log(reactive(proxy) === proxy); // true calling reactive() on a proxy returns itself


const proxy1 = reactive({})
const raw1 = {}
proxy1.nested = raw1

console.log(proxy.nested === raw1); // false nested objects inside a reactive object are also proxies

</script>

<template>
  <div class="greetings">
    <h1 class="green">reactive</h1>
    <button @click="state.count++">
  {{ state.count }}
</button>
  </div>
</template>

<style scoped>
</style>
