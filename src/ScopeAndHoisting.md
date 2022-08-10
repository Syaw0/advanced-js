## JavaScript Variable Scope and Hoisting Explained ⚓

In this post, we will learn JavaScript’s variable scope and hoisting and all the idiosyncrasies of both.

We must understand how variable scope and variable hoisting work in JavaScript, if want to understand JavaScript well. These concepts may seem straightforward; they are not. Some important subtleties exist that we must understand, if we want to thrive and excel as JavaScript developers.

---

###  **Variable Scope**  
A variable’s scope is the context in which the variable exists. The scope specifies from where you can access a variable and whether you have access to the variable in that context.



Variables have either a local scope or a global scope.

----

### **Local Variables (Function-level scope)**  
Unlike most programming languages, JavaScript does not have block-level scope (variables scoped to surrounding curly brackets); instead, JavaScript has function-level scope. Variables declared within a function are local variables and are only accessible within that function or by functions inside that function.
  
Demonstration of Function-Level Scope

```javascript
var name = "Richard";

function showName () {
    var name = "Jack"; // local variable; only accessible in this showName function
    console.log (name); // Jack
}
console.log (name); // Richard: the global variable

```

No Block-Level Scope

```javascript
var name = "Richard";
// the blocks in this if statement do not create a local context for the name variable
if (name) {
    name = "Jack"; // this name is the global name variable and it is being changed to "Jack" here
    console.log (name); // Jack: still the global variable
}

// Here, the name variable is the same global name variable, but it was changed in the if statement
console.log (name); // Jack

```

