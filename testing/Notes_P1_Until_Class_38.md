# Start

### we start with...

    create-react-app
    
**It would send you the next message...**

    Please specify the project directory:
      create-react-app <project-directory>

    For example:
      create-react-app my-react-app

    Run create-react-app --help to see all options.

**If Send a error message very quickly install...**

    npm install -g create-react-app
    
 **After that we create a react project with the name "testing"** 
    
    create-react-app testing
    
# TESTING
To files are important files:
- App.js (the root)
- App.test.js (have a default code to test renders without crashing)

### To init with testing.
- first, change te App.js file, only leave a one div, and inside write some text ('Hi there!');
- then, go to App.test.js file, and write code to verify if the text added in App.js file exist, this is the first test.
        
        expect(div.innerHTML).oContain('Hi there!');
        
- To execut the test, run inside the root directory the comand.

        npm run test
        
### Introduction to Jest
- It manages the test files to run
- Jest finds all files ending in .test.js and executes test inside of them

### Build the enviroment to practice test
- First we install redux with the next code:

        npm install --save redux react-redux
        
#### React and Redux Design
- See "03_AppDesignDiagram" file...
- ... CommentBoxComponent is responsable to the little form and submiting
- check also "04_ReduxDesign" file
- comments change the state, is an array, saveComment is the action that add comments via reducer

#### test flow
1. Look at each individual part of your application
2. Imagine telling a friend 'here whats this piece of code does'
    - you sit down next to a friend and tell the purpose of each of those parts of your application.
    - For example: here the purpose of App Component, etc.
3. Write a test to verify each part does what you expect

### Writing first test
- specify the div element will render the app
- to run the test write in terminal "npm run tes"
- **My firts test code with notes**

        import React from 'react';
        import ReactDOM from 'react-dom';
        import App from './App';

            //"it" is a global function, it will callit any time when create a test
            //Don't need to require or import because is GLOBAL
            //The first argument of it function are to be the same, the first is a description, type string
            //The second argument is the logic, the body to execute
            //Don't need to specify the name if the file to test, because the file is in this case App.js and by default it already import the file.

            //The test use JEST and it runs in the terminal, Jest simule the browser 
        it('renders without crashing', () => {
            // Create the div to simulate a div, because the test run in terminal not in browser it's a fake div
          const div = document.createElement('div');
            // React dom render an app component in a JSDOM inside of that the render
          ReactDOM.render(<App />, div);
            // Print in console and check if the CommentBox is in there
            //console.log(div.innerHTML)
            //Once you cheked confirm if the element contain the text Comment Box inside, 
            //write a expect function, first take the contain inside of the div 
            //and then that contain a text it must be exactly the same
            //that wrote in the commentbox component
          expect(div.innerHTML).toContain('Comment Box')
            //'expect' function is a global function like 'it' function. The first argument must be an object,
            //element or werever that we need to inspect inside of it. check diagram 10_expectFunction.PNG
            //Then write the match statement, in this case is toContain
            //Line of code below is going to find app component and remove app component entirely
            //Is a clean up after the test run, if not clean, the component can run during test and can affect the performance of test
          ReactDOM.unmountComponentAtNode(div);
        });

### Finding the best aproach
        //Based in the next code
        expect(div.innerHTML).toContain('Comment Box')
- In summary, the test must be check one time for any element, in the code above we check two things and it would beeing a mess, because when te app grows, i will be must dificult to check the right information.
- We going to install a library created by RBB to do the test a little bit easier because enzyme was created for react.

        npm install --save enzyme enzyme-adapter-react-16(in this seccion write the react version that you have, only the first num)
