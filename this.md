# `this` keyword in JavaScript

*`this` is a dynamic reference to the context in which a function is executed*

## `this` follows the following rules:
 - If the `new` keyword is used when calling a function, i.e. function was used as a function constructor than: `this` inside the function is the newly-created object instance
 
```
function Person(name, age) {
  // 'this' refers to the new object being created
  this.name = name;
  this.age = age;

  // Adding a method to the object
  this.greet = function() {
    console.log('Hello, my name is ' + this.name + ' and I am ' + this.age + ' years old.');
  };
}

// Using 'new' to create a new instance of the Person object
const person1 = new Person('Avinash', 32);

// Accessing properties and methods of the new object
console.log(person1.name);  // Output: Avinash
console.log(person1.age);   // Output: 32

```
#### Explanation:
- Person Function: This is a constructor function, meaning it is intended to be used with the new keyword to create new object instances.

`new Person('Avinash', 32)`

- When new is used, JavaScript automatically creates a new empty object.
The this keyword inside the Person function refers to this newly created object.
Properties like name and age are added to this new object.
person1: This is the newly created object (instance) with properties name and age, and a method greet.

- Key Points:
When using new, the function acts as a constructor.
The this keyword refers to the newly created object.
*The newly created object is implicitly returned unless a non-primitive value is explicitly returned.*

Explaination to this statement: The newly created object is implicitly returned unless a non-primitive value is explicitly returned.

- In JavaScript, when you use the new keyword with a constructor function, the function automatically returns the newly created object unless the function explicitly returns a non-primitive value (like an object, array, function, etc.).

- If the constructor function returns a primitive value (such as a number, string, boolean, etc.), that return value is ignored, and the newly created object is returned instead.

Key Points:
- If the function returns a non-primitive value (object, array, etc.), that value is returned instead of the newly created object.
If the function returns a primitive value, JavaScript ignores it and returns the newly created object.
```
function Person(name) {
  this.name = name;
  
  // Explicitly returning a primitive value (string)
  return 'This is ignored';
}

const person1 = new Person('Avinash');

console.log(person1);  // Output: Person { name: 'Avinash' }
```

#### Explanation:
Even though the constructor function returns a string ('This is ignored'), it is ignored because it's a primitive value.
The newly created object with the property name is returned instead.

`Example 2: Returning a non-primitive value (used)`
```
function Person(name) {
  this.name = name;
  
  // Explicitly returning an object (non-primitive)
  return { greeting: 'Hello, I override the instance' };
}

const person2 = new Person('Avinash');

console.log(person2);  // Output: { greeting: 'Hello, I override the instance' }

```

#### Explanation:
- In this case, the constructor explicitly returns an object ({ greeting: 'Hello, I override the instance' }), which is a non-primitive value.
Therefore, the newly created object (this) is overridden, and the returned object is the final result.
 

`Example 3: No explicit return`
```
function Animal(type) {
  this.type = type;
}

const animal1 = new Animal('Dog');

console.log(animal1);  // Output: Animal { type: 'Dog' }


```

#### Explanation:
- Since there is no explicit return in the Animal constructor, the newly created object with the property type is returned by default.

- Summary:
If a constructor returns a primitive value, JavaScript ignores it and returns the newly created object.
If a constructor returns a non-primitive value (object, array, etc.), that returned value overrides the newly created object and becomes the final result


## `this` follows the following rules[cont.]
- If this is used in a class constructor, the this inside the constructor is the newly-created object instance.

```
class Person {
  constructor(name, age) {
    // 'this' refers to the newly created object instance
    this.name = name;
    this.age = age;
  }

  // Adding a method to the class
  greet() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const person1 = new Person('Avinash', 32);

// Accessing properties and methods of the new object
console.log(person1.name);  // Output: Avinash
console.log(person1.age);   // Output: 32

// Calling the greet method
person1.greet();  // Output: Hello, my name is Avinash and I am 32 years old.

```

#### Explanation:Class Declaration:

- The Person class has a constructor method. This method is called automatically when a new object is created using new Person().
Constructor:

- Inside the constructor method (constructor(name, age)), the this keyword refers to the newly created object.

`this.name = name;` assigns the name argument to the new object’s name property.
`this.age = age;` assigns the age argument to the new object’s age property.

#### Object Instantiation:

