
<<<<<<< HEAD

`async` 呼び出しを Promise として扱い、それに `.then` をつけるだけです。

=======
That's the case when knowing how it works inside is helpful.

Just treat `async` call as promise and attach `.then` to it:
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa
```js run
async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  return 10;
}

function f() {
<<<<<<< HEAD
  // 1秒後に 10を表示
=======
  // shows 10 after 1 second
>>>>>>> e4e6a50b5762dd5dc4c0f0c58f870c64be39dcfa
*!*
  wait().then(result => alert(result));
*/!*
}

f();
```
