# Start

### we start with...

    create-react-app
    
**It must send you the next message...**

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



