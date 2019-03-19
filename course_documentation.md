# Node.js Crash Course

## What is Node.js
* ` Node.js ` is a **run-time environment** that lets **JavaScript** executed outside a browser on any machine. That's why it is used to extend **JavaScript** from a *client-side* to a *server-side* programming language.
* ` Node.js ` uses the **V8 engine** which is an engine written in C++ by Google to execute **JavaScript** on Google Chrome.

## Course prerequisites
* Before starting this crash course, you **must** know the basics of **JavaScript** and it is **recommended**(not a must) to be familiar with the following:
    * Modern ES6 syntax and features.
    * Http request.
    * JSON.
    * MVC Pattern. 

## Why use Node.js
* A few reasons make ` Node.js ` a good choice for a server-side tool:
    * Using the same programming language for both sides(client-side and server-side).
    * Popular in the industry.
    * Fast, efficient and highly scalable. 
    * Event-driven, non-blocking IO model.
        * **JavaScript** is a single-threaded programming language. Instead of running multiple threads, it uses non-blocking IO calls in an **asynchronous** way. This means that instead of supporting concurrency through multiple threads, it uses events & callbacks.
        * **Asynchronous** operations are done through events. Once you begin executing an **async** operation, you're free to do anything else and once the operation finishes you will be informed by an event which you define a callback function to be invoked on firing.
        * This approach of using events & callbacks helps in saving memory over the other approach of using multi threads[Not sure why. I guess it may save what's needed in synchronizing threads in the other approach].
        * That is why ` Node.js ` is perfect for any application except for those that are CPU intensive.

## Before Starting with a Text Editor 
* ` Node.js ` comes with a tool named ` REPL `(Read-Eval-Print-Loop) which allows you to type and run **JS** directly in the console. Just hit ` node ` and go(The same as hitting ` python ` and then typing any python code directly in the console).  

## Before Start Coding
* The first step is to head to your text editor which is **VSCode** for me, create a directory for a project and inside this directory hit ` npm init -y ` to create a ` package.json ` file with default values instead of asking you to enter those values.
* ` package.json ` file holds all your packages or dependencies and absolutely you do not include those dependencies(which downloaded in node_modules directory) in you repoository. However to run your application from another machine, you simply download your repository and hit ` npm install ` to install all the dependencies listed in ` package.json `.
* We used to install two types of packages:
    * **Deployment packages**  
    Packages needed for the app in development as well as deployment. We used to install them by adding ` --save ` to ` npm install [package_name]`. No need to add that for this type.
    * **Development packages**  
    Packages needed only in development. We used to install them by adding ` --save-dev `. We can use the shorter way by adding ` -D `. 

## Let's code
* To ` export ` something in ` Node.js `, you assign ` module.exports ` with what you want to ` export `.
```js
const Person = {
    name: "Hos",
    age: 22
};

module.exports = Person;
```
Note that this is a **default** ` export `.
* To ` import ` something, you call ` require ` and pass either the name of a module or the path to a file you want to ` import ` from.
```js
const P = require('./Person.js);  // The name of the file with export statement

console.log( P );
```
Note that I could use another name since it is a **default** ` export `.

* What if you want to ` export ` more than one thing? Simply assign ` module.exports ` an object wrapping these things.
```js
const name = "Hos";
const age = 22;

module.exports = { name, age };
```
Note that this is a **named** ` export `.

* To ` import ` one or multiple things, you wrap these things with their exact names in curly braces.
```js
const { name } = require('./Person.js');

console.log( name );
```
Note that we have to use the same name defined in ` export `.

* Note that we can not use ` export ` and ` import ` ES6 syntax because it is not supported yet by ` Node.js `. We can use them after employing a **transpiler** like ` Babel `.

* To execute a file, just hit ` node [file_name] `. You can omit the file extension ` .js `.

* Any file we create is wrapped with what's called ` Module Wrapper Function `.
```js
(function ( exports, require, module, __filename, __dirname ) {
    // Your code gets here
})
```
That's why we can use ` module.exports ` and this stuff. Note that the ` Module Wrapper Function ` is an **IIFE** lacking the last parenthesis which call the ` function `. This way lets us return the ` function ` itself. To understand the difference:
  * This is a normal **IIFE** that executes the function(It logs "Hassan").
  ```js
  (function() {
      console.log("Hassan");
  })();
  ```
  * However, this returns the function itself.
  ```js
  (function() {
      console.log("Hassan");
  });
  ```

## Node.js Core Modules

* **Path**  
` Path ` module is used to deal with paths of files and directories. 

* **fs**  
` fs ` module is used to deal with the file system.

* **os**    
` os ` module is used to get information about the running operating system.

* **url**  
` url ` module is used to deal with urls.


## Using EventEmitter
* ` EventEmitter ` is a **construct** or to be precise a ` class ` that we instantiate objects from to fire events and respond to them using a callback in an asynchronous way.
    * Let's ` require ` the ` events ` module from which we get a ` class ` that we ` extend ` to create our ` EventEmitter ` ` class `.
    ```js
    const EventEmitter = require("events");

    class SimpleEmitter extends EventEmitter {

    } 
    ```
    * Now we can use instantiate an object from ` SimpleEmitter ` ` class ` and use that object to to define an ` event ` with its callback.
    ```js
    const emitterObject = new SimpleEmitter();

    emitterObject.on( "simple_event", () => console.log( "Simple event fires" ) );
    ```
    * Now we can fire a ` simple_event `.
    ```js
    emitterObject.emit("simple_event");
    ```
    Now we should see ` Simple event fires ` in the console.

* We can also ` emit ` events and pass data with that event.
```js
emitterObject.emit("event", { name: "Hos", age: 22 });
```
* In the callback we can expect to get this data as an object.
```js
emitterObject.on("event", data => console.log(`A person with name ${data.name} and age ${data.age}`));
```
Note that we must define the event with its handler **before** ` emitting ` it.


## uuid
* ` uuid ` is a package that is used to generate random unique ids for us.
* We can install it by hitting ` npm install uuid `.
* We can use it in this way.
```js
const uuid = require("uuid");

console.log( uuid.v4() );
```
Note tha ` v4 ` is just one way to generate an id using this package.


## Ussing http
* Now we hope to create a very simple server that responds with any thing just to see how it works.
    * ` require ` the ` http ` module.
    ```js
    const http = require("http");
    ```
    * use ` createServer ` function on ` http ` to create such a server. ` createServer ` accepts a ` function ` that receives ` request ` and ` response ` objects. This function is what gets called on a client request. We can call ` write ` fuction on ` response ` object to give back a response to the client. After ` createServer ` we call ` listen ` function which aceepts on which ` port ` we listen and a callback when start listening.  
    ```js
    http.createServer( ( request, response) => {
        response.write("I'm a simple server");
        response.end();
    } ).listen( 5000, () => {
        console.log("start serving ...");
    } );
    ```
    We can connect to that server on the browser from ` localhost:5000 `. Since we used the port 5000.


## Real Server
* A real server does not give back a string. It is either the case that it gives back an **HTML page** or a **JSON**(**Rest Api**: which we are familiar with as mobile developers).

#### Api
* Let's create a server that returns back an HTML page.
    1. Create a file named ` index.html ` with basic HTML content in  the same directory where ` index.js `(the ` js ` file we are typing into) lives.
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title>Home</title>
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
        <h1>Home Page</h1>
    </body>
    </html>
    ```
    2. in ` index.js ` let's create a basic server.
    ```js
    const http = require("http");

    const server = http.createServer( ( request, response ) => {
        
    } );

    const PORT = process.env.PORT || 5000;

    server.listen( PORT, () => {
        console.log( "start serving ..." );
    } )
    ```
    Note that we take port 5000 if we failed to get the port number from the environment variable because always the port number is got from the user.
    * To get a file from the system we need 2 packages: ` path ` and ` fs `. We ` require ` them in the topmost of ` index.js `.
    ```js
    const path = require("path");
    const fs = require("fs"); 
    ```
    Then we can get the ` index.html ` inside thefunction that gets called when the client connects to the server.
    ```js
    fs.readFile(
        path.join(__dirname, "index.html"), 
        (error, content) => {
            if ( error ) throw error;

    } );
    ```
    Note that we used ` path ` to get specify the path of the file and since it lives in the same directory of ` index.js ` we use ` join ` method with 2 parameters to be joined: the ` __dirname ` which is the path of the current directory and the name of the file.  In the callback function after reading the file we return our file. We need to tell the browser what status code as well as what type of file we are returning.
    ```js
    response.writeHead( 200, { "Content-Type": "text/html" } );
    response.end( content );
    ```
    Note that we returned the file content using the function ` end `.

* We can check the url we get using ` request ` object and return back different pages based on the url.
```js
swith ( request.url ) {
    case "/":
        // return home page
        break;
    case "/about":
        // return about page
        break;
}
```
Note that we get what after ` localhost:500 ` so if we hit ` localhost:500 ` or ` localhost:500/ ` we get ` / ` if we hit ` localhost:500/about ` we get ` /about ` and so on. 

#### REST Api
* Let's return back a JSON object.
```js
case "/users":
    case "/users":
    const users = [{name: "Hos", age: 22}, {name: "Ismail", age: 24}];
    response.writeHead( 200, { "Content-Type": "application/json" } );
    response.end( JSON.stringify( users ) );
    break;
```
Note that:
  * We changed ` Content-Type ` to ` application/json `.
  * To convert something to ` JSON ` before transmitting, we use ` JSON.stringify() `.


## Deployment 
1. Now to deploy such a silly website we need to make sure that we got the port from the environment variable as we did and we need to add a script in our ` package.json ` file with name ` start ` to start the server by running the entry point ` index.js `.
```js
"scripts": {
    "start": "node index"
  },
```
2. We can use [heroku](https://www.heroku.com/) for free deployment. First sign up and then you need to install heroku CLI by hitting ` sudo snap install --classic heroku `.
3. Make sure that heroku cli get installed by hitting ` heroku --version `.
4. login to heroku through the command line by hitting ` heroku login ` and then fill your login info.
5. Make sure that you have ` git ` installed.
6. Create ` .gitignore ` filr and fill it with anything should not be included in the repo inclusing the ` node_modules ` folder.
```
node_modules
```
7. Initialize a git repository, add all files to staging area and commit[If not a git repository yet].
```
git init
git add .
git commit -m "Default commit"
```
8. hit ` heroku create `.
9. If you go to your dashboard in heroku, you find your application.
10. Open your application, go to deploy tab, grab the command that creates a git repository for you there.
```
heroku git:remote -a fathomless-taiga-82119
```
11. Finally push to your repo there.
```
git push heroku master
```
12. Now you have your app deployed and you can go there or hit ` heroku open `. It successfully works.
