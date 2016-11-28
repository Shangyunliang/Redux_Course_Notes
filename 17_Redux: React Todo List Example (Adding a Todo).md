# 17. React Todo List Example (Adding a Todo)
[Video Link](https://egghead.io/lessons/javascript-redux-react-todo-list-example-adding-a-todo)

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


const { combineReducers } = Redux;
const todoApp = combineReducers({
  todos: todos,
  visibilityFilter: visibilityFilter
});

const { createStore } = Redux;
const store = createStore(todoApp);

const { Component } = React;


let nextTodoId = 0;
class TodoApp extends Component {
  render() {
    return (
      <div>
       <input ref={node => {this.input = node;}}/>
        <button onClick={() => {
            store.dispatch({
                type: 'ADD_TODO',
                text: this.input.value,
                id: nextTodoId++
          });
          this.input.value = '';
        }}>
           Add Todo
        </button>
        <ul>
             {this.props.todos.map(todo =>
               <li key={todo.id}>
                   {todo.text}
               </li>
              )}
         </ul>
      </div>
    );
  }
}


const render = () => {
  ReactDOM.render(
    <TodoApp 
        todos={store.getState().todos}
    />,
    document.getElementById('root')
  );
};

store.subscribe(render); //第一次什么都不显示
render(); //一开始，显示0
```

#### Recap of how the application works:

We start with the `TodoApp` component. This component isn't aware of how todos are being added, but what it can do is express its desire to mutate the state by dispatching an action with a type of `'ADD_TODO'`.
   
The `text` field for the todo item to be added is taken from the input box, along with an incrementing `id` for the todo item's id.

It is common for React components to dispatch actions in Redux apps, however it's equally important to be able to render the current state.

The `TodoApp` component assumes that it will receive `todos` as a prop, and it maps the items to display a list, using the `id` as a key (see the `<ul>` section in `TodoApp`).

We render the `TodoApp` component inside the `render()` function that runs any time the state changes (as well as when the app is initilized.) The `render()` function reads the current state of the store and passes the array of todos to the TodoApp component as a prop via the line `<TodoApp todos={store.getState().todos} />`.

The `render()` function is called every time there is a change to the store, so the `todos` prop is always up to date.

#### 复习一下这个应用是如何工作的：

我们通过`TodoApp`进入。 这个组件不知道todos如何被添加，但是它能够通过dispatching一个type为 `'ADD_TODO'`的action来展示更改state的能力。

这个`text`域是为了todo的元素能够从input输入框中拿出来添加，并且todo的id是递增的。

对于Redux的APP来说，React的组件去dispatch一个action是很常见的，然而能够渲染出当前的state也同样重要。

这个`TodoApp`组件假定收到了`todos`这个属性，然后遍历他们的元素展示出来，使用`id` 作为 key(see the `<ul>` section in `TodoApp`)

我们渲染这个`TodoApp`组件， 组件中的render（）方法，只要state改变（还有当APP初始化的时候），这个`render()` 方法就会读取当前store中的state然后把todos这个array作为属性传递到TodoApp。见`<TodoApp todos={store.getState().todos} />`

`render()`方法每当store改变的时候都会被调用，所以`todos`属性一直会被更新。


#### Recap of how mutations work in Redux:
Any change to state is caused by a `store.dispatch()` call somewhere in the component.

When an action is dispatched, the `store` calls the reducer it was created with with the current state & the action being dispatched. In the case of this example, this is the `todoApp` reducer that we obtained by `const todoApp = combineReducers({todos, visibilityFilter})`.

Continuing with our example, the `'ADD_TODO'` action type is matched in the switch statement inside the `todos()` reducer, so the child `todo()` reducer is called. The `todo()` child reducer is passed `undefined` (because there is no `state` for a new todo) and the action `'ADD_TODO'` 

Inside of the `todo()` child reducer, we have a similar switch statement. Since `'ADD_TODO'` is matched, the reducer returns the initial state of the todo item (the `id` from `nextTodoId++` and `text` from the input box inside the `TodoApp` component, along with `completed: false`).

The `todos()` reducer that just called the child `todo()` reducer will then return a new array built from the existing items along with the newly created item added to the end (remember, this array is built using ES6's `...` spread operator). 

Now our combined reducer `todoApp` will use this new `todos` array as the new value for the `todos` field in the global state object. So, it's going to return a new `state` object where the `todos` field corresponds to the array with the newly added todo item.

The `todoApp` reducer is the **root reducer** in this application. It is the one the store was created with, so its next state is the next state of the Redux store, and all the listeners are notified.

The `render()` function is subscribed to the store's changes, so it is called again and gets the fresh state with `store.getState()` and passes the updated `todos` as a prop to the `TodoApp` component.

Now the cycle can be repeated.

#### 复习一下Redux工作中是如何变化的：

任何state的改变都是由`store.dispatch()`方法在组件中某处调用引起的。

当一个action被dispatch，`store`都会调用reducer，这个reducer是由当前的state和被dispatch的action组成的。 在这个例子中， `todoApp` reducer我们通过`const todoApp = combineReducers({todos, visibilityFilter})`这局获得。

继续看我们的例子，这个`'ADD_TODO'`action type跟`todos()` 这个reducer中的switch匹配，所以他的子reducer也被调用了。 这个`todo()` reducer被传进去`undefined`(因为state中没有todo)，而action类型是 `'ADD_TODO'`

在`todo()` 这个子 reducer 中，我们有一个相似的switch语句，因为 `'ADD_TODO'`已经匹配到了， 这个reducer会返回todo item的初始值（the `id` from `nextTodoId++` and `text` from the input box inside the `TodoApp` component, along with `completed: false`）。

这个`todos()` reducer 只是调用子reducer`todo()`，依照现有的元素构建一个新的数组，并且把新的元素添加在数组之后。(remember, this array is built using ES6's `...` spread operator). 

现在我们`todoApp`将一个新的`todos`放入全局的state对象中作为`todos`的value。所以，返回一个新的state对象，`todos`比原来多了一条新数据。

在这个应用中，`todoApp` reducer 是**root reducer** 。 他是由store创造的， 所以他的下一个state是Redux中store的下一个state，所有的listeners都是唤醒了。

这个`render()` 方法被store的改变绑定，所以他会被调用，然后通过 `store.getState()`刷新，并且把新的`todos`作为参数传递给`TodoApp` 组件。

现在这个流程就跑起来了。

