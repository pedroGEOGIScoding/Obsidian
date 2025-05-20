[Hooks](https://react.dev/reference/react/hooks)
## useReducer

`useReducer` is a React Hook that lets you add a [reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer) to your component.
Components with many state updates spread across many event handlers can get overwhelming. For these cases, you can consolidate all the state update logic outside your component in a single function, called a _reducer.
``` javascript
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```
---
## Reference

### useReducer(reducer, initialArg, init?)

Call `useReducer` at the top level of your component to manage its state with a [reducer.](https://react.dev/learn/extracting-state-logic-into-a-reducer)

```JavaScript
import { useReducer } from 'react';  
function reducer(state, action) {  
// ...  
}  
function MyComponent() {  
const [state, dispatch] = useReducer(reducer, { age: 42 });  
// ...
```

#### Parameters
``` javascript
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```
- `reducer`: The reducer function that specifies how the state gets updated. It must be pure, should take the state and action as arguments, and should return the next state. State and action can be of any types.
- `initialArg`: The value from which the initial state is calculated. It can be a value of any type. How the initial state is calculated from it depends on the next `init` argument.
- **optional** `init`: The initializer function that should return the initial state. If it’s not specified, the initial state is set to `initialArg`. Otherwise, the initial state is set to the result of calling `init(initialArg)`.
#### Returns

`useReducer` returns an array with exactly two values:

1. The current state. During the first render, it’s set to `init(initialArg)` or `initialArg` (if there’s no `init`).
2. The [`dispatch` function](https://react.dev/reference/react/#dispatch) that lets you update the state to a different value and trigger a re-render.
#### Caveats
- `useReducer` is a Hook, so you can only call it **at the top level of your component** or your own Hooks. You can’t call it inside loops or conditions. If you need that, extract a new component and move the state into it.
- The `dispatch` function has a stable identity, so you will often see it omitted from Effect dependencies, but including it will not cause the Effect to fire. If the linter lets you omit a dependency without errors, it is safe to do.
- In Strict Mode, React will **call your reducer and initializer twice** in order to [help you find accidental impurities.](https://react.dev/reference/react/#my-reducer-or-initializer-function-runs-twice) This is development-only behavior and does not affect production. If your reducer and initializer are pure (as they should be), this should not affect your logic. The result from one of the calls is ignored.
---
### dispatch function

The `dispatch` function returned by `useReducer` lets you update the state to a different value and trigger a re-render. You need to pass the action as the only argument to the `dispatch` function:

React will set the next state to the result of calling the `reducer` function you’ve provided with the current `state` and the action you’ve passed to `dispatch`.
#### Parameters

- `action`: The action performed by the user. It can be a value of any type. By convention, an action is usually an object with a `type` property identifying it and, optionally, other properties with additional information.
#### Returns

`dispatch` functions do not have a return value.
#### Caveats

- The `dispatch` function **only updates the state variable for the *next* render**. If you read the state variable after calling the `dispatch` function, [you will still get the old value](https://react.dev/reference/react/#ive-dispatched-an-action-but-logging-gives-me-the-old-state-value) that was on the screen before your call.
- If the new value you provide is identical to the current `state`, as determined by an [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison, React will **skip re-rendering the component and its children.** This is an optimization. React may still need to call your component before ignoring the result, but it shouldn’t affect your code.
- React [batches state updates.](https://react.dev/learn/queueing-a-series-of-state-updates) It updates the screen **after all the event handlers have run** and have called their `set` functions. This prevents multiple re-renders during a single event. In the rare case that you need to force React to update the screen earlier, for example to access the DOM, you can use [`flushSync`.](https://react.dev/reference/react-dom/flushSync)

---
## Usage

### Adding a reducer to a component

Call `useReducer` at the top level of your component to manage state with a [reducer.](https://react.dev/learn/extracting-state-logic-into-a-reducer)

`useReducer` returns an array with exactly two items:

1. The current state of this state variable, initially set to the initial state you provided.
2. The `dispatch` function that lets you change it in response to interaction.

To update what’s on the screen, call `dispatch` with an object representing what the user did, called an *action*:

React will pass the current state and the action to your reducer function. Your reducer will calculate and return the next state. React will store that next state, render your component with it, and update the UI.

`useReducer` is very similar to [`useState`](https://react.dev/reference/react/useState), but it lets you move the state update logic from event handlers into a single function outside of your component. Read more about [choosing between `useState` and `useReducer`.](https://react.dev/learn/extracting-state-logic-into-a-reducer#comparing-usestate-and-usereducer)

---
### Writing the reducer function

A reducer function is declared like this:

Then you need to fill in the code that will calculate and return the next state. By convention, it is common to write it as a [`switch` statement.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) For each `case` in the `switch`, calculate and return some next state.

Actions can have any shape. By convention, it’s common to pass objects with a `type` property identifying the action. It should include the minimal necessary information that the reducer needs to compute the next state.

The action type names are local to your component. [Each action describes a single interaction, even if that leads to multiple changes in data.](https://react.dev/learn/extracting-state-logic-into-a-reducer#writing-reducers-well) The shape of the state is arbitrary, but usually it’ll be an object or an array.

Read [extracting state logic into a reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer) to learn more.

#### Basic useReducer examples

#### Example: Form (object)

In this example, the reducer manages a state object with two fields: `name` and `age`.

```javascript
import { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'incremented_age': {
      return {
        name: state.name,
        age: state.age + 1
      };
    }
    case 'changed_name': {
      return {
        name: action.nextName,
        age: state.age
      };
    }
  }
  throw Error('Unknown action: ' + action.type);
}

const initialState = { name: 'Taylor', age: 42 };

export default function Form() {
  const [state, dispatch] = useReducer(reducer, initialState);

  function handleButtonClick() {
    dispatch({ type: 'incremented_age' });
  }

  function handleInputChange(e) {
    dispatch({
      type: 'changed_name',
      nextName: e.target.value
    }); 
  }

  return (
    <>
      <input
        value={state.name}
        onChange={handleInputChange}
      />
      <button onClick={handleButtonClick}>
        Increment age
      </button>
      <p>Hello, {state.name}. You are {state.age}.</p>
    </>
  );
}
```

---
### Avoiding recreating the initial state

React saves the initial state once and ignores it on the next renders.

Although the result of `createInitialState(username)` is only used for the initial render, you’re still calling this function on every render. This can be wasteful if it’s creating large arrays or performing expensive calculations.

To solve this, you may **pass it as an *initializer* function** to `useReducer` as the third argument instead:

Notice that you’re passing `createInitialState`, which is the *function itself*, and not `createInitialState()`, which is the result of calling it. This way, the initial state does not get re-created after initialization.

In the above example, `createInitialState` takes a `username` argument. If your initializer doesn’t need any information to compute the initial state, you may pass `null` as the second argument to `useReducer`.

#### The difference between passing an initializer and passing the initial state directly

#### Example: Passing the initializer function

This example passes the initializer function, so the `createInitialState` function only runs during initialization. It does not run when component re-renders, such as when you type into the input.

```javascript
import { useReducer } from 'react';

function createInitialState(username) {
  const initialTodos = [];
  for (let i = 0; i < 50; i++) {
    initialTodos.push({
      id: i,
      text: username + "'s task #" + (i + 1)
    });
  }
  return {
    draft: '',
    todos: initialTodos,
  };
}

function reducer(state, action) {
  switch (action.type) {
    case 'changed_draft': {
      return {
        draft: action.nextDraft,
        todos: state.todos,
      };
    };
    case 'added_todo': {
      return {
        draft: '',
        todos: [{
          id: state.todos.length,
          text: state.draft
        }, ...state.todos]
      }
    }
  }
  throw Error('Unknown action: ' + action.type);
}

export default function TodoList({ username }) {
  const [state, dispatch] = useReducer(
    reducer,
    username,
    createInitialState
  );
  return (
    <>
      <input
        value={state.draft}
        onChange={e => {
          dispatch({
            type: 'changed_draft',
            nextDraft: e.target.value
          })
        }}
      />
      <button onClick={() => {
        dispatch({ type: 'added_todo' });
      }}>Add</button>
      <ul>
        {state.todos.map(item => (
          <li key={item.id}>
            {item.text}
          </li>
        ))}
      </ul>
    </>
  );
}
```
