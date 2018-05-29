# JavaScript Concepts

## What is a function?

A function is instructions that a computer knows how to perform, given some input and the computer will perform the instructions and return some output.

## Two States of a function

A function can be defined and invoked.

> Defined can also be described as declared, assigned.

> Invoked can also be described as run, executed, processed.

## Understanding the JavaScript Runtime

> Understanding Dyslexia and how it relates to computers and how they think.

> Reading is complex. It requires our brains to connect letters to sounds, put those sounds in the right order, and pull the words together into sentences and paragraphs we can read and comprehend.
> People with dyslexia have trouble matching the letters they see on the page with the sounds those letters and combinations of letters make. And when they have trouble with that step, all the other steps are harder.

> Computers are dyslexic in the sense they have to try harder to process instructions, they have to break them down to the specific letters and combine them based on common rules and repeat this process for every single instruction everytime it is run.

---

Before we can go deep in to the JavaScript language we need to learn some common terminology and how the JavaScript Execution Engine works.

When you invoke a javascript application, the engine processes your application using a couple of concepts and how to trace through a javascript program.

* process - single threaded - execution context
* memory - global and local memory
* callstack - what is a stack? you can push contexts on and pop contexts off.

When launching a javascript process, it runs the program in interpretive environment, this environment is a single thread, which means one execution context. The execution context has a global context and then local context for each function that is executed.

Each execution context has its own memory and workspace.

The callstack keeps track of the context the application is in at a given point in time.

### Why is this important?

We need to learn how to talk about code, and the process that code is run, we need to learn how the computer runs your code. This will give us a common language to share between each other.

Lets step through the following function:

```js
const input = 8
function add2(num) {
  return num + 2
}
const result = add2(input)
```

### Draw our code process

Step by step we will draw our coding context using paper. We are going to do this several times to get our vocabulary down.

## Pure Functions (Callback functions)

A pure function is a function that has no side effects, which means it only can modify values passed in and can only return one value back. Pure functions will always return the same output given the same input.

### Pair Programming

> Fastest way to grow your skills

* Research
* One Computer - Driver | Navigator
* Takes Practice

### Research

Before you begin a pairing session, you first must research the problem as a team, create a plan.

### Driver

A Driver is the person on the keyboard, they receive the input from the navigator and enter the commands into the editor.

### Navigator

A Navigator is the person with the idea explaining what to do, they do not touch the keyboard, they must verbally explain how to accomplish the task at hand.

> It is very important that the idea person verbally explain how to do the task and the driver perform the task, if you are driving and you have an idea, you need to give up the keyboard and explain it to someone else. This will create an effective transfer of knowledge.

> For every exercise, we are going to pair, and do the each exercise twice. Once as the Driver and once as thee Navigator.

### Example 2

```js
const input = 10
function divideBy2(num) {
  return num / 2
}
const result = divideBy2(input)
```

### Example 3

```js
const input = 5
function multiplyBy2(num) {
  return num * 2
}
```

---

# Copy Array Functions

### Example 1

```js
function copyArrayAndAdd2(array) {
  let result = []
  for (let i = 0; i < array.length; i++) {
    result.push(array[i] + 2)
  }
  return result
}
const input = [1, 2, 3]
const array2 = copyArrayAndAdd2(input)
```

--

### Example 2

```js
function copyArrayAndMultiplyBy2(array) {
  let result = []
  for (let i = 0; i < array.length; i++) {
    result.push(array[i] * 2)
  }
  return result
}
const input = [1, 2, 3]
const array2 = copyArrayAndMultiplyBy2(input)
```

--

### Example 3

```js
function copyArrayAndDivideBy2(array) {
  let result = []
  for (let i = 0; i < array.length; i++) {
    result.push(array[i] / 2)
  }
  return result
}
const input = [1, 2, 3]
const array2 = copyArrayAndDivideBy2(input)
```

---

### DRY Principal

