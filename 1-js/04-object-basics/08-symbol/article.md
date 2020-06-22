
# シンボル型

仕様によると、オブジェクトのプロパティのキーは文字列型、もしくはシンボル型のいずれかです。数値ではなく、真偽値でもなく、文字列またはシンボル、それら2つの型だけです。

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
私たちはこれまで文字列だけ見てきました。今シンボルが我々に与えてくれるアドバンテージを見てみましょう。

[cut]
=======
Till now we've been using only strings. Now let's see the benefits that symbols can give us.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

## シンボル 

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
"シンボル" 値はユニークな識別子を表現します。
=======
A "symbol" represents a unique identifier.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

このタイプの値は、`Symbol()` を使って作ることができます:

```js
// id は新しい symbol です
let id = Symbol();
```

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
また、シンボルに説明を与えることもでき(シンボル名と呼びます)、デバッグ目的で便利です。
=======
Upon creation, we can give symbol a description (also called a symbol name), mostly useful for debugging purposes:
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

```js
// id は "id" という説明を持つ symbol です
let id = Symbol("id");
```

シンボルはユニークであることが保証されます。たとえ同じ説明で複数のシンボルを作ったとしても、それらは異なる値です。説明は何にも影響を与えない単なるラベルです。

例えば、ここでは同じ説明をもつ2つのシンボルがあります -- これらは等しくありません:

```js run
let id1 = Symbol("id");
let id2 = Symbol("id");

*!*
alert(id1 == id2); // false
*/!*
```

Rubyもしくは他の言語にも慣れ親しんでいる人は、 -- 間違ってはいけません。 JavaScriptのシンボルは異なります。

````warn header="Symbols は文字列への自動変換はしません"
JavaScriptにおいて、ほとんどの値は文字列への暗黙の変換をサポートしています。例えば、任意の値で `alert` を呼びだすと、たいていの値は動作します。が、シンボルは特別です。それらは自動変換されません。

例えば、この `alert` はエラーになります:

```js run
let id = Symbol("id");
*!*
alert(id); // TypeError: Cannot convert a Symbol value to a string
*/!*
```

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
もし本当にシンボルを表示したい場合は、このように `toString()` を呼ぶ必要があります:
=======
That's a "language guard" against messing up, because strings and symbols are fundamentally different and should not accidentally convert one into another.

If we really want to show a symbol, we need to explicitly call `.toString()` on it, like here:
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
```js run
let id = Symbol("id");
*!*
alert(id.toString()); // Symbol(id), これは動作します
*/!*
```

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
これは、文字列とシンボルが根本的に異なるものであり、それを変換すべきではないため、混乱を避けるための「言語によるガード」です。
=======
Or get `symbol.description` property to show the description only:
```js run
let id = Symbol("id");
*!*
alert(id.description); // id
*/!*
```

>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
````

## "隠れた" プロパティ 

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
シンボルを使うと、オブジェクトに "隠れた" プロパティを作ることができます。他のコードがアクセスしたり上書きしたりすることはありません。

例えば、オブジェクト `user` に "識別子" を格納したい場合、そのキーとしてシンボルを使うことができます:
=======
Symbols allow us to create "hidden" properties of an object, that no other part of code can accidentally access or overwrite.

For instance, if we're working with `user` objects, that belong to a third-party code. We'd like to add identifiers to them.

Let's use a symbol key for it:
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

```js run
let user = { // belongs to another code
  name: "John"
};

let id = Symbol("id");

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
user[id] = "ID Value";
alert( user[id] ); // キーとして symbol を使ってデータにアクセスできます
```

文字列 `"id"`に対して `Symbol（"id"）` を使うことの利点は何でしょうか？

それを知るため、より深い例を作ってみましょう。

別のスクリプトが、独自の目的のために自身の "id" プロパティを `user` の中に持ちたいとします。それは別のJavaScriptライブラリの場合もあり、スクリプトは完全にお互いを認識していない状況とします。
=======
user[id] = 1;

alert( user[id] ); // we can access the data using the symbol as the key
```

What's the benefit of using `Symbol("id")` over a string `"id"`?

As `user` objects belongs to another code, and that code also works with them, we shouldn't just add any fields to it. That's unsafe. But a symbol cannot be accessed accidentally, the third-party code probably won't even see it, so it's probably all right to do.

Also, imagine that another script wants to have its own identifier inside `user`, for its own purposes. That may be another JavaScript library, so that the scripts are completely unaware of each other.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

そして、そのスクリプトは自身の `Symbol("id")` を作ります。:

```js
// ...
let id = Symbol("id");

user[id] = "Their id value";
```

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
たとえ同じ名前でもシンボルは常に異なるため、衝突は起こりません。

今、同じ目的のためにシンボルの代わりに文字列 `"id"` を使ったとすると、衝突が発生する *かもしれません*。
=======
There will be no conflict between our and their identifiers, because symbols are always different, even if they have the same name.

...But if we used a string `"id"` instead of a symbol for the same purpose, then there *would* be a conflict:
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

```js run
let user = { name: "John" };

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
// 我々のスクリプトは　"id" プロパティを使います
user.id = "ID Value";

