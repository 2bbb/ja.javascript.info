<<<<<<< HEAD
`new Date` コンストラクタはデフォルトでローカルのタイムゾーンを使います。なので、覚えておくべき重要なことは 月 はゼロからスタートすることだけです。

2月は数値で 1 になります。
=======
The `new Date` constructor uses the local time zone. So the only important thing to remember is that months start from zero.

So February has number 1.
>>>>>>> b0464bb32c8efc2a98952e05f363f61eca1a99a2

```js run
let d = new Date(2012, 1, 20, 3, 12);
alert( d );
```
