## Class 38 Redux Setup
- Inside of src file create redux folders call reducers
- Inside reducers folder create two files, the first file is going to contain comment reducers, simply name comments.js
- the other file would be index.js this is house our combine reducers call
- Code below is a boiler plate to comment file reducer
```js
export default function(state = [], action) {
  switch (action.type) {
    case "X":
      break;
    default:
      return state;
  }
}
```
- Code below is a boiler plate to index redux file
```js
import { combineReducers } from "redux";
import commentsReducers from "reducers/comments";

export default combineReducers({
  comments: commentsReducers
});
```
- Inside of index.js file import {provide} from react-redux
- Import {createStore} from redux
- finally import the convine reducers, import reducer from reducers (from the reducers directory, remember that the imports are relative, other thing is the index.js will be imported atm when call the directory that contain it)
- We gonna wrap the app component inside of provider tag, and pass it as prop store = {createStore(reducers, {})} , the first arguments are all the reducers, as we import before and the second argument is the initial statate. 
```js
import React from "react";
import ReactDOM from "react-dom";
import App from "components/App";
import { Provider } from "react-redux";
import { createStore } from "redux";
import reducers from "reducers";

//I need to specify where the app component going to render,
//in this case in index.html in the div with the id root
ReactDOM.render(
  <Provider store={createStore(reducers, {})}>
    <App />
  </Provider>,
  document.querySelector("#root")
);
```
