REDUX
====================

Web apps have become very complex.

[!This is your app](tangled wires)

At some point you no longer know what happens in your app. You no longer control when, why, and how the state is updated.  You need a sane way to manage your state and data flow.

Before talking about redux lets get some backgroud.




[!awesome game screenshot](...)

Games have complex state the changes frequently based on simulations rules and user input.

Games mange updating state via a game loop:
  - starting state
  - gather user input
  - update state based on user input and simulation rules
  - render visual representation to screen
  - rinse, wash, repeat - 60 times a second!

Can we write web apps the way we write games?

Enter React.

Unlike other MV* frameworks since Backbone, React is a game changer because *it lets us write web apps the way we write games.*  We can throw a new state at the DOM as often as we please.  This means we need to think about building applications and data-flow differently.  

It's the **data flow** that matters. The UI is a side effect; your app should be able to run from the command line.




Flux
--------------------

[!before flux](http://cdn.infoq.com/statics_s1_20150902-03315u3/resource/news/2014/05/facebook-mvc-flux/en/resources/flux-react-mvc.png]

[!flux diagram](https://facebook.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

Unidirectional flow:
1. Start with a given state (which may be distributed across multiple stores)
2. Represent that state in the views and handle events from the users
3. Turn handled events into actions representing the user's intention
4. Dispatch these actions to all your stores
5. If a store cares about the action, update it's state accordingly and publish an "update" event
6. Views hear the update event and rerender themselves accordingly
7. (Other things can send actions too, like ajax responses, timers, and websockets)





Redux
--------------------

Redux is an implementation of flux.

The whole state of your app is stored in an object tree inside a single store.
The only way to change the state tree is to emit an action, an object describing what happened.
To specify how the actions transform the state tree, you write pure reducers.

That’s it!




Why Redux?
--------------------


- "A predictable state container"
  - Write applications that behave consistently, run in different environments (client, server, and native), and are easy to test
  - Single source of truth
- Smart people (including the creators of Flux) think it's the right direction
- Separate presentation layer from logic and data - you can specify the behavior of your app before even starting to write the UI.
- Control
  - It's all just functions
  - Complexity through composition
- Scalable! (in size and complexity)
- Sane way to deal with async actions
- This isn't new, the paradigm has been around for over 40 years
- Intuitive and decoupled
- Benefits of pure functional programming (easy to test, "time traveling debuggers", reproducible sessions, etc)
- Simple, only 2kb.

Ingredients:
- one store (everything in it under different nodes)
  - business data
  - app state (like "busy" or "offline")
  - ui state (like which route you are on, which panel is active, or if you should show a loading spinner)
- actions - just simple objects representing the user's intentions (or an external event, like ajax response)
- reducers - respond to different actions and update the store accordingly
  - (state, action) => state

"Redux doesn't have a Dispatcher or support many stores. Instead, there is just a single store with a single root reducing function. As your app grows, instead of adding stores, you split the root reducer into smaller reducers independently operating on the different parts of the state tree. This is exactly like there is just one root component in a React app, but it is composed out of many small components."


Redux rules:
- no mutating state!  Make your actions and reducers pure!
- normalize your state - no duplications and nothing derived





Let's see some code
--------------------

```
import { createStore } from 'redux';

 // The reducer, a pure function with (state, action) => state signature.
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + action.amount;
  case 'DECREMENT':
    return state - action.amount;
  default:
    return state;
  }
}

// The store bound with yuour reducer.  It's API is { subscribe, dispatch, getState }.
let store = createStore(counter);

// Side effects (this is where you could pass your data into react)
store.subscribe(() =>
  console.log(store.getState())
);

store.dispatch({ type: 'INCREMENT', amount: 1 });
// 1
store.dispatch({ type: 'INCREMENT', amount: 3 });
// 4
store.dispatch({ type: 'DECREMENT', amount: 2 });
// 2
```

Async
--------------------

Middleware...

- middleware - inject composable behavior into the dispatch cycle



Using with React
--------------------

React is well suited to work with redux

- Use redux-react helpers to facilitation binding
- Recommend passing relevant nodes of state into "smart" root-components, that pass props down to "dumb" middle/leaf components
- UI hierarchy matches state hierarchy


Links:
- Great docs, [RTFM](http://rackt.github.io/redux/)!!
- [Awesome redux](https://github.com/xgrommx/awesome-redux)