When we create an object using new Person('Avinash', 32), the constructor is called, and a new object is created.
The this inside the constructor refers to this new object, and the properties name and age are initialized for this object.
Accessing Properties and Methods:

After instantiating person1, we can access the properties name and age, and call the greet() method, all through the this object, which refers to person1.
Key Points:
Inside the class constructor, this refers to the instance of the class that is being created.
The this keyword is used to assign properties or methods to the newly created object.
Similar to function constructors, if new is used, the constructor automatically returns the newly created object (unless a non-primitive is explicitly returned).
This is how this operates within class constructors, helping to create and manage instances of the class.

## `this` follows the following rules[cont.]
- If apply(), call(), or bind() is used to call/create a function, this inside the function is the object that is passed in as the argument.
- If a function is called as a method (e.g. obj.method()) — this is the object that the function is a property of.
- If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, this is the global object. In the browser, the global object is the window object. If in strict mode ('use strict';), this will be undefined instead of the global object.
- If multiple of the above rules apply, the rule that is higher wins and will set the this value.
-If the function is an ES2015 arrow function, it ignores all the rules above and receives the this value of its surrounding scope at the time it is created.
`Arrow functions do not follow these rules and instead inherit this from their surrounding lexical environment.`


#### Here’s an example to demonstrate this:

Example:
```
const person = {
  name: 'Avinash',
  age: 32,
  // Regular function inside the object
  greet: function() {
    console.log('Hello, my name is ' + this.name); // 'this' refers to 'person' object
  },
  // Arrow function inside the object
  greetWithArrow: () => {
    console.log('Hello, my name is ' + this.name); // 'this' lexically inherits from outside
  }
};

person.greet();  // Output: Hello, my name is Avinash
person.greetWithArrow();  // Output: Hello, my name is undefined
```

#### Explanation:
- Regular Function greet: When person.greet() is called, this refers to the person object because the method is invoked on that object.
So, it correctly prints 'Avinash' using this.name.


