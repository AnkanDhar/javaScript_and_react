
# JavaScript Core Concepts 

1. **What is execution context?**  
   Execution Context is the environment in which JavaScript code runs. Think of it as a box created by the JS engine containing all the information needed to run a piece of code.

   There are **3 types** of execution context:
   - **A> Global Execution Context (GEC)**: Created when a JS program starts. Contains the global object (\`window\` in browsers, \`global\` in Node), global variables, and global functions.
   - **B> Functional Execution Context (FEC)**: Created every time a function is invoked. Contains the \`arguments\` object, local variables, inner functions, and a reference to the outer environment.

   __There are two phases__
   - **A> Memory allocation phase** → Allocates memory for variables/functions, sets variables to \`undefined\`, stores functions as full objects.
   - **B> Code execution phase** → JS engine executes code line-by-line, assigns values to variables, and executes functions.

2. **What is call stack?**  
   Each time a function is called, a new execution context is pushed onto the stack. First the GEC is pushed, after this a new execution context is pushed for every new function called. The execution context is popped out of the stack once the function completes its job.

3. **What is hoisting?**  
   Hoisting is JavaScript’s behavior of moving variable and function declarations to the top of their scope during the creation phase of the Execution Context.

   This means:
   - **A>** Variables are allocated in memory before code executes with \`undefined\` value.
   - **B>** \`let\` and \`const\` are also hoisted but behave differently (Temporal Dead Zone).
   - **C>** Functions are stored as full objects (function declarations are fully hoisted).

4. **What is scope and scope chain?**  
   **SCOPE**: Scope determines where a variable is accessible in code. JavaScript uses **lexical scoping**, which means scope is decided by where the variable is written in the source code, not by where the function is called. Basically, the location of a function in the source code defines what variables it can access, not from where it is called.

   __Types of scope__
   - **A> Global Scope**: Declared outside any function/block; available everywhere in the code.
   - **B> Function Scope**: Variables and functions declared inside a function are accessible only inside that function, not outside.
   - **C> Block Scope**: Introduced in ES6. \`let\` and \`const\` have this type of scope.  
     A block = \`{}\`. Block scope exists for if-blocks, loops, try-catch—basically anything with \`{}\`.

   __SCOPE CHAIN__: The scope chain is the hierarchical system that JavaScript uses to look for variables. First JS checks the current scope; if not found, it checks the outer (parent) scope step-by-step and finally reaches the global scope. If still not found → **ReferenceError**.

5. **== vs ===?**

   - \`==\` → checks only the value (performs type coercion).
   - \`===\` → checks value **and** type (no coercion).

   ```js
   console.log(2 == '2');   // true (values equal after coercion)
   console.log(2 === '2');  // false (number !== string)
   ```

6. **var vs let vs const?**

   **A> Scope and hoisting behavior:**
   - **var** = function scope (or global scope if declared outside all functions), hoisted as \`undefined\`, can be re-declared and re-assigned in the same scope.
   - **let** = block scope, hoisted but in Temporal Dead Zone (TDZ), not re-declarable in the same scope, but re-assignable.
   - **const** = same as \`let\` for scope/TDZ, but **not re-assignable** (the object it points to can still be mutated).

   **Example:**
   ```js
   console.log(a); // undefined
   // console.log(b); // ReferenceError (TDZ)

   if (true) {
     var a = 10;
     let b = 20;
     var a = 30; // ok (re-declarable in the same scope)
     // let b = 40; // SyntaxError (not re-declarable in the same scope)
   }

   console.log(a); // 30 (leaks out of the block)
   // console.log(b); // ReferenceError (if accessed directly)
   console.log(typeof b); // 'undefined' as an identifier is not declared in this scope

   const user = { name: "Ankan" };
   user.name = "AD";     // mutating the object is allowed
   // user = { name: "X" }; // TypeError (re-assigning is not allowed)

   const arr = [1, 2];
   arr[0] = 99; // allowed
   // arr = [9]; // TypeError (not allowed)
   ```

   **B> Loops and closures:**
   - **var** = shared binding; **let** = creates a new binding per iteration.

   ```js
   for (var i = 0; i < 3; i++) {
     setTimeout(() => console.log(i), 0); // 3, 3, 3
   }

   for (let i = 0; i < 3; i++) {
     setTimeout(() => console.log(i), 0); // 0, 1, 2
   }
   ```

7. **What are closures?**
