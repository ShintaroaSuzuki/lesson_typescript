---
theme: gradient
size: 16:9
_class: lead
paginate: true
---

![bg left:40% 80%](https://marp.app/assets/marp.svg)

# **Marp**

Markdown Presentation Ecosystem

https://marp.app/

---

# TypeScript 勉強会

---

# TypeScript の概要

---

# 型安全性

JavaScript のコード

```js
3 + [];
// 3

let obj = {};
obj.foo;
// undefined
```

### JavaScript はできるだけエラーを出さずに、コードを活かそうとするため、動作確認をしてみるまでバグに気づくことができません

<!--
本当に 3 と [] を足したかったのでしょうか？
本当に存在しないプロパティを参照したかったのでしょうか？
-->

---

# 型安全性

TypeScript のコード

```ts
3 + [];
// Error: Operator '+' cannot be applied to types 'number' and 'never[]'

let obj = {};
obj.foo;
// Error: Property 'foo' does not exist on type '{}'.
```
