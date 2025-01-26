# Episode 16 : JS Engine Exposed, Google's V8 Architecture

- JS runs literally everywhere from smart watch to robots to browsers because of Javascript Runtime Environment (JRE).

- JRE is like a big container which has everything which are required to run Javascript code.

- JRE consists of a JS Engine (❤️ of JRE), set of APIs to connect with outside environment, event loop, Callback queue, Microtask queue etc.

- Browser can execute javascript code because it has the Javascript Runtime Environment.

- ECMAScript is a governing body of JS. It has set of rules which are followed by all JS engines like Chakra(Internet Explorer), V8 Engine (Edge) Spidermonkey(Firefox)(first javascript engine created by JS creator himself), v8(Chrome)

- Javascript Engine is not a machine. Its software written in low level languages (eg. C++) that takes in hi-level code in JS and spits out low level machine code.

- Code inside Javascript Engine passes through 3 steps : **Parsing**, **Compilation** and **Execution**

  1. **Parsing** - Code is broken down into tokens. In "let a = 7" -> let, a, =, 7 are all tokens. Also we have a syntax parser that takes code and converts it into an AST (Abstract Syntax Tree) which is a JSON with all key values like type, start, end, body etc (looks like package.json but for a line of code in JS. Kinda unimportant)(Check out astexplorer.net -> converts line of code into AST).
  2. **Compilation** - JS has something called Just-in-time(JIT) Compilation - uses both interpreter & compiler. Also compilation and execution both go hand in hand. The AST from previous step goes to interpreter which converts hi-level code to byte code and moves to execeution. While interpreting, compiler also works hand in hand to compile and form optimized code during runtime. **Does JavaScript really Compiles?** The answer is a loud **YES**. More info at: [Link 1](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch1.md#whats-in-an-interpretation), [Link 2](https://web.stanford.edu/class/cs98si/slides/overview.html), [Link 3](https://blog.greenroots.info/javascript-interpreted-or-compiled-the-debate-is-over-ckb092cv302mtl6s17t14hq1j). JS used to be only interpreter in old times, but now has both to compile and interpreter code and this make JS a JIT compiled language, its like best of both world.
  3. **Execution** - Needs 2 components ie. Memory heap(place where all memory is stored) and Call Stack(same call stack from prev episodes). There is also a garbage collector. It uses an algo called **Mark and Sweep**.
     ![JS Engine Demo](/assets/jsengine.jpg)
     GiF Demo
     ![JS Engine Demo](/assets/jsenginegif.gif)

- Companies use different JS engines and each try to make theirs the best.
  - v8 of Google has Interpreter called Ignition, a compiler called Turbo Fan and garbage collector called Orinoco
  - v8 architecture:
    ![JS Engine Demo](/assets/jsengine.png)

<hr>

# JIT (Just-In-Time) Compilation

JIT (Just-In-Time) Compilation is a hybrid approach that combines the advantages of both interpreted and compiled languages. It allows programs to be interpreted during runtime but compiles frequently executed parts of the code into machine code on the fly, improving performance significantly.

## How JIT Works

### 1. Initial Execution
- The program begins execution as an interpreted language.
- The interpreter translates the code line by line or block by block.

### 2. Hotspot Detection
- The JIT compiler monitors which parts of the code are used most frequently (known as hotspots).
- These are usually loops or functions that are executed repeatedly.

### 3. On-the-Fly Compilation
- Once hotspots are identified, the JIT compiler translates these sections into optimized machine code.
- This compiled code is cached, so subsequent executions of the same hotspots run directly in machine code instead of being interpreted again.

### 4. Execution
- The optimized machine code is executed, significantly speeding up the program.
- Other parts of the program that aren’t hotspots continue being interpreted.

## Benefits of JIT
- **Performance Boost**: By compiling hotspots into machine code, JIT avoids repetitive interpretation, improving runtime performance.
- **Dynamic Optimization**: The JIT compiler can make optimizations based on actual runtime data, such as optimizing for the CPU architecture or inlining frequently used functions.
- **Cross-Platform Compatibility**: The base code remains portable across systems, while the JIT compiler generates machine code specific to the platform at runtime.

## Drawbacks
- **Startup Overhead**: Since the code must first be interpreted and hotspots compiled, there may be an initial delay during execution.
- **Memory Usage**: Storing both interpreted code and JIT-compiled machine code can increase memory consumption.

## Examples of JIT-Enabled Platforms
- **Java (JVM)**: The Java Virtual Machine uses JIT to execute bytecode faster by converting it into native machine code at runtime.
- **JavaScript Engines**: Engines like V8 (used in Chrome) and SpiderMonkey (used in Firefox) employ JIT to speed up JavaScript execution.
- **.NET Framework**: Microsoft's Common Language Runtime (CLR) uses JIT to execute Intermediate Language (IL) code efficiently.

JIT strikes a balance between the flexibility of interpreted languages and the performance of compiled languages.

<hr>

<hr>
# AOT (Ahead-of-Time) Compilation in JavaScript

AOT (Ahead-of-Time) Compilation in the context of JavaScript is less common compared to the Just-In-Time (JIT) compilation approach used by most modern JavaScript engines (e.g., V8, SpiderMonkey). However, it's worth exploring as it offers distinct advantages and is widely used in other programming languages or frameworks that compile JavaScript-like code.

## What is Ahead-of-Time (AOT) Compilation?

AOT compilation refers to compiling source code into machine code before runtime.  
Unlike JIT, which compiles code during the execution of the program, AOT compiles it entirely before execution.  
In JavaScript, AOT is generally associated with:

- Pre-compilation of JavaScript into a lower-level form (e.g., WebAssembly or bytecode).
- Frameworks and tools that convert JavaScript or higher-level languages into optimized, executable code during the build process.

## Where AOT is Used in the JavaScript Ecosystem

### Angular:
- Angular uses AOT to compile templates and components into JavaScript during the build process.
- This makes the application faster because the browser doesn't need to compile Angular templates at runtime.

**Benefits of AOT in Angular:**
- Smaller bundle size.
- Faster application startup.
- Detects errors at build time.

### WebAssembly (WASM):
- JavaScript can be compiled to WebAssembly, which is a low-level binary format that runs at near-native speed.
- WebAssembly uses AOT compilation to precompile code for faster performance.

### Transpilers:
- Tools like Babel or TypeScript (with the `tsc` compiler) use AOT-like techniques to compile code into JavaScript before it's run in the browser.

## Differences Between AOT and JIT

| Feature               | AOT (Ahead-of-Time)                                  | JIT (Just-In-Time)                                   |
|-----------------------|------------------------------------------------------|-----------------------------------------------------|
| **Timing**            | Code is compiled before execution.                   | Code is compiled during execution.                  |
| **Startup Time**      | Faster startup (no runtime compilation).             | Slower startup (compilation at runtime).            |
| **Optimization**      | Allows deeper optimizations during build.            | Optimizations happen dynamically.                   |
| **Flexibility**       | Requires all code to be known at build time.         | More flexible, works well with dynamic code.        |
| **Error Detection**   | Errors are caught at compile time.                   | Errors may appear at runtime.                       |

## Advantages of AOT

### 1. Improved Performance:
- Eliminates runtime compilation overhead.
- Applications start faster and are more efficient.

### 2. Error Detection at Build Time:
- AOT compilation catches syntax or type errors before the application runs.

### 3. Smaller Code Size:
- Unused code can be removed during the build process (e.g., tree-shaking).

### 4. Better Security:
- Prevents injection attacks by compiling templates into static code (e.g., Angular).

## Disadvantages of AOT

### 1. Slower Build Process:
- AOT compilation increases build time since all code is precompiled.

### 2. Less Flexibility:
- Dynamic features (like dynamically evaluating strings as code using `eval`) are harder to support with AOT.

### 3. Debugging Complexity:
- Debugging precompiled code can be challenging because the code running in the environment may differ from the original source.

## How AOT and JIT Can Coexist

Many modern systems use a hybrid approach, leveraging the benefits of both:

- **AOT for initial compilation:** Used for predictable, static code to optimize performance and detect errors early.
- **JIT for dynamic code:** Dynamically compiles unpredictable or frequently changing code at runtime.

### For example, in JavaScript:
- JIT is used in browsers and JavaScript engines like V8 for runtime optimizations.
- AOT is used in frameworks (e.g., Angular) or tools (e.g., WebAssembly) for precompiling code.

## When to Use AOT?

- When you need faster startup times and optimized performance (e.g., Single Page Applications in production).
- For projects where static analysis and error checking during the build phase are critical.
- In environments with limited runtime resources (e.g., embedded systems).

## Comparison of AOT Model with Angular and React

| Feature               | React                                                  | Angular (AOT Mode)                                       |
|-----------------------|--------------------------------------------------------|---------------------------------------------------------|
| **Template Compilation** | JSX is compiled to JavaScript at build time, but actual rendering happens dynamically at runtime. | Templates are fully compiled into JavaScript during the build process. |
| **Runtime Overhead**  | Higher: UI rendering and Virtual DOM reconciliation occur at runtime. | Lower: Precompiled templates eliminate runtime parsing. |
| **Error Detection**   | Runtime: Many issues (like invalid props) can only be detected during runtime. | Compile Time: Errors in templates or bindings are caught at build time. |
| **Performance**       | May be slower for first render (requires runtime template parsing and rendering). | Faster first render (precompiled templates).            |
| **SSR Support**       | Optional, with tools like Next.js or Gatsby.           | SSR is supported but secondary to AOT.                 |

<hr>

# 1) Mark and Sweep Algorithm in Garbage Collection

The Mark-and-Sweep algorithm is a common garbage collection technique used to manage memory in languages like JavaScript, Java, and others. It’s responsible for cleaning up memory that is no longer in use by the program. Here's how it works:

## Steps in the Mark-and-Sweep Algorithm:

### Mark Phase:
- The garbage collector starts at root objects (objects that are directly accessible from the program’s stack or global state) and marks all reachable objects.
- It traverses the object graph and follows all references (pointers) to other objects. Any object that is directly or indirectly referenced by a root object is marked as "live."

### Sweep Phase:
- After the mark phase, the garbage collector will traverse all objects in memory.
- It will sweep through all objects and reclaim memory used by objects that were not marked as "live."
- These unmarked objects are considered garbage because they are no longer reachable from the root, meaning they are no longer needed.

## Advantages of Mark and Sweep:
- Simple to implement and widely used in many programming languages.
- Can handle complex object graphs because it works recursively.

## Disadvantages:
- The sweep phase can be time-consuming if many objects are left unmarked.
- It can cause pauses during execution (called "stop-the-world" pauses) while the garbage collection happens.

# 2) Copy Elision

Copy elision is an optimization technique where the compiler removes unnecessary copies of objects, reducing the overhead of object creation and destruction.

## How It Works:
- Copy constructors or assignment operators can be expensive when objects are copied. However, sometimes these copies can be eliminated during compilation because the compiler can determine they aren't necessary.

### Example (C++):
If an object is returned from a function, the compiler can eliminate the copy by directly placing the returned object into the destination variable.

```cpp
MyObject foo() {
    MyObject obj;
    return obj; // Could involve unnecessary copy.
}

MyObject obj2 = foo(); // Without copy elision, obj might be copied here.
```
In the above case, copy elision allows the compiler to optimize the function call and avoid creating a temporary object. Instead, it can directly move the object into the destination variable. 


## Why It's Important:
Copy elision is a critical optimization for performance, especially in resource-intensive programs, because it avoids unnecessary memory allocations and copying.

## Modern Compilers:
C++ compilers (e.g., GCC, Clang) and modern compilers for other languages automatically apply copy elision when they can guarantee no side effects.

## 3) Inlining

Inlining is an optimization technique where the compiler replaces a function call with the actual body of the function, removing the overhead of making a function call.

### How It Works:
When the compiler inlines a function, instead of performing a traditional function call (which involves pushing arguments onto the stack, jumping to the function's code, and returning), it simply replaces the call with the function's body.

### Example (C++):
```cpp
inline int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(2, 3); // Instead of calling add(), the body is inserted directly here.
    return 0;
}
```
In this example, the `add()` function might be replaced directly in the call site, reducing function call overhead.

### Why It's Important:
- **Improves performance** by removing function call overhead, especially for small functions that are frequently called.
- **Helps reduce the size of the binary** (by avoiding function call stack management).

### Limitations:
- **Inlining is only beneficial for small functions.** Large functions might increase the code size unnecessarily, leading to code bloat.
- The **compiler often makes the final decision** on whether or not to inline a function based on its size, frequency of use, and other factors.

### 4) Inline Caching
Inline caching is a technique used in Just-In-Time (JIT) compilers, commonly in dynamic languages like JavaScript, to speed up property lookups by caching the result of the lookup.

#### How It Works:
When accessing an object’s property (e.g., `obj.property`), the JavaScript engine needs to find the location of that property in the object. This is normally done via a lookup process that can be slow, especially if the object’s structure is dynamic.

Inline caching speeds this process up by storing the result of the property lookup in a cache on the call site itself. The next time the same property is accessed on the same object, the engine skips the lookup and uses the cached result directly.

#### Example:
```javascript
let obj = { x: 42 };
console.log(obj.x);  // First lookup
console.log(obj.x);  // Second lookup (cached)
```
In this example:
- On the first access, the JavaScript engine might go through the process of finding the property `x` in the object.
- However, on the second access, the engine can use inline caching to directly access the property without going through the full lookup process again.

#### Why It's Important:
- Speeds up property lookups significantly in dynamic languages.
- It's particularly useful in scenarios where objects have dynamic shapes (e.g., adding properties at runtime).

#### Example in JavaScript Engines:
JavaScript engines like V8 (used in Chrome and Node.js) use inline caching to optimize property lookups and method calls.

### Summary:
- **Mark and Sweep Algorithm**: A garbage collection technique that marks live objects and sweeps away unreachable objects to reclaim memory.
- **Copy Elision**: Compiler optimization that eliminates unnecessary object copies to improve performance.
- **Inlining**: An optimization that replaces function calls with the actual function body to avoid call overhead.
- **Inline Caching**: A JIT optimization technique used to speed up property lookups by caching the result of the lookup.

Each of these concepts plays a significant role in improving the efficiency and performance of applications, whether at runtime (with garbage collection and inlining) or during compilation (with optimizations like copy elision and inline caching).

<hr>


Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=2WJL19wDH68&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/2WJL19wDH68/0.jpg" width="750"
alt="JS Engine Exposed, Google's V8 Architecture in JS Youtube Link"/></a>
