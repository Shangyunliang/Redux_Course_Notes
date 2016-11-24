# 07. Implementing Store from Scratch
[Video Link](https://egghead.io/lessons/javascript-redux-implementing-store-from-scratch)

>这是一个store的简单实现

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

const createStore = (reducer) => {
  let state;
  let listeners = [];

  const getState = () => state;

  const dispatch = (action) => {
      state = reducer(state, action);
      listeners.forEach(listener => listener());
  };
  
  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      //console.log(listeners);
      listeners = listeners.filter(l => l !== listener);
      //console.log(listeners);
    };
  };
  
  dispatch({});
  
  return { getState, dispatch, subscribe };
};


const store = createStore(counter);

const render = () => {
   document.body.innerText = store.getState();
};


//const unsubscribe = store.subscribe(render);
//unsubscribe();
store.subscribe(render);
render();

document.addEventListener('click',() => {
  store.dispatch({type: 'INCREMENT'});
});
```
