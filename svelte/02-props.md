# Props

## Declaring Props

如果你需要將變數從父 component 傳遞到子 component，你可以在子元件使用 `export` 關鍵字來宣告一個 `name` prop：

```svelte
<script>
    export let name;
</script>
```

然後你就可以在父 component 中使用 `name="world"` 將 `world` 傳入子 component 中：

```svelte
<script>
    import Nested from './Nested.svelte';
</script>

<Nested name="world" />
```

## Default Values

你可以在宣告 prop 的時候指定預設值：

```svelte
<script>
    export let name = 'world';
</script>
```

## Spread Props

假設我們有一個 component 叫做 `PackageInfo`，它有四個 props：`name`、`speed`、`version`、`website`。

```svelte
<script>
    export let name;
    export let speed;
    export let version;
    export let website;

    $: href = `https://www.npmjs.com/package/${name}`;
</script>

<p>
    The <code>{name}</code> package is {speed} fast. Download version {version} from
    <a {href}>npm</a> and <a href={website}>learn more here</a>
</p>
```

你可以使用下面的方式將這四個 props 傳入子 component：

```svelte
<script>
    import PackageInfo from './PackageInfo.svelte';

    const pkg = {
        name: 'svelte',
        speed: 'blazing',
        version: 4,
        website: 'https://svelte.dev'
    };
</script>

<PackageInfo
    name={pkg.name}
    speed={pkg.speed}
    website={pkg.website}
/>
```

也可以使用 `...` 來將物件的所有屬性傳入子 component：

```svelte
<PackageInfo {...pkg} />
```
