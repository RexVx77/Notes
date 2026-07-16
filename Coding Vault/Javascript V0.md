CH1: Variables
# Learn JavaScript

Welcome to "Learn JavaScript"! In this course, you'll learn how to program in JavaScript, the most widely-used programming language in the world.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/JXkHUaI-853x256.png)

## Learning Goals

- Learn the basic syntax of the JavaScript programming language
- Understand best practices for writing clean, efficient, and maintainable JavaScript code
- Learn about the differences between JavaScript and other popular programming languages
- Master advanced JavaScript concepts like closures, asynchronous programming, and the event loop

## Prerequisites

This course assumes you're already familiar with programming basics in at least one other language. _If you're brand new to coding I recommend starting with our [Python for beginners course](https://www.boot.dev/courses/learn-code-python) first._

For printing anything to the console, use:
`console.log(anything)` where anything can be any variable

# Basic Variables

The [`var`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var) keyword declares a variable ~~the sad way~~. For example:

```javascript
var mySkillIssues = 42;
```

Some of JavaScript's most common variable [types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures) are:

- `number`: represents both integer and floating-point numbers
- `boolean`: either `true` or `false`
- `string`: a sequence of characters
- `undefined`: a variable that hasn't been assigned a value

```javascript
var smsSendingLimit = 100;
var isAdmin = true;
var username = "wagslane";
var nothing = undefined;
```

Don't worry, we'll talk about a better way to declare variables in the next lesson.

# let and const

The `var` keyword is the "old" way to declare variables in JavaScript. These days, you should use [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) and [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) instead. The `let` keyword is for variables that can be reassigned, while `const` is for variables that can't.

```javascript
let username = "dengar_the_bh";
username = "boba_fett";

const smsSendingLimit = 1000;
```

Use `const` for values that never change, and `let` for values that do.

The `var` keyword is function-scoped instead of block-scoped, meaning when it's used inside an `if` block the variable leaks out, while `let` and `const` don't.

You'll encounter legacy code that uses `var`, so you need to know about it, but migrate it to `let` or `const` when you can.

# Why JavaScript?

I'll be honest, JavaScript is _not_ my favorite language... but I do write _a lot_ of it.

Click to hide video

Here's the deal, JavaScript is the _only_ language that runs in the browser. (okay, there's [WebAssembly](https://webassembly.org/), but that's different). If your app is accessed via a web browser, it's almost certainly gonna need some JavaScript.

JavaScript has some things I don't like:

- Not statically typed (although [TypeScript](https://www.typescriptlang.org/) helps)
- Legacy baggage (like `var` and old code that doesn't use [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise))
- Weird quirks (like `==` vs `===`)
- Always acts slightly differently in every browser/runtime

But it has some serious things going for it:

- Almost every technology company uses it somehow, so there are a lot of jobs
- C-style syntax, so it's easy to learn if you know any other C-style language
- Many built-in features that are particularly useful on the web
- Can be used for both front-end and back-end development, simplifying an org's tech stack
- Big companies have invested a lot in making it better and faster over the years
- Great support for asynchronous programming

# Comments

JavaScript has two styles of comments:

```javascript
// This is a single-line comment

/*
   This is a multi-line comment
   neither of these comments will execute
   as code
*/
```

# Numbers in JS

In Python, numbers without a decimal part are called `Integers` and fractions are `Floats`. Contrast this to JavaScript where all numbers are just a [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) type.

You're already familiar with the `number` type. Numbers aren't surrounded by quotes when created, but they can contain decimal parts and negative signs.

```javascript
let x = 2; // this is a number
x = 5.69; // this is also a number
x = -5.42; // yup, still a number
```

You can do arithmetic as you'd expect:

```javascript
let sum = 2 + 3 + 7; // 12
let difference = 5.3 - 2.1; // 3.2
let product = 2 * 3; // 6
let quotient = 6 / 2; // 3
```

# Numbers Review

Remember, in JavaScript all numbers are just "number" types. There is no distinction between "Float" types and "Integer" types.

## Addition

```js
2 + 1;
// 3
```
## Subtraction

```js
2 - 1;
// 1
```
## Multiplication

```js
2 * 2;
// 4
```
## Division

```js
3 / 2;
// 1.5 (still just a "number")
```

# Increment and Decrement

In Python we use the `+=` operator to increment a number. That operator works in JavaScript as well, but JS also has a [`++` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Increment) for when you only want to increment by `1`.

```js
let bootdevCourseRating = 4;
bootdevCourseRating++;
console.log(bootdevCourseRating); // 5
bootdevCourseRating += 5;
console.log(bootdevCourseRating); // 10
```

And of course, there is a [`--` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Decrement) for decrementing by `1`.

```js
let bootdevCourseRating = 11;
bootdevCourseRating--;
console.log(bootdevCourseRating); // 10
bootdevCourseRating -= 5;
console.log(bootdevCourseRating); // 5
```

# Undefined vs. Undeclared

The primitive [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) value represents the absence of a value... but it's not the same thing as an "undeclared" variable!

We can create an un_defined_ variable by giving it a name, but no value:

```js
let favoriteSandersonCharacter; // undefined
console.log(typeof favoriteSandersonCharacter); // "undefined"
```

However if we _never create the name_, it's "un_declared_", and undeclared variables actually throw an error (so don't do this):

```js
console.log(favoriteRothfussCharacter); // ReferenceError: favoriteRothfussCharacter is not defined
```

The worst part is that the undeclared variable error actually says "not defined"... welcome to JavaScript. 🤦‍♂️

As an aside, you can use `const` to declare a variable that is assigned to `undefined`, but you wouldn't be able to set its value later, so why would you want to?

```js
const favoriteSandersonCharacter; // SyntaxError: missing = in const declaration
const favoriteSandersonCharacter = undefined; // undefined
```

# Null vs. Undefined

If you're coming from Python, you might be thinking:

> Ah, so `undefined` is like `None`! Easy peasy.

Yes. But also... **no**. One of JavaScript's most cursed features is that it has two values for "nothing":

- [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined): It doesn't exist _at all_. In [grug-speak](https://grugbrain.dev/) `undefined` is "very nothing"
- [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null): It (kind of) exists, but it's _empty_. In grug-speak `null` is "kinda nothing"

There are [some practical differences between the two](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null#difference_between_null_and_undefined), the primary one being that `undefined` is the _default_ value of a variable when it hasn't been given a value yet.

```js
let myName;
console.log(myName); // undefined
```

To get a `null` value, you have to explicitly assign it:

```js
let myName = null;
console.log(myName); // null
```

Confusingly, `typeof` returns `"object"` for `null`:

```js
console.log(typeof null); // object
```

_To be clear, `null` **is** its own type according to the [ECMAScript specification](https://tc39.es/ecma262/#sec-ecmascript-language-types), but the "object" type report is a historical quirk that [can't be easily fixed now](https://web.archive.org/web/20071020084354/http://wiki.ecmascript.org/doku.php?id=proposals%3Atypeof)_.

In _most cases_, `null` and `undefined` work the same way, but you'll want to be consistent in _how_ you use them, and know that there are subtle behavioral differences.

I personally use `undefined` almost everywhere I would use `None` in Python or `nil` in Go. JavaScript is fairly unique in having two options. I only use `null` in cases where the behavioral difference matters, or I'm relying on external code that forces me to use `null`.

# Dynamic and Weak

Like Python, Ruby, and PHP, JavaScript is a dynamically-typed (not statically-typed) language. Its variable types are only known at _runtime_ (yes, yes, we'll talk about TypeScript in another course). **Some of us _like_ being able to see the types of our variables in our editors, okay**?

Now, _unlike Python_, it's also [weakly-typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing), meaning it will automatically convert types when you do things like add a number to a string. This can lead to some unexpected behavior if you're not careful...

```javascript
let answerToLife = 42;
let answerToTheUniverse = "42";

// obviously JavaScript thinks that adding strings
// and numbers is totally sane and normal behavior
const answerToEverything = answerToLife + answerToTheUniverse;

console.log(answerToEverything);
// "4242"
```

# Same Line Declarations

You can declare multiple variables on the same line:

```javascript
let miles = 80276, org = "Tesla";
```

The above is the same as:

```javascript
let miles = 80276;
let org = "Tesla";
```

# JavaScript's Speed

I want to be clear, comparing the "speed" or "efficiency" of programming languages is as far from an exact science as "astrology"... okay, maybe not quite _that_ bad. But there _are_ a lot of variables and moving parts, so benchmarks can be misleading.

I just want to point out a few high-level things to give you some kind of frame of reference:

1. **JavaScript is not as fast as C, Rust or Zig**. It's almost always going to be outperformed by non-garbage-collected languages.
2. **Modern JavaScript is typically JIT-compiled (just-in-time compiled) into machine code** via the [V8 engine](https://v8.dev/) at runtime. It's usually not as fast as AOT (ahead-of-time) compiled languages like Go or Java, but it's usually much faster than interpreted languages like Python or Ruby.
3. **JavaScript runs on a single thread, but has great support for asynchronous programming**. It's typically not as performant for CPU-bound tasks (like heavy math calculations), but it does well for I/O-bound tasks (like contacting a database or making an API call).

# Strings

In JavaScript, a (non-template) string can be written with either single or double quotes. For example:

- `'Hello'`
- `"Hello"`.

**Personally, we prefer double quotes**! It's important to have styling conventions so that all the code in a project looks consistent, making it easier to read and contribute to.

## Indexing

Square brackets are used to access individual characters inside a string. The characters are numbered from `0` to `length-1`. It's similar to how strings and lists work in Python, Go and many other languages.

```js
const greeting = "Hello";
console.log(greeting[0]); // 'H'
console.log(greeting[1]); // 'e'
console.log(greeting[2]); // 'l'
console.log(greeting[3]); // 'l'
console.log(greeting[4]); // 'o'
// you can also get the last char at length-1
console.log(greeting[greeting.length - 1]); // 'o'
```

The [`.length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length) property is used to get the number of characters in a string.

# Template Literals

In JavaScript, [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) are a fantastic way to interpolate dynamic values into a string. They're JavaScript's version of Python's f-strings. For example:

```javascript
const shadeOfRed = 101;
console.log(`The shade is ${shadeOfRed}`);
// The shade is 101
```

Template literals _must start and end with a backtick_, and anything inside of the dollar-sign bracket enclosure is automatically _cast_ to a string.

## Advanced Logic

You aren't limited to just variable names inside the `${}`. You can actually write valid JavaScript code directly inside them.

This means you can do math, change text styles, or run logic checks right inside the string.

```javascript
const price = 2.5;
const quantity = 3;
const item = "coffee";

console.log(`Total: $${price * quantity}`);
// Total: $7.5

console.log(`I need ${item.toUpperCase()}`);
// I need COFFEE
```

# Java vs. JavaScript

A _very_ common misconception is that Java and JavaScript are the same, or even just similar. Here's my favorite analogy for demonstrating how untrue this is:

> Java is to Java_Script_ as car is to car_pet_

Java and JavaScript are _not_ the same, and they often aren't even used for the same kinds of things.

## Java

[Java](https://www.java.com/en/) is a statically-typed, object-oriented language that compiles to byte code and runs on the Java Virtual Machine. It's used for all sorts of things, but most commonly for server-side applications, Android apps, and large enterprise systems. If this picture were a programming language, it would probably be Java:

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/njm6qva-1200x675.jpeg)

## JavaScript

[JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) is a dynamically and weakly-typed language that runs natively in the browser and out of the browser with dedicated runtimes like Node.js, Deno or Bun. It originally _only_ ran in the browser and was named "JavaScript" because back when it was created in the 90's, Java was _very_ popular and the creators wanted to get a piece of that marketing value.

## Fun Fact

[Brendan Eich](https://en.wikipedia.org/wiki/Brendan_Eich), the creator of JavaScript, was given only 10 days to create the language. He was told to make it look like Java, but to make it work in the browser. The rest is history.

# Semi-Colons in JS

In languages like C, C++, and Java, a semicolon is used as a statement terminator. For example, in C:

```c
int x = 5;
```

This allows the compiler to know when a statement ends without relying on whitespace or newlines. For example, this is also valid C:

```c
int x = 5; int y = 10;
```

In JavaScript, semicolons are _optional_ as a terminating character. They can be inserted by the JavaScript engine automatically during the parsing phase. However, **most developers (us included) prefer to use semi-colons** to avoid any confusion or errors that can arise from automatic insertion.

```javascript
// This works
let x = 5
let y = 10
```

```javascript
// But we prefer this
let x = 5;
let y = 10;
```

# String Encoding

In JavaScript, strings consist of [UTF-16 code units](https://en.wikipedia.org/wiki/UTF-16) - which basically means that most characters are represented by a 16-bit number (2 bytes). This allows for more than just the [128 ASCII characters](https://www.ascii-code.com/), but also characters from non-English languages and other symbols.

Some characters (like emojis 😀) require more than 16 bits to represent, so JavaScript uses a pair of 16-bit numbers (2 [code units](https://developer.mozilla.org/en-US/docs/Glossary/Code_unit)) to represent them.

## So... Who Cares?

Long story short, you're generally safe to use unicode characters (like emojis) in your strings. Just be aware that some characters will take up more than one "character" in the string. This is totally chill:

```js
const kermit = "🐸";
```

For this line of code, 
* `kermit.length` will give 2
* `[...kermit].length` will give 1

# Camel Case in JS

By convention in Python, Ruby, and Rust, most programmers use `snake_case` to write variable names. In JavaScript, the more-popular convention (and the one we use) is `camelCase`.

Here are some casing examples:

- `camelCase`
- `snake_case`
- `PascalCase`
- `SCREAMING_SNAKE_CASE`

Again, **prefer `camelCase`** when writing JavaScript code.

---

CH2: Comparisons

# Conditionals

`if` statements in JavaScript use parentheses around the condition:

```javascript
if (height > 4) {
  console.log("You are tall enough!");
}
```

`else if` and `else` are supported as you might expect:

```javascript
if (height > 6) {
  console.log("You are super tall!");
} else if (height > 4) {
  console.log("You are tall enough!");
} else {
  console.log("You are not tall enough!");
}
```

We'll cover some more of these, but for now, here are some common comparison operators:

- `===` equal to
- `!==` not equal to
- `<` less than
- `>` greater than
- `<=` less than or equal to
- `>=` greater than or equal to

# Comparison Operators

You should already be familiar with these inequality operators, and they work as you would expect in JavaScript:

```javascript
5 < 6; // evaluates to true
5 > 6; // evaluates to false
5 >= 6; // evaluates to false
5 <= 6; // evaluates to true
```

The [equality operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness), however, are a bit... _strange_. To compare two values to see if they are _exactly_ the same, use the [strict equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) (`===`) and [strict inequality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_inequality) (`!==`) operators:

```js
5 === 6; // evaluates to false
5 !== 6; // evaluates to true
```

The "normal" [equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality) (`==`) and [inequality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Inequality) (`!=`) operators are a bit more... _flexible_:

```js
5 == 6; // evaluates to false
5 == "5"; // evaluates to true

5 != 6; // evaluates to true
5 != "5"; // evaluates to false
```

The "strict equals" (`===`) and "strict not equals" (`!==`) compare both the value _and_ the type. The "loose equals" (`==`) and "loose not equals" (`!=`) attempt to convert and compare values of different types. With the loose versions, the string `'5'` and the number `5` are considered "equal", which, in _good_ code, is usually _not_ what you want.

**This is a fairly unique quirk of the JavaScript language**.

You can [read more about how `==` works](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality#description) if you're interested, but I'd recommend sticking with `===` and `!==` in nearly all cases.

# Logical Operators

In Python, the logical operators are simply their English names:

- `and`
- `or`
- `not`

In JavaScript, the equivalent logical operators use symbols:

- `&&` (and) - Returns `true` if _both_ conditions are `true`
- `||` (or) - Returns `true` if _either_ of the conditions are `true`
- `!` (not) - Returns `true` only if the input is `false`

```js
true && true; // true
true && false; // false
true || false; // true
false || false; // false
!false; // true
!true; // false
```

_This syntax matches many other languages like Go, Rust, and C_.

# Switch

[Switch statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) are a way to compare a variable against multiple possible values. They are similar to if-else statements, but tend to be more readable when there are many potential options.

```javascript
const os = "mac";
let creator;
switch (os) {
  case "linux":
    creator = "Linus Torvalds";
    break;
  case "windows":
    creator = "Bill Gates";
    break;
  case "mac":
    creator = "Steve";
    break;
  default:
    creator = "Unknown";
    break;
}

console.log(creator);
// Steve
```

Unlike some languages where [fall-through](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch#breaking_and_fall-through) doesn't happen by default, JavaScript **will continue** to execute the next case until it reaches a `break` or `return` statement.

_99 times out of 100, you'll want to include a `break/return` statement after each case to prevent this behavior_.

# Ternary Operator

Sometimes using 3-5 lines of code to write an if/else block is overkill. The [ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator) makes it easy to write a conditional as a single [expression](https://en.wikipedia.org/wiki/Expression_\(computer_science\)).

```js
const price = isMember ? "$2.00" : "$10.00";
```

I like to read it in English as:

> If `isMember` is true, evaluate to `$2.00`, otherwise evaluate to `$10.00`.

The same logic using if/else would be:

```js
let price;
if (isMember) {
  price = "$2.00";
} else {
  price = "$10.00";
}
```

## Why Is It Called a “Ternary”?

Ternary's latin root means "3", and it's the only JavaScript operator that takes _three_ operands.

- A condition followed by a question mark (?)
- An expression to execute if the condition is truthy followed by a colon (`:`)
- The expression to execute if the condition is falsy.

# When to Ternary

You will probably use if/else statements more often than ternaries. Ternary operations are great for _small_, single-line conditionals. I've seen developers in the professional world get... _clever_... with ternaries and it can lead to nested monstrosities like this:

```js
const vehicleName = isTruck
  ? "truck"
  : isCar
    ? "car"
    : isScooter
      ? "scooter"
      : "vehicle";
```

I mean it _works_, and it's all on one line, but it's _much_ harder to understand IMO. I'd prefer to see this:

```js
let vehicleName = "vehicle";
if (isTruck) {
  vehicleName = "truck";
} else if (isCar) {
  vehicleName = "car";
} else if (isScooter) {
  vehicleName = "scooter";
}
```

or maybe a function (we'll cover functions soon):

```js
function getVehicleName(isTruck, isCar, isScooter) {
  if (isTruck) {
    return "truck";
  }
  if (isCar) {
    return "car";
  }
  if (isScooter) {
    return "scooter";
  }
  return "vehicle";
}
```

Remember: **Code is written for humans not machines**.

# Truthy and Falsy

A ["truthy"](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) value is a value that is considered `true` when encountered in a Boolean context. In JavaScript, you don't _need_ to explicitly convert a value to a Boolean before using it in a conditional:

```javascript
if ("hello") {
  console.log("hello is truthy");
}
if (42) {
  console.log("42 is truthy");
}
// hello is truthy
// 42 is truthy
```

A ["falsy"](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) value works the same way, but for values that evaluate to `false`:

```javascript
if (!0) {
  console.log("0 is falsy");
}
if (!null) {
  console.log("null is falsy");
}
if (!undefined) {
  console.log("undefined is falsy");
}
```

Common truthy values include:

- `true`
- `42` (any number that isn't `0`)
- `"hello"` (any non-empty string)
- `[]` (an empty array)
- `{}` (an empty object)
- `function() {}` (an empty function)

Common falsy values include:

- `false`
- `0`
- `""` (an empty string)
- `null`
- `undefined`
- `NaN` (Not a Number)

# Nullish Coalescing

JavaScript is truly a language built for the web, and the web runs on... _uncertainty_.

It's kinda crazy how much production JavaScript exists solely to handle cases where a value might be `null` or `undefined`.

The [`nullish coalescing`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) operator `??` is a way to handle these cases in a more concise way.

```js
let myName = null;
console.log(myName ?? "Anonymous"); // "Anonymous"

myName = "Lane";
console.log(myName ?? "Anonymous"); // "Lane"
```

If the value on the left of `??` is `null` or `undefined`, the value on the right is returned. Otherwise, the value on the left is returned. **It's a way to set sane defaults for variables that might be empty**.

### JavaScript Fallback Operators

- **Logical OR (`||`)**: Returns the right-hand side if the left is **falsy** (`false`, `0`, `""`, `null`, `undefined`, `NaN`).
    - _Best for:_ General fallbacks where you want to treat "empty" or "zero" values as missing.
- **Nullish Coalescing (`??`)**: Returns the right-hand side **only** if the left is `null` or `undefined`.
    - _Best for:_ When `0`, `false`, or `""` are valid pieces of data that you don't want to overwrite.

**Example:**

```js
const score = 0;

const a = score || 10; // a is 10 (0 is falsy)
const b = score ?? 10; // b is 0 (0 is not nullish)
```

---

CH3: Functions

# Functions

As you might have guessed, JavaScript supports functions via the [`function` keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function).

Even JavaScript isn't crazy enough to not support functions.

```js
// function declaration
function getSum(a, b) {
  return a + b;
}

// function call
const result = getSum(60, 9);

console.log(result);
// 69
```

# Function Hoisting

In Python, a function must be defined _before_ it's used. But that's _not so_ in JavaScript! As long as a function is defined _somewhere_ in the file, it can be called even _before_ the definition.

```js
console.log(getLabel(3));
// prints 'awful'

function getLabel(numStars) {
  if (numStars > 7) {
    return "great";
  } else if (numStars > 3) {
    return "okay";
  } else {
    return "awful";
  }
}
```

This works because JavaScript ["hoists"](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) the function declaration to the top of the file _before_ the code is executed.

# Unit Test Lessons

Up until now, all the coding lessons in this course have tested your code's _console output_ (what's printed). For example, a lesson might expect your code (in conjunction with the code we provide) to `console.log` something like:

```bash
Price: 0.2
NumMessages: 18
```

If your code prints that _exact_ output, you pass. If it doesn't, you fail. **Haw-haw**.

## A New Type of Lesson

Going forward, you'll also encounter a new type of lesson: [unit tests](https://en.wikipedia.org/wiki/Unit_testing). If this isn't your first course with us, you'll know what I'm talking about, but in case you haven't, a unit test is just an automated program that tests a small "unit" of code. Usually just a function or two. Your editor will now have multiple tabs: the `main.js` file containing your code, and the `main_test.js` file containing our unit tests.

These new unit-test-style lessons will test your code's _functionality_ rather than its output. Our tests will call functions in your code with different arguments, and expect specific `return` values. If your code returns the correct values, you pass. If it doesn't, you fail.

There are two reasons for this change:

1. It's more realistic. In the real world, you'll be writing unit tests and running them against your code to make sure it works as expected.
2. You can run and debug your code with `console.log` statements, and leave those print statements in when you submit. Unlike the output-based lessons, you won't have to remove your `console.log` statements to pass.

To avoid pesky [floating-point errors](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problems), we often store prices in the currency's **base unit**. In this case, we are storing the prices in pennies, and **a dollar consists of 100 pennies.**

# Multiple Return Values

Many languages allow multiple values to be returned from a function. For example, in Python this works:

```python
def get_user():
    return "name@domain.com", 21, "active"

email, age, status = get_user()
```

However, in JavaScript, _that's not allowed_!

```js
function getUser() {
  return "name@domain.com", 21, "active";
  // DON'T DO THIS
  // it only returns 'active'
}
```

Strangely, the JavaScript code above won't actually _throw_ any sort of error, it will just silently return the `"active"` string.... which is unintuitive behavior that you probably didn't want. **You can only return one value from a function in JavaScript**!

To get around this, most developers return an object that _contains_ the values they want to return. **We'll cover objects later**.

# Functions As Values

JavaScript supports [first-class](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function) and higher-order functions, which are just fancy ways of saying "functions as values". Functions can be treated like any other data type – such as `number`s and `string`s and `boolean`s. Let's assume we have two simple functions:

```javascript
function add(x, y) {
  return x + y;
}

function mul(x, y) {
  return x * y;
}
```

We can create a new `aggregate` function that accepts a function as its 4th argument:

```javascript
function aggregate(a, b, c, arithmetic) {
  const firstResult = arithmetic(a, b);
  const secondResult = arithmetic(firstResult, c);
  return secondResult;
}
```

It calls the given `arithmetic` function (which could be `add` or `mul`, or any other function that accepts two parameters and returns a number) and applies it to three inputs instead of two. It can be used like this:

```javascript
function main() {
  const sum = aggregate(2, 3, 4, add);
  // sum is 9
  const product = aggregate(2, 3, 4, mul);
  // product is 24
}
```

# Scope

[Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope) in JavaScript defines where variables and functions are accessible in your code, and it can behave differently depending on the environment (such as a browser or [Node.js](https://en.wikipedia.org/wiki/Node.js)). There are four levels, from highest to lowest:

1. **Global Scope**:
    - Variables declared globally have the highest level of scope and can be accessed from anywhere in your code.
    - In browsers, global variables are properties of the `window` object. For example, `window.myGlobalVar = 'hello world'` defines a global variable.
    - In Node.js, global variables are properties of the `global` object: `global.myGlobalVar = 'hello world'`.
2. **Module Scope**:
    - In ES modules (both in Node.js and modern browsers), variables declared at the top level of a module are scoped to that module. They are not added to the global scope.
    - In the browser, using `<script type="module">` creates a module scope for that script.

We'll cover modules in a later chapter.

3. **Function Scope**:
    - Variables declared with `var` (we try to avoid this) are limited to the function scope. They are accessible only within that function and any nested functions.
4. **Block Scope**:
    - [ES6](https://en.wikipedia.org/wiki/ECMAScript) introduced block scope with the `let` and `const` keywords. A block is typically defined by curly braces `{}`, like in `if` statements, loops, and other blocks of code.
    - Variables declared with `let` and `const` are confined to their block, making them more predictable and reducing the chances of accidental variable hoisting.

# Anonymous Functions

Anonymous functions are true to form in that they have _no name_. They're useful when defining a function that will only be used once or to create a quick [closure](https://en.wikipedia.org/wiki/Closure_\(computer_programming\)).

Let's say we have a function `conversions` that accepts another function, `converter` as input:

```javascript
function conversions(converter, x, y, z) {
  const convertedX = converter(x);
  const convertedY = converter(y);
  const convertedZ = converter(z);
  console.log(convertedX, convertedY, convertedZ);
}
```

We _could_ define a function normally and then pass it in by name... but if we only want to use it in this one place, we can define it inline as an anonymous function:

```javascript
// using a named function
function double(a) {
  return a + a;
}
conversions(double, 1, 2, 3);
// 2 4 6
```

```javascript
// using an anonymous function
conversions(
  function (a) {
    return a + a;
  },
  1,
  2,
  3,
);
// 2 4 6
```

# Default Parameters

In JavaScript, you can specify default values for function parameters. This is particularly useful for _optional_ parameters where you want to ensure a specific default behavior if the caller does not provide certain arguments. Default parameter values can be set during the function declaration.

```javascript
function getGreeting(email, name = "there") {
  console.log(`Hello ${name}, welcome! You've registered your email: ${email}`);
}

getGreeting("lane@example.com", "Lane");
// Hello Lane, welcome! You've registered your email: lane@example.com

getGreeting("lane@example.com");
// Hello there, welcome! You've registered your email: lane@example.com
```

If the second parameter is omitted, the default value `"there"` will be used in its place. Optional parameters (those with default values) should be defined after all mandatory parameters to avoid ambiguity.

# Passing by Value

Variables in JavaScript are typically passed by value (except for objects and arrays, which we'll talk about later and are passed by reference). "Pass by value" means that when a variable is passed into a function, that function receives a _copy_ of the variable. The function is unable to mutate the caller's original data.

```javascript
let x = 5;
increment(x);
console.log(x);
// 5

function increment(x) {
  x++;
  console.log(x);
  // 6
}
```

# Immediate Invocation

You can immediately invoke a function after defining it using the not-at-all-hard-to-pronounce acronym ["IIFE" (Immediately Invoked Function Expression)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE).

```javascript
(function () {
  console.log("JavaScript: at least it's not Java");
})();
// JavaScript: at least it's not Java
```

They can also return values and take arguments:

```javascript
const result = (function (a, b) {
  return a + b;
})(1, 2);

console.log(result);
// 3
```

The function is defined and then immediately called. It looks nasty, but it's occasionally useful for a couple of reasons:

- **Scope**: It has its own scope
- **Expression**: Can be convenient for computing a value as a single expression (like above)
- **Async**: Can be used to quickly run code in an `async` function (we'll cover this later)

Note that brackets to wrap the function (i.e. `(...)`) are not compulsory.

---

CH4: Objects

# Objects

JavaScript [objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) are an almost weirdly versatile collection type. Object literals (POJOs, or "plain old JavaScript objects") are often used to store data in key-value pairs.

```javascript
const apple = {
  name: "Apple",
  radius: 2,
  color: "red",
};
```

You can access properties stored on an object using the `.` operator:

```javascript
console.log(apple.name); // prints "Apple"
console.log(apple.radius); // prints "2"
console.log(apple.color); // prints "red"
```

JavaScript objects are interesting because you'll use them the way you'd probably use a map or a dictionary in other languages (simple key-value pairs), but they can also be used for more complex things like classes and prototypes... which we'll get into later.

# No Colon

The `key: value` syntax is the normal way to create key-value pairs in an object, but if you want a key to have the same name as an existing variable, you can omit the colon and the value. These are the same:

```js
const radius = 2;
const color = "red";
const apple = {
  radius: radius,
  color: color,
};
```

```js
const radius = 2;
const color = "red";
const apple = {
  radius, // same as radius: radius
  color: color, // set explicitly for demonstration
};
```

Personally, I prefer the second example when it's applicable, just because it's less verbose.

# Updating Properties

You can update and **add new** keys (you'll see me use the words "property" and "key" interchangeably in this course) to an existing object using the `.` operator. If it exists, it's updated; if it doesn't, it's created as a new property:

```javascript
const apple = {
  name: "Apple",
  radius: 2,
  color: "red",
};

apple.numSeeds = 3; // new property
apple.color = "green"; // update property
// {"name":"Apple","radius":2,"color":"green","numSeeds":3}
```

# Nesting Properties

Objects can contain other objects. Here we've nested two object literals within the `tournament` object:

```javascript
const tournament = {
  referee: {
    name: "Sally",
    age: 25,
  },
  prize: {
    units: "dollars",
    value: 100,
  },
};
```

We can access nested properties the same way by chaining: `tournament.referee.name`

```javascript
console.log(tournament.referee.name); // Sally
console.log(tournament.prize.value); // 100
```

# Optional Chaining

Nested data can _quickly_ become hard to work with. In most production systems you'll deal with 3-4 levels of object nesting on a regular basis.

When using the normal `.` operator, if the object on the left side of the `.` is `null` or `undefined`, you'll get a `TypeError` at runtime! Thankfully, JavaScript has recently added a new operator to make dealing with this headache easier, the [optional chaining operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining): `?.`

```javascript
const tournament = {
  prize: {
    units: "dollars",
    value: 100,
  },
};

const h = tournament.referee.height;
// TypeError: Cannot read properties of undefined (reading 'height')
```

So, if you're _not sure_ whether the `referee` property exists (maybe it was sent to us over the network) we can use the optional chaining operator to avoid the error:

```javascript
const tournament = {
  prize: {
    units: "dollars",
    value: 100,
  },
};

const h = tournament.referee?.height;
// h is simply undefined, no error is thrown
```

# When to Chain

You should only use `?.` chains when you _expect_ an object may not exist. For example, if according to our business logic, a `user` _must_ have an `address` object, but the `address` object may not have a `street` property, we wouldn't use the optional chaining operator because we expect `user.address` to never be `undefined`.

```js
const street = user.address.street;
```

But if not all users have an address, we might use the optional chaining operator:

```js
const street = user.address?.street;
```

Or, if we aren't even sure if the `user` object exists:

```js
const street = user?.address?.street;
```

We don't want to overuse it because if we _expect_ that all users have objects, and we come across one that doesn't we probably _want_ an error thrown so we can see it and go fix the problem. _Good errors make debugging easier_.

# Object Methods

JavaScript objects can have `methods`, just like classes in Python or structs in Go. Objects are interesting in JavaScript because they play the role of dictionaries _and_ classes in other languages (Yes, JS also has classes, but we'll get to them later).

Methods are functions that are defined on an object. They can access and change the properties of the object in question. In the context of an object method, the [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) keyword refers to the object itself, like `self` in Python (we'll talk about the admittedly hairy `this` keyword in more detail later).

```javascript
const person = {
  firstName: "Lane",
  lastName: "Wagner",
  getFullName() {
    return this.firstName + " " + this.lastName;
  },
};

console.log(person.getFullName());
// Lane Wagner
```

# Methods Mutate

Methods can change the properties of their objects as well:

```javascript
const tree = {
  height: 256,
  color: "green",
  cut() {
    this.height /= 2;
  },
};

tree.cut();
console.log(tree.height);
// prints 128

tree.cut();
console.log(tree.height);
// prints 64
```

You might be wondering:

> Wait... I thought `tree` was a constant?!?

You're right, but in JavaScript, the `const` keyword doesn't stop you from _changing the properties_ of an object... it only stops you from reassigning the variable (`tree` in this case) to a _new_ object. **Do not trust `const` objects to have constant contents!**

# Initializing Props

If a property (key) doesn't exist when we try to access it with the `.` operator, we'll just get `undefined`. One way to check for this is by using the `!` (not) operator because `undefined` is "falsy" (meaning it evaluates to `false` in a boolean context). The syntax is simple:

```javascript
const balances = {
  lane: 100,
  breanna: 150,
  john: 200,
};

// if bob doesn't have a balance yet
// create a new prop for him
// set to 0
if (!balances.bob) {
  balances.bob = 0;
}
```

# Strings As Keys

Accessing a property like `desk.height` is great when the name of the prop is _static_, meaning you know what it is _before_ runtime. But what if the key is dynamic? Like, what if the user enters a string and you need to use that as the lookup key?

_Bracket notation solves this_.

```javascript
const desk = {
  wood: "maple",
  width: 100,
};

console.log(desk.wood);
// prints "maple"

console.log(desk["wood"]);
// also prints "maple"

const key = "wood";
console.log(desk[key]);
// also prints "maple"
```

For example, maybe the key is passed in as a parameter to a function:

```javascript
function getLastName(users, firstName) {
  return users[firstName];
}
```

# This

The [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) keyword is perhaps one of the most rage-inducing parts of JavaScript. Once you understand it, it's not _too_ bad, but it's far from intuitive (in my opinion).

Put simply, `this` refers to the context where a piece of code is executed... let's cover some of those cases:

## Global Context

`this` refers to the [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window) object in browsers or `module.exports` in Node.js (not `global`, as you might expect).

```javascript
// in a browser
console.log(this);
// Window { ... }
```

```javascript
// in Node.js
console.log(this);
// {}
```
## Strict Mode

In [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) (which we'll cover later, don't worry about it too much for now) `this` is `undefined` in the global scope in both the browser and Node.js.

```javascript
"use strict";
console.log(typeof this);
// undefined
```
## Method Context

Inside a standard [method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions) or a [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new), `this` refers to the object the method is called on.

```javascript
const myObject = {
  message: "Hello, World!",
  myMethod() {
    console.log(this);
    console.log(this.message);
  },
};
myObject.myMethod();
// { message: 'Hello, World!', myMethod: [Function: myMethod] }
// Hello, World!
```

## Arrow Functions

We'll cover arrow functions specifically in the next lesson - they're a bit of a special case.

# Arrow Functions

[Fat arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), or "arrow functions" are another way to define functions in JavaScript. Arrow functions are _newer_ than the `function` keyword, however, unlike the `let/const` syntax, arrow functions are _sometimes_ better, not _always_ better.

```js
// declaring a function without a variable
function add(x, y) {
  return x + y;
}
```

```js
// declaring a function with a variable
const add = function (x, y) {
  return x + y;
};
```

```js
// using the fat arrow syntax
const add = (x, y) => {
  return x + y;
};
```

It's so cool and not confusing at all that there are 3 ways to do (basically) the same thing...

## What's the Difference?

- Fat arrow functions are usually declared as variables, while the `function` keyword may or may not be declared as a variable.
- Fat arrow functions handle object scoping in a more intuitive way (we'll talk about this later)
- Fat arrow functions don't work as constructors (we'll talk about this later)
- Other minor [differences](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#description)

# Fat Arrows and This

One reason to choose an arrow function over a regular `function` or method is to preserve the `this` context. It's particularly useful when working with objects. To be fair, in a simple object like this, the non-arrow method makes perfect sense:

```js
const author = {
  firstName: "Lane",
  lastName: "Wagner",
  getName() {
    return `${this.firstName} ${this.lastName}`;
  },
};
console.log(author.getName());
// Prints: Lane Wagner
```

With a fat-arrow function, the `this` keyword **refers to the same context as its parent**. In essence, **fat arrow functions "preserve" the `this` context**. That's why this `this.firstName` and `this.lastName` are undefined in this example:

```js
const author = {
  firstName: "Lane",
  lastName: "Wagner",
  getName: () => {
    return `${this.firstName} ${this.lastName}`;
  },
};
console.log(author.getName());
// Prints: undefined undefined
// because `this` still refers to the global object
// and `firstName` and `lastName` are not defined globally
```

Developers working in some front-end (yuck) JavaScript frameworks (like React or Vue) _tend to use fat arrow functions often_. The `this` context can contain a _ton_ of component-wide state, and it needs to be preserved throughout nested function calls, so fat arrows make the code easier to read and write.

# Spread Syntax

JavaScript has a really nifty [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_object_literals) for moving groups of object properties around. It's a great way to copy objects and merge object properties.

```javascript
const engineering_dept = {
  lane: "grand magus",
  hunter: "software engineer",
  allan: "software engineer",
  matt: "software engineer",
  dan: "software engineer",
  waseem: "software engineer",
};

const video_dept = {
  stass: "video producer",
  alex: "video producer",
};

const all_employees = { ...engineering_dept, ...video_dept };
/*
{
  lane: 'grand magus',
  hunter: 'software engineer',
  allan: 'software engineer',
  matt: 'software engineer',
  dan: 'software engineer',
  waseem: 'software engineer',
  stass: 'video producer',
  alex: 'video producer'
}
*/
```

The spread syntax [shallow-copies](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy) the properties of the objects you're spreading. If properties have the same name, the last (right-most) object's property will overwrite the previous ones.

```javascript
const engineering_dept = {
  lane: "software engineer",
  hunter: "software engineer",
};

const video_dept = {
  lane: "cringe youtuber",
  alex: "video producer",
};

const all_employees = { ...engineering_dept, ...video_dept };
/*
{
  lane: 'cringe youtuber',
  hunter: 'software engineer',
  alex: 'video producer'
}
*/
```

# Return Objects

As we talked about earlier, in JavaScript, you can only return a single value from a function. So, when you want to return multiple values, you just return an object that contains those values.

```javascript
function doAllTheMath(x, y) {
  const sum = x + y;
  const difference = x - y;
  const product = x * y;
  const quotient = x / y;
  return {
    sum,
    difference,
    product,
    quotient,
  };
}

const results = doAllTheMath(10, 5);
console.log(results.sum);
// 15
console.log(results.difference);
// 5
console.log(results.product);
// 50
console.log(results.quotient);
// 2
```

# Destructuring

It's admittedly annoying to have to get the return values from an object by using the `.` operator. The [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#object_destructuring) lets us unpack object properties easily.

So, instead of this:

```js
const apple = {
  radius: 2,
  color: "red",
};

const radius = apple.radius;
const color = apple.color;
```

We can do this:

```js
const apple = {
  radius: 2,
  color: "red",
};

const { radius, color } = apple;
```

I use it all the time to unpack function return values:

```js
function getApple() {
  const apple = {
    radius: 2,
    color: "red",
  };
  return apple;
}

const { radius, color } = getApple();
console.log(radius); // 2
console.log(color); // red
```

Destructuring also works in function parameters, which means that if you write a function that takes an object as an argument, you can unpack the object's properties in function definition.

So, instead of this:

```js
function eatApple(apple) {
  console.log(`ate a ${apple.color} apple with a radius of ${apple.radius}`);
}
```

We can do this:

```js
function eatApple({ radius, color }) {
  console.log(`ate a ${color} apple with a radius of ${radius}`);
}
```

# Not Bound

Methods in JavaScript are _not_ bound to their object by default (as they are in languages like Python and Go). So if you use a method as a "callback" function, you may run into issues with the `this` keyword:

```javascript
const user = {
  name: "Lane",
  sayHi() {
    console.log(`Hi, my name is ${this.name}`);
  },
};

user.sayHi();
// Hi, my name is Lane

const sayHi = user.sayHi;
sayHi();
// TypeError: Cannot read properties of undefined (reading 'name')
```

This happens a lot when passing a method as a [callback function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) to another function:

```javascript
const user = {
  firstName: "Lane",
  lastName: "Wagner",
  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  },
};

function getGreeting(introduction, nameCallback) {
  return `${introduction}, ${nameCallback()}`;
}

console.log(user.getFullName());
// Lane Wagner
console.log(getGreeting("Hello", user.getFullName));
// TypeError: Cannot read properties of undefined (reading 'firstName')
```

If you want to use a method as a callback function, you'll need to [`bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) it to the object using the `bind` method:

```javascript
const boundGetFullName = user.getFullName.bind(user);
console.log(getGreeting("Hello", boundGetFullName));
```

---

CH5: Classes
# Classes

[Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) in JavaScript are a _template_ for creating objects. As we learned, unlike many other languages, it's easy to create JavaScript objects _without_ classes, but that doesn't mean classes aren't useful.

```javascript
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const user = new User("Lane", 100);
```

- The [`class`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class) declaration creates a new class
- The [`constructor`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) method is a special method that's called when a new instance of the class (object) is created
- The [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) keyword calls the constructor method and creates a new instance of the class

In many other languages like Java or C#, you must explicitly declare class fields (variables) before you can assign values to them in the constructor. However, JavaScript is much more flexible.

In JavaScript, when you use `this.variableName = value` inside the constructor, the engine automatically creates that property on the instance if it doesn't already exist.

While it isn't strictly required, modern JavaScript does allow you to explicitly declare "class fields" at the top of the class if you prefer that style for clarity:

```javascript
class Message {
  recipient;
  sender;
  body;

  constructor(recipient, sender, body) {
    this.recipient = recipient;
    this.sender = sender;
    this.body = body;
  }
}
```

This explicit declaration is mostly useful for:

1. **Readability**: It makes it immediately clear what properties the class will have.
2. **Private Fields**: If you want a variable to be private (not accessible outside the class), you _must_ declare it with a `#` prefix, like `#recipient;`.

# Private Properties

By default, all properties of a class are public, meaning they can be accessed and modified from outside the class. Here's an example:

```javascript
class Movie {
  constructor(title, rating) {
    this.title = title;
    this.rating = rating;
  }
}

const matrixMovie = new Movie("The Matrix", 9.5);
console.log(matrixMovie.title);
// The Matrix
matrixMovie.title = "The Matrix Reloaded";
console.log(matrixMovie.title);
// The Matrix Reloaded
```

Maybe we don't want our `title` to be able to be changed _anywhere in our code_. We can make it [private](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties) by prefixing it with a hash `#` and declaring it at the top of the class:

```javascript
class Movie {
  #title;
  constructor(title, rating) {
    this.#title = title;
    this.rating = rating;
  }
}

const matrixMovie = new Movie("The Matrix", 9.5);
console.log(matrixMovie.#title);
// Uncaught SyntaxError: Private field '#title' must be declared in an enclosing class
```

Private properties can still be used from within the class:

```javascript
class Movie {
  #title;
  constructor(title, rating) {
    this.#title = title;
    this.rating = rating;
  }

  getTitleAllCaps() {
    const allCaps = this.#title.toUpperCase();
    return allCaps;
  }
}

const matrixMovie = new Movie("The Matrix", 9.5);
console.log(matrixMovie.getTitleAllCaps());
// THE MATRIX
```

Encapsulation in JavaScript is typically enforced at two levels:

- **The class level**: Public/private methods using `#` for private fields
- **The module level**: Exporting only what you want to be public (we'll talk about modules later)

# Static Methods

A [`static`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static) method or property is bound to the class itself, not the instance of the class (an object). In this example, we create two instances of the `User` class:

```javascript
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const lane = new User("Lane", 30);
const allan = new User("Allan", 30);
console.log(lane.name); // Lane
console.log(allan.name); // Allan
```

In JavaScript, a class is just an object template, so when we create a static method or property the object instances can't access it. So, `static` members are often used for utility functions for the class itself.

```javascript
class User {
  static numUsers = 0;

  constructor(name, age) {
    this.name = name;
    this.age = age;
    User.numUsers++;
  }

  static getNumUsers() {
    return User.numUsers;
  }
}

const lane = new User("Lane", 30);
console.log(User.getNumUsers()); // 1
const allan = new User("Allan", 30);
console.log(User.getNumUsers()); // 2

// This doesn't work because its not a method on the object
console.log(lane.getNumUsers());
// TypeError: lane.getNumUsers is not a function
//    at main.js:20:18
```

# Getters and Setters

In JavaScript classes, getters and setters let us define special methods for getting and setting the values of properties. They look like regular methods but are accessed like properties. Here's an example using the `get` keyword:

```javascript
class User {
  constructor(name, age) {
    this._name = name;
    this.age = age;
  }

  get name() {
    return this._name.toUpperCase();
  }
}

const lane = new User("Lane", 30);
console.log(lane.name); // LANE
```

Notice that we've renamed `this.name` to `this._name` in our constructor to avoid a name collision with the getter itself.

A setter lets us control what happens when a property is _changed_. For example, we could validate a user's `age` to make sure it's not negative:

```javascript
class User {
  constructor(name, age) {
    this.name = name;
    this._age = age;
  }

  get age() {
    return this._age;
  }

  set age(value) {
    if (value < 0) {
      throw new Error("Age can't be negative.");
    }
    this._age = value;
  }
}

const lane = new User("Lane", 29);
lane.age = -5; // "Age can't be negative."
console.log(lane.age); // 29
```

Personally... I hate this. When I get or set a value in property field, I expect that I'm dealing with a raw value, not calling a custom function! There are certain patterns that make heavy use of this feature (e.g. reactive state in certain front-end frameworks), but my advice is to **only use it when you truly need it**.

# Inheritance

A class can inherit methods and properties from a parent class using the [`extends`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends) keyword:

```javascript
class Titan {
  constructor(name) {
    this.name = name;
  }
}

class BeastTitan extends Titan {
  speak(msg) {
    console.log(`${this.name} says, "${msg}"`);
  }
}

const beast = new BeastTitan("Zeke");
beast.speak("You know, it's almost like throwing a baseball");
// Zeke says, "You know, it's almost like throwing a baseball"
```

And if we want to override a method from the parent class, we can do that too:

```javascript
class Titan {
  constructor(name) {
    this.name = name;
  }

  speak() {
    // this gets overridden in the BeastTitan class
    console.log("*titan noises*");
  }
}

class BeastTitan extends Titan {
  speak() {
    console.log(`${this.name} says, "I'm the Beast Titan"`);
  }
}

const pureTitan = new Titan("Eren's mom");
pureTitan.speak();
// *titan noises*

const beast = new BeastTitan("Zeke");
beast.speak();
// Zeke says, "I'm the Beast Titan"
```

# Super

The [`super`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super) keyword allows us to call methods on an object's parent. It's often used to call a parent's constructor method when the child object has its own.

```javascript
class Titan {
  constructor(name) {
    this.name = name;
  }

  toString() {
    return `Titan - Name: ${this.name}`;
  }
}

class BeastTitan extends Titan {
  constructor(name, power) {
    // call the parent's constructor
    super(name);
    this.power = power;
  }

  toString() {
    // call the parent's `toString` method
    return `${super.toString()}, Power: ${this.power}`;
  }
}

const beast = new BeastTitan("Zeke", 9000);
console.log(beast.toString());
// Titan - Name: Zeke, Power: 9000
```

---

CH6: Prorotypes

# Prototypal Inheritance

So... I tricked you a bit. Classes are actually a fairly _new_ addition to JavaScript. See, they're not the underlying mechanism for inheritance - that's actually [prototypes](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes). Classes are just syntactic sugar for prototypes.

Every object in JavaScript has a prototype. When an object "inherits" from another object, it's really that its parent is marked as its "prototype". It's called [prototypal inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain). The built-in [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) method creates a new object with its prototype set to the given object.

```javascript
const pureTitan = {
  // (define a parent object / prototype)
  name: "Eren's mom",
  speak(msg) {
    console.log("*titan noises*");
  },
};
pureTitan.speak();
// *titan noises*

const beastTitan = Object.create(pureTitan); // (define a child)

console.log(beastTitan.name); // (accessing .name from pureTitan)
// Eren's mom

beastTitan.name = "Zeke";
beastTitan.speak = function () {
  console.log(`${this.name} says, "I'm the Beast Titan"`);
};

beastTitan.speak();
// Zeke says, "I'm the Beast Titan"
```

# Prototype Chains

Every object has a prototype, and that prototype can in turn have a prototype, creating a chain that goes all the way back to the root [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) object, whose prototype is always `null`.

**An object stores a reference to its prototype**. The [`Object.getPrototypeOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf) method returns the prototype of an object. When we create a new POJO (plain old JavaScript object), its prototype is automatically set to `Object.prototype`:

```js
const pureTitan = {
  name: "Eren's mom",
};

const beastTitan = Object.create(pureTitan);
beastTitan.name = "Zeke";

console.log(beastTitan); // { name: "Zeke" }
console.log(Object.getPrototypeOf(beastTitan)); // { name: "Eren's mom" }
console.log(Object.getPrototypeOf(Object.getPrototypeOf(beastTitan))); // {} (Object.prototype)
console.log(
  Object.getPrototypeOf(
    Object.getPrototypeOf(Object.getPrototypeOf(beastTitan)),
  ),
); // null (end of the chain)
```

## How Are Parent Members Accessed?

You might think that using [`Object.create()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) _copies_ the properties from the parent object to the child object:

```js
const pureTitan = {
  name: "Eren's mom",
};

const beastTitan = Object.create(pureTitan);
console.log(beastTitan.name); // Eren's mom
```

**But it does not**. JavaScript looks within the `beastTitan` object for the `name` property and doesn't find it because we never set one. So it checks its prototype (using `Object.getPrototypeOf(beastTitan)`), which is `pureTitan`, and finds the `name` property there. It uses that value instead.

---

CH7: Loops
# Loops

A traditional "for loop" in JavaScript looks like this:

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// 0
// 1
// 2
// 3
// 4
```

This syntax is common in C-style languages. In English, the code says:

1. Start with `i` equals `0`. (`let i = 0`)
2. Start a loop iteration if `i` is less than `5`. Otherwise, exit the loop.
3. Log `i` to the console. (`console.log(i)`)
4. Add `1` to `i`. (`i++`)
5. Go back to step 2.

# Break

The [`break`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break) keyword can be used to break out of a loop early.

```js
for (let i = 0; i < 10; i++) {
  if (i === 3) {
    break;
  }
  console.log(i);
}
// Prints:
// 0
// 1
// 2
```

You can omit the loop condition in a `for` loop to create an intentional infinite loop and then use `break` to exit, for example:

```js
for (let i = 0; ; i++) {
  if (i === 3) {
    break;
  }
  console.log(i);
}
```

No matter the end condition, when a `break` statement is encountered, the loop will exit immediately.

# Continue

The [`continue`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue) keyword stops the current iteration of a loop and immediately moves on to the next one.

```js
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    continue;
  }
  console.log(i);
}
// Prints:
// 1
// 3
// 5
// 7
// 9
```

# While

Like many other languages, JavaScript has a [`while`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while) loop. It keeps running as long as the condition is true.

```js
const jane = {
  name: "Jane",
  mom: {
    name: "Alice",
    mom: {
      name: "Lilly",
      mom: {
        name: "Granny",
      },
    },
  },
};

let currentPerson = jane;
while (currentPerson) {
  console.log(currentPerson.name);
  currentPerson = currentPerson.mom;
}
console.log("No more ancestors!");
// Jane
// Alice
// Lilly
// Granny
// No more ancestors!
```

# For...in

Sometimes its useful to loop over all the _keys_ of an object. This is _most_ useful when you're using an object as you would use a dictionary or hash map in other languages.

```javascript
let titan = {
  name: "Eren",
  power: "Attack Titan",
  age: 19,
};

for (const key in titan) {
  console.log(`${key}: ${titan[key]}`);
}
// name: Eren
// power: Attack Titan
// age: 19
```

In modern specifications, the traversal order is well-defined and consistent across implementations:

> Within each component of the prototype chain, all non-negative integer keys (those that can be array indices) will be traversed first in ascending order by value, then other string keys in ascending chronological order of property creation.

That's still not obvious to me when I read code however, so if I _really_ care about order I usually break out the keys into an array and sort them how I want. _We'll cover arrays later_.

---

CH8: Arrays
# Arrays

JavaScript's [arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) are similar to Python's lists.

One important thing about JavaScript arrays are that items in an array are _not_ required to be of the same type... Remember, JavaScript is about as loosy-goosy as programming languages get...

```js
const numbers = [1, 2, 3, 4, 5];
const strings = ["banana", "apple", "pear"];
const miscellaneous = [true, 7, "adamantium"];
```

You can index into an array using square brackets `[]`:

```js
const strings = ["banana", "apple", "pear"];
console.log(strings[0]);
// Prints: 'banana'
```

And you can `.push()` new items onto the end of an array:

```js
const drinks = [];
drinks.push("lemonade");
console.log(drinks);
// Prints: ['lemonade']
drinks.push("root beer");
console.log(drinks);
// Prints: ['lemonade', 'root beer']
```

# Array Length

The [`.length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length) _property_ returns the current length of an array.

```js
const foods = ["burger", "fries", "pizza"];
console.log(foods.length);
// Prints: 3
```

# Array Spread

Remember the [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) for merging object properties? **It works with arrays too**! It expands the elements of an array into individual elements and inserts them into another array.

```js
const nums = [1, 2, 3];
const newNums = [...nums, 4, 5, 6];
console.log(newNums);
// Prints: [1, 2, 3, 4, 5, 6]
```

# Includes

Checking whether a value exists in an array is really easy in JavaScript, just use the [`.includes()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) method.

```js
fruits = ["apple", "orange", "banana"];
console.log(fruits.includes("orange"));
// Prints: true
console.log(fruits.includes("pear"));
// Prints: false
```

Strings actually have an `.includes()` method too!

```js
const str = "Hello, world!";
console.log(str.includes("world"));
// Prints: true
console.log(str.includes("banana"));
// Prints: false
```

# For...of Loops

JavaScript has a relatively new [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) syntax to loop over a sequence without the need to keep track of the index manually. So, instead of typing out all of this:

```javascript
let woods = ["oak", "pine", "maple"];
for (let i = 0; i < woods.length; i++) {
  console.log(woods[i]);
}
// prints:
// oak
// pine
// maple
```

We can write this:

```javascript
let woods = ["oak", "pine", "maple"];
for (let wood of woods) { //wood is a copy of an element in woods, changing it can not mutate the original array
  console.log(wood);
}
// prints:
// oak
// pine
// maple
```

It's a lot like Python's `for...in` syntax, but be careful not to confuse it with JavaScript's [`for...in` syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in), which is used to loop over the _keys_ of an object. _This one has bitten me on many a tired coding session._

# Slicing Arrays

JavaScript's [`.slice` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) makes it easy to, well, slice and dice arrays:

```js
const animals = ["ant", "bison", "camel", "duck", "elephant"];
console.log(animals.slice(2));
// ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4));
// ["camel", "duck"]
console.log(animals.slice(1, 5));
// ["bison", "camel", "duck", "elephant"]
console.log(animals.slice(-2));
// ["duck", "elephant"]
console.log(animals.slice(2, -1));
// ["camel", "duck"]
console.log(animals.slice());
// ["ant", "bison", "camel", "duck", "elephant"]
```

The first argument is the starting index, and the second argument is the ending index (exclusive). If the second argument is omitted, the slice goes to the end of the array.

JavaScript doesn't support negative indexing _directly_ into arrays (like `animals[-1]`), but the `slice()` method _does_ support negative indexes.

# const Arrays

Like objects, the contents of `const` arrays _can be modified_! Again, they just can't be _reassigned_. That means we can add and remove elements, but we can't set a new array value with the assignment operator: `=`.

```js
const drinks = [];

drinks.push("lemonade");
// ["lemonade"]

drinks[0] = "soda";
// ["soda"]

drinks = ["root beer"];
// TypeError: Assignment to constant variable.
```

Personally, I'm not a fan of this quirk of the JavaScript language because, to me, `const` implies that a value won't change _at all_. However, in the case of an array all the contents can be modified as long as the assignment operator is never used to reassign the array itself.

# Destructure

You can [destructure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) an array just like you can an object.

```js
const nums = [1, 2, 3];

function double([a, b, c]) {
  return [a * 2, b * 2, c * 2];
}

const [x, y, z] = double(nums);
console.log(x, y, z);
// 2 4 6
```

If you're only interested in the first element, you can destructure just that element, from the same code:

```js
const [x] = double(nums);
console.log(x);
// 2
```

If you're unsure how many elements there are, you can use the rest operator `...` to capture the rest of the elements into a new array:

```js
const [x, ...theRestOfThem] = double(nums);
console.log(x);
// 2
console.log(theRestOfThem);
// [4, 6]
```

If you over-destructure, you'll get `undefined`:

```js
const [x, y, z, a] = double(nums);
console.log(x, y, z, typeof a);
// 2 4 6 undefined
```

The variable created with `...` is always an array; if there are no remaining elements, it will simply be an empty array `[]`

```js
const [x, y, z, ...a] = double(nums);
console.log(x, y, z, a);
// 2 4 6 []
```

---

CH9: Errors

# Error Object

Errors in JavaScript, like Go, are called [`Error`s](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error). It sure beats a silly name like "Exceptions"...

The most important property on the built-in [error object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) is the `message` property: which should be a human-readable description of the error.

```javascript
const err = new Error("We've run out of baked salmon");
console.log(err.message);
// We've run out of baked salmon
```

# Handling Errors

Errors are thrown (and _should_ be thrown by _you_) when a non-happy path is encountered in your code. For example:

- Data received from an API is not in the expected format
- The connection to a database is lost
- A user tries to log in with an incorrect password

Like Python, when an error is thrown, JavaScript yeets the program out of its current context and into the nearest [`try/catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) block, or, if there isn't one, it crashes the program. For example:

```js
const titan = {};
console.log(titan.neck.thickness);
console.log("done");
```

The code above prints:

```
TypeError: Cannot read properties of undefined (reading 'thickness')
```

It never gets to the `console.log("done")` because the error crashes the program. But what if we use a `try/catch` block? We place the potentially error-throwing code in the `try` block, and if an error is thrown, execution immediately jumps to the `catch` block (ignoring any code in the `try` block that hasn't run yet):

```js
try {
  const titan = {};
  console.log(titan.neck.thickness);
  console.log("what's a titan?");
} catch (err) {
  console.log(err.message);
}
console.log("done");
```

Which prints:

```
Cannot read properties of undefined (reading 'thickness')
done
```

# Finally

We missed a block. While a `try/catch` is the most common block you'll see, there is also a [`finally` block](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch#syntax).

> The code in the `finally` block will always be executed _before_ control flow exits the entire construct.

It's for if you want something to run _regardless_ of what nonsense happens in the `try` and `catch` blocks. In some crazy scenarios (try to avoid this), you might have an error thrown in the `catch` block. But even if that happens, the `finally` block will still run. In this example:

```js
try {
  const titan = {};
  console.log(titan.neck.thickness);
  console.log("what's a titan?");
} catch (err) {
  console.log(err.message);
} finally {
  console.log("This will always run regardless of any errors.");
}
```

This is what gets printed:

```
Cannot read properties of undefined (reading 'thickness')
This will always run regardless of any errors.
```

# Throwing Errors

Sometimes errors are thrown implicitly by the JavaScript runtime, as we saw in past examples. But sometimes we need to throw errors ourselves _explicitly_. The [`throw`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/throw) statement is how we do it:

```js
throw new Error("something went wrong");
```

It's worth mentioning that JavaScript will _allow_ you to throw anything you want, not just error objects:

```js
throw "something went wrong";
```

But for consistency and maintainability, **I recommend always throwing error objects**.

# When to try/catch

Errors are _not_ something to be scared of. Every program that runs in production handles errors on a constant basis. Our job as developers is to handle the errors gracefully and in a way that aligns with our expectations. Now, I will admit, one of the big criticisms I have of JavaScript is how hard it is to know whether I should expect a function to potentially throw an error or not.

In Go and some other languages, the function signature tells us if we should expect an error:

```go
func getMovieRecord(movieId int) (Movie, error) {
  // ...
}
```

This lets us know if we should be prepared to handle an error when we call a function. In JavaScript... we're kinda left guessing. The only way to know for sure is to read the body of the function. This _might_ tempt you to just wrap everything in tons of `try/catch` blocks, but I'd **advise against that**.

Here are some rules of thumb for knowing when to use `try/catch`:

- **Do you control the input?**
    - If the variable in question is coming from a user, an API, or some other external source, you should probably wrap its initial handling in a `try/catch` block.
- **Is the error recoverable?**
    - If the error is something you can recover from, like a network request failing, you should probably wrap it in a `try/catch` block. If not, let the program crash.
- **Are you trying to compensate for bad code?**
    - If you wrote some bad code that results in more errors than there need to be, don't wrap it in a `try/catch` block. Fix the code.
- **Is it really an abort-worthy error?**
    - In a lot of (especially front-end) JavaScript code, there are so many unknowns that it doesn't make sense to lose control of a program just because a variable is `undefined`. That's why the [optional chaining operator (`?.`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) and [nullish coalescing operator (`??`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) were introduced... use them as needed.

---

CH10: Sets

# Sets

JavaScript added support for sets (still waiting on you to do the same, Golang). A set is just a collection of unique values. [Sets](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) are fantastic for de-duplication and checking if a value exists in a collection.

```javascript
const set = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
console.log(set);
// Set { 1, 2, 3, 4, 5 }
```

Notice that all duplicate instances of `5` were removed from the set. You can also [`add`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/add) and [`delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/delete) values dynamically:

```javascript
const set = new Set();
set.add("bertholdt");
set.add("reiner");
set.add("annie");
set.add("bertholdt");
console.log(set);
// Set { 'bertholdt', 'reiner', 'annie' }

set.delete("annie");
console.log(set);
// Set { 'bertholdt', 'reiner' }
```

## Tip

The `...` spread operator also works for sets!

# Set Composition

I'll be 10000% honest, I don't often use these fancy set composition methods... but maybe I should. They're pretty neat. In most of my work I just use sets to check for existence or de-duplicate values. But, if you're versed in set theory, you might find these methods useful.

## Intersection

The [`.intersection()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/intersection) method returns a new set containing the elements that are in _both_ sets.

```js
const heroes = new Set(["eren", "mikasa", "armin", "reiner"]);
const villains = new Set(["eren", "reiner", "bertholdt", "annie"]);
const samesies = heroes.intersection(villains);
console.log(samesies);
// Set { 'eren', 'reiner' }
```
## Difference

The [`.difference()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/difference) method returns a new set containing the elements that are in the _first_ set but _not_ in the second set.

```js
const heroes = new Set(["eren", "mikasa", "armin", "reiner"]);
const villains = new Set(["eren", "reiner", "bertholdt", "annie"]);
const nonVillains = heroes.difference(villains);
console.log(nonVillains);
// Set { 'mikasa', 'armin' }
```

## Union

The [`.union()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/union) method returns a new set containing the elements that are in _either_ set.

```js
const heroes = new Set(["eren", "mikasa", "armin", "reiner"]);
const villains = new Set(["eren", "reiner", "bertholdt", "annie"]);
const everyone = heroes.union(villains);
console.log(everyone);
// Set { 'eren', 'mikasa', 'armin', 'reiner', 'bertholdt', 'annie' }
```

There are some more [set methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#set_composition), but those are the big three.

---

CH11: Maps
# Maps

JavaScript also offers (as of recently) support for _maps_: collections of key-value pairs. Map keys are unique, so adding a key that already exists will replace its value. You can [`set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set) and [`delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete) entries dynamically:

```javascript
const map = new Map();
map.set("bertholdt", "shifter");
map.set("reiner", "warrior");
map.set("annie", "shifter");
map.set("bertholdt", "colossal titan");
console.log(map);
// Map { 'bertholdt' => 'colossal titan', 'reiner' => 'warrior', 'annie' => 'shifter' }

map.delete("annie");
console.log(map);
// Map { 'bertholdt' => 'colossal titan', 'reiner' => 'warrior' }
```

Maps can be constructed from any iterable. Maps are iterable, so that means the `Map` constructor can accept a map:

```javascript
const originalMap = new Map();
originalMap.set("bertholdt", "shifter");
const mapCopy = new Map(originalMap);
console.log(mapCopy);
// Map { 'bertholdt' => 'shifter' }
```

Just like a `Map`, a `Set` can be initialized with **any iterable**. The only difference is that while a `Map` expects an iterable of pairs (like `[key, value]`), a `Set` just expects an iterable of individual elements.

# Map Keys

In JavaScript, keys can be any type... because of course they can. This is JavaScript, after all. But just because you _can_, doesn't mean you _should_... if you're a sicko, you might be wondering if this works:

```js
const map = new Map();
map.set(["hello", "there"], "general kenobi");
console.log(map.get(["hello", "there"]));
// undefined
```

It actually _doesn't_. Ha! The key is an array, but it's a _different_ array than the one we used to set the value. Sure, the _contents_ are the same, but what matters when comparing keys is that the reference to the object (or array) in memory is the same. So, unfortunately, if we use a single named variable, it _does_ work:

```js
const map = new Map();
const greetingKey = ["hello", "there"];
map.set(greetingKey, "general kenobi");
console.log(map.get(greetingKey));
// general kenobi
```

That said, this still makes me sick to look at... in 99% of cases, you should just use strings or numbers as keys.

# Map vs. Object

In Go, developers use maps _all the time_. They're the only reasonable choice you're given for a dynamic key/value store.

In JavaScript, you have two options: objects and maps, and honestly, I still use objects more often than maps just because the syntax is so simple. That said, there are some advantages to using maps:

1. **Ordered**: Map keys are ordered in an easy-to-understand way. Objects are not.
2. **Iterable**: [Maps are iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map#iterating_map_with_for...of), so you can use `for (const [key, value] of myMap)` to loop over them. Objects are not (without some extra work).
3. **Performance**: Maps are typically faster when you need to do a lot of insertions and deletions.
4. **No extra properties**: Maps don't have any extra built-in properties like `__proto__` or `constructor` that you might not want.

All this to say, in most cases either will work for your KV needs, but it's important to understand some of the benefits of maps.

---

CH12: Promises

# Synchronous vs. Asynchronous

Most code is [synchronous](https://developer.mozilla.org/en-US/docs/Glossary/Synchronous) code, which means it _runs in sequence_. Each line of code executes in order, one after the next.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/HnxfHEV-840x400.png)

Example of synchronous code:

```js
console.log("I print first");
console.log("I print second");
console.log("I print third");
```

Asynchronous or [`async`](https://developer.mozilla.org/en-US/docs/Glossary/Asynchronous) code runs _concurrently_. While the [main thread](https://developer.mozilla.org/en-US/docs/Glossary/Main_thread) continues running subsequent code, async tasks are handled _outside_ the main execution flow and run as system resources allow. A good example is with the built-in [setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout) function.

`setTimeout` accepts a function and a number of milliseconds as inputs. It sets aside the function to be run after the number of milliseconds has passed, at which point it gets queued for execution when the main thread is available:

```js
console.log("I print first");
setTimeout(
  () => console.log("I print third because I'm waiting 100 milliseconds"),
  100,
);
console.log("I print second");

// Output:
// I print first
// I print second
// I print third because I'm waiting 100 milliseconds
```

# Why Async?

We try to _mostly_ write synchronous code whenever possible because it's easier to keep track of, and therefore leads to fewer bugs. But sometimes we _need_ our code to be asynchronous. For example, whenever you update your user settings on a website, your browser needs to communicate those new settings to the server. The time it takes your HTTP request to physically travel across all the wiring of the internet can be anywhere from 10-1000 milliseconds (give or take).

It would be excruciating if your webpage froze while waiting for every network request to finish. By making network requests _asynchronously_, the webpage can continue to execute other code while waiting for the HTTP response to come back.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/HnxfHEV-840x400.png)

# Promises

A Promise in JavaScript is very similar to a promise to your friend. It's just a commitment for the future. For example, _I promise to explain promises to you_. This promise to you has 2 potential outcomes:

- It's fulfilled, meaning I eventually explained
- It's rejected, meaning I failed to explain

The [`Promise Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) represents the eventual **fulfillment or rejection** of a promise. In the meantime, while we're waiting for the promise to be fulfilled, our code continues executing. Promises are the most popular modern way to write asynchronous code in JavaScript.

## Creating a Promise

Here's a promise that, based on [random number generation](https://csrc.nist.gov/glossary/term/random_number_generator), will resolve and return the string "resolved!" or reject and return the string "rejected!" after 1 second:

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    if (getRandomBool()) {
      resolve("resolved!");
    } else {
      reject("rejected!");
    }
  }, 1000);
});

function getRandomBool() {
  return Math.random() < 0.5;
}
```

In the `new Promise((resolve, reject) => { ... })` constructor, `resolve` and `reject` are functions provided by JavaScript that you call to either successfully complete the promise (`resolve`) or signal that it failed (`reject`)

## Working With Promises

Now that we've created a promise, how do we use it?

The `Promise` object has [`.then`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) and [`.catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) methods. Think of `.then` as the _expected_ follow-up to a promise, and `.catch` as the "something went wrong" follow-up.

- If a promise _resolves_, its `.then` method will execute.
- If the promise rejects its `.catch` method will execute.

```js
promise
  .then((message) => {
    console.log(`The promise finally ${message}`);
  })
  .catch((message) => {
    console.log(`The promise finally ${message}`);
  });

// if the promise (from the first example) resolves, the output will be:
// The promise finally resolved!

// if the promise rejects, the output will be:
// The promise finally rejected!
```

# Why Promises?

Promises are the cleanest (but not the only) way to handle the common scenario where we need to make requests to a server, which is typically done via an [HTTP request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). JavaScript's built-in [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) function (that we'll cover in a later course) returns a promise.

## I/O, or “input/output”

Almost every time you use a promise it will be to handle some form of I/O. I/O, or input/output, refers to when our code needs to interact with systems outside of the (relatively) simple world of local variables and functions.

Common examples of I/O include:

- HTTP requests
- Reading files from the hard drive
- Interacting with a Bluetooth device
- Sending data to a database

Promises help us perform I/O without forcing our entire program to freeze up while we wait for a response.

# Await

The [`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) keyword is used to _wait_ for a Promise to resolve. Once it has been resolved, the `await` expression returns the value of the resolved `promise`. It's basically a more modern syntax for `.then` callbacks.

## .then Callback

```js
promise.then((message) => {
  console.log(`Resolved with ${message}`);
});
```

## await Syntax

```js
const message = await promise;
console.log(`Resolved with ${message}`);
```

Personally, I recommend using `await` over `.then` whenever possible. It's cleaner and easier to read.

## Handling Rejections

When using `await`, if the promise is rejected, it will _throw an error_. That means we can use standard `try`/`catch` blocks to handle rejections.

```js
try {
  const message = await promise;
  console.log(`Resolved with ${message}`);
} catch (error) {
  console.log(`Rejected with ${error}`);
}
```

# Async Keyword

While the `await` keyword can be used in place of `.then()` to _resolve_ a promise, the [async keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) can be used in place of [new Promise()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise) to _create_ a new promise.

When a function is prefixed with the `async` keyword, it will _automatically_ return a Promise that resolves to the return value. You can think of `async` as "wrapping" your function within a promise.

These are equivalent:

## New Promise()

```ts
function getPromiseForUserData() {
  return new Promise((resolve) => {
    fetchDataFromServer().then(function (user) {
      resolve(user);
    });
  });
}

const promise = getPromiseForUserData();
```

## Async

```ts
async function getPromiseForUserData() {
  const user = await fetchDataFromServer();
  return user;
}

const promise = getPromiseForUserData();
```

`await` can only be used inside an `async` function or at the top level of a module (file).  
In an `async` function, returning a `Promise`, will implicitly be awaited by the caller.

# then vs. await

In the early days of web browsers, promises and the `await` keyword didn't exist, so the only way to do something asynchronously was to use callbacks.

A "callback function" is a function that you hand to another function. That function then calls your callback later on. The [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout) function we've used in the past is a good example.

```js
function callbackFunction() {
  console.log("calling back now!");
}
const milliseconds = 1000;
setTimeout(callbackFunction, milliseconds);
```

The `.then()` syntax is generally _easier to use than non-`Promise` callbacks_, but `async` and `await` make handling promises _even simpler_. As a general rule, prefer `async` and `await` over `.then` and [New Promise()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)... I mean for realsies, which of the following is easier to understand?

```js
fetchRecipient()
  .then(function (recipient) {
    return fetchMessageForRecipient(recipient.id);
  })
  .then(function (message) {
    return fetchDeliveryStatus(message.id);
  })
  .then(function (status) {
    console.log(`The status is ${status}`);
  });
```

```js
const recipient = await fetchRecipient();
const message = await fetchMessageForRecipient(recipient.id);
const status = await fetchDeliveryStatus(message.id);
console.log(`The status is ${status}`);
```

The `async` and `await` keywords weren't released until _after_ the `.then` API, which is why there is still a lot of legacy `.then()` code out there.

---

CH13: The Event Loop

# Single Threaded

JavaScript is _famously_ [single-threaded](https://en.wikipedia.org/wiki/Thread_\(computing\)#Single-threaded_vs_multithreaded_programs). You have an octa-core processor? _Cool story, bro_. Your JavaScript program will only be able to use a fraction of that power.

Why? Because JavaScript is single-threaded. It can only do one thing at a time, and it can only take advantage of one core at a time. So no two computations in your JavaScript program will ever run simultaneously.

You might be thinking:

> "Gee, if it's single-threaded, it must really suck at doing many things at once."

_But not so fast_!

JavaScript is actually incredible when it comes to _asynchronous_ programming. Why? Because it's "non-blocking".

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/9mrkI6j-1105x720.png)

The simplest way to think about it is that JavaScript can only execute one instruction at a time, but it can continue processing other stuff **while it's waiting** for something external (like a network request) to complete. In other words, if the "many things" you're doing are I/O bound, like:

- Network requests
- File system operations
- Timers
- Database queries

Then JavaScript performs quite well. If they're CPU bound (like heavy calculations), JavaScript will struggle. A Node.js server will often far outperform a multi-threaded Python, Ruby, or PHP server because of its ability to handle many concurrent connections without much overhead. On the other hand, it will usually be outperformed by a multi-threaded Java, Go, C++, or Rust server when it comes to heavy computation.

# Non Blocking

So how does JavaScript manage to be so efficient with asynchronous code? The answer is the [event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop).

The event loop is a single-threaded, non-blocking, event-driven, asynchronous execution model.

Say that five times fast.

We already covered the single-threaded part, now let's grok [non-blocking](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model#never_blocking). Let's use this Python code as an example:

```python
import time

print("Start")
time.sleep(2)
print("Middle")
time.sleep(2)
print("End")
```

This code prints "Start", then waits for 2 seconds, prints "Middle", waits another 2 seconds, and finally prints "End". The `time.sleep(2)` function calls are _blocking_: they stop the program's execution until 2 seconds have passed.

Let's write a similar example in JavaScript:

```js
console.log("Start");
setTimeout(() => {
  console.log("End");
}, 4000);
console.log("Middle");
```

_This_ code prints "Start", then "Middle" _immediately_, waits 4 seconds, then prints "End". The main thread in JavaScript _cannot be blocked_. That's why `setTimeout` takes a callback function as an argument, it basically says:

> Hey, I know I can't block the program, but please Mr. JavaScript engine, can you take this function and run it for me in 4 seconds?

So, the main thread should _always_ be available to do work, and blocking (read: waiting) is delegated "for later".

The sleep function only works in an `async` function.

The `sleep` helper function is a JavaScript staple.

# The Call Stack

So, we know that JavaScript has one main thread, and that it's non-blocking. So how do these "background" tasks (like HTTP requests, setTimeout, etc.) get executed? Well, it's via the [event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) - but first, we need a little refresher on the call stack.

_This next bit assumes you know about stacks, heaps, and the call stack. If you don't, review our [memory management course](https://www.boot.dev/courses/learn-memory-management-c) first_. That said, I'll give you a _quick_ refresher. Every time a function is called, it gets added to the top of the call stack. When the function returns, it gets popped off the stack.

Let's say we have this code:

```js
function startJob() {
  console.log("Job started");
  workOnJob();
}

function workOnJob() {
  console.log("Working on job");
  finishJob();
}

function finishJob() {
  console.log("Job finished");
}

startJob();
```

The call stack will grow like this as each function is called:

```
                                     -> finishJob
                        -> workOnJob    workOnJob
[empty]    -> startJob     startJob     startJob
```

Then as each function returns, it gets popped off the stack:

```
finishJob  ->
workOnJob     workOnJob ->
startJob      startJob     startJob  -> [empty]
```

Long story short - JavaScript's call stack works the same way as any other language's call stack. But what happens when we encounter asynchronous code? _We'll cover that in the next lesson_.

# Task Queue

We understand the call stack: call a function, it's pushed onto the stack, when it returns, it's popped off. But what about _asynchronous_ code?

_Enter the task queue._

The task queue (also known as the "message queue") is where asynchronous tasks are _queued up_ to be processed. It's just a standard queue of things for our JS engine to do, nothing to be scared of. But remember: JS is _non-blocking_, so the tasks in the queue can't be handled immediately.

The rule of the task queue is simple: when the call stack is _empty_, the event loop (managed by the JS runtime) checks the task queue. If there are tasks in the queue, it pushes the first one onto the call stack to be executed. Take a look at this example again:

```js
function startJob() {
  setTimeout(() => {
    console.log("Hi I'm async!");
  }, 0);
  console.log("Job started");
  workOnJob();
}

function workOnJob() {
  console.log("Working on job");
  finishJob();
}

function finishJob() {
  console.log("Job finished");
}

startJob();
```

Because the `setTimeout` says "run this 0 milliseconds from now", you _might_ expect its callback to run instantly and produce this output:

```
Hi I'm async!
Job started
Working on job
Job finished
```

But this is what **actually happens**:

```
Job started
Working on job
Job finished
Hi I'm async!
```

Because the callback:

```js
() => {
  console.log("Hi I'm async!");
};
```

Was pushed into the task queue to be executed _after_ the call stack is empty, and it's not empty until the final nested function `finishJob` returns.

# Microtask Queue

Okay so there's _one more_ queue to be aware of: the [_microtask queue_](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

Just like the task queue, the microtask queue is a mechanism for scheduling tasks to be executed later. But it operates under different rules and is used for different purposes. The nature of microtasks is that they represent smaller, shorter-lived operations compared to tasks in the task queue. And importantly, **promises use the microtask queue** to schedule their `.then()` and `.catch()` callbacks.

There are two important differences between the task queue and the microtask queue:

- **Order of Execution**: All microtasks are executed before the next task in the task queue.
- **Addition of Microtasks**: Microtasks can add more microtasks to the queue, and those will still execute before the next "macro" task.

## So Do I Need to Care?

Well, usually... no. But sometimes yes. For the most part, you can think about promises and callbacks as just "asynchronous operations that will run later". You typically won't (and it's often a bad sign if you do) care about the exact order that their callbacks will run.

But I believe in learning stuff, so let's dive in. This example shows the difference between the "macro" (regular) task queue and the microtask queue:

```js
function main() {
  console.log("main start");

  setTimeout(() => {
    console.log("macrotask 1 finished");
  }, 0);

  Promise.resolve()
    .then(() => {
      console.log("microtask 1 finished");
    })
    .then(() => {
      console.log("microtask 2 finished");
    });

  console.log("main end");
}

main();
```

It prints:

```
main start
main end
microtask 1 finished
microtask 2 finished
macrotask 1 finished
```

The important thing to note is simply that all the microtasks run before the next task in the task queue.

# Concurrency

Okay, so we understand that:

- There's only one thread in the runtime
- The main thread can't be blocked by asynchronous tasks
- The results of asynchronous tasks are pushed into the task queue

So how does the actual concurrency work? In the case of:

```js
setTimeout(() => {
  console.log("Hi I'm async!");
}, 1000);
```

What logic makes sure that the callback function isn't pushed into the task queue until `1000` milliseconds have passed? Or regarding an HTTP request, what logic pushed the network response into the task queue when the request is complete?

The answer is _external APIs_. Things like [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout), [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch), and [`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) are all examples of external APIs that the browser or Node.js, Deno, or Bun provide – they are not part of the core JavaScript language.

The JavaScript _runtime_ (your code and the JS engine) is single-threaded, but these external APIs are _not_! The host environment can run them in the background (often on separate threads or system-level services), and **when they're done, the host environment pushes their results into the task queue** for the event loop to handle.

---

CH14: Runtimes

# JavaScript Runtimes

A runtime environment is _where your program runs_. The runtime you choose will determine things like:

- What APIs are available to your code (fetch, canvas, etc.)
- How your code is executed (JIT compiled vs interpreted)
- What dependencies you'll need in production
- Whether you run on the backend (a server) or frontend (a browser or mobile app)

## Examples of Runtimes

- The browser (we try to pretend they're the same, but in reality different browsers are different runtimes)
    
- [Node.js](https://nodejs.org/en/)
    
- [A web worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) within a browser
    
- [Deno.js](https://deno.land/)
    
- [Bun](https://bun.sh/)
    

Originally, JavaScript _only_ ran in browsers. Today, it runs almost everywhere.

## Which Runtime Should I Use?

If you're doing frontend web development, congratulations, you're using the browser. You likely have to support all the major browsers, so you'll need to know what APIs are available in each.

If you're doing backend development, you get to choose. Node.js is the oldest and most popular, Deno and Bun are newer and less mature, but have some cool features (like native TypeScript support) and claim to be faster. In reality, they're all very similar to work with. You don't need to "learn" a runtime to be able to work with it, if you understand JavaScript, you can work with any of them.

# Node.js

If you've used Python before, you're familiar with running a Python script like this:

```bash
python main.py
```

Similarly, if you install the [Node.js](https://nodejs.org/en/download/) toolchain on your local machine, you'll be able to run

```bash
node main.js
```

Before Node, the only way to run JavaScript code was in the browser. Of course, you can still do that using [your browser's dev tools!](https://developer.chrome.com/docs/devtools/console/javascript/)

## NVM

I use ["Node Version Manager"](https://github.com/nvm-sh/nvm) to manage my Node.js installation. NVM makes it easy to:

- Install multiple versions of Node
- Update your Node version
- Keep your Node version configurations separate on a per-project basis

It's kinda like `pyenv` for Python, if you're familiar with that.

# NPM

Now that you've got `node` working, its important to understand that... you probably won't use it directly very often. Instead, you'll use `npm` (Node Package Manager) to install and manage packages.

[`npm`](https://www.npmjs.com/) is a package manager for JavaScript. It's the world's largest software registry, with over 1.3 million packages of code. It's the home of many useful libraries such as:

- [is-even](https://www.npmjs.com/package/is-even)
- [cowsay](https://github.com/piuccio/cowsay)
- [left-pad](https://www.npmjs.com/package/left-pad)

If you're familiar with Python, `npm` is similar to `pip` or `uv`. If you're familiar with Go, `npm` is similar to `go get`.

## Assignment

1. [ ] Run `npm init` in the `heifer` directory and follow the prompts to create a new `package.json` file.

It's fine to just accept the defaults. You can run `npm init -y` to do that - it creates a basic `package.json` with common defaults.

_The [`package.json`](https://docs.npmjs.com/cli/v11/configuring-npm/package-json) file is what Node.js uses to manage dependencies, scripts, and other metadata about your project._

2. [ ] Edit the `main.js` file so that it prints "moo!" to the console.
3. [ ] Edit the "scripts" section of your `package.json` file:
    1. [ ] Remove the `"test"` script.
    2. [ ] Add a new script called `"start"` that runs `node main.js`.
    3. [ ] Next run `npm run start` to test it out.

_The `scripts` section of the `package.json` file is where you can define custom scripts that can be run with `npm run <script-name>`._

# ECMAScript

ECMAScript??? I thought we were learning JavaScript!

Well, we are. ECMAScript is the _standard_ that JavaScript is based on. Brendan Eich, the creator of JavaScript, tastefully commented on the name:

> ["ECMAScript was always an unwanted trade name that sounds like a skin disease."](https://en.wikipedia.org/wiki/ECMAScript#:~:text=Eich%20commented%20that%20%22ECMAScript%20was%20always%20an%20unwanted%20trade%20name%20that%20sounds%20like%20a%20skin%20disease.%22)

Its purpose is to help JavaScript (and other languages) maintain compatibility across different runtimes. ECMAScript versions are released yearly, and they have a big impact on how we write modern JavaScript. Some notable versions include:

- 2009: ECMAScript 5 (ES5) introduced `strict mode` and JSON support
- 2015: ECMAScript 6 (ES6) introduced `let` and `const` for variable declarations
- 2017: ECMAScript 8 (ES8) introduced async/await

# Polyfills and Transpilers

Most programming languages change over the years. The update from Python 2 to Python 3, for example, is a famous example of an insane amount of breaking changes that caused many a Python developer to lose more sleep and hair than they would have liked.

JavaScript has also changed a lot over the years, but the interesting thing about being a language that primarily runs in a web browser is that _you don't always control the runtime_. If you're running Python (or JS, or anything) on a _server_, then you can update your code, and at the same time update your runtime, or compiler, or interpreter. With frontend JavaScript, you're at the mercy of whatever mix of out-of-date browsers your users are running.

Enter **polyfills and transpilers**.

A [polyfill](https://en.wikipedia.org/wiki/Polyfill_\(programming\)) is an extra bit of code you include to add functionality that some browsers might not support. For example, maybe Chrome allows you to use the fancy new `Array.prototype.flat()` method, but Internet Explorer 11 doesn't. You can include a polyfill (just some extra JavaScript code) that adds that method to the `Array` prototype so that your code works in both browsers.

A [transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler) (in the context of adding new JavaScript features) is basically a polyfill on steroids. Instead of just adding a method here or a property there, a transpiler will take your _entire_ JavaScript file and convert it into an older version of JavaScript that is known to work in all browsers. For example, it might take your fancy `async` and `await` keywords and convert them into a bunch of `Promise` objects and `.then()` calls. [Babel](https://babeljs.io/) is the most popular transpiler for JavaScript.

## Assignment

Let's update our `heifer` code. Instead of statically saying `moo!`, the code should say `moo, NAME!`, where `NAME` is a variable you define and is dynamically inserted using a template literal.

Template literals (strings wrapped in backticks `` ` ``) are modern feature of JavaScript. However, older browsers may not support them. We'll use Babel to ensure compatibility.

1. [ ] Update your code in `main.js` to use string interpolation with a template literal.
2. [ ] Install Babel in your project directory to transpile modern JavaScript.

```bash
npm i -D @babel/core @babel/cli @babel/preset-env
```

- `@babel/core` – the main Babel package. It provides the core functionality for transpiling JavaScript.
- `@babel/cli` – a CLI that will let us run Babel.
- `@babel/preset-env` – a preset that automatically determines which JavaScript features need to be transpiled.

If you haven't done so already, now would be a **great** time to create a `.gitignore` file and add `node_modules/` to it.

3. [ ] Create a `.babelrc` file in your project root with the following content:

```json
{
  "presets": ["@babel/preset-env"]
}
```

_This tells Babel to transpile your code based on what's needed for older browsers. You can [set targets for `preset-env`](https://babeljs.io/docs/babel-preset-env#targets). See the docs for more info._

4. [ ] Use Babel to transpile your `main.js` file to an older version of JavaScript:

```sh
npx babel main.js --out-file main.compiled.js
```

`npx` is a **package runner** that comes with Node.js. It allows you to use local executables without installing them globally.

5. [ ] Check that the resulting `main.compiled.js` file no longer contains any template literal syntax (backticks).

---

CH15: Modules

# Modules

Back in the earliest days of JavaScript - when it was but a wee browser-only language that looked like bastardized Java - JavaScript files were _small_. A callback here, a dynamic element there, and a sprinkle of (_gasp_) [jQuery](https://jquery.com/) to make it all work.

But as the web has grown, we're not just shipping a bit of interactivity to static HTML pages anymore. We're building full-fledged applications with crazy amounts of state, logic, and dependencies. In the case of [single page applications](https://en.wikipedia.org/wiki/Single-page_application), sometimes HTML is just a shell that gets filled in with JavaScript at runtime.

Long story short, JS needed modules. In Go, you have packages. In Python, you have modules. Now, JavaScript also has modules - they're just a way to split your code into separate files and import the code you need across a large codebase.

**Modules in JavaScript exist at the file level**, just like Python. So, if you have a file called `math.js` that looks like this:

```js
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;

module.exports = {
  add,
  subtract,
};
```

You can import and use those functions in another file like this:

```js
// main.js
const { add, subtract } = require("./math.js");

console.log(add(1, 2)); // 3
console.log(subtract(1, 2)); // -1
```

This is [CommonJS](https://en.wikipedia.org/wiki/CommonJS) syntax. I'll explain what that means soon.

# CommonJS

So we've started with the _less_ preferred way of doing modules in JavaScript: [CommonJS](https://en.wikipedia.org/wiki/CommonJS). CommonJS is a module system that was created for Node.js _before_ the new [ES6 module syntax](https://nodejs.org/api/esm.html#modules-ecmascript-modules) was introduced. It's still used in Node.js today, but ever since Node added support for ES6 modules, it's become less **common**. (heh, get it?)

CommonJS is Node.js specific, you can't use it in the browser without some kind of bundler, which we'll talk about in a bit. The defining features of CommonJS are:

- The [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports) object, which is used to export stuff
- The [`require`](https://nodejs.org/api/modules.html#modules_require) function, which is used to import stuff

## Exporting

```js
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;

module.exports = {
  add,
  subtract,
};
```
## Importing

```js
// main.js
const { add, subtract } = require("./math.js");

console.log(add(1, 2)); // 3
console.log(subtract(1, 2)); // -1
```

# Strict Mode

JavaScript is a loosey-goosey language, _as we've learned_. As such, it's easy to make mistakes that are hard to catch until a user is emailing you a bug report.

[Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) is a way to opt-in to a more restrictive set of rules that help you catch errors earlier. It was introduced in ES5. Some of the big differences between strict mode and "normal" (sloppy) JavaScript are:

- Eliminates some silent errors by throwing them
- Fixes some mistakes that make it difficult for JS engines to optimize code. Strict mode code can sometimes run faster than identical code that's not in strict mode.
- Prohibits some syntax likely to be defined in future versions of ECMAScript
- In the browser, `this` is `undefined` in global scope

Long story short, when possible, **strict mode is a good idea**.

To enable strict mode, you just need to "use strict" at the top of your file:

```javascript
"use strict";

// your code here
```

You _can_ also enable strict mode for a single function:

```javascript
function strictFunction() {
  "use strict";
  // your code here
}
```

## Strict Mode in Modules

Here's the best part: you only need `"use strict";` at the top of non-es6 modules. ES6 modules, which we will learn next, are always in strict mode by default. That goes for the browser _and_ Node.js.

We'll go more in-depth on Node and browser modules in a moment.

## Assignment

JavaScript normally lets you get away with all kinds of nonsense, so let's do some JavaScript shenanigans:

1. [ ] Create a new file called `strict.js`
2. [ ] Assign `"moo!"` to an undeclared variable and `console.log` that variable

```js
message = "moo!";
console.log(message);
```

3. [ ] Run the script

```sh
node strict.js
```

This works just fine, and you should see `"moo!"` printed to the console! However, if we enable strict mode, this will throw an error instead of silently creating a global variable.

4. [ ] Enable strict mode by adding `"use strict";` at the top of the file and run the script again. This time, you should see an error.

# ES6 Modules

ES6 modules are the _preferred_ way of doing modules in JavaScript. They're (now) built into the language and are supported in modern browsers _and_ Node.js. That said, there are still some [gotchas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) to be aware of... which we'll cover.

## The Syntax

The syntax for ES6 modules is drop-dead simple. You can export stuff from a module using the [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) keyword, and import stuff using the [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) keyword. Let's look at our math example again:

### Exporting

```js
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

### Importing

```js
// main.js
import { add, subtract } from "./math.js";

console.log(add(1, 2)); // 3
console.log(subtract(1, 2)); // -1
```

# Node.js Modules

By default, Node.js uses the CommonJS syntax for modules. There are two ways you can manually switch to ES6 module syntax:

1. Add `"type": "module"` to your `package.json` file
2. Rename your module files to `.mjs` instead of `.js`

Personally, I prefer the first option. It's cleaner and easier to manage. And remember, if you're not using ES6 modules, your code will _not_ be in strict mode by default!

# Browser Modules

Modern browsers now support ES6 modules, but there are a few things to be aware of. If your HTML has "normal" old script tags, those scripts will _not_ be treated as modules. They will execute one after the other, in the order they appear in the HTML file.

```html
<script src="meFirst.js"></script>
<script src="meSecond.js"></script>
```

If you want to use ES6 modules, you need to use the `type="module"` attribute on your script tags.

```html
<script type="module" src="math.js"></script>
<script type="module" src="main.js"></script>
```

Modules have a few great advantages:

- Stuff defined in a module is _not_ in the global scope. To be accessed from another module, it must be exported.
- Modules are deferred by default, meaning they only run after the document has been parsed.
- Strict mode is enabled by default. Yay!

# Default Exports

There's one last itty-bitty piece of syntax that you'll encounter when working with modules: default exports.

Default exports are often used when you want to export a _single value_ from a module. Let's take our `math.js` example one last time:

```javascript
// math.js

export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

```javascript
// main.js

import { add, subtract } from "./math.js";
```

This exports _two_ functions. Sometimes, a developer will want to just export _one_ thing, so they can do this:

```javascript
// math.js

const add = (a, b) => a + b;
const subtract = (a, b) => a - b;

export default add;
```

Then when it's imported, you don't need to use the curly braces:

```javascript
// main.js

import add from "./math.js"; // no curly braces
```

You can even have default _and_ named exports:

```javascript
// math.js

export const subtract = (a, b) => a - b; // named export

const add = (a, b) => a + b;
export default add; // default export
```

Though you normally wouldn't do that...

## Should I Use Default Exports?

Honestly... I kinda hate them. What if I want to export more things later? Now I have to refactor _all_ of my imports and exports. It's a pain.

My _personal_ preference is to just pretend default exports don't exist, and always use named exports.

# Bundlers

[Bundlers](https://webpack.js.org/concepts/) are tools that allow you to _write_ code in a modular and easy-to-manage way, and then _bundle_ it in a way that's optimized for production. For example, you probably want a giant front-end application to exist in your codebase as many hundreds of files, but you want to serve it to your visitors as a single file or one file per page.

The point is, a bundler takes care of transforming your code from _how you're writing it_ to _how you want it to be served_. That said, popular bundlers often do a whole suite of important things, like:

- **Bundling**: Duh, we just covered this.
- **Minification**: Making your code smaller by removing unnecessary whitespace (useful when you have to serve your code to a browser via a slow connection).
- **Code Splitting**: Breaking your code into smaller chunks that can be loaded on-demand.
- **Tree Shaking**: Removing unused code from your final bundle.
- **Asset Optimization**: Optimizing images, fonts, and other assets. For example, converting images to WebP format or reducing their size.
- **Source Maps**: building files that allow you to debug your code in the browser as if it were still in its original form.

For a long time, [Webpack](https://webpack.js.org/) was the most popular bundler. It's still used, but [Vite](https://vitejs.dev/) has been gaining a lot of traction lately. Vite is often quite a bit faster than Webpack, and it's also much easier to configure in my opinion. It's built on top of (and adds more features to) [Rollup](https://rollupjs.org/), which is another popular bundler.

## Assignment

Heifer's codebase has come a long way, and it's high time for a bundler. Let's install [cowsay](https://www.npmjs.com/package/cowsay) for our "moo" program, then use [Vite](https://vitejs.dev/) to bundle it into a single file.

1. [ ] Install Cowsay.

```bash
npm i cowsay
```

2. [ ] Import `say` from `cowsay` in `main.js`.
3. [ ] Combine the `moo` function we've made with `say`. You should see a cow saying `moo, NAME!` in your terminal.

```plaintext
 ____________
< moo, there! >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

4. [ ] Install Vite:

Vite requires Node.js version 20.19.0 or higher (or 22.12.0+). Check your Node.js version with `node --version`. If you need to upgrade, visit [nodejs.org](https://nodejs.org/).

```bash
npm i -D vite
```

5. [ ] We can configure Vite to bundle our CLI by setting a `vite.config.js` file at the root of our project directory:

```js
import { defineConfig } from "vite";

export default defineConfig({
  build: {
    lib: {
      entry: "main.js",
      formats: ["es"],
    },
  },
});
```

- `build.lib` - tells Vite that we're generating a "library" bundle. In practical terms:
    - the build is not based on an HTML file or a browser runtime environment.
    - there's a single JavaScript entry which will be packaged (ideal for CLIs or libraries).
    - the output can be executed in Node.js.
- `entry: 'main.js'` - sets the entry point for the bundle as `main.js`.
- `formats: ['es']` - instructs Vite to output the bundle in ES module format.

By default Vite names the bundled file whatever you've named your project in `package.json`, but we could override this with `fileName`.  
There are a lot of other [configuration settings for Vite](https://vite.dev/config/).

6. [ ] In your `package.json`, add a build script and modify the start script to run the bundled file:

```json
{
  "scripts": {
    "build": "vite build",
    "start": "node dist/heifer.js"
  }
}
```

7. [ ] Run the build script and bundle your code:

```bash
npm run build
```

This should create a `dist` folder containing your bundled file. The bundled code might look like some Eldritch horror, but that's just bundled code for you!

You should add the `dist` folder to your `.gitignore` too. Code that can be regenerated with a command should, as a rule of thumb, be gitignored.

8. [ ] Run and test the bundled output:

```bash
npm run start
```

If all went well, you should see a cow say `moo` in the terminal!

