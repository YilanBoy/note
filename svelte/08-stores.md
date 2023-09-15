# Stores

有時候你會想要在不同的 component 之間共享一個值，這時候你可以使用 store。

## Writeable Store

在 Svelte 中，store 是一個簡單的物件，並擁有一個訂閱 `subscribe` 的方法，當 store 的值改變時，所有訂閱它的 component 都會被通知到。

首先新增一個 `stores.js` 檔案，並且在裡面建立一個 store。

```js
// stores.js
import { writable } from 'svelte/store';

export const count = writable(0);
```

接下來建立一個幫 `count` +1 的 component。

```svelte
<!-- Increment.svelte -->
<script>
    import { count } from './stores.js';

    function increment() {
        count.update((n) => n + 1);
    }
</script>

<button on:click={increment}>
    +
</button>
```

建立一個幫 `count` -1 的 component。

```svelte
<!-- Decrement.svelte -->
<script>
    import { count } from './stores.js';

    function decrement() {
        count.update((n) => n - 1);
    }
</script>

<button on:click={decrement}>
    -
</button>
```

接著在 `App.svelte` 中引入 increment 與 decrement 的 component，並顯示 `count` 的值。

```svelte
<script>
    import { count } from './stores.js';
    import Increment from './Increment.svelte';
    import Decrement from './Decrement.svelte';

    let count_value;

    count.subscribe(value => {
        count_value = value;
    });
</script>

<h1>The count is {count_value}</h1>

<Increment />
<Decrement />
```

## Auto-Subscriptions

上面的例子有個潛在的問題，如果在 component 被銷毀之前沒有取消訂閱，那麼就會導致 memory leak。

我們可以使用 `onDestroy` 來取消訂閱。

```svelte
<script>
    import { onDestroy } from 'svelte';
    import { count } from './stores.js';

    // ...

    let count_value;

    const unsubscribe = count.subscribe(value => {
        count_value = value;
    });

    onDestroy(unsubscribe);
</script>
```

> 呼叫 subscribe 方法會回傳一個 unsubscribe 方法，可以用來取消訂閱。

每次取消訂閱都要呼叫 `unsubscribe` 這個方法，這樣做很麻煩，所以 Svelte 提供了 `auto-subscriptions` 的功能，只要在 `subscribe` 前面加上 `$` 符號，就可以自動取消訂閱。

```svelte
<script>
    import { count } from './stores.js';

    // ...
</script>

<h1>The count is {$count}</h1>

<!-- ... -->
```