- Finding Documentation of ENZYME in (airbnb.io/enzyme)[https://airbnb.io/enzyme/]
- While the package is installing we can start to create a file inside of src directory, with the name (is important that has to be exactly the name written) setupTests.js inside of this file put the next code below:

```javascript
import Enzyme from 'enzyme';
//put the react version at the end
import Adapter from 'enzyme-adapter-react-16';

Enzyme.configure({ adapter: new Adapter() });
```

- setupTests.js will be the first code that the "jest" library going to execute
- We gonna use 3 different methods to create instances of our components and then writing expectentions around them. The three ways are static render, shallow render and full DOM render.
    - Static render: It gonna take our component, it gonna render the component, take all the HTML generated and then return to us an object that just contain that html.Is a static HTML, we can't interact with an input or clicks. It allows us to generate assertions about the HTML generated.
    - Shallow render: Take that component and render that component and none of his children. One example might be App component, we can't test his children component like box an list components. SHALOW RENDER IS USED WHEN WE WANT TO TEST ONE COMPONENT BY ITSELF IN ISOLATION.
      example util for our test: https://airbnb.io/enzyme/docs/api/ShallowWrapper/filter.html
        
                const wrapper = shallow(<MyComponent />);
                expect(wrapper.find('.foo').filter('.bar')).to.have.lengthOf(1);
        
     - Full DOM render: This takes and instance of a component rendersthat component and all of it's children. It then returns an object back to us. It can simulate click events or entering text or otherswise interacts with that component. It's essencially a full copie if our entire application and then somehow test interactions with the entire.

### Finally the Refactor:

- We gonna delete the test logic before writing, and react DOM import. Then import shallow enzime and so import commentBox.
- Write inside the test logic first save the App component in a constant using the shallow method, then write the expectation function ("expect()") inside write the thing that we expect.
- we then use find method, that returns an array with all the component that we want to find.

- Finally we expect only one CommentBox component so we use the lenght method and toEqual() to make shure of that. 
- The code looks like this below:

```javascript
import React from "react";
import { shallow } from "enzyme";
import App from "../App";
import CommentBox from "../CommentBox";
it("shows a comment box", () => {
  const component = shallow(<App />);
  expect(component.find(CommentBox).length).toEqual(1);
});
```

- After the testing pass try to make it break, like intead to say toEqual(1) say toEqual(6) and break it.

### First exercise
- In the last section we use Shallow render that is just to render App componentbut ot any components inside of it. So even when we search the comment box component and looks like an instance of comment box, there's just a kind of placeholder or bookmark, it really doesn't exist.

- The first exercise is to make shure if comment list is also displayed inside of  app component, the test will be write inside of the same file, then make sure that we have an instance of comment list component inside of App component.

### Absolute path import 

- System of relative imports, imports statement instead absolute reference. To do this create a new file in root with the name ".env". Then inside write "NODE_PATH=src/" it's all. Next we will make a refactor. relative path =>("../") absolute path => ("components/App")
    - The benefit is that you can now move the test and the import files around with out break it.
    
### Code Reuse with BeforeEach

- First One last refactor: We have the same line of code in both test maked. First we gonna create a function call it beforeEach and pas an arrow function, any logic inside will be executed before any test. Good place to put some common setup project. Check the code below:
```javascript
beforeEach(() => {
  const component = shallow(<App />);
});
```

- Check the scoope because the const component is read only inside of beforeEach method. To make it readeable make above of it a let with the variable component.

- In this point the code of App.test.js

```javascript
import React from "react";
import { shallow } from "enzyme";
import App from "components/App";
import CommentBox from "components/CommentBox";
import CommentList from "components/CommentList";

let component;

beforeEach(() => {
  component = shallow(<App />);
});
it("shows a comment box", () => {
  expect(component.find(CommentBox).length).toEqual(1);
});

it("show a comment List", () => {
  expect(component.find(CommentList).length).toEqual(1);
});
```
### Comment box test file

- Which enzyme we need to use for our application?
    * We gonna use **full DOM render**
- We gonna create a new test file inside of ```__test__``` directory call CommentBox.test.js
    * Inside of this file, write a similar code using in the previus test file.

#### Full DOM rendering process explaining
- link of Documentation of [full DOM rendering](https://airbnb.io/enzyme/docs/api/mount.html)
- instead using shallow method we gonna use in this case "mount" that allow us to make assertions
- We gonna use a _cleanup_ at the end of the test.

#### After Each
- After Each is an opposite of before Each.
- It gonna run after ever single test is executed.
- Writte below of before each
- We gonna use a unmount method, that unmoun the component we created
- It gonna run after it function of a test.

### The test for input component...

- We Gonna check first if the text is saving in the estate
- We can interact with the component+
- Steps
    * Find the textarea component
    * Simulate a change event
    * Provide a fake event object
    * Force the component to update
    * Assert that the text area value has changed
```js
it("has a text area that users can type in", () => {
  // Find the textarea component . . . wrapped.find("textarea");
  // Simulate a change event, to do that, find on Enzime docs. In FULL DOM Rendering section
  // simulate(event[,data]), first arg, str name, like change event, second is a mock.
  // TIP: use a real html event name.
  // Provide a fake event object  {tarjet: { value: "new comment"}
  wrapped.find("textarea").simulate("change", {
    target: { value: "new comment" }
  });
  // Force the component to update: means don't wait to asynchronous response of setState
  // To help whit that Enzime has a update method
  wrapped.update();
  // Assert that the text area value has changed, gonna use prop(key)
  expect(wrapped.find("textarea").prop("value")).toEqual("new comment");
});
```
### When the input is submitted, text area should get emptied.

- make shure first that the text area have some text
- the update
- The code below will explain.
```js
it('when form is submitted, text area get empied',()=>{
  //make shure first that the text area have some text, to simulate use the text below
  wrapped.find("textarea").simulate("change", {
    target: { value: "new comment" }
  });
  //Update the fake value
  wrapped.update();
  // simulate the submit event, must be the same of html event, not onSubmit, that is the name of the prop.
  wrapped.find("form").simulate("submit");
  // Update the change, because the state changed
  wrapped.update();
  // Now make the assertion
  expect(wrapped.find("textarea").prop("value")).toEqual("");
})
```

### Describe Statements

- Groups together certains sets.
- describe function is used to wrap a block of tests
- The first parameter is a description, the second is a call back function, that will contain inside the tests.
- It will scape to the beforeEach scope, and at the same time you can write other beforeEach funcion. It only run before the all test inside of describe function.
```js
import React from "react";
import { mount } from "enzyme";
import CommentBox from "components/CommentBox";

let wrapped;
beforeEach(() => {
  wrapped = mount(<CommentBox />);
});
afterEach(() => {
  wrapped.unmount();
});
it("has a text area and a button", () => {
  expect(wrapped.find("textarea").length).toEqual(1);
  expect(wrapped.find("button").length).toEqual(1);
});
// Describe Function run a block of test
describe("The text area", () => {
  // This before each will run before the before before each...
  beforeEach(() => {
    it("has a text area that users can type in", () => {
      wrapped.find("textarea").simulate("change", {
        target: { value: "new comment" }
      });
      wrapped.update();
      expect(wrapped.find("textarea").prop("value")).toEqual("new comment");
    });
    it("when form is submitted, text area get emptied", () => {
      wrapped.find("textarea").simulate("change", {
        target: { value: "new comment" }
      });
      wrapped.update();
      wrapped.find("form").simulate("submit");
      wrapped.update();
      expect(wrapped.find("textarea").prop("value")).toEqual("");
    });
  });
});
```


