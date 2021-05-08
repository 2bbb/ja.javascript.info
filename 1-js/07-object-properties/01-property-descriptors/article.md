
# プロパティフラグとディスクリプタ

ご存知の通り、オブジェクトはプロパティを格納することができます。

今まで、プロパティは単純な "key-value" ペアでしたが、実際にはオブジェクトプロパティはより柔軟で強力なものです。

このチャプターでは、追加の設定オプションについて説明します。

[cut]

## プロパティフラグ 

オブジェクトプロパティには、 **`value`** の他に、3つの特別な属性があります(いわゆる "フラグ" と呼ばれています)。

- **`writable`** -- `true` の場合は変更可能です。それ以外の場合は読み取り専用です。
- **`enumerable`** -- `true` だとループで列挙されます。それ以外の場合は列挙されません。
- **`configurable`** -- `true` の場合、プロパティを削除したり、これらの属性を変更することができます。

一般的にはこれらは姿を見せることが少ないため、まだ見ていませんでした。"通常の方法" でプロパティを作成するとき、これらはすべて `true` です。が、いつでもそれを変更することができます。

まず、それらのフラグを取得する方法を見てみましょう。

メソッド [Object.getOwnPropertyDescriptor](mdn:js/Object/getOwnPropertyDescriptor) で、プロパティの *完全な* 情報を参照することができます。

構文は次の通りです:
```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

`obj`
: 情報を取得するオブジェクトです。

`propertyName`
: プロパティ名です。

返却値はいわゆる "プロパティディスクリプタ" オブジェクトと呼ばれます。: それは値とすべてのフラグを含んでいます。

例:

```js run
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* プロパティディスクリプタ:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

[Object.defineProperty](mdn:js/Object/defineProperty) を使うことでフラグを変更することができます。

構文:

```js
Object.defineProperty(obj, propertyName, descriptor)
```

`obj`, `propertyName`
: 処理するオブジェクトとプロパティです。

`descriptor`
: 適用するプロパティディスクリプタです。

もし、プロパティが存在する場合、`defineProperty` はそのフラグを更新します。そうでなければ、与えられた値とフラグでプロパティを作ります。その場合に、もしフラグが指定されていなければ `false` とみなされます。

例えば、ここではプロパティ `name` はすべて偽のフラグで作られます。:

```js run
let user = {};

*!*
Object.defineProperty(user, "name", {
  value: "John"
});
*/!*

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
*!*
  "writable": false,
  "enumerable": false,
  "configurable": false
*/!*
}
 */
```

上で "通常の方法で" 作成された `user.name` と比較してください: 今やすべてのフラグは false です。もしそのようにしたくなければ、`descriptor` で `true` をセットするのがよいでしょう。

では、例を使ってフラグの影響を見てみましょう。

## 読み取り専用(Read-only) 

`writable` フラグを変更して `user.name` を読み取り専用にしてみましょう:

```js run
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
*!*
  writable: false
*/!*
});

*!*
user.name = "Pete"; // Error: Cannot assign to read only property 'name'...
*/!*
```

これで、`defineProperty` で上書きをしない限りは、誰も私たちの user.name を変えることはできません。

これは先程と同じ操作ですが、プロパティが存在しない場合です:

```js run
let user = { };

Object.defineProperty(user, "name", {
*!*
  value: "Pete",
  // 新しいプロパティに対して、true のものは明示的に列挙する必要があります
  enumerable: true,
  configurable: true
*/!*
});

alert(user.name); // Pete
user.name = "Alice"; // Error
```


## 列挙可能でない(Non-enumerable) 

今、カスタムの `toString` を `user` に追加しましょう。

通常、オブジェクトが持つ組み込みの `toString` は列挙可能ではありません。それは `for..in` では表示されません。しかし私たちが自身の `toString` を追加した場合、デフォルトではこのように `for..in` で表示されます。:

```js run
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

// デフォルトでは、両方のプロパティは列挙されます:
for (let key in user) alert(key); // name, toString
```

もし列挙されたくなければ、`enumerable:false` をセットすることができます。そうすると、組み込みのものと同じように、`for..in` ループで表示されなくなります。:

