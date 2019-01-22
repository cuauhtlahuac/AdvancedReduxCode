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