- Arrow Function greetWithArrow: The arrow function greetWithArrow does not have its own this. Instead, it lexically inherits this from the surrounding context where it was defined.
Since the arrow function is defined in the global scope (outside any other function), this refers to the global object (or undefined in strict mode).
In the global context, this.name is undefined (because there's no name property in the global scope), which is why it prints 'undefined'.
call, apply, or bind Does Not Affect Arrow Functions:
Even if you try to change the this context using call, apply, or bind, arrow functions will still not change their this because they are bound to the lexical scope.

```
const person = {
  name: 'Avinash',
};

const arrowFunc = () => {
  console.log(this.name);
};

// Trying to change 'this' using 'call'
arrowFunc.call(person);  // Output: undefined
```

#### Explanation:
- arrowFunc.call(person): Even though we are using call() to try and set this to the person object, the arrow function still uses the this from its lexical scope (likely the global object), so this.name is undefined.

#### Conclusion: 
 - Arrow functions inherit this from their surrounding lexical scope and do not get their own this.
Their this binding cannot be changed with call, apply, or bind.



# What is `this`
- In JavaScript, this is a keyword that refers to the `current execution context of a function or script`. It's a fundamental concept in JavaScript, and understanding how this works is crucial for building robust and maintainable applications.




#### Used globally
- In the global scope, this refers to the global object, which is the window object in a web browser or the global object in a Node.js environment.

```
console.log(this); // In a browser, this will log the window object (for non-strict mode).
```

- Within a regular function call: When a function is called in the global context or as a standalone function, this refers to the global object (in non-strict mode) or undefined (in strict mode).
```
function showThis() {
  console.log(this);
}
showThis(); // In non-strict mode: Window (global object). In strict mode: undefined.
```

- Within a method call: When a function is called as a method of an object, this refers to the object that the method is called on.
```
const obj = {
  name: 'John',
  showThis: function () {
    console.log(this);
  },
};

obj.showThis(); // { name: 'John', showThis: ƒ }
```
- *Note that if you do the following, it is as good as a regular function call and not a method call. this has lost its context and no longer points to obj.*
```
const obj = {
  name: 'John',
  showThis: function () {
    console.log(this);
  },
};

const showThisStandalone = obj.showThis;
// Here, we assign the function obj.showThis to a new variable showThisStandalone. This effectively detaches the method from the object obj. Now showThisStandalone is just a standalone function reference.

showThisStandalone(); // In non-strict mode: Window (global object). In strict mode: undefined.
```



- Within a function constructor: When a function is used as a constructor (called with the new keyword), this refers to the newly-created instance. In the following example, this refers to the Person object being created, and the name property is set on that object.
```
function Person(name) {
  this.name = name;
}

const person = new Person('John');
console.log(person.name); // "John"

```

- Within class constructor and methods: In ES2015 classes, this behaves as it does in object methods. It refers to the instance of the class.

```
class Person {
  constructor(name) {
    this.name = name;
  }

  showThis() {
    console.log(this);
  }
}

const person = new Person('John');
person.showThis(); // Person {name: 'John'}

const showThisStandalone = person.showThis;
showThisStandalone(); // `undefined` because all parts of a class' body are strict mode.

```

- Explicitly binding this: You can use bind(), call(), or apply() to explicitly set the value of this for a function.

Using the call() and apply() methods allow you to explicitly set the value of this when calling the function.

```
function showThis() {
  console.log(this);
}
const obj = { name: 'John' };

showThis.call(obj); // { name: 'John' }
showThis.apply(obj); // { name: 'John' }
const boundFunc = showThis.bind(obj);
boundFunc(); // { name: 'John' }

```

- Within arrow functions: Arrow functions do not have their own this context. *Instead, the this is lexically scoped, which means it inherits the this value from its surrounding scope at the time they are defined.*

In this example, this refers to the global object (window or global), because the arrow function is not bound to the person object.

```
const testingThis = {
    game: 'aaa',
    when: this,
    what: this.game,
    where: function() {
        return this.game
    },
    who: () => this.game,

};
console.log(testingThis);
console.log(testingThis.when);
console.log(testingThis.where());
console.log(testingThis.who());


// game: "aaa"what: undefinedwhen: Window {0: global, window: Window, self: Window, document: document, name: '', location: Location, …}where: ƒ ()who: () => this.game[[Prototype]]: Object
// THIS IN ARROW FUNCTIONS:12 Window {0: global, window: Window, self: Window, document: document, name: '', location: Location, …}
// THIS IN ARROW FUNCTIONS:13 aaa
// THIS IN ARROW FUNCTIONS:14 undefined
// VM434 THIS IN ARROW FUNCTIONS:1 undefined

```

#### Another example
```

const obj = {
  name: 'John',
  showThis: function () {
    const arrowFunc = () => {
      console.log(this);
    };
    arrowFunc();
  },
};

obj.showThis(); // { name: 'John', showThis: ƒ }

const showThisStandalone = obj.showThis;
showThisStandalone(); // In non-strict mode: Window (global object). In strict mode: undefined.



```

- Therefore, the this value in arrow functions cannot be set by bind(), apply() or call() methods, nor does it point to the current object in object methods.

```
const obj = {
  name: 'Alice',
  regularFunction: function () {
    console.log('Regular function:', this.name);
  },
  arrowFunction: () => {
    console.log('Arrow function:', this.name);
  },
};

const anotherObj = {
  name: 'Bob',
};

// Using call/apply/bind with a regular function
obj.regularFunction.call(anotherObj); // Regular function: Bob
obj.regularFunction.apply(anotherObj); // Regular function: Bob
const boundRegularFunction = obj.regularFunction.bind(anotherObj);
boundRegularFunction(); // Regular function: Bob

// Using call/apply/bind with an arrow function, `this` refers to the global scope and cannot be modified.
obj.arrowFunction.call(anotherObj); // Arrow function: window/undefined (depending if strict mode)
obj.arrowFunction.apply(anotherObj); // Arrow function: window/undefined (depending if strict mode)
const boundArrowFunction = obj.arrowFunction.bind(anotherObj);
boundArrowFunction(); // Arrow function: window/undefined (depending if strict mode)

```

- Within event handlers: When a function is called as a DOM event handler, this refers to the element that triggered the event. In this example, this refers to the <button> element that was clicked.

```
<button id="my-button" onclick="console.log(this)">Click me</button>
<!-- Logs the button element -->

```

- When setting an event handler using JavaScript, this also refers to the element that received the event.

```
document.getElementById('my-button').addEventListener('click', function () {
  console.log(this); // Logs the button element
});

```

- As mentioned above, ES2015 introduces arrow functions which uses the enclosing lexical scope. This is usually convenient, but does prevent the caller from defining the this context via .call/.apply/.bind. One of the consequences is that DOM event handlers will not properly bind this in your event handler functions if you define the callback parameters to .addEventListener() using arrow functions.

```
document.getElementById('my-button').addEventListener('click', () => {
  console.log(this); // Window / undefined (depending on whether strict mode) instead of the button element.
});
```


# Other notes from video

-  `this` in global space // window
- this inside a function
```
function x() {
  console.log(this);
}
x();
// the `this` corresponds to window object
```

and 

```
'use strict';
function x() {
  console.log(this);
}
x();
// this is undefined
```
#### Explaination
- `this substitution` : In non-strict mode, whenever the value of this is undefined or null it's replaced by global object, in our case window object
- the value of `this` inside a function is undefined but due to `this substitution` it's value is replaced

### Another example
- **Concept**: *The value of this keyword depends on how the function is called*
- In the following code snipets
```
'use strict';
function x() {
  console.log(this);
}
x(); // undefined
window.x() // window
```
### Explaination
- If the function is called without any reference of an object then it's undefined
- While when use window as the ***calling object*** then this becomes window
during ***runtime binding***


- ***The this keyword refers to the context where a piece of code, such as a function's body, is supposed to run***. Most typically, it is used in object methods, where this refers to the object that the method is attached to, thus allowing the same method to be reused on different objects.

- ***The value of this in JavaScript depends on how a function is invoked (runtime binding), not how it is defined.*** 

- When a regular function is invoked as a method of an object (obj.method()), this points to that object. 
- When invoked as a standalone function (not attached to an object: func()), this typically refers to the global object (in non-strict mode) or undefined (in strict mode). 
- The Function.prototype.bind() method can create a function whose this binding doesn't change, and methods Function.prototype.apply() and Function.prototype.call() can also set the this value for a particular call.

- Arrow functions differ in their handling of this: they inherit this from the parent scope at the time they are defined. This behavior makes arrow functions particularly useful for callbacks and preserving context. ***However, arrow functions do not have their own this binding. Therefore, their this value cannot be set by bind(), apply() or call() methods, nor does it point to the current object in object methods.***

# Other notes from video[cont.]

- `this` inside object`s method

```
If a function is a part of any object then it's called a method
```

```
const obj = {
  a: 10,
  x: function() {
    console.log(this);
  }
}

obj.x(); // this is obj
let val = obj.x;
val(); // window

Output:
obj.x();

The method x is called as a method of obj, so in non-strict mode, this will refer to obj.

Output: this { a: 10, x: [Function: x] }
val();

- When x is assigned to val and then called, it is no longer tied to obj. In non-strict mode, when a function is invoked like this, this defaults to the global object (window in browsers or global in Node.js).

Output: this Window (in browsers) or this global (in Node.js).

Explanation:
In non-strict mode, if a function is called as a method of an object, this refers to that object.

However, when the function is detached from the object and called on its own, this defaults to the global object (window or global), not undefined as it would in strict mode.

```

Other notes from video[cont.]
- Call apply bind methods (sharing methods)
- `this` inside arrow functions

```
const obj2 = {
  a: 5,
  x : function() {
    const yn =  () => {
      console.log(this.a);
    };
    yn();
  }
}
obj2.x();
let v = obj2.x;
console.log(v);
v();

5
THIS IN ARROW FUNCTIONS:44 ƒ () {
        const yn = () => {
            console.log(this.a);
        }
        ;
        yn();
    }
THIS IN ARROW FUNCTIONS:36 undefined
VM110 THIS IN ARROW FUNCTIONS:1 undefined

```

```
Output:
console.log(v);:

You assign obj2.x to v. Since obj2.x is a function, v is a reference to this function.
Output:
javascript
Copy code
[Function: x]
v();:

Here, you are calling v() (which is obj2.x), but now it's detached from obj2. Normally, in non-strict mode, when you invoke a function this way, this would refer to the global object.

However, arrow functions capture this from their lexical scope, meaning that yn() inside x retains the this value from the surrounding function x.

Because v() is called in the global context, this will refer to the global object (window in browsers or global in Node.js). Since the global object doesn’t have an a property, this.a is undefined.

Output:

javascript
Copy code
undefined
Explanation:
When you use an arrow function like yn(), it doesn't have its own this but instead uses this from the surrounding function (x).
Since v() is called in the global context, this.a inside yn() refers to global.a (or window.a), which is undefined because the global object doesn't have an a property.
```


Other notes from video[cont.]
- `this` inside DOM elements
- reference to HTML element