-   **If You Don’t Declare Your Local Variables, Trouble is Nigh**  
    Always declare your local variables before you use them. In fact, you should use  [JSHint](http://www.jshint.com/)  to check your code for syntax errors and style guides. Here is the trouble with not declaring local variables:
    
    ```javascript
    // If you don't declare your local variables with the var keyword, they are part of the global scope
    var name = "Michael Jackson";
    
    function showCelebrityName () {
        console.log (name);
    }
    
    function showOrdinaryPersonName () {    
        name = "Johnny Evers";
        console.log (name);
    }
    showCelebrityName (); // Michael Jackson
    
    // name is not a local variable, it simply changes the global name variable
    showOrdinaryPersonName (); // Johnny Evers
    
    // The global variable is now Johnny Evers, not the celebrity name anymore
    showCelebrityName (); // Johnny Evers
    
    // The solution is to declare your local variable with the var keyword
    function showOrdinaryPersonName () {    
        var name = "Johnny Evers"; // Now name is always a local variable and it will not overwrite the global variable
        console.log (name);
    }
    
    ```
    
-   **Local Variables Have Priority Over Global Variables in Functions**  
    If you declare a global variable and a local variable with the same name, the local variable will have priority when you attempt to use the variable inside a function (local scope):
    
    ```javascript
    var name = "Paul";
    
    function users () {
        // Here, the name variable is local and it takes precedence over the same name variable in the global scope
    var name = "Jack";
    
    // The search for name starts right here inside the function before it attempts to look outside the function in the global scope
    console.log (name); 
    }
    
    users (); // Jack
    
    ```
    

### **Global Variables**  
All variables declared outside a function are in the global scope. In the browser, which is what we are concerned with as front-end developers, the global context or scope is the window object (or the entire HTML document).

-   Any variable declared or initialized outside a function is a global variable, and it is therefore available to the entire application. For example:
    
    ```javascript
    // To declare a global variable, you could do any of the following:
    var myName = "Richard";
    
    // or even
    firstName = "Richard";
    
    // or 
    var name; //
    name;
    
    ```
    
    It is important to note that all global variables are attached to the  _window_  object. So, all the global variables we just declared can be accessed on the window object like this:
    
    ```javascript
    console.log(window.myName); // Richard;
    
     // or
    console.log("myName" in window); // true
    console.log("firstName" in window); // true
    
    ```
    
-   If a variable is initialized (assigned a value) without first being declared with the var keyword, it is automatically added to the global context and it is thus a global variable:
    
    ```javascript
    function showAge () {
        // Age is a global variable because it was not declared with the var keyword inside this function
        age = 90;
        console.log(age);// 
    }
    
    showAge (); // 90
    
    // Age is in the global context, so it is available here, too
    console.log(age); // 90
    
    ```
    
    Demonstration of variables that are in the Global scope even as they seem otherwise:
    
    ```javascript
    // Both firstName variables are in the global scope, even though the second one is surrounded by a block {}. 
    var firstName = "Richard";
    {
    var firstName = "Bob";
    }
    
    // To reiterate: JavaScript does not have block-level scope
    
    // The second declaration of firstName simply re-declares and overwrites the first one
    console.log (firstName); // Bob
    
    ```
    
    Another example
    
    ```javascript
    for (var i = 1; i <= 10; i++) {
        console.log (i); // outputs 1, 2, 3, 4, 5, 6, 7, 8, 9, 10;
    };
    
    // The variable i is a global variable and it is accessible in the following function with the last value it was assigned above 
    function aNumber () {
    console.log(i);
    }
    
    // The variable i in the aNumber function below is the global variable i that was changed in the for loop above. Its last value was 11, set just before the for loop exited:
    aNumber ();  // 11
    
    
    ```
    
-   **setTimeout Variables are Executed in the Global Scope**  
    Note that all functions in setTimeout are executed in the global scope. This is a tricky bit; consider this:
    
    ```javascript
     // The use of the "this" object inside the setTimeout function refers to the Window object, not to myObj
    
    var highValue = 200;
    var constantVal = 2;
    var myObj = {
        highValue: 20,
        constantVal: 5,
        calculateIt: function () {
     setTimeout (function  () {
        console.log(this.constantVal * this.highValue);
    }, 2000);
        }
    }
    
    // The "this" object in the setTimeout function used the global highValue and constantVal variables, because the reference to "this" in the setTimeout function refers to the global window object, not to the myObj object as we might expect.
    
    myObj.calculateIt(); // 400
    // This is an important point to remember.
    
    ```
    
-   **Do not Pollute the Global Scope**  
    If you want to become a JavaScript master, which you certainly want to do (otherwise you will be watching Honey Boo Boo right now), you have to know that it is important to avoid creating many variables in the global scope, such as this:
    
    ```javascript
    // These two variables are in the global scope and they shouldn't be here
    var firstName, lastName;
    
    function fullName () {
        console.log ("Full Name: " + firstName + " " + lastName );
    }
    
    ```
    
    This is the improved code and the proper way to avoid polluting the global scope
    
    ```javascript
    // Declare the variables inside the function where they are local variables
    
    function fullName () {
        var firstName = "Michael", lastName = "Jackson";
    
        console.log ("Full Name: " + firstName + " " + lastName );
    }
    
    ```
    
    In this last example, the function fullName is also in the global scope.
    

----------

### **Variable Hoisting**  
All variable declarations are hoisted (lifted and declared) to the top of the function, if defined in a function, or the top of the global context, if outside a function.

It is important to know that only variable declarations are hoisted to the top, not variable initialization or assignments (when the variable is assigned a value).

Variable Hoisting Example:

```javascript
function showName () {
console.log ("First Name: " + name);
var name = "Ford";
console.log ("Last Name: " + name);
}

showName (); 
// First Name: undefined
// Last Name: Ford

// The reason undefined prints first is because the local variable name was hoisted to the top of the function
// Which means it is this local variable that get calls the first time.
// This is how the code is actually processed by the JavaScript engine:

function showName () {
    var name; // name is hoisted (note that is undefined at this point, since the assignment happens below)
console.log ("First Name: " + name); // First Name: undefined

name = "Ford"; // name is assigned a value

// now name is Ford
console.log ("Last Name: " + name); // Last Name: Ford
}

```

### **Function Declaration Overrides Variable Declaration When Hoisted**  
Both function declaration and variable declarations are hoisted to the top of the containing scope. And function declaration takes precedence over variable declarations (but not over variable assignment). As is noted above, variable assignment is not hoisted, and neither is function assignment. As a reminder, this is a function assignment: var myFunction = function () {}.  
Here is a basic example to demonstrate:

```javascript 
 // Both the variable and the function are named myName
var myName;
function myName () {
console.log ("Rich");
}

// The function declaration overrides the variable name
console.log(typeof myName); // function

```

```javascript
 // But in this example, the variable assignment overrides the function declaration
var myName = "Richard"; // This is the variable assignment (initialization) that overrides the function declaration.

function myName () {
console.log ("Rich");
}

console.log(typeof myName); // string 

```

It is important to note that function expressions, such as the example below, are not hoisted.

```javascript
var myName = function () {
console.log ("Rich");
} 

```

In strict mode, an error will occur if you assign a variable a value without first declaring the variable. Always declare your variables.
