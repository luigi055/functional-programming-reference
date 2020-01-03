# What is Functional Programming?

Functional programming is a programming paradigm — a way of thinking about, and structuring your code. It has only recently begun to be adopted, was the first programming paradigm invented. Indeed, its inventionpredates computer programming itself. It’s an alternative to procedural or object-oriented programming.

Functional Programming is the direct result of the work of [Alonzo Church](https://en.wikipedia.org/wiki/Alonzo_Church), who in 1936 invented [the Lambda Calculus](https://en.wikipedia.org/wiki/Lambda_calculus). Lambda Calculus is the foundation of the LISP language, invented in 1958 by Jhon McCarthy, a foundational notion of Lambda Calculus is Immutability; which is, the notion that te values of the symbols do not change. This effectively means that a functional language has no assignment statement.

FP aficionados would say they aim to optimize for code reusability, readability, and testability. The tools in their toolboxes are functions, ways of composing them to model complex behavior, and avoiding shared state and side effects.

# What is a Function

As you can imagine, the most important thing in functional programming is, a function.

In programming, a named section of a program that performs a specific task. In this sense, a function is a type of procedure or routine. Some programming languages make a distinction between a function, which returns a value, and a procedure, which performs some operation but does not return a value.

Most programming languages come with a prewritten set of functions that are kept in a library. You can also write your own functions to perform specialized tasks.

functions is famous with several names. Different programming languages name them differently, for example, functions, methods, sub-routines, procedures, etc.

[Reference from Webopedia](https://www.webopedia.com/TERM/F/function.html)

## Defining a Function

A function definition most of the in C-based programming languages consists of a function header and a function body. Here are all the parts of a function −

Return Type − A function may return a value. The return type is the data type of the value the function returns. Some functions perform the desired operations without returning a value. In this case, the return_type is the keyword void. This is applicable in typed programming languages. In dinamic programming languages like Javascript or Python there's not need to defined the return type.

Function Name − This is the actual name of the function. The function name and the parameter list together constitute the function signature. In some programming languages like Javascript exists the concept of anonymous Function or nameless function and also define this nameless function like a expression and assign it to a variable.

Parameter List − A parameter is like a placeholder. When a function is invoked, you pass a value as a parameter. This value is referred to as the actual parameter or argument. The parameter list refers to the type, order, and number of the parameters of a function. Parameters are optional; that is, a function may contain no parameters.

Function Body − The function body contains a collection of statements that defines what the function does.

## invoking a function

Once a function is defined, It's now time to use it. you will have to call the function to perform the defined task

It is tipically called writing the name of the function and adding the parenthesis at the end.

Within the parenthesis you could pass in arguments (if the function was defined with parameter) which are the values you want your function to be invoked with.

## Differences between Parameters and Arguments

A parameter is a variable in the function definition.

An Argument is the data youpass into the function parameters.

# Pillars of Functional Programming:

- Immutability
- Purity
- First-class functions
- Recursion

## Immutability

An immutable value is a value that, once created, cannot be changed.

Being a quite harsh and strange restriction at first glance, immutability appeared to be a really valuable property having the following benefits:

- Simplified reasoning of program behavior
- Simplified application of immutable objects (no temporal coupling)
- Simplified testing
- Thread-safety by-default
- Allows safe sharing of data

### Applied inmmutability in Javascript

#### Primitives

Primitive values are immutable, objects are not.
The best example of a immutable primitive is string. Any modification to a string will result in a new string. Consider the next examples:

```
"ABCD".substring(1,3) //"BC"
"AB" + "CD"           //"ABCD"
"AB".concat("CD")     //"ABCD"
```

In all these cases new strings are created.

#### Consts

const declares a variable that cannot be reassigned. It becomes a constant only when the assigned value is immutable.
In short, using const with a primitive value defines a constant. Using const with an object value doesn’t necessarily defined a constant.

```
const book = ({
  title : "JavaScript, The good parts",
  author : "Douglas Crockford"
});
book.title = "Other title";
console.log(book);
```

#### Freezing objects

To make objects immutable we need to freeze them.
Object.freeze() can be used to freeze objects. Properties can’t be added, deleted, or changed. The object becomes immutable.

```
const book = Object.freeze({
  title : "How JavaScript Works",
  author : "Douglas Crockford"
});
book.title = "Other title";
//Cannot assign to read only property 'title'
```

Object.freeze() does a shallow freeze. The nested objects can be changed. For deep freeze we need to recursively freeze each property of type object. Here is how we can create a deepFreeze() implementation.

```
function deepFreeze(object) {
   Object.keys(object).forEach(function freezeNestedObjects(name){
   const value = object[name];
    if(typeof value === "object") {
     deepFreeze(value);
    }
  });
  return Object.freeze(object);
}
```

deepFreeze() makes objects immutable.

#### Working with array as immutable

The array data structure is not immutable in JavaScript. In order to work with arrays as immutable data structures we need to use only the pure array methods and the spread operator. The pure array methods are the ones that create a new array when something changes.

- pop(), push() and splice() are not pure
- concat(), slice() are pure

Here is an example of adding a new value to an immutable array:
const books = [ { title : "book1" }, { title : "book2"}];
//add with spread
const newBooks = [ ...books, newBook ];
//add with concat()
const newBooks = books.concat([newBook]);
Remove
Here is an example of removing the value from position index:
const newBooks = [...books.slice(0, index),
...books.slice(index + 1)];

### Referential transparency

We say that a function is referentially transparent when we can replace the expression that calls our function with the value this call produces — without changing the program’s behavior.
Even though it intuitively makes sense, we need to understand that for a function to be referentially transparent, it needs to be pure (not produce side effects).

Suppose we have a function, purchase, which buys a product for us:

```
function purchase(product) { /* ... */ }
purchase('Macbook Air');
```

We’ll call this “inline form.” In JavaScript, you can factor the inline parameter out into a variable and pass a reference without changing the behavior of your code:

```
const product = 'Macbook Air';
purchase(product);  // reference form
```

This is sometimes called referential transparency or the Substitution Principle. It refers to the idea that you can replace an expression with its value without changing behavior. This goes hand-in-hand with code being free of side effects and facilitates reasoning about program correctness.

So long as you don’t change the order of evaluation, factoring out a variable this way in JavaScript should not change the result of an expression.

### Considere Performance issue

There is still an important argument that arises against immutability, which claims on inefficeient memory consumtption caused by necessity to recreate an object each time we need to have it in a different state.

While being true, to some degree, that property of immutability has a relatively higher demand on memory, it isn’t that simple under the hood. In fact, functional languages heavily utilize the sharing of the data across created data structures, which is safe and intuitive optimization considering impossibility to change existing data.

Now it is important to point out that demand of immutability should already be very close for developers familiar with the OO notion of encapsulation.

The notion of encapsulation is mainly risen as an attempt to make the flow of the program state more predictable, and the functional principle of immutability brings this quality to the extreme.

**In Javascript Primitive values are immutable, objects are not.
The best example of a immutable primitive is string. Any modification to a string will result in a new string.**

## Purity

Purity is the property of functions not to have side-effects and any dependence on external state or time.

As we have already mentioned, Lambda calculus as the basis of functional programming, has nothing to do with the state or interaction with the real world, it’s just pure mathematical notation, therefore, such property is a default expectation from functional languages.

This sounds trivial, but it’s actually a strong requirement. This mathematical definition has significant consequences:

A function is considered pure when:

- It can not depend on anything except its input (arguments)
- It has to return a single value
- It always evaluates the same result value given the same argument value(s);
- Evaluation of the result does not cause any side effect.

This kind of functions are usually called **pure functions**

### Let's consider some examples (Javascript):

```
function coin () {
  return Math.random() < 0.5 ? 'heads' : 'tails'
}
```

The coin function is not pure, because it doesn't always produce the same result given the same (empty) input - it's not deterministic.

```
let firstName = 'krzysztof'

function uppercaseName (lastName) {
  return `${firstName.toUpperCase()} ${lastName.toUpperCase()}`
}
```

The uppercaseName function is not pure, because it depends on a variable that's out of its control. We can't be sure it will always produce the same result given the same arguments.

```
let user = {
  firstName: 'Krzysztof',
  age: '26'
}

function happyBirthday () {
  user.age = user.age + 1
}
```

The happyBirthday function is not pure, because in addition to accessing out-of-control variables, it does not return anything.

```
function calculatePrice (unitPrice, noOfUnits, couponValue = 0) {
  return unitPrice * noOfUnits - couponValue;
}
```

The calculatePrice function is pure. It doesn't use any variables out of its control, is deterministic, and we can confidently say that it will always return the same result for the same combination of input arguments.

### Benefits of Pure Functions (Purity)

Why does it all matter? There are a couple of reasons why pure functions are better than
impure ones:

- They are easier to read
  In order to understand what a function does, you only need to read its body.
- They are easier to reason about
  There’s no need to look for external dependencies, the context in which the function is called, etc. None of this matters for pure functions.
- They are easier to test
  If you want to test a function that is pure, you only need to call it with some arguments and see if the result is what you wanted it to be. No complicated setup required.
- They can be more performant
  If we know that for a given input, the function will always produce the same output, we can cache (memoize) the result so we don’t have to recalculate it when the function is called again.

Using pure functions makes your code more maintainable — because it makes it easier to manage side effects. In the next parts we will learn what side effects are and why, sadly, computer programs can’t be pure functions “all the way down”.

## First-Class Function

A “first-class function” is, unlike the “pure function”, not a practical concept that’s useful on a daily basis. It does, however, come up when thinking about characteristics of programming languages.

You can say a programming language has “first-class functions”, if functions can be used just like any other values, i.e.:

- they can be passed around,
- they can be assigned to variables,
- they can be stored in more complex data structures, like arrays or objects.

Functional programming without first-class functions would be impossible (or at least very awkward).

in JavaScript functions can be passed around — between different functions. But, why would you want to do that?
Well, passing functions in and out of functions is a common practice in functional programming — and a very powerful one is Higher Order Functions.

## Recursion

The process in which a function calls itself directly or indirectly is called recursion and the corresponding function is called as recursive function. Using recursive algorithm, certain problems can be solved quite easily.

### What is base condition in recursion?

In the recursive program, the solution to the base case is provided and the solution of the bigger problem is expressed in terms of smaller problems.

```
int fact(int n)
{
    if (n < = 1) // base case
        return 1;
    else
        return n*fact(n-1);
}
```

In the above example, base case for n < = 1 is defined and larger value of number can be solved by converting to smaller one till base case is reached.

### Why Stack Overflow error occurs in recursion?

If the base case is not reached or not defined, then the stack overflow problem may arise. Let us take an example to understand this.

```
int fact(int n)
{
// wrong base case (it may cause
// stack overflow).
if (n == 100)
return 1;

    else
        return n*fact(n-1);

}
```

If fact(10) is called, it will call fact(9), fact(8), fact(7) and so on but the number will never reach 100. So, the base case is not reached. If the memory is exhausted by these functions on the stack, it will cause a stack overflow error.
