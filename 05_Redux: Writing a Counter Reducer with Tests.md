# 05. Writing a Counter Reducer with Tests
[Video Link](https://egghead.io/lessons/javascript-redux-writing-a-counter-reducer-with-tests)

```JavaScript
function counter(state,action) {
  if (typeof state === 'undefined') {
    return 0;
  }
  
  if(action.type === 'INCREMENT') {
    return state + 1;
  } else if (action.type === 'DECREMENT') {
    return state - 1;
  }
  return state;
}
}


expect(
  counter(0, { type: 'INCREMENT' })
).toEqual(1);

expect(
  counter(1, { type: 'INCREMENT' })
).toEqual(2);

expect(
  counter(2, { type: 'DECREMENT' })
).toEqual(1);

expect(
  counter(1, { type: 'DECREMENT' })
).toEqual(0);

expect(
  counter(1, { type: 'SOMETHING_ELSE' })
).toEqual(1);


expect(
  counter(undefined, {})
).toEqual(0);


console.log('Tests passed!');
```

## 注意！
> 1. Redux中如果出现了未知的actions。需要返回当前的state
> 1. Redux中对于undefined的state，action必须考虑返回state的初始值，在这个例子里是0
