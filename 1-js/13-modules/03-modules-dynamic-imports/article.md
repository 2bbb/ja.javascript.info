
<<<<<<< HEAD
# Dynamic imports(ダイナミックインポート)

前のチャプターで説明したエクスポートとインポート文は "static(静的)" と呼ばれます。

それらは確かに静的であり、構文は非常に厳密です。

静的な場合、まず `import` の任意のパラメータを動的に生成することはできません。

モジュールパスはプリミティブな文字列でなければならず、関数呼び出しもできません。これは動作しません。:

```js
import ... from *!*getModuleName()*/!*; // Error, string だけが許可されています
```

次に、条件に応じたインポートや、実行時にインポートすることはできません。:

```js
if(...) {
  import ...; // Error, 許可されていません!
}

{
  import ...; // Error, 任意のブロックに import を置くことはできません
}
```

これは、インポート/エクスポートはコード構造のバックボーンを提供することを目的としているためです。コード構造は分析することができ、モジュールを集め一緒にまとめることができ、未使用のエクスポートを除去する(tree-shaken)ことができるので、これは素晴らしいことです。これはすべてが固定されているがゆえに可能なことです。

しかし、どのようにしてモジュールを動的にオンデマンドでインポートするのでしょう？

## import() 関数

`import(module)` 関数はどこからでも呼び出すことができます。これはモジュールオブジェクトになる promise を返します。

使用パターンは次のようになります:
=======
# Dynamic imports

Export and import statements that we covered in previous chapters are called "static".

That's because they are indeed static. The syntax is very strict.

First, we can't dynamically generate any parameters of `import`.

The module path must be a primitive string, can't be a function call. This won't work:

```js
import ... from *!*getModuleName()*/!*; // Error, only from "string" is allowed
```

Second, we can't import conditionally or at run-time:

```js
if(...) {
  import ...; // Error, not allowed!
}

{
  import ...; // Error, we can't put import in any block
}
```

That's because `import`/`export` aim to provide a backbone for the code structure. That's a good thing, as code structure can be analyzed, modules can be gathered and bundled together, unused exports can be removed ("tree-shaken"). That's possible only because the structure of imports/exports is simple and fixed.

But how can we import a module dynamically, on-demand?

## The import() function

The `import(module)` function can be called from anywhere. It returns a promise that resolves into a module object.

The usage pattern looks like this:
>>>>>>> be342e50e3a3140014b508437afd940cd0439ab7

```js run
let modulePath = prompt("Module path?");

import(modulePath)
  .then(obj => <module object>)
  .catch(err => <loading error, no such module?>)
```

<<<<<<< HEAD
あるいは、async function 内であれば `let module = await import(modulePath)` とすることができます。

このようになります:

[codetabs src="say" current="index.html"]

したがって、ダイナミックインポートは非常に簡単に使用できます。

また、ダイナミックインポートは通常のスクリプトで動作するので、`script type="module"` は必要ありません。
=======
Or, we could use `let module = await import(modulePath)` if inside an async function.

For instance, if we have the following `say.js`:

```js
// 📁 say.js
export function hi() {
  alert(`Hello`);
}

export function bye() {
  alert(`Bye`);
}
```

...Then dynamic import can be like this:

```js
let {hi, bye} = await import('./say.js');

hi();
bye();

```

Or, for the default export:

```js
// 📁 say.js
export default function() {
  alert("Module loaded (export default)!");
}
```

To import it, we need to get `default` property of the module object, as explained in the [previous chapter](info:import-export).

So, the dynamic import will be like this:

```js
let {default: say} = await import('./say.js'); // map .default to say variable

say();
```

Here's the full example:

[codetabs src="say" current="index.html"]

So, dynamic imports are very simple to use, and they allow to import modules at run-time.

Also, dynamic imports work in regular scripts, they don't require `script type="module"`.

```smart
Although `import()` looks like a function call, it's a special syntax that just happens to use parentheses (similar to `super()`).

That means that import doesn't inherit from `Function.prototype` so we cannot call or apply it.
```
>>>>>>> be342e50e3a3140014b508437afd940cd0439ab7
