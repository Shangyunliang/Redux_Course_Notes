# 11. Writing a Todo List Reducer (Adding a Todo)
[Video Link](https://egghead.io/lessons/javascript-redux-writing-a-todo-list-reducer-adding-a-todo)

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
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ];
      default:
        return state;
  }
};

const testAddTodo = () => {
  const stateBefore = [];
  const action = {
    type: 'ADD_TODO',
    id: 0,
    text: 'Learn Redux'
  };
  const stateAfter = [
    {
      id: 0,
      text: 'Learn Redux',
      completed: false
    }
  ];
  
  deepFreeze(stateBefore);
  deepFreeze(action);

  expect(
    todos(stateBefore, action)
  ).toEqual(stateAfter);
};



testAddTodo();
console.log("All test passed");
```

1. 首先，我将空数组`stateBefore`,`action`作为参数传给`reducer`.

1. 接着，`reducer`接收到`state` 和 `action`.然后 查看`action`的`type`

1. 在这个例子里，`action`的 `type` 是 `ADD_TODO` 然后在`switch`的`case`中找到对应处理方式，返回一个新的数组，该数组包含原有的`state`对象，以及一个新加入的对象。

1. 但是由于我们传入的是一个空数组，所以新的数组中只会有一个对象，但是这确实是一个全新的数组，而不是传入的那个。

1. 最后我们比较两个数组的对象是一样的，这说明redux是按照我们的预想工作的。