// ...もし後で別のスクリプトが別の目的で "id" を使ったら...

user.id = "Their id value"
// boom! 上書きされます! 同僚に危害を加えるつもりはありませんでした。が、してしまいました!
=======
// Our script uses "id" property
user.id = "Our id value";

// ...Another script also wants "id" for its purposes...

user.id = "Their id value"
// Boom! overwritten by another script!
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
```

### リテラルのシンボル

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
オブジェクトリテラルの中でシンボルを使いたい場合は、角括弧で囲む必要があります。
このように:
=======
If we want to use a symbol in an object literal `{...}`, we need square brackets around it.

Like this:
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

```js
let id = Symbol("id");

let user = {
  name: "John",
*!*
<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
  [id]: 123 // 単に "id: 123" ではありません
=======
  [id]: 123 // not "id: 123"
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
*/!*
};
```
キーとして、変数 `id` の値が必要であり、文字列 "id" ではないからです。

### シンボルは for..in ではスキップされます。

シンボリックなプロパティは `for..in` ループに参加しません。

例:

```js run
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

*!*
for (let key in user) alert(key); // name, age (no symbols)
*/!*

// symbol による直アクセスは動作します
alert( "Direct: " + user[id] );
```

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
これは一般的な "隠れている" というコンセプトの一部です。もし別のスクリプトかライブラリが我々のオブジェクトをループした際に、予期せずシンボリックプロパティにアクセスすることはありません。
=======
`Object.keys(user)` also ignores them. That's a part of the general "hiding symbolic properties" principle. If another script or a library loops over our object, it won't unexpectedly access a symbolic property.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

一方で、[Object.assign](mdn:js/Object/assign) は文字列とシンボルプロパティ両方をコピーします;

```js run
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);

alert( clone[id] ); // 123
```

ここにはパンドラの箱はありません。これは設計されたものです。この考えは、オブジェクトをクローンしたりマージするとき、通常 *全ての* プロパティがコピーされることを望みます( `id` のようなシンボルも含めて)。

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
````smart header="他の型のプロパティキーは文字列に強制されます"
オブジェクトでは、キーとして文字列かシンボルのみを使うことができます。他の型は文字列に変換されます。

例えば、プロパティのキーとして 数値 `0` を使ったとき、文字列 `"0"` になります。

```js run
let obj = {
  0: "test" // same as "0": "test"
};

// 両方のアラートは同じプロパティにアクセスします(数値 0 は文字列 "0" に変換されます)
alert( obj["0"] ); // test
alert( obj[0] ); // test (同じプロパティ)
```
````

## グローバルシンボル 

これまで見てきたように、通常はたとえ同じ名前であったとしてもすべてのシンボルは異なります。しかし、時には同じ名前のシンボルを同じエンティティにしたいときがあります。

例えば、我々のアプリケーションの異なる部分が、正確に同じプロパテイを意味するシンボル `"id"` にアクセスしたいとします。
=======
## Global symbols

As we've seen, usually all symbols are different, even if they have the same name. But sometimes we want same-named symbols to be same entities. For instance, different parts of our application want to access symbol `"id"` meaning exactly the same property.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

それを達成するために、*グローバルシンボルレジストリ* があります。その中でシンボルを作り、後でそれらにアクセスすることができます。同じ名前への繰り返しアクセスは、まったく同じシンボルを返すことが保証されます。

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
レジストリ内でシンボルを作ったり読み込むためには、`Symbol.for(key)` を使います。
=======
In order to read (create if absent) a symbol from the registry, use `Symbol.for(key)`.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

これは、グローバルレジストリのチェックを呼び出し、もし `key` として記述されたシンボルが存在する場合には、それを返します。そうでない場合には、新しいシンボル `Symbol(key)` を作り、与えられた `key` で、レジストリ内に格納されます。

例:

```js run
// グローバルレジストリから読む
let id = Symbol.for("id"); // symbol が存在しない場合、作られます

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
// 再度読み込み
=======
// read it again (maybe from another part of the code)
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
let idAgain = Symbol.for("id");

// 同じシンボル
alert( id === idAgain ); // true
```

レジストリ内のシンボルは *グローバルシンボル* と呼ばれます。コード内のどこにでもアクセス可能なアプリケーション全体のシンボルが必要な場合、これを使います。

```smart header="これは Ruby のようです"
Rubyのようないくつかのプログラミング言語では、名前毎に1つのシンボルがあります。

