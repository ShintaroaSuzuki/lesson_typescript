---
theme: gradient
size: 16:9
_class: lead
paginate: true
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
// Error: Operator '+' cannot be applied to types 'number' and 'never[]'.

let obj = {};
obj.foo;
// Error: Property 'foo' does not exist on type '{}'.
```

### TypeScript は「型」を用いて、不正なコードを "事前に" 教えてくれます

---

# コンパイル

### JavaScript のコンパイル

1. JavaScript ソース -> JavaScript AST
2. JavaScript AST -> バイトコード
3. バイトコードがランタイムによって評価される

<!--
AST(Abstract Syntax Tree: 抽象構文木): ソースコードから空白類文字（空白、タブ、改行など）を取り除き、ツリー構造のデータに整形したもの。
ランタイム: プログラムの実行環境のこと。JavaScript のランタイムは広く Node.js が使われているが、Deno, Bun などもある。
-->

---

# コンパイル

### TypeScript のコンパイル

1. TypeScript ソース -> TypeScript AST
2. **AST が型チェッカーによりチェックされる**
3. TypeScript AST -> JavaScript ソース

##### TypeScript のコンパイルには _tsc (TypeScript Compiler)_ を用いる

```shell
$ tsc src
```

---

# コンパイル

### Q.型チェックはいつ実行されるか

**A.型チェックはコンパイル時に行われるが、TypeScript にはインクリメンタルコンパイル (コードの変更と同時に再コンパイルが行われる仕組み) が備わっているため、実際には常に同期的に型チェックは行われており、エディタ上でリアルタイムでエラーを確認することができる**

<!-- ここで実演してみせる -->

---

# TypeScript の初歩

---

# TypeScript の文法

TypeScript では変数の後ろに型を宣言します

```ts
let num: number = 3;
num = "hoge";
// Error: Type 'string' is not assignable to type 'number'.
```

上記の例では、数値型として宣言した `num` に文字列型の `'hoge'` を代入しようとして怒られています

---

# 型推論

しかし、冒頭の例の通り、型を明示的に宣言しなくても TypeScript は仕事をしてくれます。

TypeScript はコードから変数の型を "ある程度" 推論することができるため、明示的に型を宣言しなくても、エラーを出力することができます。

```ts
3 + [];
// Error: Operator '+' cannot be applied to types 'number' and 'never[]'.
```

<!-- リテラル型はあえて明示的に型宣言をしなくてもよいと思います -->

---

# オブジェクト

オブジェクトの型宣言は、**オブジェクトリララル表記**というものを用います。

```ts
let obj: {
    a: number;
    b: string;
} = {
    a: 3,
    b: "x",
};
```

---

# 配列

配列の型宣言は下記のように行います。

この場合、`arr` は配列の長さを問わず、空配列も許容します。

```ts
let arr: number[] = [0, 1, 2];
```

---

# タプル

タプルは長さが固定の配列です。タプルの型宣言は次のように行います。

```ts
let tuple: [number, number] = [0, 1];
// OK

let tuple: [number, number] = [0, 1, 2];
// Error: Type '[number, number, number]' is not assignable to type '[number, number]'.
//        Source has 3 element(s) but target allows only 2.
```

タプルの中身は、可変長の要素もサポートしており、これを使うと最小限の長さについて制約を持った配列を表現できます。

```ts
let NoEmptyNumber: [number, ...number[]] = [0];
//OK

let NoEmptyNumber: [number, ...number[]] = [];
// Error: Type '[]' is not assignable to type '[number, ...number[]]'.
//        Source has 0 element(s) but target requires 1.
```

---

# 型エイリアス

型エイリアスを使うと、下記のように型に別名をつけることができます。

```ts
type Person = {
    name: string;
    age: number;
};

let tanaka: Person = {
    name: "田中",
    age: 30,
};
```

---

# オプションパラメータ

前のスライドで `Person` という型エイリアスを定義しましたが、年齢を知られたくない人もいると思うので、`age` プロパティは任意としたいです。

このときに使えるのが**オプションパラメータ**です。

```ts
type Person = {
    name: string;
    age?: number;
};

let tanaka: Person = {
    name: "田中",
    age: 30,
};

let nakano: Person = {
    name: "中野",
};
```

---

# 合併型

囚人番号で管理されている名もなき犯罪者のことも考慮すると、`Person` 型の `name` プロパティは数値も受け付けられるようにしたほうがいいかもしれません。

このような場合に使えるのが**合併型**です。

```ts
type Person = {
    name: string | number;
    age?: number;
};

let tanaka: Person = {
    name: "田中",
    age: 30,
};

let criminal: Person = {
    name: 334,
};
```

---

# 交差型

`Person` 型にはまだ考慮漏れがあります。それは、`Person` でもあり `Wolf` でもある人狼のことを考慮していない点です。

**交差型** を使って、人狼も考慮してあげましょう。

```ts
type Wolf = {
    bark: () => void;
};

let miyajima: Person & Wolf = {
    name: "宮島",
    age: 22,
    bark: () => console.log("Awoo"),
};
```

---

# TypeScript の実践

---

# ジェネリック型

---

# ルックアップ型

---

# keyof 演算子

---

# typeof 演算子

---

# ユーティリティ型

---

# 補足

---

![bg left:40% 80%](https://m.media-amazon.com/images/I/71G0osmMgSL.jpg)

# 『プログラミング TypeScript』

スケールする JavaScript アプリケーション開発

https://www.amazon.co.jp/%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0TypeScript-%E2%80%95%E3%82%B9%E3%82%B1%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8BJavaScript%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E9%96%8B%E7%99%BA-Boris-Cherny/dp/4873119049

---

![bg left:40% 80%](https://marp.app/assets/marp.svg)

# **Marp**

Markdown Presentation Ecosystem

https://marp.app/