```js run
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
*!*
  enumerable: false
*/!*
});

*!*
// これで toString は消えました:
*/!*
for (let key in user) alert(key); // name
```

列挙可能でないプロパティは `Object.keys` からも除外されます。:

```js
alert(Object.keys(user)); // name
```

## 変更できない(Non-configurable)

組み込みオブジェクトやプロパティに対しては、変更不能フラグ（`configurable:false`）がプリセットされることがあります。

変更できないプロパティは `defineProperty` で削除したり変更することができません。

例えば、`Math.PI` は読み取り専用で、列挙不可であり、変更不能です。:

```js run
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```
したがって、プログラマーは `Math.PI` の値を変えることも上書きすることもできません。

```js run
Math.PI = 3; // Error

// delete Math.PI もまた動作しません
```

変更不能なプロパティを作ることは一方通行です。それを戻すことはできません。なぜなら `defineProperty` は変更不能なプロパティでは動作しないためです。

ここでは、 `user.name` を "永遠に密封された" 定数にしています:

```js run
let user = { };

Object.defineProperty(user, "name", {
  value: "John",
  writable: false,
  configurable: false
});

*!*
// user.name またはそのフラグを変更することはできません
// これらすべて動作しません:
//   user.name = "Pete"
//   delete user.name
//   defineProperty(user, "name", ...)
Object.defineProperty(user, "name", {writable: true}); // Error
*/!*
```

```smart header="use strict の場合にのみエラーとなります"
非 strict mode では、読み取り専用プロパティなどに書き込むときにエラーは発生しません。 しかし、操作は成功しません。フラグ違反の操作は、非 strict では無視されます。
```

## Object.defineProperties

一度に多くのプロパティが定義できるメソッド [Object.defineProperties(obj, descriptors)](mdn:js/Object/defineProperties)もあります。

構文は次の通りです:

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

例えば:

```js
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

なので、一度に多くのプロパティをセットできます。

## Object.getOwnPropertyDescriptors

一度にすべてのプロパティのディスクリプタを取得するには、[Object.getOwnPropertyDescriptors(obj)](mdn:js/Object/getOwnPropertyDescriptors) を使うことができます。

`Object.defineProperties` と合わせて、オブジェクトをクローンする "フラグを意識した" 方法として使うことができます。:

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

通常、私たちがオブジェクトをクローンするとき、次のようにプロパティをコピーするために代入を使います。:

```js
for (let key in user) {
  clone[key] = user[key]
}
```

...ですが、これはフラグはコピーしません。なので、"より良い" クローンを望むなら、 `Object.defineProperties` が優先されます。

もう１つの違いは、`for..in` はシンボルプロパティを無視しますが、`Object.getOwnPropertyDescriptors` はシンボリックなものを含む *すべての* プロパティディスクリプタを返します。

## グローバルにオブジェクトを隠す

プロパティディスクリプタは個々のプロパティのレベルで動作します。

そこには、オブジェクト *全体* へのアクセスを制限するメソッドもあります。:

[Object.preventExtensions(obj)](mdn:js/Object/preventExtensions)
: オブジェクトにプロパティを追加するのを禁止します。

[Object.seal(obj)](mdn:js/Object/seal)
: プロパティの追加、削除を禁止し、既存のすべてのプロパティに `configurable: false` をセットします。

[Object.freeze(obj)](mdn:js/Object/freeze)
: プロパティの追加、削除、変更を禁止し、既存のすべてのプロパティに `configurable: false, writable: false` をセットします。

また、それらを確認する方法もあります:

[Object.isExtensible(obj)](mdn:js/Object/isExtensible)
: プロパティの追加が禁止されている場合に `false` を返します。それ以外は `true` です。

[Object.isSealed(obj)](mdn:js/Object/isSealed)
: プロパティの追加、削除が禁止されており、すべての既存のプロパティが `configurable: false` を持っている場合に `true` を返します。

[Object.isFrozen(obj)](mdn:js/Object/isFrozen)
: プロパティの追加、削除、変更が禁止されており、すべての現在のプロパティが `configurable: false, writable: false` の場合に `true` を返します。

これらのメソッドは実際にはめったに使われません。
