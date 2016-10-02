# javascript-notes

Variables
-------------
Declare a variable with `var foo;` or `var foo = "bar";`
Assign to a variable with `foo = "newvalue"`

In an assignment, the thing on the left is a _variable_ and the thing on the right is a _value_.

`foo = "sdf"` means: assign the value `"sdf"` to the variable `foo`.

`foo = bar` means: assign the value held by the variable `bar` to the variable `foo`.

`foo = myfn()` means: assign the return value of calling `myfn` to the variable `foo`"

`foo = myfn` means: assign the function called `myfn` to the variable `foo`.


Primitive types
---------------------
These are `number`, `boolean`, `string`, `function`
The first 3 are selected automatically when I assign to a variable.
* `foo = 3` would cause `foo` to be a `number`
* `foo = true` => boolean
* `foo = "hello"` => string

The last must be explicitly created using the `function` keyword, in one of two ways:
* `foo = function() { console.log("hello world") };`
* `function foo() { console.log("hello world") };`

Booleans
-------------
A boolean can either be true or false. You use them in `if` statements, mostly.

The following boolean operations exist:

*  AND: Use `&&`: `true && true` is `true`, `true && false` is false, and `false && false` is `false.
*  OR: Use `||`: `true || true` is `true`, `true || false` is `true`, and `false || false` is `false.
*   NOT: Use `!`: `!true` is `false` and `!false` is `true`

Objects and Lists
-----------------
Declare a list using square brackets: 

    var mylist = ["a", "b", "c"];

Access elements using square brackets: 

	mylist[0] // "a"

Declare an object using curly braces: 

	var myobject = { a: "eyy", b: "bee", c: "cee" };

Access elements using dots: 

	myobject.b // "bee"

Functions: parameters
--------------------
Functions are executed, or “called” using brackets.

    function myfunction() { 
	    console.log("hello world"); 
	}
    myfunction() // prints “hello world”

Functions can be given parameters:

	function myfunction2(greeting) {
	    console.log(greeting + “ world”);
    }
	myfunction2("good evening"); // prints “good evening world”
	
We say the variable `greeting` is bound in the function `myfunction2`. When `myfunction("good evening");` is run, `greeting` is bound to the value “good evening”.
You can have multiple parameters: 

    function myfunction3(param1, param2) { }

Functions: return values
-----------------------
Most functions have return values. This can be thought of as the output of the function:

    function convertToGreeting(thing) { 
	    return "hello "+thing; 
	}
    var newgreeting = convertToGreeting("train");
    console.log(newgreeting); // prints “hello train”;

Scope
-----
Scope is the list of variables that are available in a section of code.
There are two scopes: global and local.
To add to the global scope, assign to a variable without first declaring it with `var`:

    myundeclaredvariable = 100;
    
Variables in the global scope can be accessed from anywhere.

Local scope is created when functions are executed:
    
    function myfunction() {
	    var a = "something";
	    console.log(a); // prints "something"
    }
    console.log(a); // prints undefined
    
You can see here that the variable `a` was accessible within the function, but not outside of it. This is because functions have their own _local scope_ that is not accessible from outside of the function. This _local scope_ also contains all the variables that were defined in the _parent scope_, i.e. outside the function:

    var myparentscopevariable = "accessible from function";
    function myfunction() {
	    var mylocalscopevariable = "something";
	    console.log(myparentscopevariable); // prints "accessible from function"
	    console.log(mylocalscopevariable); // prints "something"
    }

State
-----
Often you will need to make variables accessible from many parts of your code. Here is an example of how to do it:

    var mystatevariable;
    function init() {
        mystatevariable = "initial value";
    }
    function run() {
        mystatevariable = "running";
    }
    init();
    console.log(mystatevariable); // prints "initial value"
    run();
    console.log(mystatevariable); // prints "running"
    
Here you can see that because the variable was declared outside of the two functions (in their _parent scope_), it can be read and assigned to by the functions.

Modifying variables in a parent scope is called _stateful programming_. The opposite is called _functional programming_: to do this properly, you aren't allowed to use parent scope variables:

    function init() {
        return "initial value";
    }
    function run() {
        return "running";
    }
    var mystatevariable = init();
    console.log(mystatevariable);
    mystatevariable = run();
    console.log(mystatevariable);

Shadowing
---------------
A common source of bugs:

	var myvar;
	function run() {
        var myvar = "running";
        console.log(myvar); // prints "running"
    }
    run();
    console.log(myvar); // prints undefined

Here, when `myvar` is assigned to in the function, we are not touching the `myvar` in the parent scope. So when we log it after running the function, it is still undefined. This is because the function has its own _local scope_, and the `var` keyword creates a new variable called `myvar` in this local scope. The assignment then acts on this new variable, instead of the variable in the parent scope. This is called *shadowing*, as the new variable _casts a shadow_ on the old so it can't be seen.

The fix:

	var myvar;
	function run() {
        myvar = "running";
        console.log(myvar); // prints "running"
    }
    run();
    console.log(myvar); // prints "running"
    
Here, because we aren't using the `var` keyword in the function, the assignment is performed on the variable in parent scope.

Events
---------
Lots of UI javascript programming relies on events. This is because it is the main system through which user actions are linked to javascript code. This is the pattern:

	myElement.on("click", handler)
	function handler() {
		console.log("clicked");
		return 3;
	}

Here you are calling the `on` function of `myElement`, passing it the event name `"click"` and the function called `handler`.

This causes the `"click"` event of `myElement` to trigger execution of the `handler` function.

Notice that `handler`, NOT `handler()`, is passed to the `on` function. This is because `handler` is a function, whereas `handler()` gives you the result of calling the function.

`myElement.on("click", handler())` would be the same as `myElement.on("click", 3)`.

You can also try:

	console.log(handler); // prints "function ..."
	console.log(handler()); // prints 3

Event Propagation
--------------------------
Sometimes, you will want to handle an event and stop it from triggering any other code – for example, stopping a "click" event from triggering a button click _and_ a background click:

	function buttonClickHandler(event) {
		// do stuff
		event.stopPropagation()
	}

Depending on the library, the function used to stop the event could be called something different, e.g. `event.preventDefault()`
