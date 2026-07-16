CH1: Types
# Learn TypeScript

Welcome to "Learn TypeScript"! In this course, you'll learn how to use TypeScript to add [static typing](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) to JavaScript.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/KqZEPTr-853x256.png)

## Learning Goals

- Learn TypeScript's type system and how it builds on JavaScript
- Understand best practices for writing clean, type-safe code
- Learn about generics, interfaces, unions, and other advanced type features
- Master real-world TypeScript patterns used in production codebases

## Prerequisites

This course assumes you're already familiar with JavaScript. _If you're not, start with our [Learn JavaScript course](https://www.boot.dev/courses/learn-javascript) first._

# Basic Types

TypeScript adds type annotations to JavaScript variables using a `:` followed by the type. The most common primitive types are:

```ts
const bootupMessage: string = "starting server...";
const port: number = 3000;
const isOnline: boolean = true;
const noValue: null = null;
const notDefined: undefined = undefined;
```

If a value doesn't match its type, TypeScript throws a compilation error:

```ts
const bootupMessage: string = 123;
// Error: Type 'number' is not assignable to type 'string'.
```

# Type Inference

TypeScript is great at [type inference](https://www.typescriptlang.org/docs/handbook/type-inference.html). Instead of explicitly declaring the type of every variable, TypeScript can infer it from the value:

```ts
// explicit (unnecessary)
const bootupLog: string = "starting server...";

// inferred (preferred)
const bootupLog = "starting server...";
```

Both are equally type-safe, but the inferred version is less typing and leaves less room for human error. **Let TypeScript infer types for you**!

# Why TypeScript

TypeScript is a [superset](https://www.cuemath.com/algebra/superset/) of JavaScript, meaning that:

- All JavaScript code is valid TypeScript code.
- All TypeScript code is not necessarily valid JavaScript code.

_But who tf cares?_ The _real_ question is, **why should I use TypeScript instead of JavaScript?** Well, TypeScript is fantastic because it:

- Catches a ton of bugs and errors at compile time (fact)
- Makes your code more readable and maintainable (imo)
- Makes it easier to refactor and scale your codebase (imo)

In short, it adds static typing to JavaScript.

Static vs dynamic typing isn't exactly a settled debate in the programming community, but for me personally, I'll take static over dynamic types any day of the week. I like to hover over variables and see their types, and I like to know that my code doesn't have an entire class of potential bugs before I even run it.

## Story Time

TypeScript was developed internally at Microsoft by C# developers (led by [Anders Hejlsberg](https://en.wikipedia.org/wiki/Anders_Hejlsberg)) who wanted static typing in their JavaScript (C# being a statically typed language). It was released to the public in 2012, and while its execution speed isn't necessarily blazingly fast, its meteoric rise in popularity is.

Click to hide video

In the video we briefly mention a "linter". A linter is a tool that automatically analyzes your code and warns you about potential errors, style issues, or bad patterns before you run it.

# What Is TypeScript

TypeScript is a _language_, but the official _implementation_ of TypeScript is the [TypeScript compiler](https://code.visualstudio.com/docs/typescript/typescript-compiling), `tsc`. Its job is simple: take TypeScript code, ensure it's valid, and then compile it into JavaScript code.

The compiler, `tsc`, has been written in TypeScript for a long time, but _very_ recently, the TypeScript team has started a [port to Golang](https://github.com/microsoft/typescript-go)! They've reported compilation speed improvements of ~10x, and I'm super excited about it.

TypeScript is _not_ supported natively by most JavaScript engines, so it needs to be compiled into equivalent JavaScript code before it can be run. This interesting fact, that TypeScript code is only type-checked _before_ it's run, has led to an interesting philosophical question:

> Is TypeScript basically just a really good [linter](https://en.wikipedia.org/wiki/Lint_\(software\))?

And honestly... _yeah, I think so_. I get it, technically it does a _lot_ more than your standard linter, but from a practical perspective, its primary benefit is to do static analysis on your _almost-JavaScript_ code and catch bugs before they happen.

## Compiled to... JavaScript?

TypeScript is interesting in that it's "compiled", but not in the traditional (compiled to binary) sense. Instead, it's compiled to JavaScript. So it's not really compiled for _performance_ reasons, but rather for _compatibility_ reasons.

**The goal of TypeScript is to write JavaScript code that's easier to work with**.

## Compilation Errors

So, in this course, if your code fails to _compile_, you'll get an error like this:

```
tsc:
Type 'string' is not assignable to type 'number'.
```

Only if the compilation is successful, _then_ we run the code. So your code needs to pass compilation, _and_ needs to run correctly.

# Any

Okay, so we know TypeScript's _purpose_ is to add static types to JavaScript, and we know all JavaScript is valid TypeScript.

In practical terms, what that means is when you compile plain JavaScript code using `tsc`, your codebase is _full_ of [`any`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) types.

The `any` type is exactly what it sounds like - a type that can be anything. The purpose of types, really, is to _narrow down_ the possible values that a variable can hold. From that perspective, `any` is the most _useless_ type because it doesn't narrow anything down at all! But it's important because it allows you to _opt out_ of type-checking for a variable.

The `any` type is super useful when you migrate an existing JavaScript codebase to TypeScript. The (very simplified) process is:

1. Change file extensions from `.js` to `.ts`
2. Get `tsc` running without errors (often works out of the box, due to `any`)
3. Slowly over time, replace `any`'s with more specific types

Boot.dev's front-end used to be JavaScript! We went through this exact process a couple of years ago getting it all converted to TypeScript and slowly purging the `any`'s.

---

CH2: Functions

# Function Type Syntax

One of the most useful places for explicit types is in function signatures. For example:

```typescript
function createMessage(name: string, a: number, b: number): string {
  return `${name} scored ${a + b}`;
}
```

The `: type` after each parameter specifies that parameter's type, and the `: type` after _all_ the parameters specifies the return type. It works the same way with arrow functions:

```typescript
const createMessage = (name: string, a: number, b: number): string => {
  return `${name} scored ${a + b}`;
};
```

# Inferred Return Types

So you know how we discussed that:

```ts
const myPowerLevel = 9000;
```

Is better TypeScript than:

```ts
const myPowerLevel: number = 9000;
```

What follows is a bit of personal opinion, but I think the same is _generally_ true for function _return_ (not parameter) types.

Instead of this:

```ts
function divide(a: number, b: number): number {
  return a / b;
}
```

We can write this:

```ts
function divide(a: number, b: number) {
  return a / b;
}
```

And TypeScript infers the output type as `number`.

# Void

The TS-specific [`void`](https://www.typescriptlang.org/docs/handbook/2/functions.html#void) type represents the return value of functions that _don't_ return a value.

```ts
function logMessage(message: string): void {
  console.log(message);
  // nothing is returned here!
}
```

In JavaScript, a function without a `return` statement returns `undefined` by default... but that's kinda vague. TypeScript uses the `void` keyword to indicate that truly _nothing_ is returned.

In other words, `void` more explicitly communicates the _intent_ that a function returns _nothing_.

# Function Types

Functions themselves are values in JavaScript (and by extension, TypeScript), which means they must also have a type, right? You might think:

> Oh, easy. They're probably some "`function`" type.

Not so fast. Function types are much more specific than that. In TypeScript, a function's type includes information about its parameters and return value.

## Defining Function Types

The syntax for a function type looks like this:

```ts
(param1: type1, param2: type2, ...) => returnType
```

For example, a function that takes two numbers and returns a number:

```ts
(a: number, b: number) => number;
```

and both of these functions are of that type:

```ts
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
```

# Type Alias

It can get _really_ cumbersome to write out long custom types whenever you want to use them. For example, maybe we have a function that accepts another function as input. Let's use a totally make-believe example, something that sets a timeout:

```ts
function setLoggerTimeout(
  loggerCallback: (s1: string, s2: string) => string,
  delay: number,
) {
  // do something
}
```

That's a nasty function signature... let's use the [`type` keyword](https://www.typescriptlang.org/docs/handbook/declaration-files/by-example.html#reusable-types-type-aliases) instead to create a type alias:

```ts
type LoggerCallback = (s1: string, s2: string) => string;
```

Now anytime we need to use _this specific kind_ of function (one that accepts two strings and returns a string), we can just use `LoggerCallback`:

```ts
function setLoggerTimeout(loggerCallback: LoggerCallback, delay: number) {
  // do something
}
```

Muuuuuch better! It's easy to read and reusable! Why is that important? It's less prone to copying errors as we use in other places in our code. And in the future, if we want to change it, we only have to modify the type declaration rather than everywhere it's used.

# Importing Types

With certain TypeScript configurations you _can_ import types directly from a module:

```ts
import { User, Post } from "./models";
```

But it's much safer and _more efficient_ to use the `import type` syntax:

```ts
import type { User, Post } from "./models";
```

This way TypeScript _knows_ that you're only importing types, and it can drop the imports so they don't generate extra JavaScript code when your project is compiled. This syntax also works:

```ts
import { type User, type Post } from "./models";
```

But personally I prefer the first one. It's more concise and keeps all my type imports in one place.

---

CH3: Unions

# Unions

As someone that writes a lot of Go, union types are the thing I'm _most_ jealous of in TypeScript. **I love them**.

[Union types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) use the pipe symbol (`|`) and allow you to specify that a value can be _one of several_ types.

```typescript
// userId is a string OR a number
let userId: string | number;
userId = "user_42";
userId = 42;
```

Unions are perfect for when a value could be one of several types. One _really cool_ thing about TypeScript is that conditional checks actually change the type of a variable. This is called "type narrowing". Take a look:

```typescript
function safeSquare(val: string | number): number {
  if (typeof val === "string") {
    val = parseInt(val, 10);
  }
  // now val is only a number
  return val * val;
}

let result = safeSquare("5");
console.log(result);
// 25

result = safeSquare(5);
console.log(result);
// 25
```

# Optional Parameters

You can specify function parameters as _optional_ with a question mark (`?`) after the name:

```typescript
function greet(name: string, title?: string): string {
  if (title) {
    return `Hello, ${title} ${name}!`;
  }
  return `Hello, ${name}!`;
}

greet("Gandalf");           // "Hello, Gandalf!"
greet("Gandalf", "Wizard"); // "Hello, Wizard Gandalf!"
```

There are two rules to keep in mind:

1. Optional parameters must come **after** all required parameters. For example, this code won't compile:

```typescript
// Error: Required parameter cannot follow optional parameter
function greet(title?: string, name: string): string {
  // ...
}
```

2. Optional params have an `undefined` automatically unioned on the specified type. If the value is omitted, it's `undefined` instead of the specified type.

```typescript
function greet(name: string, title?: string): string {
  // inside the function, title
  // is a string | undefined
}
```

# Default Parameters

Default parameters provide fallback values for optional arguments.

```typescript
function newCharacter(name: string, role: string = "warrior"): string {
  return `${name} is a ${role}`;
}

console.log(newCharacter("Gandalf"));
// Gandalf is a warrior
console.log(newCharacter("Gandalf", "wizard"));
// Gandalf is a wizard
```

When you use default parameters, you do _not_ need to mark the parameter as optional by using `?`. When using a default value, the parameter type can be automatically inferred, so don't specify it:

```typescript
function countdown(start = 10): void {
  // start is a number
  console.log(`Counting down from ${start}...`);
}
```

...well, unless you need to _widen_ the type.

# Literal Types

Many other statically typed languages (including Go) don't have nearly as extensive and powerful type systems as TypeScript. It should be obvious because it's in the name, but TypeScript truly has a massive type system.

[Literal types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) are incredibly powerful for narrowing the possible values of a variable.

- A string can have an infinite number of values.
- A number can have an infinite number of values.

So what if we want to declare a "direction" variable?

```typescript
function move(direction: string) {
  // Implementation...
}
```

This kinda sucks... direction can be any string! To be fair, in many languages enums are used to solve this problem. And while TypeScript does have enums, which we'll cover later, literal types are a more lightweight solution. A literal value can be used as a type:

```ts
function move(direction: "north") {
  // Implementation...
}
```

Now `direction` can _only_ be "north"!

# Value Unions

Take another look at our last example of a literal type:

```typescript
function move(direction: "north") {
  // Implementation...
}
```

To make it a bit more useful, let's combine that idea with a union type:

```typescript
function move(direction: "north" | "south" | "east" | "west") {
  // Implementation...
}
```

And then let's refactor it to make a new "Direction" type that we can reuse:

```typescript
type Direction = "north" | "south" | "east" | "west";

function move(direction: Direction) {
  // Implementation...
}
```

# Template Literal Types

This is one of the more unhinged features of TypeScript (at least in my opinion), but it is really cool and insanely powerful when you find a good use case for it.

Remember literal types and type unions?

```ts
type Class = "wizard" | "warrior" | "rogue";
```

Well, you can also create literal types using string templates:

```ts
type Hero = `elf ${Class}`;
```

The type of `Class` expands _automatically_ to the possible values, so the above is the same as:

```ts
type Hero = "elf wizard" | "elf warrior" | "elf rogue";
```

You can also get crazy and combine all the combinations of two types:

```ts
type Class = "wizard" | "warrior" | "rogue";
type Race = "elf" | "human" | "dwarf";
type Hero = `Hero: ${Race} ${Class}`;
// Hero: elf wizard | Hero: elf warrior | Hero: elf rogue | Hero: human wizard | Hero: human warrior | Hero: human rogue | Hero: dwarf wizard | Hero: dwarf warrior | Hero: dwarf rogue
```

You can also create types that enforce a simple pattern match. For example:

```ts
type LogRecord = `${string}: ${number}`;

// this is valid because it's a string followed by a colon and a number
const criticalErr: LogRecord = "CRITICAL: 69";

// these are all invalid
const criticalErr: LogRecord = "CRITICAL 92";
const criticalErr: LogRecord = "CRITICAL: 92a";
const criticalErr: LogRecord = "92: CRITICAL";
```

# Giant Unions

So what happens if we create an absolute _monstrosity_ of a union type? It can happen faster than you'd expect... Say we're building a `MoveMessage` type describing a message about a character's movement in a game:

```typescript
type Distance = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9;
type Class =
  | "Warrior"
  | "Rogue"
  | "Mage"
  | "Cleric"
  | "Paladin"
  | "Druid"
  | "Hunter"
  | "Shaman";
type MoveMessage =
  `The ${Class} moves ${Distance}, ${Distance}, ${Distance}, ${Distance}, then ${Distance} spaces.`;

const message: MoveMessage = "The Warrior moves 6, 2, 5, 4, then 7 spaces.";
```

There's a good chance you'll run into an error like this:

> **Error: Union type too complex to represent.**

This happens because we've tried to create an explicit union of types that has **exploded** in size. There are hundreds of thousands of possible combinations in the type above. Even if we remove a couple of the `Distance` values:

```typescript
type MoveMessage =
  `The ${Class} moves ${Distance}, ${Distance}, then ${Distance} spaces.`;
```

When I hover the `MoveMessage` type in my editor, I see:

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/yN89FAv-1280x234.png)

there are still over `5,000` combinations! TypeScript doesn't like that - it can slow down your editor and compilation times to a crawl. So, at a certain point, `tsc` says "enough is enough".

This is a good example of a phrase you might hear in the TypeScript community: "Type Masturbation". I know it's a bit crass, but I didn't invent the term. It just means that you can go too far with trying to create hyper specific types.

_Maybe a `string` would have sufficed after all_.

---

CH4: Arrays

# Arrays

The most common way to declare an [array](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#arrays) is using the bracket notation, `string[]`, `number[]`, etc.:

```typescript
function trainJedi(jediKnights: string[]) {
  for (let knight of jediKnights) {
    console.log(`Training ${knight}...`);
  }
}

trainJedi(["Dooku", "Qui-Gon", "Xanatos"]);
// Training Dooku...
// Training Qui-Gon...
// Training Xanatos...
```

# Type Parameters

TypeScript offers an alternative way to declare arrays using type parameter syntax: `Array<T>`, which, for now, just know that it's basically the same as the "normal" `T[]` syntax. You'll see both versions in the wild.

These function declarations are the _same_:

```typescript
// Using bracket notation
function assignLightsaberColors(name: string, colors: string[]): void {
  // ...
}
// Using generic type parameter syntax
function assignLightsaberColors(name: string, colors: Array<string>): void {
  // ...
}
```

You can also use _either_ syntax when declaring variables:

```typescript
const colors: string[] = [
  "blue",
  "green",
  "purple",
  "red",
  "orange",
  "white",
  "darksaber",
];
const midichlorianCounts: Array<number> = [
  1000, 5000, 12000, 20000, 27000, 40000,
];
```

Later, when we talk about [generics](https://www.typescriptlang.org/docs/handbook/2/generics.html), it will make a bit more sense why you might use `Array<T>` over `T[]` - and the answer is mostly because it will feel _consistent_ with other generic types.

In the common case, I prefer `number[]` over `Array<number>`. It looks like an array (square brackets) and it's a bit faster to type.

# Heterogeneous Arrays

If you can do it in JavaScript, you can model it in TypeScript. It might not always be _pretty_... but in this case it is!

In languages like Go, you can't have an array that contains different types - at least not without using something a bit more complex like a struct or an interface. But in TypeScript, we can just [union](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) the types!

```typescript
// TypeScript infers the type as (string | number)[]
let lightsaberStyles = [1, 2, "double", "shoto"];

function describe(style: string | number): string {
  console.log(`Wield ${style} lightsaber`);
}

lightsaberStyles.forEach(describe);
// Wield 1 lightsaber
// Wield 2 lightsaber
// Wield double lightsaber
// Wield shoto lightsaber
```

Just use a pipe `|` to create union types. Easy!

# Rest Parameters

[Rest parameters](https://www.typescriptlang.org/docs/handbook/2/functions.html#rest-parameters) allow an indefinite number of final arguments, and brings them into the function body as an array. They're denoted by three dots (`...`) before the parameter name.

```typescript
function gatherParty(partyName: string, ...adventurers: string[]): string {
  return `${partyName} consists of: ${adventurers.join(", ")}`;
}

const msg = gatherParty("The Fellowship", "Frodo", "Sam", "Gandalf");
console.log(msg);
// "The Fellowship consists of: Frodo, Sam, Gandalf"
```

Don't confuse rest parameters with the similar but different [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).

You've used rest parameters before, maybe without even realizing! `console.log` accepts rest parameters.

# Evolving Any

When you create a new empty array, TypeScript infers it as `any[]`.

```typescript
let inventory = [];
// inventory: any[]
```

If you then push a type into it, TypeScript will infer the array as that type.

```typescript
inventory.push(42);
// inventory: number[]
```

Where it gets weird is that you're actually still allowed to push other types into the array, it just keeps updating the underlying type:

```ts
inventory.push("robe");
// inventory: (number | string)[]
```

This is so fascinating because if we had explicitly typed the array as `number[]`, we would have gotten an error when trying to push a string into it.

```typescript
let inventory: number[] = [];
inventory.push("robe");
// Error: Argument of type 'string' is not assignable to parameter of type 'number'
```

The "evolving any" is a special type inference feature. It's not very useful if you're trying to _restrict_ what can be pushed into an array within the initial scope, but it _is_ useful outside of that scope. Let me show you what I mean. Let's say I make a function like this:

```typescript
function getConfig() {
  let config = [];
  // config: any[]
  config.push("api-key");
  // config: string[]
  config.push(8080);
  // config: (string | number)[]
  return config;
}
```

Within `getConfig`, the array feels like `any`... I can just keep adding stuff. However, when I _use_ `getConfig`:

```typescript
let config = getConfig();
// config: (string | number)[]

config.push(false);
// Error: Argument of type 'boolean' is not assignable to parameter of type 'string | number'
```

_Now_ I get an error! The evolving any _stops_ evolving when it's passed around.

---

CH5: Objects

# Object Literal Types

Okay, I know I said unions were my favorite thing, _and that's true when comparing TS to Go_. But when it comes to the most useful "upgrade" from JavaScript to TypeScript, it's adding types to objects.

[Object literal types](https://www.typescriptlang.org/docs/handbook/2/objects.html) allow you to describe the shape of an object:

```typescript
function logSaiyan(saiyan: { name: string; power: number }) {
  console.log(`${saiyan.name} has power level: ${saiyan.power}!`);
  // ...
}
```

Or, more likely, you'll define the object type first:

```typescript
type Saiyan = {
  name: string;
  power: number;
};

function logSaiyan(saiyan: Saiyan) {
  console.log(`${saiyan.name} has power level: ${saiyan.power}!`);
  // ...
}
```

It's **so** nice to get a little red squiggly line in your editor when you misspell a property name in TypeScript! JavaScript won't fail until you run it...

# Extra Properties

_Most of the time_, when you pass an object to a function in TypeScript, it's:

- Okay to have _more_ properties than those defined in the function's parameter type
- Not okay to have _missing_ properties

However, when you pass an object _literal_ directly to a function, TypeScript performs what's called "excess property checking". Which means it _also_ will not allow extra properties.

For example, say we have this type:

```typescript
type Spaceship = {
  name: string;
  speed: number;
};
```

and we make an object with one extra property:

```ts
const falcon = {
  name: "Millennium Falcon",
  speed: 75,
  weapons: 4,
};
```

We can pass this object to a function that expects a `Spaceship`:

```typescript
function pilot(ship: Spaceship) {
  console.log(`Piloting ${ship.name} at ${ship.speed} light-years per hour`);
}

// this is fine
pilot(falcon);
```

But interestingly, if we pass in the same object _literal_ (no variable assignment), TypeScript will throw an error:

```typescript
// Error: Object literal may only specify known properties, and 'weapons' does not exist in type 'Spaceship'.
pilot({ name: "Millennium Falcon", speed: 75, weapons: 4 });
```

_It's also worth noting that many of these kinds of rules are configurable in the [`tsconfig.json`](https://www.typescriptlang.org/tsconfig) file, which we'll cover later. We'll mostly refer to default behavior in this course_.

# Optional Object Properties

The following is used [_way_ more often](https://www.youtube.com/shorts/ksBNx1vBm_0) than most of us would like, but it is incredibly useful. Optional properties can be added to an object type with the [`?`](https://www.typescriptlang.org/docs/handbook/2/objects.html#optional-properties) operator:

```typescript
type Superhero = {
  name: string;
  strength: number;
  cape?: boolean; // cape is optional
};
```

That means that the type of `.cape` is actually `boolean | undefined`, just like optional function parameters.

Do _not_ go overboard with optional props... require all the fields that _should_ be there! It will make your life easier with far fewer runtime checks that look like this:

```ts
function fight(superhero: Superhero) {
  if (!superhero.cape) {
    // contact edna mode
  }
  // do the happy path thing
}
```

# Empty Object Type

Say I innocently create a new empty object:

```typescript
let newUser = {};
```

Then go to add properties to it later:

```typescript
// Property 'name' does not exist on type '{}'
newUser.name = "Lane";
```

**TypeScript doesn't like that**!

It makes sense, we never told TypeScript which properties to allow... but here's what's really crazy: _this_ is actually allowed:

```typescript
let newUser = {};
newUser = "Lane";
```

_Yup_. You can reassign the variable, which initially held an empty object to a _string_. In fact, you can reassign it to anything except `null` or `undefined`, because everything else is technically an object! So, to get back to our first example, what you probably _want_ to do is just predefine the allowed field(s):

```typescript
type User = {
  name: string;
};

let newUser: User = {
  name: "Lane",
};
```

# Discriminated Unions

A union of two primitive types, like `string | number`, is really simple: it's a string or a number. The same is true of object types, but it can be tricky to know which type you're dealing with. That's where "discriminant properties" (or "tags") come in handy. It's just a property that tells you which type you're dealing with, and makes it easy to use conditional logic to handle each type. What is special about it is it can only be one value.

Unions of objects with a discriminant property are called ["discriminated unions"](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#discriminated-unions) or "tagged unions".

```typescript
type MultipleChoiceLesson = {
  kind: "multiple-choice"; // Discriminant property
  question: string;
  studentAnswer: string;
  correctAnswer: string;
};

type CodingLesson = {
  kind: "coding"; // Discriminant property
  studentCode: string;
  solutionCode: string;
};

type Lesson = MultipleChoiceLesson | CodingLesson;

function isCorrect(lesson: Lesson): boolean {
  switch (lesson.kind) {
    case "multiple-choice":
      return lesson.studentAnswer === lesson.correctAnswer;
    case "coding":
      return lesson.studentCode === lesson.solutionCode;
  }
}
```

Discriminated unions are **really** useful when you need to account for another new shape because TypeScript can ensure you handle all the possible cases. If we make these changes:

```typescript
type TrueFalseLesson = {
  kind: "true-false"; // Discriminant property
  question: string;
  studentAnswer: boolean;
  correctAnswer: boolean;
};

type Lesson = MultipleChoiceLesson | CodingLesson | TrueFalseLesson;
```

TypeScript will throw an error: `Function lacks ending return statement and return type does not include 'undefined'`. Which reminds us to add a third case:

```ts
function isCorrect(lesson: Lesson): boolean {
  switch (lesson.kind) {
    case "multiple-choice":
      return lesson.studentAnswer === lesson.correctAnswer;
    case "coding":
      return lesson.studentCode === lesson.solutionCode;
    case "true-false":
      return lesson.studentAnswer === lesson.correctAnswer;
  }
}
```

Do you have to use `kind` as the "tag"? Not _technically_. Should you use "kind"? Yes, follow the convention.

 # Sets

TypeScript has a built-in type for [sets](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), which are collections of unique values. You can ensure that all the values in the set are of the same type by specifying a type parameter: `<T>`.

```typescript
// A Set that contains only strings
const justiceLeague = new Set<string>();

justiceLeague.add("Green Arrow");
justiceLeague.add("Flash");

// Error: Argument of type '2' is not assignable to parameter of type 'string'
justiceLeague.add(2);
```

An array can be converted into a set, which automatically removes duplicate values:

```typescript
// A Set automatically removes duplicate values from an array
const names = ["plasticman", "firestorm", "plasticman"];
const justiceLeague = new Set<string>(names);

console.log(justiceLeague);
// Set { 'plasticman', 'firestorm' }
```

Sets also have a few other interesting methods and properties:

- [`delete()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/delete)
- [`has()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/has)
- [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/forEach)
- [`size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/size)

```typescript
const justiceLeague = new Set<string>(["Atom", "Black Canary", "Blue Beetle"]);

console.log(justiceLeague.size); // 3

justiceLeague.delete("Blue Beetle");
console.log(justiceLeague.has("Blue Beetle")); // false

justiceLeague.forEach((member) => console.log(member));
// Atom
// Black Canary
```

# Maps

TypeScript (obviously) also has a [built-in](https://en.wikipedia.org/wiki/Function_\(computer_programming\)#Built-in_function) for [maps](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), which are collections of key-value pairs. You can specify the types of the keys and values using type parameters `<K, V>`.

```typescript
// A Map with string keys and number values
const podracerSpeeds = new Map<string, number>();

podracerSpeeds.set("Anakin Skywalker", 947);
podracerSpeeds.set("Sebulba", 941);

podracerSpeeds.set("R2-D2", true);
// Error: Argument of type 'true' is not assignable to parameter of type 'number'

podracerSpeeds.set(420, 69);
// Error: Argument of type 'number' is not assignable to parameter of type 'string'
```

A map is a "set-like" object, and as such uses the [`size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size) property instead of `length`.

```typescript
console.log(podracerSpeeds.size);
// 2
```

How to easily iterate over a map:

```typescript
for (const [racer, speed] of podracerSpeeds) {
  console.log(`${racer} raced at ${speed} speed`);
}
// Anakin raced at 947 speed
// Sebulba raced at 941 speed
```

Here's the most important methods of a map, [`get`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/get), [`delete`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete), and [`has`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has).

```typescript
console.log(podracerSpeeds.get("Sebulba"));
// 941

console.log(podracerSpeeds.has("Sebulba"));
// true

podracerSpeeds.delete("Sebulba");
console.log(podracerSpeeds.get("Sebulba"));
// undefined
```

# Dynamic Keys

Sometimes, you won't know all of an object's property names in advance. For example, say you're building a customer management system where employees can add custom key/value pairs to customer records:

```
- favoriteColor: "blue"
- favoriteFood: "pizza"
- favoriteAnimal: "cat"
- etc
```

You can't know what the user will add ahead of time, but you still want to model the data in your program.

You can define [dynamic keys using an index signature](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures):

```typescript
type UserMetrics = {
  [key: string]: number;
};
```

This type says "this object can have any number of properties if the keys are strings and the values are numbers."

```ts
const metrics: UserMetrics = {
  wordsPerMinute: 50,
  errors: 2,
  timeOnPage: 120,
};

metrics["refreshRate"] = 60; // OK
metrics["theme"] = "dark"; // Error: Type 'string' is not assignable to type 'number'
```

# Dynamic Default Properties

So there's this (seemingly) weird but useful thing that you'll see in the wild:

```typescript
type FormData = {
  [field: string]: string;
  email: string;
  password: string;
};
```

If what you're concerned about is which types are _allowed_ in the object, you might wonder why `email` and `password` are even there. After all, you can specify _any_ string key/value pairs in this type, right?

**You use this syntax to _require_ certain properties**, in this case, `email` and `password`. The type above says:

> The object must have an `email` and `password` property, and it can have any number of additional string properties.

Here's another example:

```typescript
type FormData = {
  [field: string]: string | number | boolean;
  email: string;
  password: string;
  age: number;
};
```

This type says:

> The object must have an `email` (string), `password` (string), and `age` (number) property, but it can have any number of additional string, number, or boolean properties.

I'd strongly advise against overusing this pattern. Only use dynamic keys when you truly need _unknown_ keys. If you have optional keys, just use the `?` operator.

# PropertyKey

With dynamic property keys we've only used the `string` type so far, and most of the time, that's all you need. However, JavaScript also supports number and symbols as property keys. TypeScript actually has a **built-in type called [`PropertyKey`](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#handbook-content)** that represents all possible property key types:

```typescript
// this is a built-in type
type PropertyKey = string | number | symbol;
```

A [`symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) is a unique and immutable data type that may be used as an object property name. It's kinda like a `string`, but it's guaranteed to be unique.

So, instead of:

```ts
type InfrastructureTags = {
  [key: string]: any;
};
```

We can allow number and symbol keys (the JS default) like this:

```ts
type InfrastructureTags = {
  [key: PropertyKey]: any;
};

const janesServer: InfrastructureTags = {
  name: "Jane's Server",
  1: 420,
  [Symbol("role")]: "Admin",
};
```

To read or write a symbol-keyed property, use the symbol itself with bracket notation. Dot notation won't work.

```ts
const ROLE = Symbol("role");
const user = { [ROLE]: "Admin" };
user[ROLE]; // "Admin"
// user.ROLE; // undefined
```

# Readonly Modifier

The [`readonly` modifier](https://www.typescriptlang.org/docs/handbook/2/objects.html#readonly-properties) is _very_ similar to the `const` keyword in JavaScript. It's an added feature of TypeScript that lets us mark _object properties_ as read-only, meaning they can't be changed after initialization.

Normal object properties are fully mutable, but if we use `readonly`, we can make a property immutable:

```typescript
type Point = {
  readonly x: number;
  y: number;
};
```

Now we can create a new point like this:

```typescript
const point: Point = {
  x: 10,
  y: 20,
};
```

And we can update the `y` property just fine:

```typescript
point.y = 30; // OK
```

But if we try to update the `x` property, TypeScript will throw an error:

```typescript
// Error: Cannot assign to 'x' because it is a read-only property
point.x = 15;
```

`readonly` is pretty awesome. Just keep in mind that you probably want `readonly` properties _less often_ than you want `const` variables, because re-creating entire objects and copying all the fields can become painful and verbose if you do it too often.

# “As Const” and Object.freeze

The [`as const` assertion](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-inference) creates a readonly type using literal values:

```typescript
const colorsConst = ["red", "green", "blue"] as const;

// Error: Property 'push' does not exist on type 'readonly ["red", "green", "blue"]'
colorsConst.push("yellow");
```

It works great with objects too, and unlike most utility types and `Object.freeze()`, it automatically makes all nested structures `readonly` as well:

```typescript
const configConst = {
  apiUrl: "https://api.cobrakai.com",
  admins: {
    johnny: "lawrence",
    daniel: "larusso",
  },
  features: ["no mercy", "not crying", "winning too much"],
} as const;

// Error: Cannot assign to 'apiUrl' because it is a read-only property
configConst.apiUrl = "https://api.karate.com";

// Error: Cannot assign to 'johnny' because it is a read-only property
configConst.admins.johnny = "larusso";

// Error: Property 'push' does not exist on type 'readonly ["no mercy", "not crying", "winning too much"]'
configConst.features.push("sweep the leg");
```

## `Object.freeze()`

The [`Object.freeze()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) method is a built-in JavaScript function that prevents modifications to the top level of an object at _runtime_. It makes the object immutable, but it does not affect TypeScript's type system.

```typescript
const frozenConfig = Object.freeze({
  apiUrl: "https://api.cobrakai.com",
  admins: {
    johnny: "lawrence",
    daniel: "larusso",
  },
  features: ["no mercy", "not crying", "winning too much"],
});

// Error: Cannot assign to 'apiUrl' because it is a read-only property
frozenConfig.apiUrl = "https://api.karate.com";

// This is fine because nested properties are not frozen automatically
frozenConfig.admins.johnny = "kreese";

// This is also fine because the array is not frozen
frozenConfig.features.push("sweep the leg");
```

TypeScript is smart enough to recognize that `Object.freeze` is being called, so it gives us a nice compile-time error when we try to modify the top-level properties. And because `Object.freeze()` is a runtime operation, it will still fail at runtime if a mutation actually happens. It's worth mentioning that in [non-strict mode JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), mutations of a frozen object are silently ignored, but in strict mode, they throw a `TypeError`.

### Compile-time vs. Runtime

- **Compile-time** refers to when the TypeScript compiler (`tsc`) checks your code for type errors before it ever runs.
- **Runtime** refers to when the JavaScript code is actually executing in an environment like a browser or Node.js.

### `as const` (Compile-time)

The `as const` assertion is a signal specifically for the TypeScript compiler. It tells TypeScript to treat every value in the object or array as a literal type (e.g., the string `"Kreese"` instead of just any `string`) and marks every property as `readonly`.

- **Error behavior:** If you try to modify a property in your IDE or during a build, TypeScript will throw a red squiggly line and a compile-time error.
- **Limitation:** Once the code is compiled to JavaScript, `as const` completely disappears. It provides zero protection while the program is actually running.

### `Object.freeze()` (Runtime)

`Object.freeze()` is a standard JavaScript function. It physically locks the object in memory during execution.

- **Error behavior:** If you try to modify a frozen object at runtime in **strict mode**, JavaScript will throw a `TypeError`. In non-strict mode, it will fail silently.
- **TypeScript Integration:** TypeScript is smart enough to see you calling `Object.freeze()`, so it will _also_ provide compile-time errors.
- **Limitation:** Unlike `as const`, `Object.freeze()` is "shallow." If your object had a nested object inside it, that nested object would still be mutable at runtime unless you froze it too.

### Comparison Summary

|Feature|`as const`|`Object.freeze()`|
|---|---|---|
|**When it works**|Compile-time|Runtime (and Compile-time)|
|**Deep Immutability**|Yes (Recursive)|No (Shallow only)|
|**Emits JS code**|No (Erased)|Yes|
|**Primary Goal**|Strict type inference|Protecting data in memory|
# Satisfies

The [`satisfies` operator](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator) validates that a **value matches a specific type** without changing its inferred type. It solves a common pain point in TypeScript's type system. Before `satisfies`, you often had to choose between:

1. Letting TypeScript infer types (good for flexibility, but might miss errors)
2. Using explicit type annotations (good for catching errors, but loses narrowed type information)

Here's an example:

```typescript
// Using type inference (flexible but might miss errors)
const colors = {
  red: "#ff0000",
  green: "#00ff00",
  blue: 255, // same as hex "#0000ff"

  // "classic Lane-style typo" - Allan
  yelow: "#ffff00",
};
```

To get around this, we can create an explicit type, and use that to catch typos:

```ts
type ColorMap = {
  red: string | number;
  green: string | number;
  blue: string | number;
  yellow: string | number;
};

const colorsTyped: ColorMap = {
  red: "#ff0000",
  green: "#00ff00",
  blue: 255,
  // Error: "yelow" is not in type ColorMap
  yelow: "#ffff00",
};
```

The trouble is that because our `ColorMap` type uses `string | number` for the values, we lose the more specific type information:

```ts
// redHex is now 'string | number'
// where it used to be 'string'
type redHex = typeof colorsTyped.red;
```

This means that despite the value `colorsTyped.red` being a string, its `string | number` type will cause errors if you try to call string methods on it:

```ts
// Error: Property 'toUpperCase' does not exist on type 'string | number'
const redUpper = colorsTyped.red.toUpperCase();
```

The `satisfies` operator gives us the best of both worlds:

```typescript
type ColorMap = {
  red: string | number;
  green: string | number;
  blue: string | number;
  yellow: string | number;
};

const colorsSatisfies = {
  red: "#ff0000",
  green: "#00ff00",
  blue: 255,
  yellow: "#ffff00",
  // Error: "yelow" is not in type ColorMap
  // yelow: "#ffff00"
} satisfies ColorMap;

// We keep the narrowed types!
type RedHexSatisfies = typeof colorsSatisfies.red;
const redUpper = colorsSatisfies.red.toUpperCase(); // "#FF0000"
```

# Function Overloads

JavaScript is very lenient when it comes to function signatures, and TypeScript gives us a way to take advantage of that flexibility while still maintaining type safety: [function overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads).

First, we define a function that can be called in multiple ways:

```typescript
function formatEmployeeMessage(
  employee: Employee,
  isNew?: boolean,
  onBoardedDate?: Date,
): string {
  if (!isNew) {
    return `Employee: ${employee.name}, Dept: ${employee.dept}`;
  }
  return `Employee: ${employee.name}, New: Yes, Onboarded: ${onBoardedDate}`;
}

type Employee = {
  name: string;
  dept: string;
};
```

Used as-is, this function can be called in 3 different ways:

- `formatEmployeeMessage(employee)`
- `formatEmployeeMessage(employee, boolean)`
- `formatEmployeeMessage(employee, boolean, Date)`

But we can constrain the function to only allow certain combinations of parameters by using function overloads.

```typescript
// note: function overloads need to be declared above the implementation
function formatEmployeeMessage(employee: Employee): string;
function formatEmployeeMessage(
  employee: Employee,
  isNew: true,
  onBoardedDate: Date,
): string;
```

Now, it's impossible to call `formatEmployeeMessage(employee, boolean)` without _also_ passing in a date. Basically we're saying, "If the employee is new, you must _also_ pass in a date". This works:

```ts
const employee: Employee = { name: "Joe Exotic", dept: "Zoo" };
const msg = formatEmployeeMessage(employee);
console.log(msg);
// Employee: Joe Exotic, Dept: Zoo
```

We can also do this:

```ts
const employee: Employee = { name: "Carole Baskin", dept: "Big Cat Rescue" };
const msg = formatEmployeeMessage(employee, true, new Date());
console.log(msg);
// Employee: Carole Baskin, New: Yes, Onboarded: 2023-10-01T00:00:00.000Z
```

But this will throw an error:

```ts
const employee: Employee = { name: "Dillon Passage", dept: "Zoo" };
// Error: No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
const msg = formatEmployeeMessage(employee, true);
```

---

 CH6: Tuples

# Tuples

A [**tuple**](https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types) is a special kind of array where each position has a **specific, known type**.

```typescript
const nameAndAge: [string, number] = ["Rose Tyler", 24];
```

The existence of tuples in TypeScript has me using them where I _never_ would have used an array in JavaScript. The fact that the length is fixed and the type of index is known makes them _much_ more safe to use for small collections.

## Be Explicit With Tuples

You need to provide explicit typing with tuples! This is a tuple:

```ts
// [string, number]
const nameAndAge: [string, number] = ["John Jones", 104];
```

But if we remove the type, it's inferred as an array of `string | number`:

```ts
// (string | number)[]
const nameAndAge = ["Martha Jones", 24];
```

With a `(string | number)[]` you can do this:

```ts
const nameAndAge = ["Martha Jones", 24];
nameAndAge[1] = "Donna Noble";
```

But with a tuple, TypeScript will provide an error (which is probably what you want). So, always explicitly type your tuples!

```ts
const nameAndAge: [string, number] = ["Martha Jones", 24];
// Error: Type 'string' is not assignable to type 'number'.
nameAndAge[1] = "Donna Noble";
```

# Readonly

Tuples in TypeScript are (guh) still arrays under the hood, so counterintuitively you _can_ still push to them and pop from them. This is a bit of a gotcha, tuples in most languages are fixed length.

Getting out-immutable'd by Python is a sad state of affairs.

```ts
const nameAndAge: [string, number] = ["Martha Jones", 24];
nameAndAge.push("Donna Noble");
```

So you still need to be careful about underlying array length... that is, unless you use [`readonly`](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#readonly-and-const) tuples, which is really the only way I use tuples.

```ts
const nameAndAge: readonly [string, number] = ["Martha Jones", 24];
// Error: Property 'push' does not exist on type 'readonly [string, number]'.
nameAndAge.push("Donna Noble");
```

Much better! I use `readonly` any time I possibly can, it's kinda like using `const` over `let` whenever possible. However, keep in mind that `readonly` is TypeScript specific, which means it's enforced at compile time, but not at runtime (like `const` is).

# Tuples vs. Objects

So why would you have to use a tuple instead of an object? For example, this:

```ts
function getCoordinates(): [number, number] {
  return [40.7128, -74.006]; // latitude, longitude
}
```

Instead of this:

```ts
function getCoordinatesAsObject(): { lat: number; lng: number } {
  return { lat: 40.7128, lng: -74.006 };
}
```

Coordinates have a conventional order (latitude, then longitude), so a tuple's positional semantics fit well. Objects are clearer for named access (`coords.lat`).

## Counterexample

I would probably model a `User` as an object:

```ts
type User = { name: string; age: number; email: string };
const user: User = { age: 60, name: "Lane", email: "super@secret.com" };
```

A tuple like `[string, number, string]` would be confusing – `user[0]` for name? `user[2]` for email? Objects' descriptive keys are more intuitive.

# Destructuring Tuples

Sometimes tuples are also useful when you want to return multiple values from a function (which is impossible in JS/TS), but you don't want to create a new object type just to do so. A tuple, along with destructuring, is a handy way to return "positional" data.

```typescript
function getName(fullName: string): [string, string] {
  const parts = fullName.split(" ");
  return [parts[0], parts[1]];
}

const [firstName, lastName] = getName("Frodo Baggins");
```

## Nested Destructuring

There's nothing stopping you from destructuring nested tuples and objects all at once. Use this example to answer the question:

```typescript
type UserWithAddress = [string, { city: string; country: string }];

const userData: UserWithAddress = [
  "Aragorn",
  { city: "Minas Tirith", country: "Gondor" },
];

const [userName, { city, country }] = userData;
console.log(city);
// ?
```

# Named Tuples

To be fair, position-based access isn't very descriptive. Luckily, you can **label tuple elements** (sometimes called "named tuples"). So, instead of this:

```typescript
type UserData = [string, number, boolean];
```

We can do this:

```ts
type UserDataLabeled = [name: string, age: number, isAdmin: boolean];
```

Labels make your code more "self-documenting".

You might hear people say "there's no such thing as self-documenting code". Those people are just mad because they write terrible code. If you name things well and keep things simple, you'll still need comments _occasionally_, but you won't need them as _often_.

When you hover over a variable in your editor, you'll see names instead of just positions:

```typescript
// Your editor shows the full type:
// [name: string, age: number, isAdmin: boolean]
function getUser(): UserDataLabeled {
  return ["Frodo", 33, false];
}
```

## Labels Are Just Documentation

The labels are quite literally just names for the TypeScript tooling, they don't change how the values are accessed. Say I have a named tuple like this:

```typescript
const user: [name: string, age: number] = ["Bilbo", 111];
```

And then I try to destructure in reverse order:

```typescript
const [age, name] = user;
console.log(age); // "Bilbo"
console.log(name); // 111
```

The variable names I choose when destructuring _don't matter_: **only the positions do**.

# Optional Elements in Tuples

Like object properties, you can make tuple elements optional using the `?` modifier:

```typescript
type HttpResponse = [statusCode: number, data: string, error?: string];

// Both of these work!
const successResponse: HttpResponse = [200, "Success!"];
const errorResponse: HttpResponse = [404, "", "Resource not found"];
```

## Optional Values Are Last

Similar to optional function parameters, all required elements must come before optional elements. This does _not_ work:

```ts
type HttpResponse = [statusCode: number, data?: string, error: string];
```

But this does:

```ts
type HttpResponse = [statusCode: number, data?: string, error?: string];
```

## Optional Types Are Potentially Undefined

All optional elements are automatically unioned with `undefined`.

```typescript
type UserInfo = [name: string, age: number, address?: string];

function handleUserInfo(user: UserInfo) {
  const [name, age, address] = user;
  // name: string
  // age: number
  // address: string | undefined
}
```

Personally when I have a bunch of optional properties, I prefer to just use an object type most of the time. I'm less worried about length checks and such with objects.

# Tuple Rest Elements

TypeScript allows tuples to have a variable number of elements of a specific type using **rest elements**. This is nice when you want a tuple to have a fixed-length beginning but a flexible-length ending:

```typescript
// A tuple with a rest element
type NameAndScores = [string, ...number[]];

// All of these are valid
const nameAndScores: NameAndScores = ["Alphonse", 69, 420, 300];
const nameAndScores: NameAndScores = ["Winry", 42];
const nameAndScores: NameAndScores = ["Edward"];
```

This idea of flexibly sized tuples honestly barely feel like tuples to me... it feels like arrays with some type constraints... but I digress.

One great use case for rest elements would be to model a command line argument pattern:

```typescript
type Command = [name: string, ...args: string[]];

const gitCommit: Command = ["git", "commit", "-m", "Add new feature"];
const npmInstall: Command = ["npm", "install", "typescript"];

// Function that handles commands
function executeCommand([cmd, ...args]: Command) {
  console.log(`Executing ${cmd} with arguments: ${args.join(", ")}`);
}
```

It says "I _need_ a command string, but everything after that is optional". Pretty neat. Remember, the whole point of a great type system is to more accurately (and narrowly) model the valid states of your program.

---

CH7: Intersections

# Intersections of Types

An [intersection type](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types) combines multiple types into one with the `&` operator. The resulting intersection type satisfies **all** the component types simultaneously.

```typescript
type IndividualContributor = {
  id: number;
  name: string;
  tasks: string[];
};

type Manager = {
  directReports: number[];
};

type GoodManager = IndividualContributor & Manager;

const hunter: GoodManager = {
  id: 1,
  name: "Hunter Backmann",
  tasks: ["Fixing Lane's B*llsh*t code", "Vibe Coding"],
  directReports: [2, 3, 4],
};
```

A `GoodManager` must have all the properties of both an `IndividualContributor` and a `Manager`. When you intersect object types, TypeScript merges their properties:

```typescript
type Point2D = {
  x: number;
  y: number;
};

type Point3D = Point2D & {
  z: number;
};

// Equivalent to:
// type Point3D = {
//   x: number;
//   y: number;
//   z: number;
// };
```

# The Never Type

In TypeScript, the [`never`](https://www.typescriptlang.org/docs/handbook/2/functions.html#never) type represents values that _can't_ occur... sounds useless, right?

**Well, it's not**. Take a look at this function that _should_ handle 3 cases:

```ts
function handleStatusCode(code: 200 | 404 | 500) {
  if (code === 200) {
    console.log("OK");
    return;
  }
  if (code === 404) {
    console.log("Not Found");
    return;
  }
  throw new Error(`Unknown status code: ${code}`);
}
```

But it only handles `200` and `404`! TypeScript isn't throwing any compiler errors, but we can configure it to do so! See, after each conditional, the type of `code` is narrowed down:

```ts
function handleStatusCode(code: 200 | 404 | 500) {
  if (code === 200) {
    console.log("OK");
    return;
  }
  // code is now 404 | 500
  if (code === 404) {
    console.log("Not Found");
    return;
  }
  // code is now 500
  throw new Error(`Unknown status code: ${code}`);
}
```

If we assign `code` to `never`, TypeScript will complain unless `code` has actually been narrowed down to _no possible values_.

```ts
function handleStatusCode(code: 200 | 404 | 500) {
  if (code === 200) {
    console.log("OK");
    return;
  }
  if (code === 404) {
    console.log("Not Found");
    return;
  }
  // Type '500' is not assignable to type 'never'.
  const err: never = code;
  return err;
}
```

And now it's fixed by simply handling every case properly:

```ts
function handleStatusCode(code: 200 | 404 | 500) {
  if (code === 200) {
    console.log("OK");
    return;
  }
  if (code === 404) {
    console.log("Not Found");
    return;
  }
  if (code === 500) {
    console.log("Internal Server Error");
    return;
  }
  // no errors! code is never
  const err: never = code;
  return err;
}
```

# Intersecting Incompatible Types

What happens when we intersect types with overlapping properties?

```ts
type Saiyan = {
  name: string;
  powerLevel: number;
};

type Human = {
  name: string;
  age: number;
};

type SaiyanHuman = Saiyan & Human;
```

We get this `SaiyanHuman` type that's the equivalent of:

```ts
type SaiyanHuman = {
  name: string;
  powerLevel: number;
  age: number;
};
```

It merges the properties of both `Saiyan` and `Human`, and because `name` overlaps, it safely combines the two types and appears once in the resulting type.

## When Things Go Wrong

What happens if the `name` field were incompatible types? For example:

```ts
type Saiyan = {
  name: "goku" | "vegeta";
  powerLevel: number;
};

type Human = {
  name: "krillin" | "yamcha";
  age: number;
};

type SaiyanHuman = Saiyan & Human;
```

Now the `name` property can't possibly satisfy both! Humans must be `krillin` or `yamcha`, and Saiyans must be `goku` or `vegeta`. So, the `name` property in `SaiyanHuman` becomes `never`, which in turn makes the _entire_ `SaiyanHuman` type `never`.

```ts
// Type '{}' is not assignable to type 'never'
const theLaneagen: SaiyanHuman = {};
```

It's TypeScript saying, "Hey, the SaiyanHuman type is impossible, do something else." Most of the time, the solution here is to redesign your types to avoid incompatible intersections and _make sense_, like either nest the types inside the new type of modify the variables.

# Intersections vs. Unions

So we've covered how [unions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) (`|`) and [intersections](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types) (`&`) are both used to smoosh types together... but which should you use?

## Unions

- Use the `|` operator (suspiciously similar to the logical OR operator)
- Widen the resulting type (more possible values)
- Useful for modeling mutually exclusive options or states

## Intersections

- Use the `&` operator (like logical AND)
- Narrow the resulting type (fewer possible values)
- Useful for combining multiple constraints or adding _more_ required properties to existing types

```ts
type Human = {
  name: string;
  age: number;
};

type Elf = {
  name: string;
  ears: "pointy";
};

// Must have name, age, and pointy ears
type ElfHuman = Human & Elf;

// Must have name (shared between Human and Elf)
// Can have either age or ears (or both)
type ElfOrHuman = Human | Elf;
```

- Use unions to say your type is "this OR that"
- Use intersections to say your type is "this AND that", or sometimes more simply, "this with the additional properties of that"

# Super Set Unions

So we know we can drastically narrow a primitive type like "number" by using a union of literal types. For example, maybe only 3 error codes are valid:

```typescript
type ErrorSlugs = "OK" | "NOT_FOUND" | "INTERNAL_ERROR";
```

This works great if these are the only valid error codes, but what if:

1. Any string can be used as an error slug
2. "OK", "NOT_FOUND", and "INTERNAL_ERROR" are the most common values and we like to have them show up in autocomplete

TypeScript has a hacky way for us to express this: super set unions.

```typescript
type ErrorCodes = "OK" | "NOT_FOUND" | "INTERNAL_ERROR" | (string & {});
```

You might be wondering,

> "Why wouldn't I just use `string` - the set of allowed values is the same?"

And you're right, but there's one subtle difference. By adding `(string & {})`, TypeScript won't change which values are _allowed_. Any string is _allowed_. But it will _still give us autocomplete_ in our editor for the values "OK", "NOT_FOUND", and "INTERNAL_ERROR".

Normally, if you define a type like this:

```typescript
type Status = "employed" | "unemployed" | string;
```

TypeScript looks at that and thinks: "Well, since `'employed'` and `'unemployed'` are both strings, they are already included in the set of all strings." It then "collapses" the type down to just `string`.

When the type collapses to `string`, your IDE (like VS Code) loses the specific literal values, and you lose that helpful autocomplete for your "default" options.

### The Solution: The Intersection Hack

By using `(string & {})`, we are technically creating an **Intersection Type**.

1. `string` represents all string values.
2. `{}` represents any non-nullish value (anything that isn't `null` or `undefined`).

When you intersect them, the result is still effectively just a string. However, TypeScript sees `(string & {})` as a distinct "type" from the primitive `string`. Because they are technically different types in the eyes of the compiler's optimization logic, it prevents the union from collapsing.

### The Result

Because the union doesn't collapse, the TypeScript Language Service keeps the specific literals (`"employed"`, etc.) separate in its memory.

- **Validation:** Any string is still accepted because `(string & {})` matches any string.
- **Developer Experience:** When you start typing, the IDE sees the literal strings as "special" members of the union and suggests them in the autocomplete dropdown.

It's a "hack" because we aren't actually changing the logic of what a string is; we're just tricking the compiler into keeping our autocomplete suggestions alive!

---

CH8: Interfaces

# Interfaces

As it turns out, there are two ways to define object types: the `type` keyword (as we've seen with type aliases) and `interface`s:

```ts
type Superhero = {
  name: string;
  powers: string[];
  isAvenger: boolean;
};

interface Superhero {
  name: string;
  powers: string[];
  isAvenger: boolean;
}
```

I'm a huuuuge fan of having multiple ways to do the same things in a language... /s

In 9/10 scenarios, they work the same way, but there are a few key differences that we'll cover in this chapter. For now, just know that I recommend using `type` in [most cases](https://www.typescriptlang.org/docs/handbook/2/objects.html#interface-extension-vs-intersection), but there are a few scenarios where `interface` is the better choice, which we'll talk about later.

# Extending Interfaces

This is the exception to my previous rule of thumb that "you should prefer `type` over `interface`". Interfaces are a bit better when it comes to extending other interfaces (inheriting properties).

With types, you use the `&` (intersection) operator to extend types:

```typescript
type Character = {
  name: string;
  level: number;
};

type Wizard = Character & {
  spellbook: string[];
  mana: number;
};
```

With interfaces, you use the `extends` keyword:

```typescript
interface Character {
  name: string;
  level: number;
}

interface Wizard extends Character {
  spellbook: string[];
  mana: number;
}
```

In both cases, a `Wizard` now has all four properties: `name`, `level`, `spellbook`, and `mana`.

## Why Is “Interface Extends” Usually Better?

To quote [Microsoft's wiki](https://github.com/microsoft/TypeScript/wiki/Performance#preferring-interfaces-over-intersections):

> Interfaces create a single flat object type that detects property conflicts, which are usually important to resolve! Intersections on the other hand just recursively merge properties, and in some cases produce `never`. Interfaces also display consistently better, whereas type aliases to intersections can't be displayed in part of other intersections. Type relationships between interfaces are also cached, as opposed to intersection types as a whole. A final noteworthy difference is that when checking against a target intersection type, every constituent is checked before checking against the "effective"/"flattened" type.
> 
> For this reason, extending types with interfaces/extends is suggested over creating intersection types.

Put simply, **with interfaces the developer ergonomics are a bit better and compilation is a bit faster**.

# Extending Multiple Interfaces

You can extend multiple interfaces at once:

```typescript
type Character = {
  name: string;
  level: number;
};

interface Magical {
  mana: number;
  castSpell(spell: string): void;
}

interface Physical {
  strength: number;
  attack(): void;
}

interface BattleMage extends Character, Magical, Physical {
  combineAttacks(): void;
}
```

`BattleMage` now has all 7 properties and methods:

- `name`
- `level`
- `mana`
- `castSpell`
- `strength`
- `attack`
- `combineAttacks`

# Overriding Interface Properties

You can override properties from the base interface, but the new type must be compatible with the original:

```typescript
interface Character {
  rank: string | number;
  name: string;
  level: number;
}

interface Wizard extends Character {
  // Wizards only have a number rank
  // This is allowed because
  // `number` is assignable to `string | number`
  rank: number;
  mana: number;
}
```

But you can't change to an incompatible type:

```typescript
interface Character {
  rank: string;
  name: string;
  level: number;
}

interface Wizard extends Character {
  // This breaks because `number` is
  // not assignable to `string`
  rank: number;
  mana: number;
}
```

# Declaration Merging

Okay, so here's the quirk that makes me recommend `type` over `interface` in _most_ cases. Declaration merging is, in my experience, mostly a footgun. Sure, it's useful in certain niche cases (like modifying the global `Window` type in front-end code), but most of the time it leads to confusing bugs.

When you declare the same interface (use the same name) multiple times, all the declarations are _merged_. This:

```typescript
interface Spaceship {
  name: string;
}

interface Spaceship {
  engines: number;
}

interface Spaceship {
  lightSpeed: boolean;
}
```

Is the same as this:

```typescript
interface Spaceship {
  name: string;
  engines: number;
  lightSpeed: boolean;
}
```

If you use the `type` keyword instead, you'll get an error that you can't redeclare the type (which is probably what you want):

```typescript
type Spaceship = {
  name: string;
};

// Duplicate identifier 'Spaceship'
type Spaceship = {
  engines: number;
};
```

---

CH9: Enums

# Enums

[Enums](https://www.typescriptlang.org/docs/handbook/enums.html) are a set of defined constants. The simplest form of enum is a numeric enum:

```typescript
enum Direction {
  North, // 0
  East, // 1
  South, // 2
  West, // 3
}

let myDirection: Direction = Direction.North;
console.log(myDirection); // Outputs: 0
```

The killer feature of enums is that in your code you can have nicely named identifiers like `Direction.North`, and under the hood you can have simple unique values, like `0`. Typescript automatically increments the underlying values for us as we define new enums.

You can also explicitly set the values, and TypeScript will ensure they're unique:

```typescript
enum StatusCode {
  OK = 200,
  Created = 201,
  BadRequest = 400,
  Unauthorized = 401,
  NotFound = 404,
}
```

## Bidirectional Mapping

Numeric enums are bidirectional, which just means you can easily convert from the underlying value to the name and vice versa:

```typescript
const directionValue: number = Direction.South;
// 2
const directionName: string = Direction[directionValue];
// "South"
```

# String Enums

Numeric enums can be nice when:

- You actually want numbers
- You really want to eke out every last bit of performance (numbers use less memory than strings)

But often, string enums are easier to work with if you _just_ want labels.

```typescript
enum LogLevel {
  ERROR = "ERROR",
  WARN = "WARN",
  INFO = "INFO",
  DEBUG = "DEBUG",
}

function structuredLog(message: string, level: LogLevel) {
  console.log(`[${level}] ${message}`);
}

structuredLog("User not found", LogLevel.ERROR);
// Outputs: [ERROR] User not found
```

When enums only exist within your code, numeric enums are totally fine. They start to get _really_ hairy when you need to serialize them to JSON or store them in a database. There's nothing worse than debugging API responses and seeing this:

```json
{
  "id": "94e83b65-ae9c-47f4-b788-d3f4fd085067",
  "name": "Lane",
  "user_type": 7 // what the h*ck is 7?!?!?
}
```

# Enum Compilation

Unlike most TypeScript features, enums generate additional JavaScript code at runtime. Let's see what happens when we compile this enum (we'll talk about how to compile manually later):

```typescript
enum Class {
  Rogue,
  Mage,
  Warrior,
  Priest,
}
```

We get a JavaScript object that looks like this:

```javascript
var Class;
(function (Class) {
  Class[(Class["Rogue"] = 0)] = "Rogue";
  Class[(Class["Mage"] = 1)] = "Mage";
  Class[(Class["Warrior"] = 2)] = "Warrior";
  Class[(Class["Priest"] = 3)] = "Priest";
})(Class || (Class = {}));
```

This generated code creates the bidirectional mapping that we talked about before:

- From name to value: `Class["Rogue"] = 0`
- From value to name: `Class[0] = "Rogue"`

## String Enum Compilation

Strings compile in a _similar_ way:

```typescript
enum Class {
  Rogue = "Rogue",
  Mage = "Mage",
  Warrior = "Warrior",
  Priest = "Priest",
}
```

Compiles to:

```javascript
var Class;
(function (Class) {
  Class["Rogue"] = "Rogue";
  Class["Mage"] = "Mage";
  Class["Warrior"] = "Warrior";
  Class["Priest"] = "Priest";
})(Class || (Class = {}));
```

String enums do _not_ support reverse mapping - the compiled JavaScript only maps from name to string value, not the other way around.

# Const Enums

There's a special variant of enums, [`const enums`](https://www.typescriptlang.org/docs/handbook/enums.html#const-enums), which are completely removed during compilation and replaced with their literal values. Unlike regular enums, they don't ship extra mapping code.

```typescript
const enum Direction {
  North = "NORTH",
  East = "EAST",
  South = "SOUTH",
  West = "WEST",
}

const whereWinterComesFrom = Direction.North;
```

Const enums are more performant, but do come with some limitations:

1. **No computed values**: They can reference other enum members, but can't use arbitrary expressions.

```typescript
const enum FavoriteActor {
  BradPitt = "Brad Pitt",
  AngelinaJolie = "Angelina Jolie",
  // this is okay, it references enum members
  BestCouple = FavoriteActor.BradPitt + " and " + FavoriteActor.AngelinaJolie,
}

const enum FavoriteActor {
  BradPitt = "Brad Pitt",
  AngelinaJolie = "Angelina Jolie",
  // this is not okay
  // const enum member initializers must be constant expressions
  BestCouple = getBestCouple(),
}
```

2. **Mapping issues**: Const enums don't have runtime representation, so getting the name from the number isn't possible.

```typescript
const enum Direction {
  North, // 0
  East, // 1
  South, // 2
  West, // 3
}

const directionValue = Direction.West;

// This errors:
// A const enum member can only be accessed using a string literal.(2476)
const directionName = Direction[directionValue];

// and if you do use a string literal, it just returns the value again
const directionValueAgain = Direction["West"];
// 3
```

I'd only use `const enums` when I'm really concerned about performance and bundle size.

# Enums vs. Union Types

If you've been paying attention, you might be wondering "Why would I ever do this":

```typescript
enum CardSuit {
  Hearts = "Hearts",
  Diamonds = "Diamonds",
  Clubs = "Clubs",
  Spades = "Spades",
}
```

When you could do this:

```typescript
type CardSuit = "Hearts" | "Diamonds" | "Clubs" | "Spades";
```

## Pros of Unions

- Unions are what you use for complex types, so it feels consistent to use them for primitives as well
- Unions don't add any additional runtime code
- It's less verbose to write a union

## Pros of Enums

- Enums are slightly easier to refactor because if you change the value of a label (e.g. "Hearts" to "hearts"), you don't have to change the string literal in every place you use it.
- If you're using numerical enums, then the reverse mapping can be useful _I guess_.
- `CardSuit.Hearts` provides more context than just `"Hearts"`. That said, any good editor is going to say `type CardSuit` on hover, so it's not a _huge_ win.

Personally, I use unions over enums pretty much every time. In fact, Anders Hejlsberg (the creator of TypeScript) [has said](https://www.youtube.com/watch?v=vBJF0cJ_3G0&t=1012s) that they might not even add enums to TypeScript if they were starting over.

---

CH10: Type Narrowing
# Narrowing Types

[**Type narrowing**](https://www.typescriptlang.org/docs/handbook/2/narrowing.html) is the simple process of making a type more and more specific as you write your code. As a _general rule_ (don't abuse it, for the love...) the more specific your types, the better. With narrower types:

- Your editor tooling will be more helpful
- Your code will self-document much better
- You'll catch more errors at compile time.

## Conditional Narrowing

One of the coolest features of TypeScript is how smart it is about recognizing how types are being narrowed in "regular" code. For example:

```typescript
type WitcherCharacter = {
  type: "witcher";
  name: string;
  magicPower: boolean;
};

type StarWarsCharacter = {
  type: "star-wars";
  name: string;
  forceSensitive: boolean;
};

type Character = WitcherCharacter | StarWarsCharacter;

function fight(player1: Character, player2: Character) {
  if (player1.type === "witcher" && player2.type === "witcher") {
    // I don't need to type cast (convert)
    // player1 and player2 to WitcherCharacter - TypeScript
    // does that automatically because this branch of the
    // conditional narrows the type
    fightWitcher(player1, player2);
  } else if (player1.type === "star-wars" && player2.type === "star-wars") {
    // same thing here
    fightStarWars(player1, player2);
  } else {
    throw new Error("Can't fight characters from different universes");
  }
}

function fightWitcher(player1: WitcherCharacter, player2: WitcherCharacter) {
  // witcher specific logic
}

function fightStarWars(player1: StarWarsCharacter, player2: StarWarsCharacter) {
  // star wars specific logic
}
```

# Unknown Type

We've talked about the [`any`](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) type in TypeScript, and how it can represent anything - it's the "widest" type.

The [`unknown`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-0.html#new-unknown-top-type) type can be used for similar purposes, but it's a much safer alternative because it _forces_ you to explicitly assert the type before using it in a specific way.

## The “any” Problem

The `any` type basically turns off TypeScript's type checking:

```typescript
function processData(data: any) {
  // TypeScript allows this even though it might crash
  console.log(data.toLowerCase());

  // TypeScript allows this too - it's like we're using plain JavaScript
  return data.nonExistentMethod();
}

// No errors when calling the function
processData(42); // Will crash at runtime
```

As we talked about earlier, when you take plain JavaScript code and run it through TypeScript tooling, almost everything is `any` by default.

## The “unknown” Solution

The `unknown` type doesn't allow that kind of tomfoolery:

```typescript
function processData(data: unknown) {
  // Error: Object is of type 'unknown'
  console.log(data.toLowerCase());

  // Error: Object is of type 'unknown'
  return data.nonExistentMethod();
}
```

With `unknown`, you can still assign any value to it (e.g. call this function with any value), but you can't _use_ that value in a meaningful way without first checking its type:

```typescript
function processData(data: unknown) {
  // We do a type assertion
  if (typeof data === "string") {
    // Now TypeScript knows data is a string
    console.log(data.toLowerCase());
    return data;
  }
  if (typeof data === "number") {
    // Now TypeScript knows data is a number
    return data * 2;
  }

  // Throw an error for other types
  // that we can't handle
  throw new Error("Expected string data");
}
```

## When to Use `unknown`

Unknown is a _fantastic_ alternative to `any` when it comes to dealing with values that are coming into your program from the outside world (e.g. user input, API responses, etc.). It forces you to add type checks at that I/O boundary so that you can then be confident working with the data _inside_ your program.

# Type Hierarchy

All the types in TypeScript can be arranged into a hierarchy of sorts. I've drawn one below:

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/X3YIzJS-1086x720.png)

Obviously I didn't list every type, but the goal here is to show you how narrowing works in TypeScript, and _why_ you should care. Some key points:

- The types at the top of hierarchy are the "widest". They encompass the most possible values, and _very little_ is known about them.
- The types at the bottom of the hierarchy are the "narrowest". They encompass the fewest possible values, and _a lot_ is known about them.
- `any` and `unknown` are at the top of the hierarchy, the weird one is actually the `any` type, because it just breaks all the rules, allowing you to do whatever you want with it.
- `never` is at the bottom of the hierarchy, because it represents values that _can't_ occur.
- Types below are assignable to the connected types above them, but not the other way around. For example:
    - `"armin"` is assignable to `"armin" | "eren"` which is assignable to `string` which is assignable to `any`.
    - `"armin" | "eren"` is _not_ assignable to `"armin"` because what would happen if the value happened to be `"eren"`?
- Interestingly, `undefined` and `null` are assignable to `void` (at least, when `strictNullChecks` is disabled), but not the other way around.

# Narrowing Using In

The [`in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in) operator checks if a property exists in an object, which is fantastic for type narrowing in object literals.

```typescript
type TextMessage = {
  content: string;
  sentAt: Date;
};

type ImageMessage = {
  caption: string;
  sentAt: Date;
};

type VideoMessage = {
  duration: number;
  sentAt: Date;
};

type Message = TextMessage | ImageMessage | VideoMessage;

function displayMessage(message: Message) {
  if ("content" in message) {
    // TypeScript knows this is a TextMessage
    // because it's the only one with a 'content' property
    console.log(`Text content is: ${message.content}`);
  } else if ("caption" in message) {
    // TypeScript knows this is an ImageMessage
    // because it's the only one with an 'caption' property
    console.log(`Image caption is ${message.caption}`);
  } else {
    // TypeScript knows this is a VideoMessage because
    // it's the only other option
    console.log(`Video length is ${message.duration}`);
  }
}
```

## Discriminated Unions vs. 'in' Checks

You might have noticed that this kind of logic feels _very_ similar to using discriminated unions, and you're correct. Here's the same types with an explicit discriminant property:

```typescript
type TextMessage = {
  kind: "text";
  content: string;
  sentAt: Date;
};

type ImageMessage = {
  kind: "image";
  caption: string;
  sentAt: Date;
};

type VideoMessage = {
  kind: "video";
  duration: number;
  sentAt: Date;
};
```

My recommendation is to prefer a discriminated union when you have full control of the types, but if you're using types from a library or package, or have another reason you don't want extra properties, the `in` operator is a great alternative.

 # Type Predicates

Sometimes the built-in type guards ([`typeof`](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html), [`instanceof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof), etc.) aren't enough.

TypeScript allows you to create your own type guards using [**type predicates**](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates). We do that by creating a function that:

- Accepts a wide type that we want to narrow
- Returns a boolean indicating if the value is of the desired type
- Uses the type predicate syntax `value is Type` in the return type

For example, here's a function that reports if a value is a string:

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function processValue(value: unknown) {
  if (isString(value)) {
    // TypeScript knows value is a string here
    console.log(value.toUpperCase());
  }
}
```

For simple stuff like this, we could have just inlined the `typeof` check:

```ts
function processValue(value: unknown) {
  if (typeof value === "string") {
    // TypeScript knows value is a string here
    console.log(value.toUpperCase());
  }
}
```

But type predicates become really useful when the logic to check the type is a bit more complex. So we have a situation where one type, in this case, the `ManagerAdmin` type, shares properties with both other types:

```typescript
interface ManagerAdmin {
  accessLevel: number;
  numEmployees: number;
}

interface Admin {
  accessLevel: number;
  payrollDate: Date;
}

interface Manager {
  numEmployees: number;
}
```

We can encapsulate the slightly more complex logic in a type predicate function:

```typescript
function isManagerAdmin(
  boss: ManagerAdmin | Admin | Manager,
): boss is ManagerAdmin {
  return "numEmployees" in boss && "accessLevel" in boss;
}
```

```ts
// boss is a `ManagerAdmin | Admin | Manager`
if (isManagerAdmin(boss)) {
  // TypeScript knows boss is a ManagerAdmin here
  console.log(`Managing ${boss.numEmployees} employees`);
}
```

# Exhaustive Checks

If you've ever heard a Rust enjoyer (and let's be honest, if you know one, you've heard from them) talk about how great the [Rust programming language](https://www.rust-lang.org/) is, you've probably heard them mention "pattern matching" and "exhaustive checks".

To be fair, it's a pretty cool idea. Say we have this union type:

```ts
type Notif = "email" | "sms" | "push";
```

and we have this function that uses it:

```ts
function sendNotification(notif: Notif) {
  switch (notif) {
    case "email":
      return "Sending email";
    case "sms":
      return "Sending SMS";
    case "push":
      return "Sending push notification";
  }
  return "Unknown notification type";
}
```

This might be a very reasonable way to write JavaScript code, but that final `return "Unknown notification type";` is actually redundant in good TypeScript code. The `switch` statement is exhaustive, and TypeScript is smart enough to know that `return "Unknown notification type";` is actually unreachable code, and will give us a compiler error (assuming we have configured `tsc` to do so)!

_Design your types so that you get these kinds of useful errors._

# Guard Clauses

Guard clauses (a fancy way of saying "early returns") are my favorite way to quickly narrow types within a function. Peak production TypeScript code is often riddled with `undefined` and `null` types due to the nature of I/O and external APIs, so this is a classic pattern:

```typescript
function processName(name: string | null | undefined) {
  if (name === null || name === undefined) {
    return "";
  }
  // TypeScript knows name is a string here
  return name.toUpperCase();
}
```

Now, an empty string keeps processName's behavior straightforward, (always returning a string), but depending on your use case, it might make more sense to throw an error instead:

```typescript
function processName(name: string | null | undefined) {
  if (name === null || name === undefined) {
    throw new Error("Name is required");
  }
  // TypeScript knows name is a string here
  return name.toUpperCase();
}
```

Interestingly, throwing an error still narrows the type, but it doesn't change the function signature - this function still just returns a string. That's because errors in JavaScript and TypeScript are a control flow mechanism, not a type mechanism, so you do just kind of need to be aware, "hey this function can throw, I need to handle that".

In cases where my program won't break on an empty string, I might just coalesce to an empty string instead of throwing an error. This happens all the time with optional fields in web apps.

# Type Assertion

Sometimes you know more about a value's type than TypeScript does... it's rare but it happens. The [`as` keyword](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions) is the "trust me, bro" of TypeScript.

In the Boot.dev codebase, we have some places where we _know_ a query parameter is a `string`, but [`Vue`](https://vuejs.org/) (our front-end framework) uses `string | string[]` for query params... which makes sense because query params can be arrays, but we know in many cases (because our back-end controls this) that it's always a string.

So, we have something like this:

```typescript
// Property 'toLowerCase' does not exist on type 'string | string[]'
const userId = route.query?.userId.toLowerCase();
```

But _we_ know it's never an array, so we just use `as string` to do this:

```ts
const userId = (route.query?.userId as string).toLowerCase();
```

We also capture values that come across the network as `unknown` and then use `as` to assert them into the shape we expect a given network response to be:

```typescript
type User = {
  id: string;
  name: string;
};

async function getUserRaw(userId: string): Promise<unknown> {
  const response = await fetch(`/api/users/${userId}`);
  return response.json();
}

export async function getUser(userId: string) {
  const data = await getUserRaw(userId);
  // here data is still just "unknown"
  // so we assert it to a User type
  return data as User;
}
```

## Angle Bracket Syntax

There is an alternative syntax for type assertions using angle brackets and the type before the value:

```typescript
const userIdRaw = <string>route.query?.userId;
const userId = userIdRaw.toLowerCase();
```

## When to Do Type Assertions

- **I try to avoid them**. I'd rather use actual conditional narrowing over assertions unless I'm extremely confident. Conditional narrowing is safer because it doesn't involve assumptions.
- I prefer the `as` syntax over the angle bracket syntax. It's clearer, easier to read, and easier to write.

# Double Assertion

TypeScript won't allow you to assert absolute nonsense:

```typescript
const num = 42;

// Error: Conversion of type 'number' to type
// 'string' may be a mistake because neither
// type sufficiently overlaps with the other.
const str = num as string;
```

The `number` and `string` types have no overlap, making this assertion likely to be a mistake, so TypeScript complains. We _can_ get around this with a double assertion:

```typescript
const id = 42;

// This works - but is very unsafe!
const userId = id as unknown as string;

// Now TypeScript treats this as a string
console.log(userId.toUpperCase());
// Compiles, but still CRASHES at runtime!
```

I've never used this in production code. If you see this in the wild, pray that the author was a 100x engineer that knew what they were doing.

# Non-Null Assertion

It's common for TypeScript libraries to assume that a value can be `null` or `undefined` even when _you_ know it can't be. You can assert that it's not with the [non-null assertion (`!`)](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#non-null-assertion-operator) operator. It tells the compiler that a value cannot be `null` or `undefined`, even when the type system thinks it _might_ be.

```ts
// assume getCleanedText returns a string | null
import { getCleanedText, sendText } from "./text-utils";

const cleanedText = getCleanedText("some text");
// cleanedText is string | null
// but we know that it's not null because we passed in a valid string

// sendText expects a string, so we use a non-null assertion
sendText(cleanedText!);
```

You'll also see this fairly often when working with optional properties that you know exist:

```typescript
interface User {
  id: string;
  name?: {
    first: string;
    last: string;
  };
}

// we don't control the User type (it's imported from a library)
// but we know that we always use the `name` property
sentText(user.name!.first);
```

The same rules-of-thumb as `as` assertions apply here imo. Only use non-null assertions when you're crazy-confident that the value can't be `null` or `undefined`. Just use a conditional guard clause if there is any uncertainty. This is always safer, albeit more verbose:

```typescript
function sendTextSafely(text: string | null) {
  if (text === null) {
    throw new Error("Text is required");
  }
  sendText(text);
}
```

---

CH11: Classes

# Classes

Classes in TypeScript work mostly the same way that they do in JavaScript, but with the added benefit of static typing. One of the biggest differences is that you'll see type annotations on all the class properties at the top level of the class declaration.

```typescript
class Hero {
  name: string;
  health: number;

  constructor(name: string, health: number) {
    this.name = name;
    this.health = health;
  }

  attack(damage: number): void {
    console.log(`${this.name} attacks for ${damage} damage!`);
  }

  getHealth() {
    return this.health;
  }
}

// Create an instance
const geralt = new Hero("Geralt", 100);
geralt.attack(25);
// "Geralt attacks for 25 damage!"
console.log(geralt.getHealth());
// 100
```

# Private Class Members

JavaScript added support for private class members in ES2022 with the `#` syntax. TypeScript respects that syntax, and will give you compilation errors if you try to access private members outside of the class.

```typescript
class SecretAgent {
  // a private field
  #id: string;

  constructor(id: string) {
    this.#id = id;
  }

  // a public method
  getCodeName(): string {
    const idToCodeNameMap: Record<string, string> = {
      "007": "James Bond",
      "006": "Alec Trevelyan",
      // Add more mappings as needed
    };
    return idToCodeNameMap[this.#id] || "Unknown Agent";
  }
}

const bond = new SecretAgent("007");
console.log(bond.getCodeName()); // "James Bond"

// Property '#id' is not accessible outside class 'SecretAgent' because it has a private identifier.
console.log(bond.#id);
```

In plain JavaScript, we'd only get the error at _runtime_, but with the same syntax in TypeScript, we get the error at _compile time_. Much better!

# TypeScript Public and Private

JavaScript's `#` private fields didn't come until ES2022, but TypeScript developers had wanted public/private/protected access modifiers for a _long_ time, so TypeScript added support for `private` and `protected` before then. So a lot of older TypeScript code uses the keyword syntax.

To create `private` members the TypeScript-only way, you use the `private` keyword:

```typescript
class SecretAgent {
  // private field using the private keyword
  private id: string;

  constructor(id: string) {
    this.id = id;
  }

  // a public method
  getCodeName(): string {
    const idToCodeNameMap: Record<string, string> = {
      "007": "James Bond",
      "006": "Alec Trevelyan",
      // Add more mappings as needed
    };
    return idToCodeNameMap[this.id] || "Unknown Agent";
  }
}

const bond = new SecretAgent("007");
console.log(bond.getCodeName()); // "James Bond"

// Property 'id' is private and only accessible within class 'SecretAgent'
console.log(bond.id); // This will cause a compilation error
```

# Protected Data Members

The [`protected` keyword](https://www.typescriptlang.org/docs/handbook/2/classes.html#protected) is unique to TypeScript in that it's _not_ part of the EcmaScript standard. It allows you to define members that are accessible within the class _and its subclasses_, but not from outside the class. It's like "private but also accessible to subclasses".

```typescript
class Character {
  protected health: number;

  constructor(health: number) {
    this.health = health;
  }

  protected takeDamage(amount: number): void {
    this.health -= amount;
    if (this.health < 0) {
      this.health = 0;
    }
  }
}

class Fighter extends Character {
  constructor(health: number) {
    super(health);
  }

  public fight(damage: number): void {
    // Can access protected members from the parent class
    this.takeDamage(damage);
    console.log(`Fighter took ${damage} damage. Health: ${this.health}`);
  }
}

const fighter = new Fighter(100);
fighter.fight(30);

// Error: Property 'health' is protected and only accessible within class 'Character' and its subclasses
console.log(fighter.health);

// Error: Property 'takeDamage' is protected and only accessible within class 'Character' and its subclasses
fighter.takeDamage(10);
```

The `protected` keyword does _not_ have a native JavaScript alternative. I personally don't use it very often. I tend to use `#` private fields whenever possible, or just leave them public if subclasses need access.

# Abstract Classes and Methods

An [`abstract`](https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members) class is a class that _cannot be instantiated directly_. It's a template for inheritance, forcing subclasses to implement specific methods or properties. Say we have this `Shape` class

```typescript
abstract class Shape {
  size: "small" | "medium" | "large";
  constructor(size: "small" | "medium" | "large") {
    this.size = size;
  }

  abstract calculateArea(): number;

  displayArea(): void {
    console.log(`The area of this shape is ${this.calculateArea()}`);
  }
}
```

We can't do this:

```typescript
// Error: Cannot create an instance of an abstract class
const shape = new Shape("small");
```

Within an abstract class, `abstract` methods (like `calculateArea` above) _do not_ have an implementation because the implementation _must_ be provided by the subclass. However, it can still have regular methods (like `displayArea` above) which are then shared by all subclasses.

So, we can create a `Circle` class that extends `Shape` and implements the `calculateArea` method:

```typescript
class Circle extends Shape {
  radius: number;
  constructor(size: "small" | "medium" | "large") {
    super(size);
    if (this.size === "small") {
      this.radius = 5;
    } else if (this.size === "medium") {
      this.radius = 10;
    } else {
      this.radius = 15;
    }
  }
  calculateArea(): number {
    return Math.PI * this.radius * this.radius;
  }
}
```

And of course, the `Circle` class _can_ be instantiated:

```typescript
const circle = new Circle("medium");
circle.displayArea();
// The area of this shape is 314.1592653589793
```

Like `protected`, the `abstract` keyword does _not_ exist in native JavaScript. It's a TypeScript-only feature that helps enforce rules on subclasses at compile time. The `abstract` keyword and abstract-only methods are removed from the compiled code.

# Classes Implement Interfaces

Classes can implement interfaces using the [`implements` clause](https://www.typescriptlang.org/docs/handbook/2/classes.html#implements-clauses). This enforces that the class adheres to the structure defined by the interface. Say we have two interfaces:

```typescript
interface Vehicle {
  make: string;
  model: string;
}

interface Drivable {
  drive(distance: number): void;
}
```

And we have a class that we want to implement (have the properties and methods of) both interfaces:

```typescript
class ElectricCar {
  make: string;
  model: string;
}
```

We can add a clause to the class definition to implement both interfaces. However, because at the moment, the class doesn't have a `drive` method, TypeScript will throw an error:

```typescript
// Error: Class 'ElectricCar' incorrectly implements interface 'Drivable'.
class ElectricCar implements Vehicle, Drivable {
  make: string;
  model: string;
}
```

So, now we're reminded to add the `drive` method, and we do so:

```ts
class ElectricCar implements Vehicle, Drivable {
  make: string;
  model: string;

  // not required by the interfaces, but it's
  // okay to add extra properties
  private isRunning: boolean = false;

  constructor(make: string, model: string) {
    this.make = make;
    this.model = model;
    this.isRunning = false;
  }

  drive(distance: number): void {
    this.isRunning = true;
    console.log(`Driving ${distance} miles`);
  }
}
```

We can now _use_ an instance of `ElectricCar` as a `Vehicle` or `Drivable`:

```typescript
const myCar = new ElectricCar("Tesla", "Model S");

function testDrive(vehicle: Vehicle) {
  console.log(`Testing ${vehicle.make} ${vehicle.model}`);
}

testDrive(myCar); // "Testing Tesla Model S"

function takeForARide(drivable: Drivable) {
  drivable.drive(10);
}

takeForARide(myCar); // "Driving 10 miles"
```

# Classes vs. Interfaces and Types

You might be wondering when you should use a full-blown class to create reusable object types over interfaces and type aliases. There are 3 ways to model the same thing!

```ts
class Hero {
  name: string;
  health: number;
}

interface Hero {
  name: string;
  health: number;
}

type Hero = {
  name: string;
  health: number;
};
```

If you're an object-oriented programmer, you might be more comfortable with classes and the extra features they provide. Classes can basically do everything that interfaces can do, and more. Some of the most notable things you _can't_ do with interfaces and type aliases are:

- Have private, protected, static, and abstract members
- Have dedicated constructors
- Have method implementations predefined on all instances

And on the other hand:

- Type aliases and interfaces have no runtime overhead
- Type aliases and interfaces have fewer features, and as a result, are simpler to work with when you don't need the extra features
- Type aliases and interfaces are more flexible, especially when working with plain objects because they're not tied to the class implementation (signature only)
- Interfaces can be extended and merged in ways that types and classes can't

Personally, I'm a simple man. I tend to use type aliases when I can, and only reach for the additional features of classes when I feel I need them, which is rare in web development, at least in my opinion.

# The “this” Type

Luckily TypeScript is smart enough to handle the funky `this` keyword for us, because as JavaScript developers, we know that the only question more difficult than "what is the meaning of life?" is "what is the value of `this`?".

```typescript
class Counter {
  private count: number = 0;

  increment(): void {
    // 'this' is implicitly typed as Counter
    this.count++;
  }

  getCount(): number {
    // 'this' is implicitly typed as Counter
    return this.count;
  }
}
```

## Explicit `this` Parameters

TypeScript is pretty smart (especially newer versions) and usually infers the type of `this` correctly. However, if you want to explicitly control the type of `this`, you can use the [special `this` parameter](https://www.typescriptlang.org/docs/handbook/2/functions.html#declaring-this-in-a-function):

```typescript
class Counter {
  private count: number = 0;

  increment(this: Counter, n: number): void {
    // 'this' is explicitly typed as Counter
    // the `this` parameter is not available at runtime
    // it is only used for type checking
    this.count += n;
  }

  getCount(this: Counter): number {
    // 'this' is explicitly typed as Counter
    return this.count;
  }
}

const counter = new Counter();
counter.increment(5);
console.log(counter.getCount());
// 5
```

# Parameter Properties

TypeScript has a neat shorthand feature called [parameter properties](https://www.typescriptlang.org/docs/handbook/2/classes.html#parameter-properties) that allows you to declare and initialize class properties directly in the constructor parameters. This eliminates the need to separately declare properties and then assign them in the constructor body.

## Without Parameter Properties

Normally, you'd write a class like this:

```typescript
class Hero {
  name: string;
  health: number;
  private level: number;

  constructor(name: string, health: number, level: number) {
    this.name = name;
    this.health = health;
    this.level = level;
  }
}
```

## With Parameter Properties

With parameter properties, you can achieve the same result with much less code:

```typescript
class Hero {
  constructor(
    public name: string,
    public health: number,
    private level: number,
  ) {}
}
```

In the example above, the constructor body `{}` is intentionally empty because parameter properties handle declaration and initialization. Other class methods should be defined in the class body, outside the constructor.

By adding an access modifier (`public`, `private`, `protected`, or `readonly`) to a constructor parameter, TypeScript automatically:

1. Declares a property with the same name and type
2. Assigns the parameter value to that property

## Limitation

Parameter properties work with TypeScript's `private` keyword, but **not** with JavaScript's `#` private field syntax. If you need truly private fields using the `#` syntax, you must declare them separately:

```typescript
class Hero {
  #secretPower: string;

  constructor(
    public name: string,
    secretPower: string,
  ) {
    this.#secretPower = secretPower;
  }
}
```

---

CH12: Utility Types

# Single Source of Truth

It's incredibly common for a TypeScript codebase to amass a truly absurd number of custom type definitions - hundreds of interfaces and types, all with slightly different numbers of fields for a lot of the same "entities". You might run into crazy stuff like:

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  age: number;
}

interface UserWithoutId {
  name: string;
  email: string;
  age: number;
}
```

_This is bad_. We generally try to avoid redefining the same types over and over, and instead try to follow a ["single source of truth"](https://en.wikipedia.org/wiki/Single_source_of_truth) approach. For example, here we could refactor a bit:

```typescript
interface UserWithoutId {
  name: string;
  email: string;
  age: number;
}

interface User extends UserWithoutId {
  id: string;
}
```

There are other techniques to do this even more cleanly that we'll cover later in this chapter.

In other words, we try to **define our types once**, and build type systems that rely on inference and type transformations to _derive_ the types we need automatically. That way, when we make changes, we only have to do it in one place. In our second example, updating `UserWithoutId` will now automatically update `User` as well: a big win.

# Partial Utility Type

There are several built-in [utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) that _transform_ existing types into new ones. One of the most useful is [`Partial<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype), which makes all properties of a type optional. For example:

```typescript
type User = {
  id: string;
  name: string;
  email: string;
};

// Without Partial
function updateUser(
  userId: string,
  userInfo: {
    id?: string;
    name?: string;
    email?: string;
  },
) {
  // ...
}

// With Partial
function updateUser(userId: string, userInfo: Partial<User>) {
  // ...
}
```

Instead of copy/pasting the type definition, the `Partial<T>` utility type allows us to generate a new type based on an existing one. That also means if the original is ever updated, the new type created with `Partial<T>` type will automatically have those changes!

## Nested Objects

`Partial<T>` only makes the top-level properties optional. For example:

```ts
type User = {
  id: string;
  name: string;
  preferences: {
    theme: string;
    notifications: boolean;
  };
};
```

If we use `Partial<User>`, the resulting type would look like this:

```ts
// same as 'type LooseyGooseyUser = Partial<User>'
type LooseyGooseyUser = {
  id?: string;
  name?: string;
  preferences?: {
    theme: string;
    notifications: boolean;
  };
};
```

The `theme` and `notifications` properties are still required (assuming `preferences` is provided).

# Required Utility Type

The [`Required<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#requiredtype) utility type does the opposite of `Partial<T>` - it forces all properties of a type to be required, even those that were originally optional.

## Using `Required<T>`

Here's a practical example of using `Required<T>`:

```typescript
interface BlogPost {
  title: string;
  content: string;
  tags?: string[];
  publishDate?: Date;
  author?: {
    id: string;
    name?: string;
  };
}

// All properties are now required
type MyRequiredBlogPost = Required<BlogPost>;

// MyRequiredBlogPost is equivalent to:
// {
//   title: string;
//   content: string;
//   tags: string[];
//   publishDate: Date;
//   author: {
//     id: string;
//     name?: string;
//   };
// }
```

As before, the `Required<T>` utility type is _not_ recursive, it only affects the top-level properties.

# Readonly Utility Type

The [`Readonly<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#readonlytype) utility creates a new type where all the top-level properties are [`readonly`](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#readonly-and-const), preventing them from being reassigned after initialization.

```typescript
interface UserProfile {
  id: string;
  name: string;
  preferences: {
    readonly theme: "light" | "dark";
    notifications: boolean;
  };
}

type ConstantUserProfile = Readonly<UserProfile>;

// this is the same as
// type ConstantUserProfile = {
//   readonly id: string;
//   readonly name: string;
//   readonly preferences: {
//     readonly theme: "light" | "dark";
//     notifications: boolean;
//   };
// }
```

# Readonly Utility Type

The [`Readonly<T>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#readonlytype) utility creates a new type where all the top-level properties are [`readonly`](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#readonly-and-const), preventing them from being reassigned after initialization.

```typescript
interface UserProfile {
  id: string;
  name: string;
  preferences: {
    readonly theme: "light" | "dark";
    notifications: boolean;
  };
}

type ConstantUserProfile = Readonly<UserProfile>;

// this is the same as
// type ConstantUserProfile = {
//   readonly id: string;
//   readonly name: string;
//   readonly preferences: {
//     readonly theme: "light" | "dark";
//     notifications: boolean;
//   };
// }
```

# Pick Utility Type

The [`Pick<T, K>` utility type](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys) creates a new type by selecting a _subset_ of properties from an existing type. For example:

```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  description: string;
  category: string;
  inStock: boolean;
  images: string[];
  reviews: { user: string; rating: number; text: string }[];
}

type ProductSummary = Pick<Product, "id" | "name" | "price">;

const productList: ProductSummary[] = [
  { id: "p1", name: "Keyboard", price: 79.99 },
  { id: "p2", name: "Mouse", price: 59.99 },
];

const invalidProduct: ProductSummary = {
  id: "p3",
  name: "Headphones",
  price: 99.99,
  // TSC error:
  // Object literal may only specify known properties, and 'description' does not exist in type 'ProductSummary'.
  description: "Noise cancelling headphones",
};
```

`Pick` is _very_ useful for creating quick types for functions that don't need _everything_ from the original type.

# Omit Utility Type

The [`Omit<T, K>`](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys) utility type is the opposite of `Pick<T, K>`. It creates a new type by _excluding_ a set of properties from an existing type. I find this one to be very useful when removing sensitive or unnecessary properties from a type. For example, maybe you need to remove a password field from a user object before responding to an API request.

```typescript
interface DatabaseUser {
  id: string;
  username: string;
  email: string;
  passwordHash: string;
  createdAt: Date;
  updatedAt: Date;
}

// Create a safe user representation without sensitive data
type PublicUser = Omit<DatabaseUser, "passwordHash" | "updatedAt">;

function getUserProfile(userId: string): PublicUser {
  // Fetch user from database...
  const dbUser: DatabaseUser = {
    id: userId,
    username: "johndoe",
    email: "john@example.com",
    passwordHash: "$2a$12$...",
    createdAt: new Date("2023-01-15"),
    updatedAt: new Date()
  };

  // Convert to PublicUser (explicit conversion for clarity)
  const publicUser: PublicUser = {
    id: dbUser.id,
    username: dbUser.username,
    email: dbUser.email,
    createdAt: dbUser.createdAt

    // TSC error:
    // Object literal may only specify known properties, and 'passwordHash' does not exist in type 'PublicUser'.
    passwordHash: dbUser.passwordHash,
  };

  return publicUser;
}
```

---

CH13: Generics

# Generics

Generics are one of TypeScript's most powerful features. They allow you to create reusable logic that works with many types rather than a single one. Think of a data structure like a Queue or a Stack. They can hold any type of data, so it would be really annoying to reimplement them for every type:

- `NumberQueue`
- `StringQueue`
- `UserQueue`
- `etc.`

Generics let us create a single `Queue<T>` type that can work with any type `T`. The best part is that when we use that queue _with_ a specific type, TypeScript won't lose that type information! **Generics are a way to reuse behavior across types without resorting to `any`**.

_You may have already noticed, but TypeScript's utility types are all generics! For example, `Partial<T>` is a generic type that takes a type `T` and returns a new type with all properties of `T` set to optional_.

The `ExampleGeneric<T>` syntax is an example of a generic type parameter. `ExampleGeneric` is the name of the generic type (e.g. `Array`, `Promise`, etc., basically anything that can operate on the various types), and `T` is the name of the type parameter. `T` is just a variable name, we could call it anything, but `T` is a common convention.

## Creating Custom Generic Functions

Say we're building some client-side code (frontend, I know, ewww) and we make _tons_ of [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) requests to a backend server. We might have a lot of code that looks like this:

```ts
async function fetchFromApi(url: string) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    return await response.json();
  } catch (error) {
    console.error("Error fetching data:", error);
    return undefined;
  }
}
```

We _could_ lazily leave this as-is, but then it will always return a `Promise<any>`, which has no useful type information. Instead, let's make it generic:

```ts
async function fetchFromApi<T>(url: string): Promise<T | undefined> {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    return await response.json();
  } catch (error) {
    console.error("Error fetching data:", error);
    return undefined;
  }
}
```

Now whenever we call it, we just specify the type we expect to get back:

```ts
const comments = await fetchFromAPI<Comment[]>(
  "https://api.example.com/posts/1/comments",
);

const user = await fetchFromApi<User>("https://api.example.com/user/1");

const posts = await fetchFromApi<Post[]>("https://api.example.com/posts");
```

# Multiple Type Parameters

There's no need to be limited to just a single [type parameter](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-functions) in TypeScript! They're just parameters, and you can have as many as you need (but please don't go crazy).

To _prove_ that `T` is just a random name, I'm going to use longer names in this lesson... but just be aware that short capital letters like `T`, `U`, `V`, etc. are the most common convention for generic type parameters.

Let's create a function that "transforms" its inputs. It takes as input:

- An array of items of type `InputType`
- A function that takes an item of type `InputType` and returns an item of type `OutputType`

And it returns a new array of items of type `OutputType`.

```ts
function transform<InputType, OutputType>(
  inputs: InputType[],
  update: (item: InputType) => OutputType,
): OutputType[] {
  const outputs: OutputType[] = [];
  for (const input of inputs) {
    const output = update(input);
    outputs.push(output);
  }
  return outputs;
}
```

See how long that function signature gets? That's why `T` and `U` are so popular...

If you've been paying close attention, we basically just built our own version of [`Array.prototype.map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

Now we can use our own custom transformers with our custom transform function!

```ts
type Human = {
  name: string;
  age: number;
};

const humans: Human[] = [
  { name: "Eren", age: 15 },
  { name: "Mikasa", age: 16 },
  { name: "Armin", age: 15 },
];

const titanTransformer = (human: Human): string => `${human.name} is a titan!`;

const titanNames = transform<Human, string>(humans, titanTransformer);
console.log(titanNames);
// ['Eren is a titan!', 'Mikasa is a titan!', 'Armin is a titan!']
```

Without changing our `transform` function, we can use it to transform entirely different types of data:

```ts
const numbers = [1, 2, 3, 4, 5];
const double = (num: number): number => num * 2;

const doubledNumbers = transform<number, number>(numbers, double);
console.log(doubledNumbers);
// [2, 4, 6, 8, 10]
```

# Generic Constraints

Sometimes you need your generic function to know _something_ about the types it operates on. The examples we've used so far don't know _anything_ about the types they're using:

```ts
async function fetchFromApi<T>(url: string): Promise<T | undefined>;
```

In `fetchFromApi`, `T` could be _anything_.

Constraints are just interfaces that allow us to write generics that only operate within the constraints of a given interface type. In the example above, the `any` constraint is the same as the empty interface because it means the type in question can be _anything_.

We can use the `extends` keyword to constrain the type parameter to have certain properties, for example:

```typescript
interface HasCost {
  cost: number;
}

function applyDiscount<T extends HasCost>(vals: T[], discount: number): T[] {
  const arr: T[] = [];
  for (const val of vals) {
    val.cost *= discount;
    arr.push(val);
  }
  return arr;
}
```

The `applyDiscount` function works in a type-safe way _on any type that has a `.cost` property_, and again, because we're still using generics here, type information will _not_ be lost when the function returns.

```ts
const shoes = [
  {
    size: 12.5,
    country: "US",
    cost: 120,
  },
  {
    size: 12.5,
    country: "US",
    cost: 110,
  },
];

const tvs = [
  {
    framerate: 120,
    brand: "Samsung",
    cost: 500,
  },
  {
    framerate: 240,
    brand: "Vizio",
    cost: 300,
  },
];

const people = [
  {
    name: "Lane",
  },
  {
    name: "Breanna",
  },
];

const discountedShoes = applyDiscount(shoes, 0.3);
const discountedTVS = applyDiscount(tvs, 0.5);

// Error:
// Argument of type '{ name: string; }[]' is not assignable to parameter of type 'HasCost[]
// ... also you can't buy people what is wrong with you???
const discountedPeople = applyDiscount(people, 0.2);
```

# Type Parameters for Types

Type parameters aren't just limited to functions and methods! You can use type parameters to create generic types as well! For example:

```ts
interface Store<T> {
  get(id: string): T;
  save(id: string, item: T): void;
  list(): T[];
}
// also works with type aliases using
// type Store<T> = { ... }
```

Now a `Store` can be anything that implements the methods above, but _what is stored_ doesn't matter. Next we can create a function that _uses_ the store, again, not caring about what is stored inside of it:

```ts
function addAndGetItems<T>(store: Store<T>, id: string, newItem: T): T[] {
  store.save(id, newItem);
  return store.list();
}
```

Finally, we can create a `Store` that specifically deals with `Product` types:

```ts
type Product = {
  name: string;
  price: number;
};

const productStore = {
  products: {} as Record<string, Product>,
  get(id: string): Product {
    return this.products[id];
  },
  save(id: string, item: Product): void {
    this.products[id] = item;
  },
  list(): Product[] {
    return Object.values(this.products);
  },
};
```

And we can use it like this:

```ts
const newStore = addAndGetItems(productStore, "laneslaptop", {
  name: "Laptop",
  price: 999,
});
console.log(newStore);
// [{ "name": "Laptop", "price": 999 }]
const finalStore = addAndGetItems(productStore, "allanstoaster", {
  name: "Toaster",
  price: 50,
});
console.log(finalStore);
// [{ "name": "Laptop", "price": 999 }, { name: 'Toaster', price: 50 }]
```

We could also create a store for something entirely different!

```ts
type Homunculus = {
  title: string;
  abilities: string[];
};

const homunculusStore = {
  homunculi: {} as Record<string, Homunculus>,
  get(id: string): Homunculus {
    return this.homunculi[id];
  },
  save(id: string, item: Homunculus): void {
    this.homunculi[id] = item;
  },
  list(): Homunculus[] {
    return Object.values(this.homunculi);
  },
};
```

and it will still work with `addAndGetItems`:

```ts
const newHomunculus = addAndGetItems(homunculusStore, "laneslaptop", {
  title: "Laptop",
  abilities: ["fast", "strong"],
});
console.log(newHomunculus);
// [{ "title": "Laptop", "abilities": ["fast", "strong"] }]
```

# Type Parameters for Types

Type parameters aren't just limited to functions and methods! You can use type parameters to create generic types as well! For example:

```ts
interface Store<T> {
  get(id: string): T;
  save(id: string, item: T): void;
  list(): T[];
}
// also works with type aliases using
// type Store<T> = { ... }
```

Now a `Store` can be anything that implements the methods above, but _what is stored_ doesn't matter. Next we can create a function that _uses_ the store, again, not caring about what is stored inside of it:

```ts
function addAndGetItems<T>(store: Store<T>, id: string, newItem: T): T[] {
  store.save(id, newItem);
  return store.list();
}
```

Finally, we can create a `Store` that specifically deals with `Product` types:

```ts
type Product = {
  name: string;
  price: number;
};

const productStore = {
  products: {} as Record<string, Product>,
  get(id: string): Product {
    return this.products[id];
  },
  save(id: string, item: Product): void {
    this.products[id] = item;
  },
  list(): Product[] {
    return Object.values(this.products);
  },
};
```

And we can use it like this:

```ts
const newStore = addAndGetItems(productStore, "laneslaptop", {
  name: "Laptop",
  price: 999,
});
console.log(newStore);
// [{ "name": "Laptop", "price": 999 }]
const finalStore = addAndGetItems(productStore, "allanstoaster", {
  name: "Toaster",
  price: 50,
});
console.log(finalStore);
// [{ "name": "Laptop", "price": 999 }, { name: 'Toaster', price: 50 }]
```

We could also create a store for something entirely different!

```ts
type Homunculus = {
  title: string;
  abilities: string[];
};

const homunculusStore = {
  homunculi: {} as Record<string, Homunculus>,
  get(id: string): Homunculus {
    return this.homunculi[id];
  },
  save(id: string, item: Homunculus): void {
    this.homunculi[id] = item;
  },
  list(): Homunculus[] {
    return Object.values(this.homunculi);
  },
};
```

and it will still work with `addAndGetItems`:

```ts
const newHomunculus = addAndGetItems(homunculusStore, "laneslaptop", {
  title: "Laptop",
  abilities: ["fast", "strong"],
});
console.log(newHomunculus);
// [{ "title": "Laptop", "abilities": ["fast", "strong"] }]
```

# Generic Type Inference

You may have already noticed this, but in most contexts, TypeScript can infer type parameters by the actual parameters you pass in, so you won't need to specify them. Let's take our titan transformer example again:

```typescript
function transform<InputType, OutputType>(
  inputs: InputType[],
  update: (item: InputType) => OutputType,
): OutputType[] {
  const outputs: OutputType[] = [];
  for (const input of inputs) {
    const output = update(input);
    outputs.push(output);
  }
  return outputs;
}

type Human = {
  name: string;
  age: number;
};

const humans: Human[] = [
  { name: "Eren", age: 15 },
  { name: "Mikasa", age: 16 },
  { name: "Armin", age: 15 },
];

const titanTransformer = (human: Human): string => `${human.name} is a titan!`;
```

Previously, we explicitly passed in `<Human, string>` as the type parameters:

```ts
const titanNames = transform<Human, string>(humans, titanTransformer);
console.log(titanNames);
```

But in this case, there's no need because TypeScript knows that our `humans` variable is an array of `Human` objects, and the `titanTransformer` function takes a `Human` and returns a `string`. So we can just call:

```ts
const titanNames = transform(humans, titanTransformer);
```

# Generic Classes

We're kinda beating a dead horse at this point.

Look, we get it, you can add type parameters to almost anything in TypeScript...

**So yeah, classes can be generic too**. To keep it somewhat interesting, let's combine a few concepts:

- `InMemoryRepository` is a generic class
- It implements a generic interface (`Repository<T>`)
- `T` is constrained to have an `id` property

```typescript
interface Repository<T> {
  getAll(): T[];
  getById(id: string): T | undefined;
  save(item: T): void;
}

class InMemoryRepository<T extends { id: string }> implements Repository<T> {
  private items: T[] = [];

  getAll(): T[] {
    return [...this.items];
  }

  getById(id: string): T | undefined {
    return this.items.find((item) => item.id === id);
  }

  save(item: T): void {
    const index = this.items.findIndex((i) => i.id === item.id);
    if (index >= 0) {
      this.items[index] = item;
    } else {
      this.items.push(item);
    }
  }
}
```

There are a few things to note about the example above:

- The purpose of using the `implements` keyword is to ensure that the class adheres to the `Repository<T>` interface - TypeScript will yell at us if our `InMemoryRepository` class can't be used as a `Repository<T>`.
- While any old `Repository<T>` doesn't need an `id` property, our `InMemoryRepository` does.
- An `InMemoryRepository` can be used to hold _any_ type of object, as long as it has an `id` property. And all the implementation logic is shared between all the different possible types.

Let's create an `InMemoryRepository` for `Shinigami`:

```ts
interface Shinigami {
  id: string;
  name: string;
}

const deathNoteRepo = new InMemoryRepository<Shinigami>();
deathNoteRepo.save({ id: "1", name: "Ryuk" });
deathNoteRepo.save({ id: "2", name: "Rem" });
console.log(deathNoteRepo.getAll());
```

Of course, if we try to create an `InMemoryRepository` for something that doesn't have an `id` property, TypeScript will yell at us:

```ts
interface Psychopaths {
  name: "Light Yagami" | "L";
}

// Error: Type 'Psychopaths' does not satisfy the constraint '{ id: string; }'
const psychopathRepo = new InMemoryRepository<Psychopaths>();
```

---

CH14: Conditional Types

# Conditional Types

We're now getting into some pretty advanced stuff that, while useful in really tricky modelling situations, is not something you'll need in application level code.

As a general rule, advanced TypeScript features are more useful in library code that needs to be more flexible, abstract, and reusable. Application level TypeScript code is generally much simpler and more concrete... [grug](https://grugbrain.dev/) make object type, grug use object type... grug happy.

[Conditional types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html) allow us to create new types based on _conditions_ within the type system, they take this form:

```ts
type NewType = SomeType extends OtherType ? TrueType : FalseType;
```

It reads like a ternary expression: "If `SomeType` extends (satisfies) `OtherType`, then `NewType` is `TrueType`; otherwise, it's `FalseType`."

Here's a simple example:

```typescript
type IsString<T> = T extends string ? true : false;

// Usage
type Result1 = IsString<"hello">; // true
type Result2 = IsString<42>;      // false
type Result3 = IsString<string>;  // true
```

In this example, `IsString` is a conditional type that checks if the type parameter `T` extends `string`. If it does, the resulting type is `true`; otherwise, it's `false`. TypeScript actually ships with some built-in conditional types:

- [`type Extract<T, U> = T extends U ? T : never;`](https://www.typescriptlang.org/docs/handbook/utility-types.html#extracttype-union)
- [`type Exclude<T, U> = T extends U ? never : T;`](https://www.typescriptlang.org/docs/handbook/utility-types.html#excludeuniontype-excludedmembers)
- [`type NonNullable<T> = T extends null | undefined ? never : T;`](https://www.typescriptlang.org/docs/handbook/utility-types.html#nonnullabletype)

As usual, the question is, "when the heck is this useful???" Well, let's say we have some events that can fire in our front end application:

```ts
type ClickEvent = { type: "click"; x: number; y: number };
type KeyEvent = { type: "key"; key: string };
type MouseMoveEvent = { type: "mousemove"; x: number; y: number };
type FormEvent = { type: "submit"; formId: string };

type Event = ClickEvent | KeyEvent | MouseMoveEvent | FormEvent;
```

It may be useful to dynamically create a type that only includes "mouse-related" events: the ones that have an `x` and `y` property. We can use the [`Extract`](https://www.typescriptlang.org/docs/handbook/utility-types.html#extracttype-union) conditional type to do so:

```ts
// Extract is a TS built-in type, this is the implementation:
type Extract<T, U> = T extends U ? T : never;

// This is an example of how it can be used:
type MouseRelatedEvents = Extract<Event, { x: number; y: number }>;
```

Now `MouseRelatedEvents` is the same as:

```ts
type MouseRelatedEvents = ClickEvent | MouseMoveEvent;
```

The difference is that it's _dynamic_. If we add more events to the `Event` union, `MouseRelatedEvents` will automatically include them if they match the condition (i.e., if they have `x` and `y` properties).

# Infer

The [`infer` keyword](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-within-conditional-types), when used inside a conditional type, lets us use the type of a value from the true branch. For example:

```typescript
type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
```

The `GetReturnType` is a conditional utility type that extracts the return type of a function type `T`. We can use it like this:

```ts
function greet() { return "Hello, world!"; }
function sum(a: number, b: number) { return a + b; }

type GreetReturnType = GetReturnType<typeof greet>; // string
type SumReturnType = GetReturnType<typeof sum>;     // number
```

You might be wondering, "Why `infer R` instead of just `R`?" Basically "because TypeScript syntax says so". See, this is the type we're trying to "match" in the conditional, because the return value can be anything:

```ts
(...args: any[]) => any;
```

But we can't use `any`, because we're trying to capture the type in a type variable, so we use `R`. Buuuuut TypeScript needs to know that `R` is a type variable, so that's what the `infer` keyword does. It says "hey, I made this new type variable `R`, and I want you to remember that in the conditional's return statement, assuming the conditional is true".

# Mapped Types

Remember dynamic properties?

```ts
type UserMetrics = {
  [key: string]: number;
};
```

Well, [mapped types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html) are a way to create new types with dynamic properties based on _existing_ types. For example, say we have a `Soldier` type:

```ts
type Soldier = {
  name: string;
  age: number;
  branch: "garrison" | "military police" | "survey corps";
};
```

And we want to create a new type that has the same properties, but all of them are optional. We can do that with a mapped type:

```ts
type OptionalSoldier = {
  [K in keyof Soldier]?: Soldier[K];
};
```

- The `keyof` operator gets the keys of the `Soldier` type
- The `in` keyword iterates over them
- The `?` makes each property optional
- The `Soldier[K]` gets the value type each property maps to

It results in a type that's the same as:

```ts
type OptionalSoldier = {
  name?: string;
  age?: number;
  branch?: "garrison" | "military police" | "survey corps";
};
```

The obvious benefit, of course, is that if we update `Soldier`, `OptionalSoldier` automatically updates too.

## Changing the Values

Mapped types are really useful for making properties `optional` or `readonly`, but it's an incredibly powerful (and potentially dangerously confusing) tool. You can also use them to change the value type of properties:

```ts
type StringifiedSoldier = {
  [K in keyof Soldier]: string;
};
```

Which is the same as:

```ts
type StringifiedSoldier = {
  name: string;
  age: string;
  branch: string;
};
```

To create a new empty object of type `Blank<T>`, a type assertion is likely the easiest way:

```ts
const result = {} as Blank<T>;
```

# Mapped Types With Conditionals

So if mapped types are a step down the road to type tom-foolery, conditional mapped types are _one more_ leap. I'm not saying they're not cool, or that they're not useful (they are in certain scenarios), but you can create some _really_ hard to read code if you're not careful. So use them wisely.

Let's take our `OptionalSoldier` example from before:

```typescript
type Soldier = {
  name: string;
  age: number;
  branch: "garrison" | "military police" | "survey corps";
};

type OptionalSoldier = {
  [K in keyof Soldier]?: Soldier[K];
};
```

What if instead of making _all_ the properties optional, we instead wanted to filter any non-string properties? We can do that with a conditional mapped type:

```ts
type FilteredSoldier = {
  [K in keyof Soldier]: Soldier[K] extends string ? Soldier[K] : never;
};
```

The conditional: `Soldier[K] extends string` only evaluates to `true` (and thus the property is included as `Soldier[K]`) if the property is assignable to `string`. Otherwise, it evaluates to `never`, and the property is excluded. One _really cool_ thing to note, is that because we used `Soldier[K]` in the conditional, the more specific type of the `branch` property is preserved, resulting in a type of:

```ts
type FilteredSoldier = {
  name: string;
  // age: never;
  branch: "garrison" | "military police" | "survey corps";
};
```

The `extends Function | object` can be used to check if a type is a function or an object.

# Extracting Keys from Types

Mapped types don't just let you build new object types – they can also be used to **extract keys**. Say we have this object type:

```ts
type Soldier = {
  name: string;
  age: number;
  branch: "garrison" | "military police" | "survey corps";
};
```

Now imagine you want to get just the **keys** of the fields that are `string`-based – maybe for a filter, a dropdown, or feeding to an LLM that summarizes records. First, we create an object where each key **either returns the key name, or `never`**:

```ts
type StringKeys<T> = {
  [K in keyof T]: T[K] extends string ? K : never;
};
```

That gives you something like:

```ts
type Result = {
  name: "name";
  age: never;
  branch: "branch";
};
```

Now **we index into that type** using all of its keys:

```ts
type StringKeyUnion<T> = StringKeys<T>[keyof T];
```

We've made the object into a union of its values:

```ts
type Keys = StringKeyUnion<Soldier>;
// "name" | "branch"
```

---

CH15: Local Development

# Install TypeScript

We've been doing a ton of TypeScript work in the browser, let's drop down to your machine for this last chapter.

I'm going to assume you already have [`node` and `npm`](https://nodejs.org/en/) installed, if you don't go do that now. We cover all that in our [Learn JavaScript](https://boot.dev/courses/learn-javascript) course.

## Assignment

1. [ ] Install TypeScript globally. Run the following command:

```bash
npm install -g typescript
```

2. [ ] You should now be able to run the TypeScript compiler with the `tsc` command. Check its version:

```bash
tsc -v
```

If you don't get back a valid version, either your installation might have failed, or the install location is not in your `PATH`. For example, for myself on Mac, `tsc` is in `/Users/wagslane/.nvm/versions/node/v20.11.1/bin`, so I just need to ensure my `PATH` includes that directory.
# tsconfig.json

The [`tsconfig.json`](https://www.typescriptlang.org/tsconfig/) file configures TypeScript compiler behavior. There are _a lot_ of options when it comes to configuring TypeScript, and they change the way the compiler and tooling work. It's often not fair to say "In TypeScript X will throw an error" because there are almost always options that can be set to make that not true.

## A Simple `tsconfig.json`

```json
{
  "compilerOptions": {
    "lib": ["esnext"],
    "target": "esnext",
    "strict": false
  }
}
```

- [`lib`](https://www.typescriptlang.org/tsconfig/#lib) specifies the library files to include in the compilation. It's what APIs are available for us to use in _our code_.
- [`target`](https://www.typescriptlang.org/tsconfig/#target) specifies the ECMAScript target version for the _JavaScript output_.

`esnext` is great for us, because we're "starting a new project" and we want to use the latest. I recommend pegging this to a specific version before you ship version 1.0 of your project. e.g. `es2024`.

## Assignment

1. [ ] Create a new Node project

```sh
npm init -y
```

2. [ ] Add a `tsconfig.json` file to the root of your directory.
    - [ ] Set `lib` and `target` to `esnext`
    - [ ] Set `strict` to `false`
3. [ ] Create an `index.ts` file with following code:

```ts
function main(): void {
  const projectName = "support.ai";
  welcome(projectName);
}

function welcome(name) {
  return "Hello, " + name.toLowerCase();
}

main();
```

Okay, this one's a bit weird, but just roll with it for now.

4. [ ] Run `tsc` to make sure it compiles. You should see a newly compiled `index.js` file.

# More tsconfig.json

We're not going to poke through every option in the [`tsconfig.json`](https://www.typescriptlang.org/tsconfig/) file... that's what docs are for (it's like really really really long). But let's at least cover some of the most common stuff. From most to least important compiler options (imo):

- [`lib`](https://www.typescriptlang.org/tsconfig/#lib): Add `dom` and `dom.iterable` (note: lowercase) to the list of libraries to allow all the browser APIs if you're writing front-end code.
- [`strict`](https://www.typescriptlang.org/tsconfig/#strict): If `true`, enables all strict type checking options. I strongly recommend it for new projects. You might need to turn it off if you're migrating an existing JS project.
- [`skipLibCheck`](https://www.typescriptlang.org/tsconfig/#skipLibCheck): If `true`, skips type checking of all declaration files (which means it won't try to type check your infinitely large `node_modules` folder). Drastically speeds up compilation time.
- [`verbatimModuleSyntax`](https://www.typescriptlang.org/tsconfig/#verbatimModuleSyntax): If `true`, simplifies some weirdness with importing and exporting types, basically it forces you to import and export types using the `import type` syntax. I recommend it.
- [`esModuleInterop`](https://www.typescriptlang.org/tsconfig/#esModuleInterop): If `true`, allows you to use `import` syntax with CommonJS modules. Very useful if you need to work with CommonJS (Node) code.
- [`moduleDetection`](https://www.typescriptlang.org/tsconfig/#moduleDetection): If set to `force`, will consider everything to be a module, which is what you want in any new project.
- [`noUncheckedIndexedAccess`](https://www.typescriptlang.org/tsconfig/#noUncheckedIndexedAccess): If `true`, adds `undefined` to the type of any indexed access, which can prevent some runtime errors. I recommend it.

# Declaration Files

If you've ever seen funky looking `.d.ts` files and wondered what they are, they're _declaration files_. They only contain type information - no runtime code is allowed. They're very useful for defining the types for JavaScript code that _exists_ in your app, but that doesn't have any type information.

For example, in Boot.dev we support login with Google. We use TypeScript in our codebase, but we just include Google's JavaScript library in our HTML as per their instructions. Because we want the static type hints in our editors, we have this `globals.d.ts` file in our project:

```typescript
declare global {
  interface Window {
    google: Google;
  }
}

interface Google {
  accounts: {
    id: {
      renderButton: (
        a: HTMLElement,
        b: {
          type?: string;
          theme?: string;
          size?: string;
          text?: string;
          shape?: string;
          width?: number;
        },
      ) => void;
      prompt: () => void;
      cancel: () => void;
      initialize: ({ client_id: string, callback }) => void;
      disableAutoSelect: () => void;
      revoke: (client_id: string, callback) => void;
    };
  };
}

export {};
```

It just says, "Hey, there's a global variable called `google` on the `window` object, and it has this shape." Now we can use `window.google` in our code and get type hints in our editor. It doesn't do anything for us at runtime, but it makes our lives much easier when writing the code.

When debugging, if you make changes, be sure to recompile with `tsc` and hard refresh the browser page.

# Using JS Libraries

When you're starting a new TypeScript project, you're all bright-eyed and bushy tailed, thinking to yourself, "Gee, I'm gonna have amazing type safety all throughout my project".

And then your project manager walks in and says, hey you're gonna need to `npm install pregnantgoku` and use that for this feature. Much to your dismay, `pregnantgoku` doesn't have any type definitions!

You have a couple of options:

1. Allow the `any` types to flow through your code
2. Create your own type definitions
3. Check [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) and see if they have definitions for the library

DefinitelyTyped is a community-driven repository of type definitions for popular JavaScript libraries, and it's a great place to start. That said, this is a course about learning how stuff works, so let's focus on creating your own type definitions for an existing JS library. It's not hard!

You just create a new file in your project. For example, `pregnantgoku.d.ts` and add the following:

```typescript
declare module "pregnantgoku" {
  export function kamehameha(target: [number, number]): void;
  export type Saiyan = {
    name: string;
    monthsAlong: number;
    powerLevel: number;
  };
}
```

Now TypeScript will use this type information when you import `pregnantgoku` in your code.

For internal modules, you could just export the types. For example, if you have a `pregnantgoku.js` file, you could write a `pregnantgoku.d.ts` file like this:

```ts
export function kamehameha(target: [number, number]): void;
export type Saiyan = {
  name: string;
  monthsAlong: number;
  powerLevel: number;
};
```

# TypeScript Language Server

We've used `tsc` from the command line to compile our TypeScript code, but that's often _not_ how it's used in practice... at least not directly. In many projects, TypeScript is primarily used as a [language server](https://code.visualstudio.com/docs/languages/typescript) to provide type checking and other features in your IDE. It powers stuff like:

- Auto-completion
- Type checking (showing and underlining errors in your editor)
- Jumping to definitions

That's why we sometimes joke about TypeScript being a "glorified linter". There are a few reasons I point out this distinction, but the first is to understand that your editor tooling and your build tooling are _separate_. If your editor is using TypeScript 4 and one `tsconfig.json` file, but your build tooling is using TypeScript 5 and another `tsconfig.json` file, you can run into scenarios where your editor and what's being compiled in production are out of sync.

All this to say... **keep them in sync**! Most editors are smart at this and do it automatically, but it's worth knowing how to check what your editor is using under the hood.

1. Most editors will default to using the version of TypeScript installed in your project. If you have a `node_modules` folder, it will use that version.
2. If you don't, or you don't have your editor opened to the right project directory, it will likely use:
    - The version of TypeScript installed globally on your machine
    - The version of TypeScript bundled in the editor itself
    - The version of TypeScript specified in your editor settings (e.g. `.vscode/settings.json`, `.zed/settings.json`, etc.)

_Pin the version of TypeScript in your project to avoid this confusion_, and make sure your editor is using _that_ version.

## Restarting the TS Server

Language servers are notorious for kinda just... getting stuck. If you notice that your editor is not picking up on type errors, or is not providing auto-completion, step #1 should probably be to restart the TypeScript language server.

- In VS Code: `Cmd/Ctrl+Shift+P` > `TypeScript: Restart TS Server`
- In Zed: `Cmd/Ctrl+Shift+P` > `Restart Language Server`
- In Neovim: `:LspRestart`

# TypeScript Ignore

I try _really hard_ to avoid using these, but sometimes they are the best choice among a host of bad choices.

`// @ts-ignore`: _Ignores the next line's errors_.

```typescript
// @ts-ignore
const x: number = "not a number"; // Error suppressed
```

`// @ts-nocheck`: _Disables type checking for the entire file_.

```ts
// @ts-nocheck

const x: number = "not a number"; // No error

const sum(x: number, y: number): string {
  return x + y; // No error
}
```

These comments do what they say on the box: dangerously suppress type errors. Use them _very_ sparingly, or ideally, not at all.

# Vanilla Vite

Okay we've built our little TypeScript script, and used `tsc` to compile it manually. But let's scrap all that and scaffold a front-end project using [Vite](https://vite.dev/).

It's quite rare these days to build a front-end application without _some_ sort of build tool. I'm not saying you can't – we just did – I'm just saying I don't see it often in the wild. Especially considering most front-end frameworks (React, Vue, Svelte, etc.) have their own build tools.

## What Is Vite?

[Vite](https://vitejs.dev/) is a new(ish) build tool that focuses on speed and simplicity. It does a few key things that make it great for many projects, and even vanilla TypeScript apps:

- **Fast**: Vite uses the Golang-powered [esbuild](https://esbuild.github.io/) to do heavy lifting, which is partially why it's so fast. It also uses [Rollup](https://rollupjs.org/) under the hood for production builds.
- **Simple**: Vite is simple to use and configure, at least for a JS/TS tool (look, we're comparing it to [Webpack](https://webpack.js.org/) here, so it's all relative).
- **Server**: Vite has a built-in development server that serves your files and handles hot module replacement (HMR) for you.
- **Compiler**: Vite compiles your code in development on the fly, so you don't have to manually rebuild each time you make a change.

## Assignment

1. [ ] Install Vite

```bash
npm i -D vite
```

2. [ ] Commit _all_ your old code and delete it, even the `tsconfig.json` and `package.json` files. We won't need it where we're going, we want a clean slate.
3. [ ] Create a new Vite project using the vanilla TypeScript template:

```sh
npm create vite@latest . -- --template vanilla-ts
```

4. [ ] Install the dependencies:

```sh
npm i
```

5. [ ] Run the development server and take a look at the simple webpage it is serving:

```sh
npm run dev
```

Take a look at the `package.json`. Notice that it has `vite` and `typescript` as dependencies now. The `dev` command we ran simply runs `tsc` to compile your code and `vite` to serve it, all in one step.

6. [ ] Notice that when you click the "count is x" button in the browser, the count increments.
7. [ ] Update `src/counter.ts` to increment it by `10` instead of `1` each time it's clicked.

Notice that when you update the code and save the file, Vite will automatically recompile your code and update the browser for you!

