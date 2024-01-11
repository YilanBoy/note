# Classes and Styles

## Class Directive

你可以根據一個值來添加或移除一個元素的 class。

```svelte
<script>
    let active = true;

    function toggle() {
        active = !active;
    }
</script>

<button
    type="button"
    class="button {active ? 'active' : ''}"
    on:click={toggle}
>
    I'm a button
</button>

<style>
    button .active {
        background-color: #ff0;
    }
</style>
```

你也可以將其拆開成兩個屬性：

```svelte
<button
    type="button"
    class="button"
    class:active={active}
    on:click={toggle}
>
    I'm a button
</button>
```

如果變數的名稱與 class 的名稱相同，你可以使用 `class:active` 來簡化：

```svelte
<button
    type="button"
    class="button"
    class:active
    on:click={toggle}
>
    I'm a button
</button>
```

## Style Directive

同樣的方式也可以使用在 style 上：

```svelte
<script>
    let isRed = true;
</script>

<div style="color: {isRed ? 'red' : ''}">
    This will be red
</div>

<div
    style:color={isRed ? 'red' : ''}
>
    This will be red too
</div>
```

## Component Styles

你可以傳入一個 CSS 變數來設定組件的樣式：

```svelte
<!-- App.svelte -->
<Box --color="red" />
```

```svelte
<!-- Box.svelte -->
<div class="box" />

<style>
    .box {
        width: 5em;
        height: 5em;
        border-radius: 0.5em;
        margin: 0 0 1em 0;
        /* 這裡接收外部傳入 Box component 的 CSS 變數 --color */
        background-color: var(--color, #ccc);
    }
</style>
```
