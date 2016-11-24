# 03. Pure and Impure Functions
[Video Link](https://egghead.io/lessons/javascript-redux-pure-and-impure-functions)

>Pure function 没有明显的副作用。如访问网络，或者调用数据库等。Pure function只包含计算数据。可以确信的是如果你用了一组相同的输入参数，你会得到相同的返回值，这是可以预测的。

>另外，Pure function 不会修改传入进去的值，如果squareAll，她接收了一组参数items，但是它在内部不会复写他。相反，会用map创建一个新的arry返回。

**Pure:**
```JavaScript
function square(x) {
  return x * x;
}
function squareAll(items) {
  return items.map(square);
}
```

>Impure function 在内部可能会去访问网络或者修改数据，他们有一些其他操作，会间接影响到其他状态。它们会操作DOM，会修改传入进去的值。

**Impure:**
```JavaScript
function square(x) {
  updateXInDatabase(x);
  return x * x;
}
function squareAll(items) {
  for (let i = 0; i < items.length; i++) {
    items[i] = square(items[i]);
  }
}
```
