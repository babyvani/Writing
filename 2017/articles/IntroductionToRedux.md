# 8 steps to get started with Redux and React - A roadmap ⚛ 💡 🏁

[<img src="https://images.unsplash.com/photo-1476445704028-a36e0c798192?dpr=2&auto=format&fit=crop&w=767&h=511&q=80&cs=tinysrgb&crop=">](https://unsplash.com/photos/NFs6dRTBgaM)

In ["Understanding MVC Architecture with React"](https://medium.com/@ddcreationstudi/understanding-mvc-architecture-with-react-6cd38e91fefd#.r66jqp0ly) I came to the conclusion that *Redux* seems to be the perfect fit for structuring apps with *React*. Now I want to provide a roadmap on how to use it in practice.
As a reminder: *Redux* builds on Flux as a refined MVC pattern and allows to manage the state out of one hand.

<img src="https://raw.githubusercontent.com/reactjs/redux/master/logo/logo-title-dark.png" alt="redux logo" height="200" align="right">

## 📄 Table of contents
  * [1. Set up your environment 🔻](#1-set-up-your-environment)
  * [2. Set up your components and Layout 🔻](#2-set-up-your-components-and-layout)
  * [3. Set up routing 🔻](#3-set-up-routing)
  * [4. Set up *Redux* store 🔻](#4-set-up-redux-store)
  * [5. Plan *Redux* Actions and Reducers 🔻](#5-plan-redux-actions-and-reducers)
  * [6. Integrate Store with *React* Router 🔻](#6-integrate-store-with-react-router)
  * [7. Updating State with Reducers 🔻](#7-updating-state-with-reducers)
  * [8. Debugging 🔻](#8-debugging)
  * [Conclusion](#conclusion)
  * [Dive deeper - some useful links](#dive-deeper-some-useful-links)

---

>"In theory there's no difference between theory and practice. But, in practice, there is."
― [Jan L. A. Van De Snepscheut, Computer Scientist](https://en.wikiquote.org/wiki/Jan_L._A._van_de_Snepscheut)

---

## 1. Set up your environment 🔻
Set up your desired environment. [React boilerplates ➡️](http://andrewhfarmer.com/starter-project/) are a great way to get going. Be sure to understand the corresponding bundler at least a little bit.

## 2. Set up your components and Layout 🔻
When creating your Components and how the fit together remind yourself to [think in React](https://facebook.github.io/react/docs/thinking-in-react.html).

## 3. Set up routing 🔻
React Router is the perfect tool for switching content components in your main components. Get yourself comfortable with the [React Docs](https://facebook.github.io/react/docs/react-api.html) and understand how to handle and fit elements together (e.g.`cloneElement` will `Clone and return a new React element using element as the starting point. The resulting element will have the original element's props with the new props merged in shallowly.`).
>[React Router](https://github.com/ReactTraining/react-router/blob/master/docs/Introduction.md) is a powerful routing library built on top of *React* that helps you add new screens and flows to your application incredibly quickly, all while keeping the URL in sync with what's being displayed on the page.

Be sure to include `this.props.children` in your main component that is rendered in your main path, so props can be passed downwards.

## 4. Set up *Redux* store 🔻

[<img src="http://jslancer.com/wp-content/uploads/2016/09/rre-2.png" alt="credit to http://jslancer.com/2016/09/28/migrating-my-first-angularjs-app-to-reduxangularjs/" height="200" align="middle">](http://jslancer.com/2016/09/28/migrating-my-first-angularjs-app-to-reduxangularjs/)
🔖 Image credit to [JS Lancer](http://jslancer.com/2016/09/28/migrating-my-first-angularjs-app-to-reduxangularjs/)
- create or fetch your data/content
- create a (default)state
- create a [store](http://redux.js.org/docs/api/createStore.html)

>Creates a *Redux* store that holds the complete state tree of your app.
>There should only be a single store in your app.

- if you want to sync history with store, use [syncHistoryWithStore](https://www.npmjs.com/package/react-router-redux)

>This library helps you keep that bit of state in sync with your *Redux* store. We keep a copy of the current location hidden in state.

## 5. Plan *Redux* Actions and Reducers 🔻
Set [actions](http://redux.js.org/docs/basics/Actions.html) to handle data that is created in user interaction (e.g. clicking "like").
>Actions are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using store.dispatch().

Set [reducers](http://redux.js.org/docs/basics/Reducers.html) to change the state accordingly to the happened actions. A reducer takes the action and a copy of the current state. Create a reducer for each state and combine all of them in a main reducer.

## 6. Integrate Store with *React* Router 🔻
Use [react-redux](http://redux.js.org/docs/basics/UsageWithReact.html) to implement the state into the *React* router.
>*Redux* works especially well with libraries like *React* and Deku because they let you describe UI as a function of state, and *Redux* emits state updates in response to actions.

Connect the router to the store with [`<Provider>`](https://github.com/reactjs/react-redux/blob/master/docs/api.md#api).


## 7. Updating State with Reducers 🔻

Every time you run an action, every reducer is going to run and actions will be dispatched.
- the `connect` method of React-redux allows to pass props to a component
- [`bindActionCreators`](http://redux.js.org/docs/api/bindActionCreators.html) wrap action objects into dispatch calls, so that they may be invoked directly.
- [`mapStateToProps`](https://github.com/reactjs/react-redux/blob/master/docs/api.md) and [`mapDispatchToProps`](https://github.com/reactjs/react-redux/blob/master/docs/api.md) connect the data from the store to a certain component.
- render state into components with `this.props` on each component
- pass onClick functions with `this.props`
- handle the click in your according reducer file with copying the old state and setting a new one
- *as reminder on React* ➡️ use `ref` to get the data of the input field
- *practical tip* ➡️ get your data first, then render it out in JSX
- split up reducers with [reducer compositions](http://redux.js.org/docs/basics/Reducers.html) and handle just a slice of the state
- to get data asynchronously use [redux-thunk](https://github.com/gaearon/redux-thunk) or [redux-saga](https://github.com/redux-saga/redux-saga) for example

## 8. Debugging and Monitoring🔻

- use the [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
- [Sentry](https://sentry.io/) provides a lot of error tracking features
- to hotreload reducers: recompile the root reducer and change the store, with accepting the hotreload and re-requiring the reducer.
- use the [Redux Developer Tools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd) to visualize the reducer process and timetravel


## Conclusion
I wrote this roadmap while I was creating the ["Reduxstagram"-App](https://learnredux.com/). I think it's one of the better tutorials, as usual from Wes Bos, however I realized it's absolutely necessary to read the docs to understand the concept of *Redux* with React. I have also realized that my knowledge on *React* is also not good enough - make sure to be confident with *React* alone.

I used the roadmap as guide for another app and it gave me a good direction for setting up a basic structure.

```
Please leave comments, feedback and suggestions as I am always trying to improve.
Share your thoughts - it's never been easier 😄
```

## Dive deeper - some useful links
- [🔀 "Learn Redux" - Wes Bos (Great Video Tutorial)](https://learnredux.com/)
- [🔀 "List of *React* Starter Projects" - Andrew H Farmer](http://andrewhfarmer.com/starter-project/)
- [🔀 "Thinking in React" - facebook](https://facebook.github.io/react/docs/thinking-in-react.html)
- [🔀 "Docs" - Redux](http://redux.js.org/docs/basics/)



<!-- Written by Daniel Deutsch (deudan1010@gmail.com) -->
