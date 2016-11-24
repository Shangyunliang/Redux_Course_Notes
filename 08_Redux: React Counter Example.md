# 08. React Counter Example
[Video Link](https://egghead.io/lessons/javascript-redux-react-counter-example)

>React Counter Example

```HTML
<!DOCTYPE html>
<html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
  
       <script src="https://fb.me/react-15.1.0.js"></script>
       <script src="https://fb.me/react-dom-15.1.0.js"></script>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/3.0.4/redux.js"></script>
       <title>JS Bin</title>
    </head>
    <body>
          <div id='root'></div>
    </body>
</html>
```
```JavaScript
const counter = (state = 0,action) => {
   switch(action.type) {
     case 'INCREMENT':
       return state + 1;
     case 'DECREMENT':
       return state - 1;
     default:
       return state;
   }
}


const Counter = ({
   value,
   onIncrement,
   onDecrement
}) => (
  <div>
     <h1>{value}</h1>
     <button onClick={onIncrement}>+</button>
     <button onClick={onDecrement}>-</button>
  </div>
);

const { createStore } = Redux;
const store = createStore(counter);

const render = () => {
  ReactDOM.render(
    <Counter
       value={store.getState()}
       onIncrement={()=>
                   store.dispatch({
                    type: 'INCREMENT'
                   })
        }
       onDecrement={()=>
                   store.dispatch({
                    type: 'DECREMENT'
                   })
        }
        />,
       document.getElementById('root')
  );
};

store.subscribe(render);
render();
```

#### To recap how it works...

When the `Counter` is rendered, we specify that its value should be taken from the Redux store's current state. When the user presses a button, the corresponding action is dispatched to the Redux store.

The reducer specifies how the next state is calculated based on the current state and the action being dispatched.

Finally, we subscribe to the Redux store so our `render()` function runs any time the state changes so our `Counter` gets the current state.

当我们的Counter被render，这里他的value值应该从Redux store中的state拿出来。当我们按下按钮时，相应的action会被dispatch到Redux tore中。

这个reducer 明确如何在当前state之上计算下一个state，以及action如何被转发。

最后，我们subscribe Redux store ， 这样state改变随时我们的render（）方法都会被调用。 所以我们的Counter能获取到当前的state。
