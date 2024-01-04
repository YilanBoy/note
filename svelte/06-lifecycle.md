# Lifecycle

每個 component 都有自己的生命週期，當 component 被建立、更新或銷毀時，會觸發不同的生命週期方法。

## onMount

`onMount` 方法會在 component 被建立後觸發，我們可以在這個方法中做一些初始化的動作，例如：註冊事件監聽器、初始化一些資料等等。

```svelte
<script>
    import { onMount } from 'svelte';

    onMount(() => {
        console.log('onMount');
    });
</script>
```

## beforeUpdate 與 afterUpdate

`beforeUpdate` 會在 DOM 更新之前立刻觸發。而 `afterUpdate` 正好相反，會在 DOM 與資料同步之後立刻觸發。

```svelte
<script>
    import { onMount, beforeUpdate, afterUpdate } from 'svelte';

    let count = 0;

    onMount(() => {
        console.log('onMount');
    });

    beforeUpdate(() => {
        console.log('beforeUpdate');
    });

    afterUpdate(() => {
        console.log('afterUpdate');
    });
</script>

<button on:click={() => count += 1}>
    Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

需要注意的是，`beforeUpdate` 會在 component 建立好之後觸發一次。

## tick

`tick` 與其他生命週期方法不同，你可以在任何地方呼叫它。它會回傳一個 promise，並且立刻將等待中的狀態更新套用到 DOM 上。

當你更新了 component 的狀態 (state)，Svelte 其實並不會立刻更新 DOM，而是會在下一個 microtask 執行更新。這樣做的好處是可以將多次的資料更新合併成一次，以避免產生太多次的 DOM 更新。

```svelte
<script>
    import { tick } from 'svelte';

    let text = `Select some text and hit the tab key to toggle uppercase`;

    // you have to use async functions
    async function handleKeydown(event) {
        if (event.key !== 'Tab') return;

        event.preventDefault();

        // `this` is the <textarea> element
        const { selectionStart, selectionEnd, value } = this;
        const selection = value.slice(selectionStart, selectionEnd);

        const replacement = /[a-z]/.test(selection)
            ? selection.toUpperCase()
            : selection.toLowerCase();

        text =
            value.slice(0, selectionStart) +
            replacement +
            value.slice(selectionEnd);

        // use tick to make sure the DOM has updated
        await tick();

        this.selectionStart = selectionStart;
        this.selectionEnd = selectionEnd;
    }
</script>

<textarea
    value={text}
    on:keydown={handleKeydown}
/>

<style>
    textarea {
        width: 100%;
        height: 100%;
        resize: none;
    }
</style>
```
