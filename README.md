# JavaScript Detailed Notes

A (semi) detailed overview of the JavaScript language and how it works under the hood.

- [The JS Engine, Execution Context and The Call Stack](#the-js-engine-execution-context-and-the-call-stack)
- [Hoisting in JS](#hoisting-in-js)
- [Window Object](#window-object)
- [Scope & Lexical Environment](#scope--lexical-environment)
- [Block Scope](#block-scope)
- [Shadowing](#shadowing)
- [Closures](#closures)
- [Interview Questions](#interview-questions)
- [Creator](#creator)

## The JS Engine, Execution Context and The Call Stack

- In JS, eveything happens inside the Execution Context

- EC has 2 parts

  1. Memory Component aka. Variable Environment
  2. Code Component aka. Thread of Execution

- JS is Synchronous, Single-Threaded (Similar to a procedural language like C)

- Each time JS is run a new global EC is created and each function call creates a new EC

- EC happens in 2 steps

  1. Memory Creation: The engine goes through the code and allocates memory for variables, functions etc.
  2. Execution: The code is executed line by line and the necessary functions are called, thus, creating new ECs recursively until the inner ECs are "return"ed to the outer EC
     - Once the execution of an EC is finished and the resulting value is returned, said EC is deleted and its memory is freed. The same is also true for the global EC

- The EC management is done by the Call Stack

  - The Call Stack is a stack of ECs which follows the LIFO method (Last In, First Out)
  - The global EC is the first layer of the call stack and each consecutive function call which creates a new EC is layered on top respectively
  - The JS engines use the call stack to keep track ECs and continue the thread of execution for outer ECs once execution of the current stack frame is finished

- Other Names of the Call Stack:
  - Execution Context Stack
  - Program Stack
  - Control Stack
  - Runtime Stack
  - Machine Stack

## Hoisting in JS

- For the following points, note that "declaration" != "initialization"

  - declaration: declared but could be empty with no value or statement
  - initialization: declared and refrences a value

- As well as "not defined" != "undefined"

  - not defined: No such identifier in the current scope
  - undefined: An empty identifier which is usually unintentional

- Hoisting is when certain JS declarations are moved(hoisted) to the top of the current scope upon executions and can be refrenced.

- Hoisting is applied to the following declarations but behaviour for each of them varies:
  1. var: Can be accessed before initialization but returns "undefined"
  2. const: Throws a RefrenceError if accessed before initialization
  3. let: Same as const
  4. class: Same as const
  5. function: Can be refrenced and accessed with no issues
     - Note that, only function declarations are hoisted but function expressions and arrow functions are not.

### Temporal Deadzone

- TDZ is the period between entering a scope and the actual declaration of a variable within that scope. During this phase, the variable exists but is inaccessible. Accessing it before the Initialization results in a ReferenceError. e.g. "let".

## Window Object

- Every device that runs JS creates a global object on runtime

- The global object on web browsers is the "window" object by default; even the inner ECs have their global "this" bound to the window object by default.

## Scope & Lexical Environment

- Scope is the area of the program where an item (be it variable, constant, function, etc.) that has an identifier name is recognized and can be refrenced

- Lexical Environment is a data structure that stores the variables and functions that are defined in the current scope and all of the outer scopes. It is also known as the lexical scope or the lexical closure

## Block Scope

- Block is any code that is grouped between 2 curly braces

- Code blocks are also called compound statements

- The only way to group multiple statements is to place them inside of a block

- The only way to use multiple statements in place of a single one is to use a compound statement aka. code block

- Block Scope refers to the identifiers (declarations) that can be refrenced inside of a Block

- In other words, declarations that are accessable in the currenly running Execution Context are block scoped.

- Some declarations are block scoped and some aren't:
  1. Block Scoped: const, let, class, function, "arrow functions" etc.
  2. Non Block Scoped: var etc.

## Shadowing

- Variable shadowing occurs when a variable with the same name is declared in an inner scope, such as a function or a block, as a variable in an outer scope. In such cases, the variable in the inner scope shadows or hides the variable in the outer scope. This means that any references to the variable within the inner scope will refer to the inner variable, effectively "shadowing" the outer variable.

## Closures

- A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment).

  - Meaning, that if a function is returned from another scope, it also has refrences to that scope (lexical environment) and the declarations it contained.
  - In other words a "closure" is a returned function "enclosed" with its lexical environment.
  - In other words (again), a "closure" is a returned function that can access the variables of its outer scope; even if said scope is not on the call stack.

- Note that, Surrounding State, Lexical Environment, Lexical Scope, Lexical Closure, Parent Scope and Outer Scope, all refer to the same thing; which is the scope <b><u>IN</u></b> which, a function is declared, and not the scope the function itself creates.

- In essence, Closures take a snapshot of their Lexical Environment and refrences to that environment, point to the last value of their declarations.

  - Meaning, if a variable at the top of the scope is initialized and is reassigned AFTER the declaration of the returned function, the closure contains the refrence to the last state of said variable and not the state it was in, when the function was declared.

    ```javascript
    // For Example:

    function closure() {
      let x = 1; // INITIALIZTION <--

      function f() /* Block Scoped function */ {
        return x + 1; // Can access x cause of lexical scope
      }

      console.log(f()); // "2"

      x = 10; // REASSIGNED NEW VALUE <--

      console.log(f()); // "11"

      return f;
    }

    const closureFunction = closure(); // returns the f function with its lexical env.

    const returnedFromF = closureFunction(); // returns x + 1

    console.log(returnedFromF); // "11" and NOT "2"
    // Because the last state of x, before closure, was 10
    ```

- Not only Closures can refrence parent scope but they can also refrence grandparent scope, grand grandparent scope and so on.

  ### Common usecases of Closures

  - Module Design Patterns
  - Currying
  - With Functions Like "once", "setTimeout", etc.
  - Memoize
  - Async State Management
  - Iterators
  - etc.

## Interview Questions

- What is a closure?
- Is ()() valid JS syntax? If yes, what does it do?
- Are let declarations closed over?
- Are function parameters closed over?
- Do grandchild scopes enclose outer scope chains as well?
- What is shadowing? Explain.
- Name some advantages of closures.
- What is data hiding and encapsulation?
- What is a function Constructor in JS?
- Disadvantages of closures?
  - Hgih memory consumption
  - Doesn't undergo garbage collection if not handled properly
  - Can possibly lead to memory leaks
- What is garbage collection?
- Give an overview of JS Engines' auto garbage collection?

## Creator

### Ali Ghelichkhani

- https://github.com/I-Do-CS

DISCLAIMER: Some of the material above, which I'm too lazy to refrence, have been gathered from the web and I am not the original writer.
