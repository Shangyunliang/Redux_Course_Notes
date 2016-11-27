# 10. Avoiding Object Mutations with `Object.assign()` and `...spread`
[Video Link](https://egghead.io/lessons/javascript-redux-avoiding-object-mutations-with-object-assign-and-spread)

```HTML
<!DOCTYPE html>
<html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <script src="https://cdn.bootcss.com/react/15.4.0/react.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/3.0.4/redux.js"></script>
      <script src="https://cdn.bootcss.com/react/15.4.0/react-dom.js"></script>
      <script src="https://wzrd.in/standalone/expect@latest"></script>
      <script src="https://wzrd.in/standalone/deep-freeze@latest"></script>
      <title>JS Bin</title>
    </head>
    <body>
          <div id='root'></div>
    </body>
</html>
```

```JavaScript
/*这种方式不可以因为修改了原来的对象属性**/
// const toggleTodo = (todo) => {
//   todo.completed = !todo.completed;
//   return todo;
// };

/*可以，返回一个新对象**/
// const toggleTodo = (todo) => {
//   return {
//     id: todo.id,
//     text: todo.text,
//     completed: !todo.completed
//   };
// };

/*可以，assign拷贝一个新对象，如果后面出现了多个属性重复多次，属性值不同，以最后一个为准。覆盖前面**/
// const toggleTodo = (todo) => {
//   return Object.assign({}, todo, {
//     completed: !todo.completed
//   });
// };

/*可以，这种...方式在es7中，目前使用babel支持**/
const toggleTodo = (todo) => {
  return {
    ...todo,
    completed: !todo.completed
  };
};

const testToggleTodo = () => {
  const todoBefore = {
    id: 0,
    text: 'Learn Redux',
    completed: false
  };
  const todoAfter = {
    id: 0,
    text: 'Learn Redux',
    completed: true
  };
  
  deepFreeze(todoBefore);
  
  expect(
    toggleTodo(todoBefore)
  ).toEqual(todoAfter);
};

testToggleTodo();
console.log('All tests passed.');
```

> 这一小节是针对传递对象时候，如何再不修改原有对象下，返回新的对象。因为redux中，要求pure function。
