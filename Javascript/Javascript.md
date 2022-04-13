# JavaScript Cheat Sheet 

What is Javascript?

JavaScript is a cross-platform, object-oriented scripting language used to make webpages interactive (e.g., having complex animations, clickable buttons, popup menus, etc.). There are also more advanced server side versions of JavaScript such as Node.js, which allow you to add more functionality to a website than downloading files (such as realtime collaboration between multiple computers). Inside a host environment (for example, a web browser), JavaScript can be connected to the objects of its environment to provide programmatic control over them.

JavaScript (JS) is a dynamic [interpreted language](#interpreted-lang) that powers the web. It is widely used in browsers (where JS scripts are interpreted by [JavaScript engines](#javascript-engine) like Chrome's [V8](https://developers.google.com/v8/)) and increasingly on servers (on a [Node.js](https://nodejs.org) runtime environment).

JS is a [prototype-based](#prototype-based) scripting language with [first-class functions](#first-class-functions) and [dynamic typing](#dynamic-type). Because of its super flexibility, JS supports multiple styles of programming including [imperative](#imperative), [object-oriented](#object-oriented) and [functional](#functional).

Here's what all those big words above mean: 
- <a name="interpreted-lang"></a>__Interpreted Language__: a language (eg. JS, Python) in which most of its implementations execute instructions directly, without previously compiling a program into machine-language instructions like compiled languages do (eg. C++)
- <a name="javascript-engine"></a>__JavaScript Engine__: a virtual machine that interprets and executes JS
- <a name="prototype-based"></a>__Prototype-Based__: unlike classical OOP with classes and objects, in JS, objects are cloned from other objects, and all objects have _prototypes_ (kinda like the template they inherit from)
- <a name="first-class-functions"></a>__First-Class Functions__:  JS supports passing functions as arguments to other functions, returning them as the values from other functions, and assigning them to variables or storing them in data structures
- <a name="dynamic-type"></a>__Dynamically Typed__: The "type" of all variables is only interpreted at run-time unlike statically typed languages where all variables have a type at compile-time
- <a name="imperative"></a>__Imperative Programming__: Statement based programming
- <a name="object-oriented"></a>__Object-Oriented Programming__: Object based programming
- <a name="functional"></a>__Functional Programming__: Function based programming

JavaScript contains a standard library of objects, such as Array, Date, and Math, and a core set of language elements such as operators, control structures, and statements. Core JavaScript can be extended for a variety of purposes by supplementing it with additional objects; for example:

- <a name="Client-side"></a> JavaScript extends the core language by supplying objects to control a browser and its Document Object Model (DOM). For example, client-side extensions allow an application to place elements on an HTML form and respond to user events such as mouse clicks, form input, and page navigation.
- <a name="Server-side"></a> JavaScript extends the core language by supplying objects relevant to running JavaScript on a server. For example, server-side extensions allow an application to communicate with a database, provide continuity of information from one invocation to another of the application, or perform file manipulations on a server.

## Quick Access Links 
1. [Basics](#basics)
    1. [Primitives](#primitives)
    2. [Operators](#operators)
    3. [Variables](#variables)
    4. [Logic and Control Structures](#logic)
    5. [Arrays](#arrays)
3. [Objects and Functions](#objects-and-functions)
    1. [Objects](#objects)
    2. [Functions](#functions)
    3. [Bind, Call and Apply](#bind)
4. [Function Execution, Variable Scope, Closures & Callbacks](#execution)
    1. [Hoisting](#hoisting)
    2. [Scope Chain](#scope-chain)
    3. [Closures](#closures)
    4. [Callbacks](#callbacks)
    5. [Arrow Function](#arrow-function) 
5. [ES6 Features](#es6-features)
    1. [Default Parameters](#default-parameter)
    2. [Spread Operator](#spread-operator)
    3. [REST Paramaters](#rest-parametetrs)
    4. [Destructuring](#destructuring)


<a name="basics"></a>
## 1. Basics 
__Everything__ in JS is either an object or a primitive.
```javascript

// This is a single line comment,
/* and this is a 
multiline comment */

// Semicolons (;) to terminate lines are optional
// However, the JS engine will (usually) automatically insert semicolons upon seeing '\n'
// This can cause some weird behaviour so ALWAYS use semicolons
doStuff();
```
<a name="primitives"></a>
### i. Primitives: Number, String, Boolean (and some special ones) 

```javascript
//Number: An integer or floating point number
3; // = 3
1.5; // = 1.5

// Some basic arithmetic works as you'd expect.
1 + 1; // = 2
0.1 + 0.2; // = 0.30000000000000004 (funky floating point arithmetic--be careful!)
8 - 1; // = 7
10 * 2; // = 20
10 ** 2; // =100 (10 raised to the power 2) same as Math.pow(10, 2)
35 / 5; // = 7

// Including uneven division.
5 / 2; // = 2.5

// Bitwise operations also work; when you perform a bitwise operation your float
// is converted to a signed int *up to* 32 bits.
1 << 2; // = 4

// Precedence is enforced with parentheses.
(1 + 3) * 2; // = 8

// There are special primitive values:
Infinity; // result of e.g. 1/0
-Infinity; // result of e.g. -1/0
NaN; // result of e.g. 0/0
undefined; // never use this yourself. This is the default value for "not assigned"
null; // use this instead. This is the programmer setting a var to "not assigned"

// Boolean: true and false
true;
false;

// Strings are created with single quotes (') or double quotes (").
'abc';
"Hello, world";

// You can access characters in a string with `charAt`
"This is a string".charAt(0);  // = 'T'

// ...or use `substring` to get larger pieces.
"Hello world".substring(0, 5); // = "Hello"
"Hello world".slice(0, 5); // does the same thing
"Hello world".substr(0, 5); // yet again

// `length` is a property, so don't use ().
"Hello".length; // = 5

// Searching strings
"Mary had a little lamb".search("had"); // returns 5
"Mary had a little lamb".indexOf("zebra"); // returns -1
"Mary had a little lamb".includes("had"); //returns true (ES7). includes() will return true if the parameter provided is in the string, and false otherwise.

// String to a character array
"one two three four".split(" "); // ['one', 'two', 'three', 'four']

// String replace
"happy birthday henry!".replace("h", "H"); // "Happy birthday Henry!"

// String Tenplate Literals 
let firstName = "Aayush";
let lastName = "Agrawal";
console.log(`My name is ${firstName} ${lastName}`);

parseInt("24")  //24
parseInt("24.987")  //24
parseInt("28dayslater")  //28

parseFloat("24")  //24
parseFloat("24.987")  //24.987
parseFloat("i ate 3 shramps")  //NaN
// ES6 also introduces Symbol as a new primitive type
// But I'll add that on here once I actually figure out what it is

```
<a name="operators"> </a>
### ii. Operators aka weirdly written functions 
```javascript
// Operators have both a precedence (order of importance, like * before +) 
// and an associativity (order of evaluation, like left-to-right)
// A table of operators can be found here 
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

// Negation uses the ! symbol
!true; // = false
!false; // = true

// There's shorthand operators for performing math operations on variables:
someVar += 5; // equivalent to someVar = someVar + 5;
someVar *= 10; // someVar = someVar * 10;

// and an even-shorter-hand  operators for adding or subtracting 1
someVar++; 
someVar--; 

// Strings are concatenated with +
"Hello " + "world!"; // = "Hello world!"

// Comparison Operators
1 < 10; // = true
1 > 10; // = false
2 <= 2; // = true
2 >= 2; // = true

// and are compared with < and >
"a" < "b"; // = true

// Strict Equality is ===
// Strict meaning both type AND value are the same
1 === 1; // = true
2 === 1; // = false

// Strict Inequality is !==
1 !== 1; // = false
2 !== 1; // = true

// == allows for type coercion (conversion) and only checks if the values are equal after coercion
"5" == 5; // = true
null == undefined; // = true

// ...which can result in some weird behaviour...so use === whenever possible
13 + !0; // 14
"13" + !0; // '13true'

// false, null, undefined, NaN, 0 and "" are falsy; everything else is truthy.
// Note that 0 is falsy and "0" is truthy, even though 0 == "0".
0 == false // true

// We can use this to our advantage when checking for existence
if (x) { //doSomething };

// Or to set default values
x = x || "default" 
// if x exists, do nothing and short-circuit, else set x to a default

// but a problem arises if x = 0. It exists, but will coerce to false
// be wary of this

// figuring out types of literals and vars
typeof "Hello"; // = "string"
typeof 42; // = "number"
typeof undefined // = "undefined"
typeof null // = 'object' THIS IS A JS BUG!

// figuring out if an object is an instance of another object
// checks all the way down the prototype chain
var x = {}
x instanceof Object // = true
x instanceof Function // = false

```
<a name="variables"></a>
### iii. Variables 
```javascript
// Variables are declared with the `var` keyword. JavaScript is dynamically
// typed, so you don't need to specify type. Assignment uses a single `=`
// character.
var someVar = 5;

// if you leave the var keyword off, you won't get an error...
someOtherVar = 10;

// ...but your variable will be created in the global scope, not in the scope
// you defined it in.

// Variables declared without being assigned to are set to undefined.
var someThirdVar; // = undefined

// let statement declares a block-scoped local variable
let x = 1;

//Constants are block-scoped, much like variables declared using the let keyword. The value of a constant can't be changed through reassignment
const PI = 3.14;

// let vs  var 
// var can be declared multiple times in the same scope but this in not the case with let or const
// var is not blocked scope but let or const is blocked scope
// var is hoisted up but let or const are not.
// At the top level of programs and functions, let, unlike var, does not create a property on the global object
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined

function varTest() {
  var x = 1;
  {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}

function redeclarationTest() {
    var x = 0;
    var x = 1; // can be redeclared
    // let x = 1; // can't use the variable x
    let y = 1;
    // let y = 0; // can't be redecalred 
  {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

<a name="logic"></a>
### iv. Logic and Control Structures 
```javascript
// The `if` structure works as you'd expect.
var count = 1;
if (count === 3){
    // evaluated if count is 3
} else if (count === 4){
    // evaluated if count is 4
} else {
    // evaluated if it's not either 3 or 4
}

// As does `while`.
while (true){
    // An infinite loop!
}

// Do-while loops are like while loops, except they always run at least once.
var input;
do {
    input = getInput();
} while (!isValid(input))

// The `for` loop is the same as C++ and Java:
// initialisation; continue condition; iteration.
for (var i = 0; i < 5; i++){
    // will run 5 times
}

// && is logical AND, || is logical OR
if (house.size === "big" && house.colour === "blue"){
    house.contains = "bear";
}
if (colour === "red" || colour === "blue"){
    // colour is either red or blue
}

const movieReviews = {
    Arrival                : 9.5,
    Alien                  : 9,
    Amelie                 : 8,
    'In Bruges'            : 9,
    Amadeus                : 10,
    'Kill Bill'            : 8,
    'Little Miss Sunshine' : 8.5,
    Coraline               : 7.5
};

// We CAN iterate over the keys of an object
for (let movie of Object.keys(movieReviews)) {
    console.log(`You rated ${movie} - ${movieReviews[movie]}`);
}

for (let movie in movieReviews) {
    console.log(`You rated ${movie} - ${movieReviews[movie]}`);
}

// The `switch` statement checks for equality with `===`.
// use 'break' after each case 
// or the cases after the correct one will be executed too. 
grade = 'B';
switch (grade) {
  case 'A':
    console.log("Great job");
    break;
  case 'B':
    console.log("OK job");
    break;
  case 'C':
    console.log("You can do better");
    break;
  default:
    console.log("Oy vey");
    break;
}

// ternary operator
let status = "online";
let color = status === "online"? "green": "red"; 
```
<a name="arrays"> </a>
### v. Arrays 
```javascript
// Arrays are ordered lists of values, of any type.
var myArray = ["Hello", 45, true];

// Their members can be accessed using the square-brackets subscript syntax.
// Array indices start at zero.
myArray[1]; // = 45

// Arrays are mutable and of variable length (dynamically sized arrays)
myArray.push("World"); // adds to the end
myArray.length; // = 4

// Add/Modify at specific index
myArray[3] = "Hello";

//ES7 includes() can be used with arrays to check for the presence of a value. it is case sensitive
let names = ["Samuel", "Hamilton", "Eric"];
names.includes("Samuel"); //true
names.includes("samuel"); //false
names.includes("John"); //false

// other array methods
[1,2,3].push(4); // [1,2,3,4]
[1,2,3].pop(); // [1,2]
[1,2,3].unshift(4); // [4,1,2,3]
[1,2,3].shift(); // [2,3]

[1,2,3].concat([4,5]); // [1,2,3,4,5]
["Samuel", "Hamilton", "Eric"].includes("Samuel"); // true, it is case sensitive 
[1,2,3].reverse(); //[3,2,1]
[1,2,3].join(); //1,2,3
[1,2,3].join('-'); //1-2-3
[1,2,3,4,5].slice(2); //[3,4,5]
[1,2,3,4,5].slice(2,4); //[3,4]
[1,2,3,4,5].slice(-3); //[3,4,5]
[1,2,3,4,5].slice(-3,-1); //[3,4]
[1,2,3,4,5].slice(); //[1,2,3,4,5]... copy
[1,2,3,4,5].splice(1,0,6,7); //[1,6,7,2,3,4,5]
[1,6,7,2,3,4,5].splice(1,2); // [1,2,3,4,5]
[1,10,2].sort(); // 1,10,2] ... string comparison
```

<a name="objects-and-functions"></a>
## 3. Objects and Functions 
<a name="objects"></a>
### i. Objects 
An object is simply an unordered collection of key-value pairs.
```javascript
// They can be made literally:
var myObj = {key1: "Hello", key2: "World"};
// or using the Object constructor:
var myObj = new Object();

// Keys are strings, but quotes aren't required if they're a valid
// JavaScript identifier. Values can be any type including other objects.
var myObj = {myKey: "myValue", "my other key": 4};

// Objects can even contain functions (called methods)
// When functions attached to an object are called, they can access the object
// they're attached to using the `this` keyword.
var myObj = { 
  name: "Destiny's Child",
  sayMyName: function() {
    console.log(this.name);
  }
}
myObj.sayMyName(); // outputs "Destiny's Child"

// Object attributes can also be accessed using the subscript syntax,
myObj["my other key"]; // = 4

// ... or using the dot syntax, provided the key is a valid identifier.
myObj.myKey; // = "myValue"

// Objects are mutable; values can be changed and new keys added.
myObj.myThirdKey = true;

// If you try to access a value that's not yet set, you'll get undefined.
myObj.myFourthKey; // = undefined

// iterating through objects
for(var property in myObj) { // do something }

// JSON (JavaScript Object Notation) is just a special case of Object literal notation
// where the keys are strings wrapped in quotes
var json_stuff = {
  "firstName": "John",
  "lastName": "Doe",
  "Age": 25
}

// JS Object => JSON
JSON.stringify(myObj);

// JSON => JS Object
JSON.parse(json_stuff);
```
<a name="functions"></a>
### ii. Functions 
Functions are special kinds of objects! Functions can have their own methods and properties just like other objects, but they're rarely used in that way. 

Remember, functions in JS are first-class. Meaning they can be assigned and passed just like any other variable can.

Functions are special in that they have an optional name property and a code property (which is the body of the function that actually does stuff). The function's code is executed by the invocation operator `()`.

```javascript
// JavaScript functions are declared with the `function` keyword.
// This is a function statement
function myFunction(thing){
    return thing.toUpperCase();
}

// This is a function expression
var makeUpperCase = function() {
    return think.toUpperCase();
}

// Note that the value to be returned must start on the same line as the
// `return` keyword, otherwise you'll always return `undefined` due to
// automatic semicolon insertion. Watch out for this when using Allman style.
function myFunction()
{
    return // <- semicolon automatically inserted here
    {
        thisIsAn: 'object literal'
    }
}
myFunction(); // = undefined

// JavaScript functions are first class objects, so they can be reassigned to
// different variable names and passed to other functions as arguments - for
// example, when supplying an event handler:
function myFunction(){
    // this code will be called in 5 seconds' time
}
setTimeout(myFunction, 5000);
// Note: setTimeout isn't part of the JS language, but is provided by browsers
// and Node.js.
```
Function objects don't even have to be declared with a name - you can write an __anonymous function__ definition directly into the arguments of another.

``` javascript
setTimeout(function(){
    console.log("It's been 5 seconds!");
    // this code will be called in 5 seconds time
}, 5000);
```

This has led to a common pattern of __"immediately-executing anonymous functions"__, which prevent temporary variables from leaking into the global scope. The function expression is wrapped in parenthesis and then is invoked using `()`

``` javascript
(function(){
    var temporary = 5;
})();
temporary; // raises ReferenceError
permanent; // = 10
```

An important distinction: primitives __Pass by Value__ while objects __Pass by Reference__
```javascript
// Primitives are passed by value
var i = 2;
function double(i){  i*2;  } // another i is created with the same value in a different execution context
double(i);
console.log(i); // still 2 

// Objects (including functions) are passed by reference
var obj = { hero: "Superman" };
function bestSuperhero(obj){
  obj.hero = "Batman";
}
bestSuperhero(obj);
console.log(obj.hero); // = "Batman"
```
The `this` keyword within methods, always refers to the object that the method is tied to. However, if the method has an inner function, its `this` refers to the global object. Some regard this as a bug in JS, so the good practice is to create and use a variable called `self`.

```javascript
var c = {
    name: 'The c object',
    log: function() {
        var self = this;
        
        self.name = 'Updated c object';
        console.log(self);

        var setname = function(newname) {
            self.name = newname;   
        }
        setname('Updated again! The c object');
        console.log(self);
    }
}
c.log(); // outputs "Updated again! The c object"
```

<a name="bind"></a>
### iii. Bind, Call and Apply 
Functions that aren't tied to objects can use `this` and still be useful. Consider this example.
```javascript
var cow = { says: "moo" };
var dog = { says: "woof" };
var pig = { says: "oink" };

function speak(times) { 
  for(i = 0; i < times; i++) {
    console.log(this.says);
  }
}
speak(4); // error because this is the global object which doesn't have a 'says' property
```
To use speak, we need to use the .bind, .call or .apply methods, which are available to __all__ functions. The first parameter of these functions is the object which becomes `this` within the function.

```javascript
// bind creates a copy of the function it's being called on
var cowSpeak = speak.bind(cow);
cowSpeak(4); // outputs "moo moo"

// call directly executes the function with the first parameter being 'this'
// and all the other parameters being the function's parameters
speak.call(dog, 3); // outputs "woof woof woof"

// apply directly executes the function with the first parameter being 'this'
// and all the other function parameters being passed in as an array
speak.apply(pig, [1]); // outputs "oink"
```
The call and apply methods allow us to do something called __Function Borrowing__.
```javascript
var darthVader = { 
  son: "Luke",
  saying: function(){
    console.log(this.son + ", I am your father");
  }
};
var luke = { son: "Ben" };

darthVader.saying.call(luke);
// borrowing Darth Vader's saying
// outputs "Ben, I am your father"
```
The bind method allows us to do __Function Currying__.
```javascript
// Creating a copy of a function with preset parameters
function multiply(a,b){ return a*b }

// first parameter can be 'this' or null or anything--doesn't matter since 'this' is not used
// the second parameter (a) is permanently set to 2
var double = multiply.bind(this, 2);
double(16); // outputs 32

```

<a name="execution"></a>
## 4. Function Execution, Variable Scope, Closures & Callbacks 

A few important concepts:
- __Global__ means not inside a function. The global object is 'window' in browsers.
- The __Lexical Environment__ is where something sits physically in the code
- __'this'__ is a reference to the object that the currently running method is tied to (by default it's tied to the global object)
- The __Execution Context__ consists of the environment (state of variables) of the function currently being evaluated. It also includes 'this' and a reference to the outer environment (aka what object is sitting outside this function _lexically_)
- The __Execution Stack__ or __Call Stack__ is the "stack" of execution contexts with the global execution context being the bottommost. When the program flow enters a function, a new execution context is popped onto the call stack, and when the function returns, it is popped off.

<a name="hoisting"></a>
### i. Hoisting 
Before actually executing any of the code, the JS engine first looks at all the variable declarations and function statements and sets aside some memory space for them effectively moving them to the top of the code. This is known as __hoisting__.
```javascript
// Variable example

function a(){
  console.log(x);
  var x = 2;
  console.log(x);
}
a(); // outputs 'undefined' then 2

// Function a is essentially equivalent to:
function a(){
  var x; // the var declaration is hoisted up to the top and is set to undefined
  console.log(x); // outputs undefined
  x = 2; // the variable assignment stays. It is NOT hoisted.
  console.log(x); // outputs 2
}

// Function example

a(); // will run properly
b(); // will fail with TypeError because b isn't assigned until line 4
function a() { }
var b = function() { }

The above is equivalent to:
function a() {} // the function statement is hoisted
var b;
a();
b(); // = undefined(); invoking undefined raises an error
b = function() {}
```

<a name="scope-chain"></a>
### ii. Scope Chain 
To find a variable when functions are running, JS looks further than just the variable environment of the currently executing context, it also looks at the outer environment (the environment to which this function is _lexically_ attached). This process continues looking all the way down to the global environment in a process known as the __scope chain_.

```javascript
function b() {
    console.log(myVar);
}

function a() {
    let myVar = 2;
    b();
}

let myVar = 1;
a();

// function b does not have a myVar variable so it looks to its outer environment
// here, b is lexically attached to the global object, and myVar exists in the global environment
// therefore, it logs '1'


// JavaScript has function scope; functions get their own scope but other blocks
// do not.
if (true){
    var i = 5;
}
i; // = 5 - not undefined as you'd expect in a block-scoped language
```

<a name="closures"></a>
### iii. Closures 
One of JS's most powerful features is __closures__. Whenever a function is nested within another function, the inner function has access to all the outer function's variables even after the outer function exits.
After the outer function exits, it is popped off the Execution Stack, however if any of its variables are referenced in the inner function, then those variables are "closed into" the inner function's Execution Context and are accessible by the inner function.

```javascript
// Example 1

function sayHelloInFiveSeconds(name){
    var prompt = "Hello, " + name + "!";
    // Inner functions are put in the local scope by default, as if they were
    // declared with `var`.
    function inner(){
        alert(prompt);
    }
    setTimeout(inner, 5000);
    // setTimeout is asynchronous, so the sayHelloInFiveSeconds function will
    // exit immediately, and setTimeout will call inner afterwards. However,
    // because inner is "closed over" sayHelloInFiveSeconds, inner still has
    // access to the `prompt` variable when it is finally called.
}
sayHelloInFiveSeconds("Adam"); // will open a popup with "Hello, Adam!" in 5s


// Example 2

function buildFunctions() {
    var arr = [];    
    for (var i = 0; i < 3; i++) {
        arr.push(
            function() {
                console.log(i);   
            }
        )
    }
    return arr;
}

var fs = buildFunctions();
fs[0]();
fs[1]();
fs[2]();
// all 3 of these will log '3' because after buildFunctions finishes, i = 3
// when fs[0..2] are invoked, they look for the value of i and they all see 3

// Avoid creating functions in loops. It will lead to bad behaviour.
```

<a name="callbacks"></a>
### iv. Callbacks 
__Callbacks__ are simply functions passed as arguments to other functions to be run when the other functions are finished.

```javascript
function alertWhenDone(callback){
  // do some work
  callback(); // invoke the callback right before exiting
}

alertWhenDone(function(){
  alert("I am done!"); 
});

// Callback making use of the JavaScript Timer
setTimeout(function(){
  alert("3 seconds have passed.");
}, 3000);
```

<a name="arrow-function"></a>
### v. Arrow Function
An arrow function expression is a compact alternative to a traditional function expression, but is limited and can't be used in all situations.
There are differences between arrow functions and traditional functions, as well as some limitations:
- Arrow functions don't have their own bindings to this or super, and should not be used as methods.
- Arrow functions don't have access to the new.target keyword.
- Arrow functions aren't suitable for call, apply and bind methods, which generally rely on establishing a scope.
- Arrow functions cannot be used as constructors.
- Arrow functions cannot use yield, within its body.

// Basic Usage
```javascript
let empty = () => {}; // An empty arrow function returns undefined
let fruits => (["orange", "banana", "apple", "mango"]
                "grapes", "strawberry"); // round brackets are added if the expression is very long

(() => 'foobar')(); // Returns "foobar" // (this is an Immediately Invoked Function Expression)

var simple = a => a > 15 ? 15 : a;
simple(16); // 15
simple(10); // 10

let max = (a, b) => a > b ? a : b;

// Easy array filtering, mapping, ...

var arr = [5, 6, 13, 0, 1, 18, 23];

var sum = arr.reduce((a, b) => a + b); // 66   reduce(callback(accumulate, currentValue), initialValue)

var even = arr.filter(v => v % 2 == 0); // [6, 0, 18]

var double = arr.map(v => v * 2); // [10, 12, 26, 0, 2, 36, 46]

var num = arr.find(no=> no === 5); // 5

arr.forEach(num=> console.log(num));

var isEveryGreaterThanTwo = arr.every(num=> num > 2); // false
var isAnyGreaterThanTwo = arr.some(num=> num > 2); // true

var asc = arr.slice().sort((a,b) => a-b);
var desc = arr.slice().sort((a,b) => b-a);

// More concise promise chains
promise.then(a => {
  // ...
}).then(b => {
  // ...
});

// Parameterless arrow functions that are visually easier to parse
setTimeout( () => {
  console.log('I happen sooner');
  setTimeout( () => {
    // deeper code
    console.log('I happen later');
  }, 1);
}, 1);
``` 
<a name="es6-features"></a>
## 5. ES6 Features 

<a name="default-parameters"></a>
### i. Default Parameters
```javascript
function multiply(a, b=1) {
    return a*b;
}
multiply(4); //4
multiply(4,5); //20
```

<a name="spread-operator"></a>
### ii. Spread Operator
```javascript
// Spread for function calls - expands an iterable (array, string, etc.) into a list of arguments
Math.max(1,2,3,4,5,6); //6
let arr = [1,2,3,4,5,6]
Math.max(arr); //NaN
Math.max(...arr); //6

// Spread in array literals - create a new array using an existing array
const arr1 = [1,2,3];
const arr2 = [4,5,6];
const arr = [...arr1, ...arr2]; // [1,2,3,4,5,6]

// Spread in Onject literals - copies properties from one object into another object
let obj1 = { foo: 'bar', x: 42 };
let obj2 = { foo: 'baz', y: 13 };

let clonedObj = { ...obj1 };// Object { foo: "bar", x: 42 }
let mergedObj = { ...obj1, ...obj2 , z: 32};// Object { foo: "baz", x: 42, y: 13, z: 32}
let obj = {...[1,2,3]}; // {0:1, 1:2, 2:3}
```

<a name="rest-parameters"></a>
### iii. REST Paramaters
collects all remaining arguments into an actual array
```javascript
funtion sumAll(...nums){
    let total = 0;
    for(let n of nums){
     total += n;   
    }
    return total;
}

sumAll(1,2,3,4,5); //15
sumAll(1,2,3); //6
```
<a name="destructuring"></a>
### iv. Destructuring
The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.
```javascript
// destructuring array examples
const foo = ['one', 'two'];
const [red, yellow, green, blue] = foo; //red=one yellow=two green=undefined blue=undefined 

let a, b;
[a, b] = [1, 2]; //a=1 b=2
[a=5, b=7] = [1]; //a=1, b=7

let a, b, rest;
[a, b, , ...rest] = [10, 20, 30, 40, 50]; // a=10 b=20 c=[40, 50]

// destructuring objects
const user = {
    id: 42,
    first: "john",
    last: "carter",
    isVerified: true
};
const {id, isVerified} = user;
const {id, first, ...other} = user; // id=42, first=john, other={last:carter, isverified: true}

const o = {p: 42, q: true};
const {p: foo, q: bar} = o; // foo=42 bar=true

const {a = 10, b = 5} = {a: 3}; // a=10,b=5
const {a: aa = 10, b: bb = 5} = {a: 3}; //aa=10,bb=5

//param destructuring
print = ({first, last}) => console.log(`{first} {last}`);
print(user);
```
