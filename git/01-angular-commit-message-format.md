---
layout: default
parent: Git
nav_order: 1
---

# Angular Commit Message Format

由 Angular 團隊制定的 commit message 規範

每一個 commit message 都應該包含 header、body 與 footer

```text
<header>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

實際範例

```text
fix($compile): couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

Closes #392
Breaks foo.bar api, foo.baz should be used instead
```

## 參考資料

- [GitHub - Contributing to Angular](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format)
