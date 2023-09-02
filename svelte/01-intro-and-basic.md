# Intro and Basics

Svelte 是一個新穎的現代前端框架。

與其他前端框架相比，他並不是將整個框架包在前端網頁中，而是採用預先編譯的方式，
因此相較於其他框架，打包後的檔案會更小，效能會更好。

此外 svelte 的語法也非常容易學習，幾乎快要與原生 javascript 相同。

比較一下 vue 與 svelte 在宣告狀態的寫法 (declare state)。

## 語法比較

### Vue 3

```vue
<script setup>
  import { ref } from "vue";
  const name = ref("John");
</script>

<template>
  <h1>Hello {{ name }}</h1>
</template>
```

### Svelte

```svelte
<script>
  let name = "John";
</script>

<h1>Hello {name}</h1>
```

語法上是不是簡單很多呢？

## 實作一個計數器

用 svelte 實作一個 counter。

首先是首頁的樣板：

```svelte
<script>
  // 引入 counter
  import Counter from "./Counter.svelte";
</script>

<main>
  <!-- 傳入參數 -->
  <Counter title="My Counter" />
</main>
```

Counter 的內容：

```svelte
<script>
  // 接收參數
  export let title = "My default title";

  let count = 0;

  // computed
  $: doubled = count * 2;

  onMount(() => {
    console.log("lifecycle hook");
  });

  function increment() {
    count++;
  }

  function decrement() {
    count--;
  }
</script>

<div>
  <h2>{title}</h2>
  <div>
    <div>Count is {count}</div>
    <div>Doubled is {doubled}</div>
    <div>
      <button on:click="{decrement}">Decrement</button>
      <button on:click="{increment}">Increment</button>
    </div>
  </div>
</div>
```

## 參考資料

- [Laracasts - Svelte Crash Course](https://laracasts.com/series/svelte-crash-course)
- [Component party](https://component-party.dev/)
