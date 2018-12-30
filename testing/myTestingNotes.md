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
- In summary, the test must be check one time for any element, in the code above we check two things and it would beeing a mess, because when te app grows, i will be must dificult to check the right information
- We going to install a library created by RBB to do the test a little bit easier because enzyme was created for react.

        npm install --save enzyme enzyme-adapter-react-16(in this seccion write the react version that you have, only the first num)
        
- While the package is installing we can start to create a file inside of src directory, with the name (is important that has to be exactly the name written) setupTests.js inside of this file put the next code below:

        import Enzyme from "enzime";
        //put the react version at the end
        import Adapter from "enzime-adapter-react-16";

        Enzyme.configure({ adapter: new Adapter() });
        
- setupTests.js will be the first code that the "jest" library going to execute
- We gonna use 3 different methods to create instances of our components and then writing expectentions around them. The three ways are static render, shallow render and full DOM render.

    -Static render: It gonna take our component, it gonna render the component, take all the HTML generated and then return to us an object that just contain that html.Is a static HTML, we can't interact with an input or clicks. It allows us to generate assertions about the HTML generated.
    - Shallow render: Take that component and render that component and none of his children. One example might be App component, we can't test his children component like box an list components. SHALOW RENDER IS USED WHEN WE WANT TO TEST ONE COMPONENT BY ITSELF IN ISOLATION.
      example util for our test: https://airbnb.io/enzyme/docs/api/ShallowWrapper/filter.html
        
                const wrapper = shallow(<MyComponent />);
                expect(wrapper.find('.foo').filter('.bar')).to.have.lengthOf(1);
        
     - Full DOM render: This takes and instance of a component rendersthat component and all of it's children. It then returns an object back to us. It can simulate click events or entering text or otherswise interacts with that component. It's essencially a full copie if our entire application and then somehow test interactions with the entire.
        
Finding Documentation of ENZYME in airbnb.io/enzyme

### Backup to serviceWorker.js code in the src directory.

    // This optional code is used to register a service worker.
    // register() is not called by default.

    // This lets the app load faster on subsequent visits in production, and gives
    // it offline capabilities. However, it also means that developers (and users)
    // will only see deployed updates on subsequent visits to a page, after all the
    // existing tabs open on the page have been closed, since previously cached
    // resources are updated in the background.

    // To learn more about the benefits of this model and instructions on how to
    // opt-in, read http://bit.ly/CRA-PWA

    const isLocalhost = Boolean(
      window.location.hostname === 'localhost' ||
        // [::1] is the IPv6 localhost address.
        window.location.hostname === '[::1]' ||
        // 127.0.0.1/8 is considered localhost for IPv4.
        window.location.hostname.match(
          /^127(?:\.(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}$/
        )
    );

    export function register(config) {
      if (process.env.NODE_ENV === 'production' && 'serviceWorker' in navigator) {
        // The URL constructor is available in all browsers that support SW.
        const publicUrl = new URL(process.env.PUBLIC_URL, window.location.href);
        if (publicUrl.origin !== window.location.origin) {
          // Our service worker won't work if PUBLIC_URL is on a different origin
          // from what our page is served on. This might happen if a CDN is used to
          // serve assets; see https://github.com/facebook/create-react-app/issues/2374
          return;
        }

        window.addEventListener('load', () => {
          const swUrl = `${process.env.PUBLIC_URL}/service-worker.js`;

          if (isLocalhost) {
            // This is running on localhost. Let's check if a service worker still exists or not.
            checkValidServiceWorker(swUrl, config);

            // Add some additional logging to localhost, pointing developers to the
            // service worker/PWA documentation.
            navigator.serviceWorker.ready.then(() => {
              console.log(
                'This web app is being served cache-first by a service ' +
                  'worker. To learn more, visit http://bit.ly/CRA-PWA'
              );
            });
          } else {
            // Is not localhost. Just register service worker
            registerValidSW(swUrl, config);
          }
        });
      }
    }

    function registerValidSW(swUrl, config) {
      navigator.serviceWorker
        .register(swUrl)
        .then(registration => {
          registration.onupdatefound = () => {
            const installingWorker = registration.installing;
            if (installingWorker == null) {
              return;
            }
            installingWorker.onstatechange = () => {
              if (installingWorker.state === 'installed') {
                if (navigator.serviceWorker.controller) {
                  // At this point, the updated precached content has been fetched,
                  // but the previous service worker will still serve the older
                  // content until all client tabs are closed.
                  console.log(
                    'New content is available and will be used when all ' +
                      'tabs for this page are closed. See http://bit.ly/CRA-PWA.'
                  );

                  // Execute callback
                  if (config && config.onUpdate) {
                    config.onUpdate(registration);
                  }
                } else {
                  // At this point, everything has been precached.
                  // It's the perfect time to display a
                  // "Content is cached for offline use." message.
                  console.log('Content is cached for offline use.');

                  // Execute callback
                  if (config && config.onSuccess) {
                    config.onSuccess(registration);
                  }
                }
              }
            };
          };
        })
        .catch(error => {
          console.error('Error during service worker registration:', error);
        });
    }

    function checkValidServiceWorker(swUrl, config) {
      // Check if the service worker can be found. If it can't reload the page.
      fetch(swUrl)
        .then(response => {
          // Ensure service worker exists, and that we really are getting a JS file.
          const contentType = response.headers.get('content-type');
          if (
            response.status === 404 ||
            (contentType != null && contentType.indexOf('javascript') === -1)
          ) {
            // No service worker found. Probably a different app. Reload the page.
            navigator.serviceWorker.ready.then(registration => {
              registration.unregister().then(() => {
                window.location.reload();
              });
            });
          } else {
            // Service worker found. Proceed as normal.
            registerValidSW(swUrl, config);
          }
        })
        .catch(() => {
          console.log(
            'No internet connection found. App is running in offline mode.'
          );
        });
    }

    export function unregister() {
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.ready.then(registration => {
          registration.unregister();
        });
      }
    }

