# Reducer Composition with `combineReducers()`
[Video Link](https://egghead.io/lessons/javascript-redux-reducer-composition-with-combinereducers)

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
//第一次执行不会进入到这一层 默认必须在一层reducer给
const todo = (state = [], action) => {
  console.log('todo init');  
  switch (action.type) {
    case 'ADD_TODO':
      return  {
          id: action.id,
          text: action.text,
          completed: false
        };
    case 'TOGGLE_TODO':        
        if (state.id !== action.id) {
          return state;
        }
        return {
          ...state,
          completed: !todo.completed
        };
    default:
        return state;
  }
};

//第一次进入到这一层找到state的默认值 必须给默认，否则崩溃
const todos = (state = [], action) => {
  console.log('todos init');  
  console.log(state);
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        todo(undefined,action)
      ];
   /*
   map方法返回一个新数组，拿出state中第一个参数，
   如果参数id和action中的id不相等，直接返回，如果相等，
   把comleted属性改掉，再返回。依次遍历。最后组成数组
   后返回。
   **/
    case 'TOGGLE_TODO':
      return state.map(t => todo(t,action));
  default:
        return state;
  };
};


const visibilityFilter = (
    state = 'SHOW_ALL',
    action
) => {
   console.log('visiblityFilter init');
    switch(action.type) {
      case 'SET_VISIBILITY_FILTER':
        return action.filter;
      default: 
        return state;
    }
};

/*
由于经常写这种形式的调用。Redux提供函数combineReducers
es6写法:
const todoApp = combineReducers({
  todos,
  visibilityFilter
});
**/

const { combineReducers } = Redux;
const todoApp = combineReducers({
  todos: todos,
  visibilityFilter: visibilityFilter
});

// const todoApp = (state = {}, action) => {
//   console.log(...state);
//   console.log(action);
//   /*
//   [object Object] {
//       type: "@@redux/INIT"
//   }
//   "todos init"
//   **/
//   return {
//      todos: todos(
//          state.todos,
//          action
//      ),
//     visibilityFilter: visiblityFilter(
//       state.visibilityFilter,
//       action
//     )
//   };
// };

const { createStore } = Redux;
const store = createStore(todoApp);



console.log('Initial state');
console.log(store.getState());
console.log('--------------');

console.log('Dispatching ADD_TODO.');
store.dispatch({
  type: 'ADD_TODO',
  id: 0,
  text: 'Shangyunliang'
});

console.log('Current state:');
console.log(store.getState());
console.log('---------------');

console.log('Dispatching ADD_TODO.');
store.dispatch({
  type: 'ADD_TODO',
  id: 1,
  text: 'Go shopping'
});
console.log('Current state:');
console.log(store.getState());
console.log('---------------');
console.log('---------------');

console.log('Dispatching TOGGLE_TODO.');
store.dispatch({
  type: 'TOGGLET_TODO',
  id:0
});
console.log('Current state:');
console.log(store.getState());
console.log('---------------');

console.log('Dispatching SET_VISIBILITY_FILITER');
store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
});

console.log('Current state:');
console.log(store.getState());
console.log('---------------');


console.log("All test passed");
```

> 这一节主要是讲了combineReducer，这个方法用来管理多个模块不同的的reducer。 

> 另外es6中中key value一样，可以只写key。
