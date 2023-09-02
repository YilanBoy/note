# Reactivity

## Assignment

Svelte 擁有強大的即時反應功能，用於保持 DOM 與應用程序狀態同步。舉個例，響應事件。

```svelte
<script>
  let count = 0;

  function increment() {
    // event handler code goes here
    count += 1;
  }
</script>

<button>
  Clicked {count}
  {count === 1 ? 'time' : 'times'}
</button>
```

Svelte 的反應性是**基於賦值 (assignment)** 的，只要賦值給一個變數，Svelte 就會自動更新 DOM (Document Object Model)。

## Declarations

只要 component 的 state 發生變化，Svelte 會自動更新 DOM。

有時候你希望某個 component state 會是另外一個 state 經過計算處理後的結果，這時候你可以使用 `$:` 來宣告。

```svelte
<script>
  let count = 0;
  $: doubled = count * 2;
```

> 雖然 `$:` 看上去有點奇怪，但這其實是一個合法的 JavaScript 語法。

之後我們就可以在 HTML 中使用 `doubled` 這個變數。

```svelte
<p>{count} doubled is {doubled}</p>
```

> 請注意，反應式聲明和語句將在其他程式碼之後，但在呈現 component markup 之前執行。

## Statement

除了宣告變數之外，你也可以在 `$:` 中使用 JavaScript 的陳述句 (statement)。

```svelte
<script>
  let count = 0;

  $: if (count >= 10) {
    alert(`count is dangerously high!`);
    count = 9;
  }
</script>
```

## Updating Arrays and Objects

因為 Svelte 的即時反應功能是基於賦值的，所以如果你想要更新陣列或物件，你必須要重新賦值。而不是使用 `push`、`pop`、`shift`、`unshift`、`splice`、`sort`、`reverse` 這些方法。

```svelte
<script>
let numbers = [1, 2, 3];

function addNumber() {
  // it will not trigger reactivity
  numbers.push(numbers.length + 1);
}
</script>
```

如果想要更新陣列，你可以這麼使用。

```svelte
<script>
let numbers = [1, 2, 3];

function addNumber() {
  // it will trigger reactivity
  numbers = [...numbers, numbers.length + 1];
}
</script>
```

一個簡單的經驗法則：更新的變量的名稱必須出現在賦值的左側。
