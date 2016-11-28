# 18. React Todo List Example (Toggling a Todo)
[Video Link](https://egghead.io/lessons/javascript-redux-react-todo-list-example-toggling-a-todo)

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
          completed: !state.completed
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
               <li key={todo.id}
                  onClick={()=> {
                      store.dispatch({
                           type: 'TOGGLE_TODO',
                           id: todo.id
                          });
                   }}
                     style={{textDecoration:
                           todo.completed?
                           'line-through':
                           'none'
                           }}>
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

> 此小节添加了toggle_todo的功能。