(Don't Repeat Yourself)

https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

How do we make the above functions DRY?

```js
function copyArrayAndModify(modifier, array) {
  let result = []
  for (let i = 0; i < array.length; i++) {
    result.push(modifier(array[i]))
  }
  return result
}

function add2(num) {
  return num + 2
}

const input = [1, 2, 3]
const array2 = copyArrayAndModify(add2, input)
```

### Exercises

Create your copy and modify function and three callback modifier functions:

* add2
* multiplyBy4
* divideBy2

Then use the `copyArrayAndModify` function to use this input array [1,2,3].

Use the input array and add2 to each item of the array and assign it to the variable array2.

Take array2 and multiplyBy4 for each item of the array and assign it to the variable array3.

Take array3 and divideBy2 for each item of the array and assign it to thee variable array4.

## Assertions

* Array1 should equal [1,2,3]
* Array2 should equal [2,4,5]
* Array3 should equal [8,16,20]
* Array4 should equal [4, 8, 10]

---

## Closures

### What is a closure?

Closures allow us to return values (function or objects) that can still see the memory of the execution context of a previous closures function invocation.

```js
function foo() {
  let x = 0

  function set(v) {
    x = v
  }

  function get() {
    return x
  }

  return {
    set: set,
    get: get
  }
}

let bar = foo()
console.log(bar.get())
bar.set(42)
console.log(bar.get())
```

> How can we take this concept and apply it to our copy array functions?

What if we created a copyArrayAndModify function that accepted the modifier function and returned a function that took the array?

```js
function copyArrayAndModify(modifier) {
  function copyArray(array) {
    let result = []
    for (let i = 0; i < array.length; i++) {
      result.push(modifier(array[i]))
    }
    return result
  }

  return copyArray
}

const add2 = copyArrayAndModify(function(v) {
  return v + 2
})
const multiply4 = copyArrayAndModify(function(v) {
  return v * 4
})
const divideBy2 = copyArrayAndModify(function(v) {
  return v / 2
})

const array1 = [1, 2, 3]
const array2 = add2(array1)
const array3 = multiply4(array2)
const array4 = divideBy2(array3)
```

### Introducing Map

> Map is a core building block of functional programming, it leverages containers, like `array` objects and it apply's modifier functions to the values inside the container.

```js
// copy and modify array function
function map(modifier, array) {
  let result = []
  for (let i = 0; i < array.length; i++) {
    result.push(array[i])
  }
  return result
}

// callback functions
const add2 = function(v) {
  return x + 2
}
const multiply4 = function(v) {
  return v * 4
}
const divideBy2 = function(v) {
  return v / 2
}

const array1 = [1, 2, 3]
const array2 = map(add2, array1)
const array3 = map(multiply4, array2)
const array4 = map(divideBy2, array3)
```

Lets go further using closures we can make creator functions that return us a function.

```js
const add = function(num1) {
  return function(num2) {
    return num1 + num2
  }
}

const add2 = add(2)
const array1 = [1, 2, 3]
const array2 = map(add2, array1)
```

> try to create an add, subtract, multiply and divideBy `creator functions` and repeat the same exercise. (Switch Pairs)

## Review Creator Exercises

Lets review our creator functions

## Partial Application and Curry

Sometimes you may want to use a function in different ways or on different execution contexts.

For Example, you may want an add function, and you may have both a and b.

```
function add(a,b) {
  return a + b
}
```

But sometimes, you may know in advance you want an add function, but you do not know all the values up front.

```
function add(a) {
  return function(b) {
    return a + b
  }
}
```

> Wouldn't be nice if we had a function that could return either the result if both a and b were provided in, and a function if only a was provided.

This is called partial application.

```
function add(a,b=null) {
  function do(v) {
    return a + v
  }
  return b ? do(b) : do
}
```

This may be a little hard to read at first, with the ternary function, lets write out long hand.

```js
function add(a,b) {
  // default b to null
  if (!b) {
    b = null
  }

  function do(v) {
    return a + v
  }

  if (b !== null) {
    return do(v)
  } else {
    return do
  }
}
```

## Exercise - Create Add, Subtract, Multiply and Divide Creator Functions that use Partial Application and apply them to the following application.

```js
const array1 = [1, 2, 3]
const array2 = map(add(2), array1)
const array3 = map(multiply(4), array2)
const array4 = map(divideBy(2), array2)
```

## Compose

> Can we apply all of the mapper functions while only looping once?

Certainly, we can do this imperatively:

```
let result = []
for(let i = 0; i < array.length; i++) {
  let v1 = array[i] + 2
  let v2 = v1 * 4
  let v3 = v2 / 2
  result.push(v3)
}
return result
```

> But how can we do this in a re-usable way, where we can take advantage of our creator functions.

Since we are using pure functions that return a result, we can pass it to each function and create a combination function.

```js
const add2AndMultiply4AndDivideBy2 = v => divideBy2(2, multiply(4, add(2, v)))
const array4 = map(add2AndMultiply4AndDivideBy2, [1, 2, 3])
```

> This is such a common pattern that we have discrete mapper functions that we want to combine together.

```js
v => e(f(g(v)) === compose(e,f,g)
```

> Here is what it would look like with our map function

```
const result = map(
  compose(
    divideBy(2),
    multiply(4),
    add(2)
  ), [1,2,3]
)
```

So what does compose look like?

```
function compose(...fns) {
  return function(v) {
    let result = v
    for(let i = fns.length -1; i < 0; i--) {
      result = fns[i](result)
    }
    return result
  }
}
```

## Exercise

Use the compose function and your Add, Subtract, Multiply, and DivideBy to find the result of the following problem:

> Take the array [1,2,3] and add 2 to each item, then multiply 4 then divide 2.

## Filters

> What if we wanted to take an array and return an array that only included some of the values of the array? This is called filtering, it is a common term in data processing that could be confusing at first. But basically, we have a group of data elements, lets say numbers, and we only want to return a subset, lets say numbers divisble by 5.

The filter function is very similar to the map function, we take an array, and a predicate function that can only return a true or false.

> A predicate function is a function that returns a true or false

```
function filter(predicate, array) {
  let result = []
  for(let i = 0; i < array.length; i++) {
    if (predicate(array[i])) {
      result.push(array[i])
    }
  }
  return result
}
```

## Lets trace the execution of the filter function

## Reduce function

### Review

* Map function Transforms values in a container
* Filter function limits values in a container based on a predicate function
* Reduce returns a value based on a reducer function

### Common Reducers

* count reduce function
* sum reduce function

```js
const inc = add(1)
const count = reduce(inc, [1, 2, 3])
```

```js
const sum = reduce(add, 0, [1, 2, 3])
```

### Lets step through a reduce implementation

```js
function reduce(reducer, initialValue, array) {
  let result = initialValue
  for (let i = 0; i < array.length; i++) {
    result = reducer(result, array[i])
  }
  return result
}
```

### Map implemented as a reducer

```js
function map(mapper, array) {
  return reducer(
    function(acc, value) {
      acc.push(mapper(value))
      return acc
    },
    [],
    array
  )
}
```

### Filter implemented as a reducer

```js
function filter(predicate, array) {
  return reducer(
    function(acc, value) {
      if (predicate(value)) {
        acc.push(value)
      }
      return acc
    },
    [],
    array
  )
}
```
