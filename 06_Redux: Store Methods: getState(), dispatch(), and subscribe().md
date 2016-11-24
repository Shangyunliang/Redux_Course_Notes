# 06. Store Methods: `getState()`, `dispatch()`, and `subscribe()`
[Video Link](https://egghead.io/lessons/javascript-redux-store-methods-getstate-dispatch-and-subscribe)

```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/3.0.4/redux.js"></script>
  <title>JS Bin</title>
</head>
<body>
</body>
</html>
```

```JavaScript
const counter = (state = 0, action) => {
   switch(action.type) {
     case 'INCREMENT':
       return state + 1;
     case 'DECREMENT':
       return state - 1;
     default:
       return state;
   }
}

const { createStore } =　Redux;
// var createStore = Redux.createStore;
// import { createStore } from 'redux';
const store = createStore(counter);

const render = () => {
   document.body.innerText = store.getState();
};


store.subscribe(render);
render();

document.addEventListener('click',() => {
  store.dispatch({type: 'INCREMENT'});
});
```


#### `store` 有三个重要的方法:
1. `getState()` retrieves the current state of the Redux store. If we ran `console.log(store.getState())` with the code above, we could get `0` since it is the initial state of our application.
1. `getState()` 会得到Redux store中当前的state。如果我们运行`console.log(store.getState())`。会得到0. 因为这个state的初始化状态是0
2. `dispatch()` is the most commonly used. It is how we dispatch actions to change the state of the application. If we run `store.dispatch( { type: 'INCREMENT' });` followed by `console.log(store.getState());` we will get `1`。
2. `dispatch()`是用到最多最普遍的方法。他告诉我们如何dispatch action去更改这个应用中的state。如果我们运行`store.dispatch( { type: 'INCREMENT' });` 然后紧跟着`console.log(store.getState());` 我们会得到`1`
3. `subscribe()` registers a callback that the redux store will call any time an action has been dispatched so you can update the UI of your application to reflect the current application state.
3. `subscribe()`注册了一个回调，以便Redux store可以任何时间都能调用任何已经被dispatched的action。所以你可以更新你的UI会反应在你应用的state中。
```JavaScript