JavaScriptでは、ご覧の通りそれはグローバルシンボルのことです。
```

### Symbol.keyFor

グローバルシンボルでは、`Symbol.for(key)` は名前によってシンボルを返すだけでなく、逆方向の呼び出しもあります:
`Symbol.keyFor(sym)`, これは逆のことをします: グローバルシンボルを元に、名前を返します。

例:

```js run
// get symbol by name
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
// symbol から name を取得
=======
// get name by symbol
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
`Symbol.keyFor` は内部ではそのシンボルのキーを探すためにグローバルシンボルレジストリを使っています。従って、非グローバルのものに対しては動作しません。もしシンボルが非グローバルの場合、見つけることはできず、`undefined` を返します。
=======
The `Symbol.keyFor` internally uses the global symbol registry to look up the key for the symbol. So it doesn't work for non-global symbols. If the symbol is not global, it won't be able to find it and returns `undefined`.

That said, any symbols have `description` property.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

例:

```js run
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, global symbol
alert( Symbol.keyFor(localSymbol) ); // undefined, not global

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
alert( Symbol.keyFor(Symbol("name2")) ); // undefined, 引数はグローバルシンボルではありません
=======
alert( localSymbol.description ); // name
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
```

## システムシンボル 

JavaScriptが内部的に使用する多くの "システム" シンボルが存在し、それを使うことでオブジェクトの様々な側面を微調整することができます。

それらは[よく知られているシンボル](https://tc39.github.io/ecma262/#sec-well-known-symbols) テーブルの仕様にリストされています。

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- ...等.

例えば、`Symbol.toPrimitive` はオブジェクトからプリミティブへの変換を記述することができます。私たちはすぐにそれを使うところを見ていきます。

他のシンボルについても、該当する言語の機能を学ぶときに分かるようになるでしょう。

## サマリ 

`Symbol` はプリミティブ型で、ユニークな識別子のためのものです。

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
シンボルはオプションの記述と合わせて呼ばれる `Symbol()` で生成されます。

シンボルは、たとえ同じ名前を持ったとしても常に異なった値です。もし同じ名前のシンボルを同じものにしたいなら、グローバルレジストリを使う必要があります: `Symbol.for(key)` は名前として `key` をもつグローバルシンボルを返します(必要なら作ります)。`Symbol.for` の複数回の呼び出しは全く同じシンボルを返します。
=======
Symbols are created with `Symbol()` call with an optional description (name).

Symbols are always different values, even if they have the same name. If we want same-named symbols to be equal, then we should use the global registry: `Symbol.for(key)` returns (creates if needed) a global symbol with `key` as the name. Multiple calls of `Symbol.for` with the same `key` return exactly the same symbol.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

シンボルは2つの主なユースケースがあります:

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
1. "隠れた" オブジェクトのプロパティ。
    もし別のスクリプト、またはライブラリに "属している" オブジェクトにプロパティを追加したい場合、シンボルを作り、プロパティのキーとしてそれを使うことができます。シンボリックなプロパティは `for..in` には現れないため、リストされることはありません。また、直接アクセスされることもありません。なぜなら、別のスクリプトは我々のシンボルを持っていないため、そのアクションに介入することはできません。
=======
1. "Hidden" object properties.
    If we want to add a property into an object that "belongs" to another script or a library, we can create a symbol and use it as a property key. A symbolic property does not appear in `for..in`, so it won't be accidentally processed together with other properties. Also it won't be accessed directly, because another script does not have our symbol. So the property will be protected from accidental use or overwrite.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md

    従って、シンボリックプロパティを使うことで、必要なオブジェクトに何かを "こっそり" 隠すことができます。そしてそれは他人には見えません。

2. `Symbol.*` としてアクセス可能なJavaScriptで使われている多くのシステムシンボルがあります。いくつかの組み込みの振る舞いを変更するためにそれらを使うことができます。例えば、チュートリアルの後半で[iterables](info:iterable)のための `Symbol.iterator`, [object-to-primitive conversion](info:object-toprimitive)を設定するための `Symbol.toPrimitive` などを使います。

<<<<<<< HEAD:1-js/04-object-basics/03-symbol/article.md
技術的には、シンボルは100%隠れる訳ではありません。全てのシンボルを取得する組み込み関数[Object.getOwnPropertySymbols(obj)](mdn:js/Object/getOwnPropertySymbols) があります。また、シンボリックなものも含めてオブジェクトの *全ての* キーを返す[Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys)と呼ばれる関数もあります。なので、それらは本当に隠れてはいません。しかしほとんどのライブラリや組み込み関数、構文構造は共通の合意に忠実です。そして、前述の方法を明示的に呼び出す人は、それらがやっていることをよく理解しているでしょう。
=======
Technically, symbols are not 100% hidden. There is a built-in method [Object.getOwnPropertySymbols(obj)](mdn:js/Object/getOwnPropertySymbols) that allows us to get all symbols. Also there is a method named [Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys) that returns *all* keys of an object including symbolic ones. So they are not really hidden. But most libraries, built-in functions and syntax constructs don't use these methods.
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa:1-js/04-object-basics/08-symbol/article.md
