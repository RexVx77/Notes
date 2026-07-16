CH1: Introduction
# The Console

The "console" shows you the text output of your program. It's right beneath the code editor.

To see what's happening in our code, we need to _print it to the console_ by using the `print()` function.

We'll learn more about functions later, but for now, just know that `print()` function will print anything you put inside its parentheses, like this:

```py
print("Greetings, adventurer!")
```

# What Is “Code”?

Code is just a series of instructions for a computer to follow one after another. Programs can have _a lot_ of instructions.

The Boot.dev backend has `46,119` lines of code as I write this... and it's smaller than many other programs!

Remember how we used the `print()` instruction to print _text_ to the console, like this?

```py
print("this is a string of text")
```

We can also use it to print _numbers_. Numbers, unlike plain [text strings](https://en.wikipedia.org/wiki/String_\(computer_science\)), _are not_ surrounded in quotes. This prints the number `42`:

```py
print(42)
```

## Adding Numbers

[Addition](https://en.wikipedia.org/wiki/Addition) is one of the most common instructions in programming. This _also_ prints the number `42`:

```py
print(40 + 2)
```

First it calculates the sum inside the parentheses, _then_ it prints the result.

# Multiple Instructions

Code runs in order, starting at the top of the program. For example:

```py
print("this prints first")
print("this prints second")
print("this prints last")
```

Each `print()` instruction prints on a new line.

# Syntax Errors

["Syntax"](https://en.wikipedia.org/wiki/Syntax_\(programming_languages\)) is jargon for "valid code that the computer can understand." For example, the following code has _invalid_ syntax:

```py
print("hello world')
```

It has mismatched quotes around the string `hello world`. One is a single quote `'` and the other is a double quote `"`.

# Syntax Errors Quiz

**Syntax**: The rules for how [expressions](https://en.wikipedia.org/wiki/Expression_\(computer_science\)) and [statements](https://en.wikipedia.org/wiki/Statement_\(computer_science\)) should be structured in a language. For example, in Python, the following is _correct_ syntax:

```py
print("hello world")
```

While in a different programming language, like Go, the correct syntax would be:

```go
fmt.Println("hello world")
```

Syntax errors aren't the _only_ kind of problems you can run into when coding, for example:

- **A bug in your logic**. Your code is _valid_, and will _run_, but it does something unexpected.
- **It's too slow**. Your code is _valid_ and does _what's expected_, but it does it slowly.

_In this course, we're just concerned with syntax and logic errors. We'll cover performance issues in a later course_.

---

CH2: Variables

# Variables

[Variables](https://www.cs.utah.edu/~germain/PPS/Topics/variables.html) are how we _store_ data as our program runs. Up 'til now we've been _printing_ data by passing it straight into [`print()`](https://docs.python.org/3/library/functions.html#print). Now we're going to _save_ the data in variables so we can reuse it and change it _before_ printing it.

## Creating Variables

A "variable" is just a name that we give to a value. For example, we can make a new variable named `my_height` and set its value to `100`:

```py
my_height = 100
```

Or we can define a variable called `my_name` and set it to the text string `"Lane"`:

```py
my_name = "Lane"
```

We have the freedom to choose any name for our variables, but they should be _descriptive_ and consist of a single ["token"](https://en.wikipedia.org/wiki/Lexical_analysis#Token), meaning continuous text with underscores separating the words.

## Using Variables

Once we have a variable, we can access its value by using its name. For example, this will print `100`:

```py
print(my_height)
```

And this will print `Lane`:

```py
print(my_name)
```

# Variables Vary

Variables are called "variables" because they can hold any value and that value can change (it varies).

For example, this code prints `20`:

```py
acceleration = 10
acceleration = 20
print(acceleration)
```

The line `acceleration = 20` _reassigns_ the value of `acceleration` to 20. It _overwrites_ whatever was being held in the `acceleration` variable before (10 in this case).

# Math

Now that we know how to store and change the value of variables let's do some math! Here are examples of common mathematical operators using Python syntax.

## Addition

```py
my_sum = a + b
```
## Subtraction

```py
my_difference = x - y
```
## Multiplication

```py
my_product = c * d
```
## Division

```py
my_quotient = a / b
```

## Order of Operations

Parentheses can be used to [order math operations](https://www.mathsisfun.com/operation-order-pemdas.html):

```py
first = 5
second = 7
third = 9
average_value = (first + second + third) / 3
print(average_value)
```

Which prints `7`, the average of `5`, `7`, and `9`.

# Negative Numbers

Negative numbers in Python work the way you probably expect. Just add a minus sign:

```py
my_negative_num = -420
```

# Comments

Comments don't do... anything. They are _ignored_ by the Python interpreter. That said, they're good for what the name implies: adding comments to your code in plain English (or whatever language you speak).

## Single Line Comment

A single `#` makes the rest of the line a comment:

```py
# speed describes how fast the player
# moves in meters per second
speed = 2
```

## Multi-Line Comments (Aka [docstrings](https://peps.python.org/pep-0257/))

You can use triple quotes to start and end multi-line comments as well:

```py
"""
    the code found below
    will print 'Hello, World!' to the console
"""
print("Hello, World!")
```

This is useful if you don't want to add the `#` to the start of each line when writing paragraphs of comments.

# Variable Names

Variable names _must not_ have spaces. They're continuous strings of characters.

The creator of the Python language himself, [Guido van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum), [implores us](https://peps.python.org/pep-0008/#function-and-variable-names) to use `snake_case` for variable names. What _is_ snake case? It's just a style for writing variable names. Here are some examples of different casing styles:

|Name|Description|Code|Language(s) that recommend it|
|---|---|---|---|
|Snake Case|All words are lowercase and separated by underscores|`num_new_users`|Python, Ruby, Rust|
|Camel Case|Capitalize the first letter of each word _except the first one_|`numNewUsers`|JavaScript, Java|
|Pascal Case|Capitalize the first letter of each word|`NumNewUsers`|C#, C++|
|No Casing|All lowercase with no separation|`numnewusers`|...just don't do this|

To be clear, your Python code will still _work_ with Camel Case or Pascal Case, but can we please just have nice things? We just want some consistency in our craft.

If you won't use snake case for _you_, do it for _me_. I beg you.

# Basic Variable Types

Python has several basic [data types](https://en.wikipedia.org/wiki/Data_type).

## Strings

In programming, snippets of text are called "[strings](https://docs.python.org/3/library/stdtypes.html#textseq)." They're lists of characters _strung_ together. We create strings by wrapping the text in single quotes or double quotes. That said, **double quotes are preferred**.

```py
name_with_single_quotes = 'boot.dev' # not so good
name_with_double_quotes = "boot.dev" # so good
```
## Numbers

Numbers are _not_ surrounded by quotes when they're declared.

**An [integer](https://docs.python.org/3/c-api/long.html) (or "int") is a number without a decimal part**:

```py
x = 5 # positive integer
y = -5 # negative integer
```

**A [float](https://docs.python.org/3/library/functions.html#float) is a number with a decimal part**:

```py
x = 5.2
y = -5.2
```
## Booleans

A ["Boolean"](https://docs.python.org/3/c-api/bool.html#boolean-objects) (or "bool") is a type that can only have one of two values: `True` or `False`. As you may have heard, computers really only use 1's and 0's. These 1's and 0's are just `True/False` boolean values.

```py
is_tall = True
is_short = False
```

# F-Strings in Python

Have you ever played old-school Pokemon and chosen a funny name so that the in-game messages would come out funny?

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/wh2qsdq-500x370.png)

In Python, we can create strings that contain dynamic values with the [f-string](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals) syntax.

```py
num_bananas = 10
bananas = f"You have {num_bananas} bananas"
print(bananas)
# You have 10 bananas
```

- Add an `f` to the start of quotes to create an f-string: `f"this is easy!"`
- Use curly brackets `{}` around a variable to [interpolate](https://en.wikipedia.org/wiki/String_interpolation) (put) its value into the string.

You can also use an f-string directly inside a `print()` call, without assigning it to a variable first. It's just a string – don't overthink it!

# NoneType Variables

Not all variables have a value. We can make an "empty" variable by setting it to [`None`](https://docs.python.org/3/library/constants.html#None). `None` is a special value in Python that represents the absence of a value. It is _not_ the same as zero, False, or an empty string.

```py
my_mental_acuity = None
```

The value of `my_mental_acuity` in this case is `None` until we use the assignment operator, `=`, to give it a value.

## None Is NOT a String

[NoneType](https://docs.python.org/3/library/types.html#types.NoneType) is _not_ the same as a string with a value of "None":

```py
my_none = None # this is a None-type
my_none = "None" # this is a string with the value "None"
```

# NoneType Quiz

As we mentioned in the last exercise, the `None` keyword is used to define an "empty" variable.

So when would you _use_ it? One use case is to represent that a value hasn't been determined yet, for example, an uncaptured input. Maybe your program is waiting for a user to enter their name. You might start with a variable:

```py
username = None
```

Then later in the code, once the user has entered their name, you can assign it to the `username` variable:

```py
username = input("What's your name? ")
```

Remember, it's crucial to recognize that `None` is not the same as the string `"None"`. They look the same when printed to the console, but they are different data types. If you use `"None"` instead of `None`, you will end up with code that looks correct when it's _printed_ but fails the _tests_. In that case, printing the _type_ would distinguish between the two values.

```py
str_none = "None"
actual_none = None

print(str_none) # None
print(actual_none) # None

print(type(str_none)) # <class 'str'>
print(type(actual_none)) # <class 'NoneType'>
```

# Dynamic Typing

Python is [dynamically typed](https://en.wikipedia.org/wiki/Type_system#Static_and_dynamic_type_checking_in_practice), which means a variable can store any type, and that type can _change_.

For example, if I make a number variable, I can later change that variable to a string:

```py
speed = 5
speed = "five"
```

## But Like, Maybe Don't

In almost all circumstances, it's a _bad idea_ to change the type of a variable. The "proper" thing to do is to just create a new one. For example:

```py
speed = 5
speed_description = "five"
```

## What Is Non-Dynamic Typing?

Languages that aren't dynamically typed are [statically typed](https://developer.mozilla.org/en-US/docs/Glossary/Static_typing), such as Go and Typescript (one of which you'll learn in a later course depending on your chosen track). In a statically typed language, if you try to assign a value to a variable of the wrong type, you'll get a compile-time error and the program won't run.

If Python were statically typed, the first example from before wouldn't allow the second line, `speed = "five"`. The computer would give an error along the lines of `you can't assign a string value ("five") to a number variable (speed)`.

# Math With Strings

When working with strings the `+` operator performs a "[concatenation](https://en.wikipedia.org/wiki/Concatenation)," which is a fancy word that means "joining two strings." _Generally speaking, it's better to use string interpolation with `f-strings` over `+` concatenation_.

```py
first_name = "Lane "
last_name = "Wagner"
full_name = first_name + last_name
print(full_name)
# prints "Lane Wagner"
```

`full_name` now holds the value `"Lane Wagner"`.

Notice the extra space at the end of `"Lane "` in the `first_name` variable. That extra space is there to separate the words in the final result: `"Lane Wagner"`.

# Multi-Variable Declaration

We can save space when creating many new variables by declaring them on the same line:

```py
sword_name, sword_damage, sword_length = "Excalibur", 10, 200
```

Which is the same as:

```py
sword_name = "Excalibur"
sword_damage = 10
sword_length = 200
```

Any number of variables can be declared on the same line, and variables declared on the same line _should_ be related to one another in some way so that the code remains easy to understand.

We call code that's easy to understand "clean code."



You need to use the correct type-specific syntax for the variables. Here are some examples:

|Type|Rule|Example|
|---|---|---|
|String|Double quotes|"Hello"|
|Integer|Whole number|42|
|Float|Use a Decimal|2.0|
|Boolean|True or False|True|

---

CH3: Functions

# Functions

Functions allow us to _reuse_ and _organize_ code. For example, say we have some code that calculates the area of a circle:

```py
radius = 5
area = 3.14 * radius * radius
```

That works! The problem is when we want to calculate the area of _other_ circles, each with its own radius. We _could_ just copy the code and change the variable names like this:

```py
radius = 5
area1 = 3.14 * radius * radius

radius2 = 7
area2 = 3.14 * radius2 * radius2
```

But we want to _reuse_ our code! Why would we want to redo our work? What if we wanted to calculate the area of thousands of circles??? **That's where functions help.**

Instead, we can define a new function called `area_of_circle` using the `def` keyword.

```py
def area_of_circle(r):
    pi = 3.14
    result = pi * r * r
    return result
```

Let's break this `area_of_circle` function down:

- It takes one input (aka "parameter" or "argument") called `r`
- After the `:`, the _indented lines_ form the function body – this is the code block that will run each time we use (aka "call") the function
- It `return`s a single value (the output of the function). In this case, it's the value stored in the `result` variable

To ["call"](https://en.wikibooks.org/wiki/Python_Programming/Functions#Function_Calls) this function (fancy programmer speak for "use this function") we can pass in any number as the argument (in this case, `5`) for the parameter `r`, and capture the output into a new variable:

```py
area = area_of_circle(5)
print(area)
# 78.5
```

1. `5` goes in as the input `r`
2. The body of the function runs, which stores `78.5` in the `result` variable within the function body
3. The function returns the `result` variable, which means the `area_of_circle(5)` expression evaluates to `78.5`
4. `78.5` is stored in the `area` variable and then printed

Because we've already _defined_ the function, now we can use it as many times as we want with different inputs!

```py
area = area_of_circle(6)
print(area)
# 113.04

area = area_of_circle(7)
print(area)
# 153.86
```

# Function Review

Functions are tricky! It takes a minute to get used to them, but after that they'll be second nature to you. You might find yourself slowing down a bit in this chapter, and if you do, that's totally normal.

Click to hide video

Let's break down this function line by line so you can understand every nook and cranny of it.

```py
def area_of_circle(r):
    pi = 3.14
    result = pi * r * r
    return result

radius = 5
area = area_of_circle(radius)
print(area)
# 78.5
```

Here's a chronological explanation of what happens when the above code is executed:

1. `def area_of_circle(r):`
    
    The `area_of_circle` function is defined for later use, but _not_ called. It accepts a single input, the arbitrarily named `r`. The body of the function (`pi = 3.14`... etc) is ignored for now.
    
2. `radius = 5`
    
    A new variable called `radius` is created and set to the value `5`.
    
3. `area_of_circle(radius)`
    
    The `area_of_circle` function is called with `radius` (in this case 5) as the input. Finally, we jump back to the function definition.
    
4. `def area_of_circle(r):`
    
    We will now start executing the body of the function, and `r` is set to `5`.
    
5. `pi = 3.14`
    
    A new variable called `pi` is created with a value of `3.14`.
    
6. `result = pi * r * r`
    
    Some simple math is evaluated (`3.14 * 5 * 5`) and stored in the `result` variable.
    
7. `return result`
    
    The result variable is returned from the function as output.
    
8. `area = area_of_circle(radius)`
    
    The returned value is stored in a new variable called `area` (in this case `78.5`).
    
9. `print(area)`
    
    The value of `area` is printed to the console.

# Multiple Parameters

Functions can have multiple parameters ("parameter" being a fancy word for "input"). For example, this `subtract` function accepts 2 parameters: `a` and `b`.

```py
def subtract(a, b):
    result = a - b
    return result
```

It's the argument's **position** that determines which parameter receives it (at least, for now). The first argument goes to the first parameter, the second to the second, and so on. In this example, the `subtract` function is called with `a = 5` and `b = 3`:

```py
result = subtract(5, 3)
print(result)
# 2
```

Here's an example with four parameters:

```py
def create_introduction(name, age, height, weight):
    first_part = "Your name is " + name + " and you are " + age + " years old."
    second_part = "You are " + height + " meters tall and weigh " + weight + " kilograms."
    full_intro = first_part + " " + second_part
    return full_intro
```

It can be called like this:

```py
my_name = "John"
my_age = "30"

intro = create_introduction(my_name, my_age, "1.8", "80")
print(intro)
# Your name is John and you are 30 years old. You are 1.8 meters tall and weigh 80 kilograms.
```

The `pass` statement is a placeholder that does nothing. 

# Printing vs. Returning

Some new developers get hung up on the difference between `print()` and `return`.

It can be particularly confusing when the test suite we provide simply prints the output of your functions to the console. It makes it _seem_ like `print()` and `return` are interchangeable, _but they are not_!

- `print()` is a function that:
    1. Prints a value to the console
    2. Does _not_ return a value
- `return` is a keyword that:
    1. Ends the current function's execution
    2. Provides a value (or values) back to the caller of the function
    3. Does _not_ print anything to the console (unless the return value is later `print()`ed)

## Printing to Debug Your Code

Printing values and running your code is a great way to debug your code. You can see what values are stored in various variables, find your mistakes, and fix them. Add print statements and run your code as you go! It's a great habit to get into to make sure that each line you write is doing what you expect it to do.

In the real world it's rare to leave `print()` statements in your code when you're done debugging. Similarly, you need to remember to remove any `print()` statements from your code before submitting your work here on Boot.dev because it will interfere with the tests!

# Where to Declare Functions

You've probably noticed that a variable needs to be declared _before_ it's used. For example, the following doesn't work:

```py
print(my_name)
my_name = 'Lane Wagner'
# NameError: 'my_name' is not defined
```

It needs to be:

```py
my_name = 'Lane Wagner'
print(my_name)
# Lane Wagner
```

Code executes in _order from top to bottom_, so a variable needs to be created before it can be used. That means that if you define a function, you can't call that function until _after_ it has been defined.


# Order of Functions

All functions _must_ be defined before they're used.

You might think this would make structuring Python code hard because the order of the functions needs to be _just right_. As it turns out, there's a simple trick that makes it super easy.

Most Python developers solve this problem by defining _all_ the functions in their program first, then they call an "entry point" function _at the end of the file_. That way _all_ of the functions have been read by the Python interpreter before the first one is called.

Conventionally this "entry point" function is usually called `main` to keep things simple and consistent.

```py
def main():
    health = 10
    armor = 5
    add_armor(health, armor)

def add_armor(h, a):
    new_health = h + a
    print_health(new_health)

def print_health(new_health):
    print(f"The player now has {new_health} health")

# call entrypoint at the end
main()
```

# None Return

When no return value is specified in a function, it will automatically return `None`. For example, maybe it's a function that prints some text to the console, but doesn't explicitly return a value. The following code snippets all return the same value, `None`:

```py
def my_func():
    print("I do nothing")
    return None
```

```py
def my_func():
    print("I do nothing")
    return
```

```py
def my_func():
    print("I do nothing")
```

# Multiple Return Values

A function can return more than one value by separating them with commas.

```py
def cast_iceblast(wizard_level, start_mana):
    damage = wizard_level * 2
    new_mana = start_mana - 10
    return damage, new_mana # return two values
```

## Receiving Multiple Values

When calling a function that returns multiple values, you can assign them to multiple variables.

```py
damage, mana = cast_iceblast(5, 100)
print(f"Damage: {damage}, Remaining Mana: {mana}")
# Damage: 10, Remaining Mana: 90
```

When `cast_iceblast` is called, it returns two values. The first value is assigned to `damage`, and the second value is assigned to `mana`. Just like function inputs, it's the _order_ of the values that matters, not the variable names. We could just as easily have named the variables `one` and `two`:

```py
one, two = cast_iceblast(5, 100)
print(f"Damage: {one}, Remaining Mana: {two}")
# Damage: 10, Remaining Mana: 90
```

Descriptive variable names make your code easier to understand, so name them well!

## What Happened to the Variables?

The `damage` and `new_mana` variables from `cast_iceblast`'s function body only exist _inside_ of the function. They can't be used outside of the function. More on that later when we talk about scope.

# Parameters vs. Arguments

Parameters are the names used for inputs when _defining_ a function. Arguments are the values of the inputs supplied when a function is _called_.

To reiterate, **arguments are the actual values** that go into the function, such as `42.0`, `"the dark knight"`, or `True`. **Parameters are the names** we use in the function definition to refer to those values, which at the time of writing the function, can be whatever we like.

That said, this is all semantics, and frankly developers are really lazy with these definitions. You'll often hear the words "arguments" and "parameters" used interchangeably.

```py
# a and b are parameters
def add(a, b):
    return a + b

# 5 and 6 are arguments
sum = add(5, 6)
```

# Default Values

In Python you can specify a [default](https://docs.python.org/3/glossary.html#term-parameter) value for a function parameter. It's nice for when a function has parameters that are "optional." You as the function definer can specify a specific default value in case the caller doesn't provide one.

A default value is created by using the assignment (`=`) operator in the function signature.

```py
def get_greeting(email, name="there"):
    print("Hello", name, "welcome! You've registered your email:", email)
```

```py
get_greeting("lane@example.com", "Lane")
# Hello Lane welcome! You've registered your email: lane@example.com
```

```py
get_greeting("lane@example.com")
# Hello there welcome! You've registered your email: lane@example.com
```

If the second parameter is omitted, the default `"there"` value will be used in its place. As you may have guessed, for this structure to work, optional parameters (the ones with defaults) must come _after_ all required parameters.

You can multiply a number by a decimal to get a percentage of a number. For example:

30% of 50 is `50 * 0.3`

---

CH4: Scope

# Scope

Scope refers to _where_ a variable or function name is available to be used. For example, when we create variables in a function (such as by giving names to our parameters), that data is _not_ available outside of that function.

## Example

```py
def subtract(x, y):
    return x - y
result = subtract(5, 3)
print(x)
# ERROR! "name 'x' is not defined"
```

When the `subtract` function is called, we assign 5 to the variable `x`, but `x` only exists in the code _within_ the `subtract` function. If we try to print `x` outside of that function, then we won't get a result. In fact, we'll get a big fat error.

# Global Scope

So far we've been working in the global scope. That means that when we define a variable or a function, that name is accessible in _every other place_ in our program, even within other functions.

For example:

```py
pi = 3.14

def get_area_of_circle(radius):
    return pi * radius * radius
```

Because `pi` was declared in the parent "global" scope, it is usable within the `get_area_of_circle()` function.

---

CH5: Testing and Debugging

---

CH6: Computing

# Python Numbers

In Python, numbers without a decimal point are called `Integers` – just like they are in mathematics.

Integers are simply whole numbers, positive or negative. For example, `3` and `-3` are both examples of integers.

Arithmetic can be performed as you might expect:

## Addition

```py
2 + 1
# 3
```
## Subtraction

```py
2 - 1
# 1
```
## Multiplication

```py
2 * 2
# 4
```

## Division

```py
3 / 2
# 1.5 (a float)
```

This one is actually a bit different – division on two integers will actually produce a [float](https://docs.python.org/3/tutorial/floatingpoint.html). A `float` is, as you may have guessed, the number type that allows for decimal values.

# Numbers Review

## Integers

In Python, numbers without a decimal point are called `Integers`.

Integers are simply whole numbers, positive or negative. For example, `3` and `-3` are both examples of integers.

## Floats

A float is, as you may have guessed, the number type that allows for decimal values.

```py
my_int = 5
my_float = 5.5
```

# Floor Division

Python has great out-of-the-box support for mathematical operations. This, among other reasons, is why it has had such success in artificial intelligence, machine learning, and data science applications.

Floor division is like normal division except the result is [floored](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions) afterward, which means the result is rounded down to the nearest integer. The `//` operator is used for floor division.

```py
7 // 3
# 2 (an integer, rounded down from 2.333)
-7 // 3
# -3 (an integer, rounded down from -2.333)
```

# Exponents

Python has built-in support for exponents – something most languages require a `math` library for.

```py
# reads as "three squared" or
# "three raised to the second power"
3 ** 2
# 9
```

Sometimes exponents are also shown _in text_ using the caret symbol (`^`):

`5^3` = 53

# Changing in Place

It's fairly common to want to change the value of a variable based on its current value.

```py
player_score = 4
player_score = player_score + 1
# player_score now equals 5
```

```py
player_score = 4
player_score = player_score - 1
# player_score now equals 3
```

Don't let the fact that the expression `player_score = player_score - 1` is not a valid mathematical expression confuse you. _It doesn't matter_, it _is valid code_. It's valid because the way the expression should be read in English is:

> Assign to `player_score` the current value of `player_score` minus 1

In this operation, the right-hand side (`player_score - 1`) is calculated first. Once we have the result, we update `player_score` with this new value.

# Plus Equals

Python makes reassignment easy when doing math. In JavaScript or Go, you might be familiar with the `++` syntax for incrementing a number variable by 1. In Python, we use the `+=` [in-place operator](https://docs.python.org/3/library/operator.html#in-place-operators) instead.

```py
star_rating = 4
star_rating += 1
# star_rating is now 5
```

## Other Operators

The other in-place operators work similarly:

```py
star_rating = 4
star_rating -= 1
# star_rating is now 3

star_rating = 4
star_rating *= 2
# star_rating is now 8

star_rating = 4
star_rating /= 2
# star_rating is now 2.0
```

# Scientific Notation

As we covered earlier, a `float` is a positive or negative number **with a fractional part**.

You can add the letter `e` or `E` followed by a positive or negative integer to specify that you're using [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation).

```py
print(16e3)
# Prints 16000.0

print(7.1e-2)
# Prints 0.071
```

If you're not familiar with scientific notation, it's a way of expressing numbers that are too large or too small to conveniently write normally.

In a nutshell, the exponent following the `e` specifies how many places to move the decimal: right when the exponent is positive, left when it's negative.

## Underscores for Readability

Python also allows you to represent large numbers in the decimal format using underscores as the [delimiter](https://en.wikipedia.org/wiki/Decimal_separator#Digit_grouping) instead of commas to make it easier to read.

```py
num = 16_000
print(num)
# Prints 16000

num = 16_000_000
print(num)
# Prints 16000000
```
Numbers in scientific notation are floats. For example, `1.024e18` is actually equivalent to `1,024,000,000,000,000,000.0` (note the `.0` at the end). 

# Logical Operators

You're probably familiar with the logical operators `and` and `or`.

Logical operators deal with [boolean values](https://en.wikipedia.org/wiki/Boolean_data_type), `True` and `False`.

The logical `and` operator requires that _both_ inputs are `True` to return `True`. The logical `or` operator only requires that _at least one_ input is `True` to return `True`.

For example:

```py
True and True == True
True and False == False
False and False == False

True or True == True
True or False == True
False or False == False
```

## Python Syntax

```py
print(True and True)
# prints True

print(True or False)
# prints True
```

## Nesting With Parentheses

We can nest logical expressions using parentheses.

```py
print((True or False) and False)
```

First, we evaluate the expression in the parentheses, `(True or False)`. It evaluates to `True`:

```py
print(True and False)
```

`True and False` evaluates to `False`:

```py
print(False)
```

So, `print((True or False) and False)` prints "False" to the console.

# Not

We skipped a very important logical operator – `not`. The `not` operator reverses the result. It returns `False` if the input was `True` and vice-versa.

```py
print(not True)
# Prints: False

print(not False)
# Prints: True
```

# Binary Numbers

[Binary numbers](https://en.wikipedia.org/wiki/Binary_number) are just "base 2" numbers. They work the same way as "normal" base 10 numbers, but with two symbols instead of ten.

- Base-2 (binary) symbols: `0` and `1`
- Base-10 (decimal) symbols: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`

Click to hide video

Each digit's place value is double the place to its right. In a 4-digit binary number, the place values are:

- Eights
- Fours
- Twos
- Ones

For example, `0101` means:

- `0` eights
- `1` four
- `0` twos
- `1` one

So `0101` is `5` in decimal.

## Binary in Python

You can write an integer in Python using binary syntax using the `0b` prefix:

```py
print(0b0001)
# Prints 1

print(0b0101)
# Prints 5
```

Leading 0s are often added for visual consistency but do not change the value of a binary number.

# Bitwise “&” Operator

Bitwise operators are similar to logical operators, but instead of operating on boolean values, they apply the same logic to all the bits in a value by column. For example, say you had the numbers `5` and `7` represented in [binary](https://en.wikipedia.org/wiki/Binary_code). You could perform a bitwise AND operation and the result would be `5`.

- `0101` is 5
- `0111` is 7

```text
0101
&
0111
=
0101
```

A `1` in binary is the same as `True`, while `0` is `False`. So really a bitwise operation is just a bunch of logical operations that are completed in tandem by column.

```py
0 & 0 = 0

1 & 1 = 1

1 & 0 = 0
```

Ampersand `&` is the bitwise AND operator in Python. "AND" is the _name_ of the bitwise operation, while ampersand `&` is the _symbol_ for that operation. For example, `5 & 7 = 5`, while `5 & 2 = 0`.

- `0101` is 5
- `0010` is 2

```text
0101
&
0010
=
0000
```

## Binary Notation

When writing a number in binary, the prefix `0b` is used to indicate that what follows is a binary number. `0b10` is two in binary, but `10` without the `0b` prefix is simply ten.

- `0b0101` is 5
- `0b0111` is 7

## Putting It Together

```py
0b0101 & 0b0111
# equals 5

binary_five = 0b0101
binary_seven = 0b0111
binary_five & binary_seven
# equals 5
```

# Bitwise “|” Operator

As you may have guessed, the bitwise "or" operator is similar to the bitwise "and" operator in that it works on binary rather than boolean values. However, the bitwise "or" operator "or"s the bits together. Here's an example:

- `0101` is 5
- `0111` is 7

```text
0101
|
0111
=
0111
```

A `1` in binary is the same as `True`, while `0` is `False`. So a bitwise operation is just a bunch of logical operations that are completed in tandem. When two binary numbers are "or"ed together, the result has a `1` in any place where _either_ of the input numbers has a `1` in that place.

`|` is the bitwise "or" operator in Python. `5 | 7 = 7` and `5 | 2 = 7` as well!

- `0101` is 5
- `0010` is 2

```text
0101
|
0010
=
0111
```

# Converting Binary

Fantasy Quest needs to [migrate](https://en.wikipedia.org/wiki/Data_migration) old data from strings that _look like binary_ to the integers that the binary strings represent. For example:

- `"100" -> 4`
- `"101" -> 5`
- `"10010" -> 18`

The built-in [int()](https://docs.python.org/3/library/functions.html#int) function can convert a binary string to an integer. It takes a second argument that specifies the base of the number (binary is base 2). For example:

```py
# this is a binary string
binary_string = "100"

# convert binary string to integer
num = int(binary_string, 2)
print(num)
# 4
```

---

CH7: Comparisons

# Comparison Operators

When coding it's necessary to be able to compare two values. `Boolean logic` is the name for these kinds of comparison operations that always result in `True` or `False`.

The operators:

- `<` "less than"
- `>` "greater than"
- `<=` "less than or equal to"
- `>=` "greater than or equal to"
- `==` "equal to"
- `!=` "not equal to"

For example:

```py
5 < 6 # evaluates to True
5 > 6 # evaluates to False
5 >= 6 # evaluates to False
5 <= 6 # evaluates to True
5 == 6 # evaluates to False
5 != 6 # evaluates to True
```

# Comparison Operator Evaluations

When a comparison happens, the result of the comparison is just a boolean value, it's either `True` or `False`.

Take the following two examples:

```py
is_bigger = 5 > 4
```

```py
is_bigger = True
```

In both of the above cases, we're creating a `Boolean` variable called `is_bigger` with a value of `True`.

Because `5` is greater than `4`, `is_bigger` is assigned the value of `True`.

# If Statements

It's often useful to only execute code if a certain condition is met:

```py
if CONDITION:
    # do some stuff here

# code after the if block may still run regardless
```

For example, in this code:

```py
def show_status(boss_health):
    if boss_health > 0:
        print("Ganondorf is alive!")
        return
    print("Ganondorf is unalive!")
```

if `boss_health` is greater than `0`, then this will be printed:

```text
Ganondorf is alive!
```

Otherwise, this will be printed:

```text
Ganondorf is unalive!
```

Without a `return` in the `if` block, `Ganondorf is unalive` would always be printed:

```py
def show_status(boss_health):
    if boss_health > 0:
        print("Ganondorf is alive!")
    print("Ganondorf is unalive!")
```

This code could print both messages:

```text
Ganondorf is alive!
Ganondorf is unalive!
```

When you only want code within an `if` block to run, use `return` to exit the function early.

Indentation is what tells Python whether the body of a function or the if statement has ended. Don't forget the colon after your if statement `:`; it is a required part of the syntax!

# If-Else

An `if` statement can be followed by zero or more `elif` (which stands for "else if") statements, which can be followed by zero or one `else` statements.

For example:

```py
if score > high_score:
    print("High score beat!")
elif score > second_highest_score:
    print("You got second place!")
elif score > third_highest_score:
    print("You got third place!")
else:
    print("Better luck next time")
```

First the `if` statement is evaluated. If it is `True` then the if statement's body is executed and all the other `elif`s and the `else` are ignored.

If the first `if` is false then the next `elif` is evaluated. Likewise, if it is `True` then its body is executed and the rest are ignored.

If none of the `if` or `elif` statements evaluate to `True` then the final `else` statement will be the only body executed.

# If-Else Practice

Here are some basic rules with if/else blocks.

- You can't have an `elif` or an `else` without an `if`
- You **_can_** have an `else` without an `elif`

Remember, to check if two values are the same use the `==` operator.

```py
are_equal = 5 == 6
# are_equal is False

are_equal = 6 == 6
# are_equal is True
```

# If-Else Practice

Here are some basic rules with if/else blocks.

- You can't have an `elif` or an `else` without an `if`
- You _can_ have an `else` without an `elif`

# Boolean Logic

Boolean logic refers to logic dealing with boolean (`True` or `False`) values. For example,

Dogs must have four legs _and_ weigh less than 100 kilograms. (Both conditions must be true)

Cars are cool if they go faster than 200 MPH, _or_ if they are electric. (At least one condition must be true)

## Logical Operators Review

As we discussed earlier, the logical operators `and` and `or` can be used to perform boolean logic.

### And Review

The `and` operator returns `True` if _both_ of the conditions on either side evaluates to `True`:

```py
def is_dog(num_legs, weight):
    return num_legs == 4 and weight < 100
```

Let's go over how this function evaluates the parameters `num_legs=4` and `weight=99`:

```py
return 4 == 4 and 99 < 100
```

```py
return True and True
```

```py
return True
```

Let's see what would happen with `3` and `98` instead:

```py
return 3 == 4 and 98 < 100
```

```py
return False and True
```

```py
return False
```

### Or Review

The `or` operator returns `True` if _at least one_ of the conditions on either side evaluates to `True`:

```py
def is_car_cool(speed, is_electric):
    return speed > 200 or is_electric
```

Let's use a non-electric car that can do 250:

```py
return 250 > 200 or False
```

```py
return True or False
```

```py
return True
```

---

CH8: Loops

# Loops

Loops are a programmer's best friend. Loops allow us to do the same operation multiple times without having to write it explicitly each time.

For example, let's pretend I want to print the numbers 0-9.

I could do this:

```py
print(0)
print(1)
print(2)
print(3)
print(4)
print(5)
print(6)
print(7)
print(8)
print(9)
```

Even so, it would save me a lot of time typing to use a _loop_. Especially if I wanted to do the same thing _one thousand_ or _one million_ times.

A _"for loop"_ in Python is written like this:

```py
for i in range(0, 10):
    print(i)
```

`i` is a variable that takes on each value from `0` to `9`, one at a time. In English, the code says:

1. Start with `i` equals `0`. (`i in range(0)`)
2. If `i` is greater than or equal to 10 (`range(0, 10)`), exit the loop. Else:
    - Print `i` to the console. (`print(i)`)
    - Add `1` to `i`. (`range` defaults to incrementing by 1)
    - Go back to step `2`.

The result is that the numbers `0-9` are logged to the console in order.

The numbers `a` and `b` in `range(a, b)` are _inclusive_ of `a` and _exclusive_ of `b`. So `range(0, 10)` includes `0` but not `10`.

## Whitespace Matters in Python!

The body of a for-loop _must_ be indented, otherwise you'll get a syntax error.
# Range Continued

The `range()` function we've been using in our `for` loops actually has an optional 3rd parameter: the "step."

```py
for i in range(0, 10, 2):
    print(i)
# prints:
# 0
# 2
# 4
# 6
# 8
```

The "step" parameter determines how much to add to `i` in each iteration of the loop. You can even go backwards:

```py
for i in range(3, 0, -1):
    print(i)
# prints:
# 3
# 2
# 1
```

# While

Python has another type of loop, the `while` loop. It's a loop that continues `while` a condition remains `True`. The syntax is simple:

```py
while 1:
    print("1 evaluates to True")

# prints:
# 1 evaluates to True
# 1 evaluates to True
# (...continuing)
```

The example above is hardcoded to continue forever, creating an infinite loop. Typically, a `while` loop condition is a comparison or variable, and it determines when the loop ends:

```py
num = 0
while num < 3:
    num += 1
    print(num)

# prints:
# 1
# 2
# 3
# (the loop stops when num >= 3)
```

# Continue Statement

Sometimes, while looping through a sequence, you may find items that you want to _skip_. Python (like many programming languages) provides a way to do this: the `continue` statement.

`continue` means "go directly to the next iteration of this loop." Whatever else was supposed to happen in the current iteration is skipped.

Let's say we want to print all the numbers from 1 to 50, but skip every 7th number. We can use `continue` to do this, by keeping track of a counter:

```py
# Remember, `range` is inclusive of the start, but exclusive of the end
counter = 0
for number in range(1, 51):
    counter = counter + 1

    if counter == 7:
        counter = 0 # Reset the counter
        continue # Skip this number

    print(number)
```

What we'll see printed are all the numbers from 1 to 50, except for 7, 14, 21, 28, 35, 42, and 49.

## Avoiding Work

A `continue` statement _immediately_ halts the current iteration and jumps to the next one, which saves the program from doing unnecessary work.

For example, if we're calculating square roots, we might want to skip negative numbers. `continue` lets us move on to the next number without wasting any time:

```py
for number in range(-5, 5):
    if number < 0:
        continue  # Skip negatives

    print(f"The square root of {number} is {number ** 0.5}")
```

This would print:

```text
The square root of 0 is 0.0
The square root of 1 is 1.0
The square root of 2 is 1.4142135623730951
The square root of 3 is 1.7320508075688772
The square root of 4 is 2.0
```

Using `continue` to avoid pointless work can make your code run faster, which is especially helpful if the loop includes time-consuming computations.

# Break Statement

We can use `continue` to skip to the next iteration in a loop, but what if we want to exit the loop entirely? That's where the `break` statement comes in.

```py
for n in range(42):
    print(f"{n} * {n} = {n * n}")
    if n * n > 150:
        break

# 0 * 0 = 0
# 1 * 1 = 1
# 2 * 2 = 4
# 3 * 3 = 9
# 4 * 4 = 16
# 5 * 5 = 25
# 6 * 6 = 36
# 7 * 7 = 49
# 8 * 8 = 64
# 9 * 9 = 81
# 10 * 10 = 100
# 11 * 11 = 121
# 12 * 12 = 144
# 13 * 13 = 169
```

This code _would_ loop from 0 all the way to 41, but it actually _exits early_. Once `n * n` is greater than 150, the `break` statement executes, stopping the loop.

---

CH9: Lists

# Lists

A natural way to organize and store data is in a `List`. Some languages call them "arrays," but in Python we just call them lists. Think of all the apps you use and how many of the items in the app are organized into lists.

For example:

- An X (formerly Twitter) feed is a list of posts
- An online store is a list of products
- The state of a chess game is a list of moves
- This list is a list of things that are lists

Lists in Python are declared using square brackets, with commas separating each item:

```py
inventory = ["Iron Breastplate", "Healing Potion", "Leather Scraps"]
```

Lists can contain items of any data type, in our example above we have a `List` of strings.

# Lists Continued

Sometimes when we're manually creating lists it can be hard to read if all the items are on the same line of code. We can declare the list using multiple lines if we want to:

```py
flower_types = [
    "daffodil",
    "rose",
    "chrysanthemum"
]

player_ages = [
    23,
    18,
    31,
    42
]
```

Writing it this way helps with readability and organization, especially if there are many items or if some of the items are too long. Keep in mind this is just a styling change. The code will run correctly either way.

# Counting in Programming

In the world of programming, counting is a bit strange!

We don't start counting at `1`, we start at `0` instead.

## Indexes

Each item in a list has an index that refers to its spot in the list.

Take the following list as an example:

```py
names = ["Bob", "Lane", "Alice", "Breanna"]
```

- Index 0: `Bob`
- Index 1: `Lane`
- Index 2: `Alice`
- Index 3: `Breanna`

# Indexing Into Lists

Now that we know how to create new lists, we need to know how to access specific items in the list.

We access items in a list directly by using their _index_. Indexes start at 0 (the first item) and increment by one with each successive item. The syntax is as follows:

```py
best_languages = ["JavaScript", "Go", "Rust", "Python", "C"]
print(best_languages[1])
# prints "Go", because index 1 was provided
```

# List Length

The length of a List can be calculated using the [`len()`](https://docs.python.org/3/library/functions.html#len) function. It takes an iterable (such as a string or list) and returns the number of items present.

```py
fruits = ["apple", "banana", "pear"]
length = len(fruits)
# 3 items in fruits

len("supercalifragilisticexpialidocious")
# 34 characters
```

Don't be fooled by the fact that the length is not equal to the index of the last element. In fact, it will always be one greater because the starting index is zero!

# List Updates

We can also change the item that exists at a given index. For example, we can change `Leather` to `Leather Armor` in the `inventory` list in the following way:

```py
inventory = ["Leather", "Iron Ore", "Healing Potion"]
inventory[0] = "Leather Armor"
# inventory: ['Leather Armor', 'Iron Ore', 'Healing Potion']
```

# Appending in Python

It's common to create an empty list then fill it with values using a loop. We can add values to the end of a list using the `.append()` method:

```py
cards = []
cards.append("nvidia")
cards.append("amd")
# the cards list is now ['nvidia', 'amd']
```

# Pop Values

`.pop()` is the opposite of `.append()`. Pop removes the last element from a list and returns it for use. For example:

```py
vegetables = ["broccoli", "cabbage", "kale", "tomato"]
last_vegetable = vegetables.pop()
# vegetables = ['broccoli', 'cabbage', 'kale']
# last_vegetable = 'tomato'
```

While `.pop()` typically removes the last item of a list, it can also be used to remove an item at a specific index. For example, `vegetables.pop(1)` would remove `"cabbage"` from the list. This can be useful when you need to remove items from other positions in your list.

# Counting the Items in a List

Remember that we can [iterate](https://en.wiktionary.org/wiki/iterate) over all the elements in a list using a loop. For example, the following code will print each item in the `sports` list.

```py
for i in range(0, len(sports)):
    print(sports[i])
```

# No-Index Syntax

In my opinion, Python has _the most elegant_ syntax for iterating directly over the items in a list without worrying about index numbers. If you don't need the index number you can use the following syntax:

```py
trees = ['oak', 'pine', 'maple']
for tree in trees:
    print(tree)
# Prints:
# oak
# pine
# maple
```

`tree`, the variable declared using the `in` keyword, directly accesses the value in the list rather than the index of the value. If we don't need to update the item and only need to access its value then this is a more clean way to write the code.

## Infinity

The built-in [float()](https://docs.python.org/3/library/functions.html#float) function can create a numeric floating point value of negative infinity. Instead of initializing a base value like `0` or `-100000`, we can use `float("-inf")` to represent negative infinity. Because _every value_ will be greater than negative infinity, we can use it as a starting point to help us achieve our goal of finding the max value.

```py
negative_infinity = float("-inf")
positive_infinity = float("inf")
```

# Modulo Operator in Python

## Find the Remainder:

The [modulo](https://en.wikipedia.org/wiki/Modulo_operation) operator can be used to find the remainder after a division operation. For example, `7` [modulo](https://en.wikipedia.org/wiki/Modulo_operation) `2` would be `1`, because 2 can be multiplied evenly into 7 at most 3 times:

`2 * 3 = 6`

Then there is 1 _remaining_ to get from `6` to `7`.

`7 - 6 = 1`

The modulo operator is the percent sign: `%`. It's important to recognize modulo is _not_ a percentage though! That's just the symbol we're using.

```py
remainder = 8 % 3
# remainder = 2
```

An odd number is a number that when divided by `2`, the remainder is _not_ `0`.

# Slicing Lists

Python makes it easy to slice and dice lists to work only with the section you care about. One way to do this is to use the simple slicing operator, which is just a colon `:`.

With this operator, you can specify where to start and end the slice, and how to step through the original list. List slicing returns a _new list_ from the existing list.

The syntax is as follows:

```py
my_list[ start : stop : step ]
```

For example:

```py
scores = [50, 70, 30, 20, 90, 10, 50]
# Display list
print(scores[1:5:2])
# Prints [70, 20]
```

The above reads as "give me a slice of the `scores` list from index 1, up to but not including 5, skipping every 2nd value." _All of the sections are optional_.

## Omitting Sections

You can also omit various sections ("start," "stop," or "step"). For example, `numbers[:3]` means "get all items from the start up to (but not including) index 3." `numbers[3:]` means "get all items from index 3 to the end."

```py
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers[:3] # Gives [0, 1, 2]
numbers[3:] # Gives [3, 4, 5, 6, 7, 8, 9]
```
## Using Only the “step” Section

```py
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers[::2] # Gives [0, 2, 4, 6, 8]
```
## Negative Indices

Negative indices count from the end of the list. For example, `numbers[-1]` gives the last item in the list, `numbers[-2]` gives the second last item, and so on.

```py
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers[-3:] # Gives [7, 8, 9]
```

# List Operations – Concatenate

Concatenating two lists (smushing them together) is easy in Python, just use the `+` operator.

```py
total = [1, 2, 3] + [4, 5, 6]
print(total)
# Prints: [1, 2, 3, 4, 5, 6]
```

# List Operations – Contains

Checking whether a value exists in a list or not is also really easy in Python: just use the `in` keyword to check for presence, or `not in` to check for absence.

```py
fruits = ["apple", "orange", "banana"]
print("banana" in fruits)
# Prints: True
```

```py
fruits = ["apple", "orange", "banana"]
print("banana" not in fruits)
# Prints: False
```

# List Deletion

Python has a built-in keyword [del](https://docs.python.org/3/tutorial/datastructures.html#the-del-statement) that deletes items from objects. In the case of a list, you can delete specific indexes or entire slices.

```py
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]

# delete the fourth item
del nums[3]
print(nums)
# Output: [1, 2, 3, 5, 6, 7, 8, 9]

# delete the second item up to (but not including) the fourth item
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]
del nums[1:3]
print(nums)
# Output: [1, 4, 5, 6, 7, 8, 9]

# delete all elements
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9]
del nums[:]
print(nums)
# Output: []
```

# Tuples

[Tuples](https://docs.python.org/3/library/stdtypes.html#typesseq-tuple) are collections of data that are ordered and unchangeable. You can think of a tuple as a `List` with a fixed size. Tuples are created with round brackets:

```py
my_tuple = ("this is a tuple", 45, True)
print(my_tuple[0])
# this is a tuple
print(my_tuple[1])
# 45
print(my_tuple[2])
# True
```

While it's typically considered bad practice to store items of different types in a List, it's not a problem with Tuples. Because they have a fixed size, it's easy to keep track of which indexes store which types of data.

Tuples are often used to store very small groups (like 2 or 3 items) of data. For example, you might use a tuple to store a dog's name and age.

```py
dog = ("Fido", 4)
```

There is a special case for creating single-item tuples. You must include a comma so Python knows it's a tuple and not regular parentheses:

```py
dog = ("Fido",)
```

Because Tuples hold their data, multiple tuples can be stored within a list. Similar to storing other data in lists, each tuple within the list is separated by a comma. When accessing a list of tuples, the first index selects which tuple you want to access, the second selects a value _within_ that tuple.

```py
my_tuples = [
    ("this is the first tuple in the list", 45, True),
    ("this is the second tuple in the list", 21, False)
]
print(my_tuples[0][0]) # this is the first tuple in the list
print(my_tuples[0][1]) # 45
print(my_tuples[1][0]) # this is the second tuple in the list
print(my_tuples[1][2]) # False
```

## Tuple Unpacking

You can easily assign the values of a tuple to variables using unpacking.

```py
dog = ("Fido", 4)
dog_name, dog_age = dog
print(dog_name)
# Fido
print(dog_age)
# 4
```

When you return multiple values from a function, you're actually returning a tuple.

## Split a String Into a List of Words

The [`.split()`](https://docs.python.org/3/library/stdtypes.html#str.split) method in Python is called on a string and returns a list by splitting the string based on a given delimiter. If no delimiter is provided, it will split the string on whitespace. Here's a quick example:

```py
message = "hello there sam"
words = message.split()
print(words)
# Prints: ["hello", "there", "sam"]
```
## Join a List of Strings Into a Single String

The [`.join()`](https://docs.python.org/3/library/stdtypes.html#str.join) method is called on a _delimiter_ (what goes between all the words in the list), and takes a list of strings as input.

```py
list_of_words = ["hello", "there", "sam"]
sentence = " ".join(list_of_words)
print(sentence)
# Prints: "hello there sam"
```

---

CH10: Dictionaries

# Dictionaries

[Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) in Python are used to store data values in `key` -> `value` pairs. Dictionaries are a great way to store groups of information.

```py
# use curly braces
# add key-value pairs
car = {
  "brand": "Toyota",
  "model": "Camry",
  "year": 2019,
}
```

Here the `car` variable is assigned to a dictionary `{}` containing the keys `brand`, `model` and `year`. The keys' corresponding values are `Toyota`, `Camry` and `2019`.

# Duplicate Keys

Because dictionaries rely on unique keys, you can't have two of the same key in the same dictionary. If you try to use the same key twice, the first value will simply be overwritten.

# Accessing Dictionary Values

Dictionary elements must be accessible somehow in code, otherwise they wouldn't be very useful.

A value is retrieved from a dictionary by specifying its corresponding key in square brackets. The square brackets look similar to indexing into a list.

```py
car = {
    "make": "Toyota",
    "model": "Camry"
}
print(car["make"])
# Prints: Toyota
```

# Setting Dictionary Values

You don't need to create a dictionary with values already inside. It is common to create a blank dictionary then populate it later using dynamic values. The syntax is the same as getting data out of a key, just use the assignment operator (`=`) to give that key a value.

```py
planets = {}
planets["Earth"] = True
planets["Pluto"] = False
print(planets["Pluto"])
# Prints False
```

# Updating Dictionary Values

If you try to set the value of a key that already exists, you'll end up just updating the value of that key.

```py
planets = {
    "Pluto": True,
}
planets["Pluto"] = False
print(planets["Pluto"])
# Prints False
```

# Deleting Dictionary Values

You can delete existing keys using the `del` keyword.

```py
names_dict = {
    "jack": "bronson",
    "jill": "mcarty",
    "joe": "denver"
}

del names_dict["joe"]

print(names_dict)
# Prints: {'jack': 'bronson', 'jill': 'mcarty'}
```

## Deleting Keys That Don't Exist

Notice that if you try to delete a key that doesn't exist, you'll get an _error_.

```py
names_dict = {
    "jack": "bronson",
    "jill": "mcarty",
    "joe": "denver"
}

del names_dict["unknown"]
# ERROR HERE, key doesn't exist
```

## Checking for Existence

If you're unsure whether a key exists in a dictionary, use the `in` keyword.

```py
cars = {
    "ford": "f150",
    "toyota": "camry"
}

print("ford" in cars)
# Prints: True

print("gmc" in cars)
# Prints: False
```

# Iterating Over a Dictionary in Python

We can iterate over a dictionary's keys using the same no-index syntax we used to iterate over the values in a list. With access to the dictionary's keys, we also have access to their corresponding values.

```py
fruit_sizes = {
    "apple": "small",
    "banana": "large",
    "grape": "tiny"
}

for name in fruit_sizes:
    size = fruit_sizes[name]
    print(f"name: {name}, size: {size}")

# name: apple, size: small
# name: banana, size: large
# name: grape, size: tiny
```

We could have just as easily set the `name` variable to `key` or simply `k`.

# Ordered or Unordered?

As of Python version `3.7`, dictionaries are _ordered_. In Python `3.6` and earlier, dictionaries were _unordered_.

Because dictionaries are ordered, the items have a defined order, and that order will _not_ change.

Unordered means that the items do _not_ have a defined order.

**The takeaway is that if you're on Python `3.7` or later, you'll be able to iterate over dictionaries in the same order every time.**

---

CH11: Sets

# Sets

[Sets](https://docs.python.org/3/tutorial/datastructures.html#sets) are _like_ Lists, but they are _unordered_ and they guarantee _uniqueness_. Only _ONE_ of each value can be in a set.

```py
fruits = {"apple", "banana", "grape"}
print(type(fruits))
# Prints: <class 'set'>

print(fruits)
# Prints: {'banana', 'grape', 'apple'}
```

## Add Values

You can [`.add()`](https://docs.python.org/3/library/stdtypes.html#set.add) values to a set. Think of `.add()` like `append` but for sets!

```py
fruits = {"apple", "banana", "grape"}
fruits.add("pear")
print(fruits)
# Prints: {'pear', 'banana', 'grape', 'apple'}
```

No error will be raised if you add an item already in the set, and the set will remain unchanged.

## An Empty Set

Because the empty bracket `{}` syntax creates an empty dictionary, to create an _empty_ set, you need to use the `set()` function.

```py
fruits = set()
fruits.add("pear")
print(fruits)
# Prints: {'pear'}
```

## Set Iteration

```py
fruits = {"apple", "banana", "grape"}
for fruit in fruits:
    print(fruit)
    # Prints:
    # banana
    # grape
    # apple
```

Note: Sets are unordered, so the order of iteration is _not_ guaranteed.

* Convert a list to a set (duplicates lost): `set(list_name)`
* Convert a set to a list: `list(set_name)`

# Sets Quiz

[Sets](https://docs.python.org/3/tutorial/datastructures.html#sets) are _like_ Lists, but they are _unordered_ and they guarantee _uniqueness_. Only _ONE_ of each value can be in a set.

```py
fruits = {"apple", "banana", "grape", "apple"}
print(type(fruits))
# Prints: <class 'set'>

print(fruits)
# Prints: {'banana', 'grape', 'apple'}
```

## Add Values

You can [`.add()`](https://docs.python.org/3/library/stdtypes.html#set.add) values to a set.

```py
fruits = {"apple", "banana", "grape"}
fruits.add("pear")
print(fruits)
# Prints: {'banana', 'grape', 'pear', 'apple'}
```

No error will be raised if you add an item already in the set, and the set will remain unchanged.

## Empty Set

Because the empty bracket `{}` syntax creates an empty dictionary, to create an _empty_ set, you need to use the `set()` function.

```py
fruits = set()
fruits.add("pear")
print(fruits)
# Prints: {'pear'}
```
## Iterate Over Values in a Set (Order Is Not Guaranteed)

```py
fruits = {"apple", "banana", "grape"}
for fruit in fruits:
    print(fruit)
    # Prints:
    # banana
    # grape
    # apple
```
## Removing Values

```py
fruits = {"apple", "banana", "grape"}
fruits.remove("apple")
print(fruits)
# Prints: {'banana', 'grape'}
```

# Set Subtraction

You can use some of the "normal" mathematical operations on sets. For example, you can subtract one set from another. It removes all the values in the second set from the first set.

```py
set1 = {"apple", "banana", "grape"}
set2 = {"apple", "banana"}
set3 = set1 - set2

print(set3)
# Prints: {'grape'}
```

---

CH12: Errors

# Errors and Exceptions in Python

You've probably encountered some errors in your code from time to time if you've gotten this far in the course. In Python, there are two main kinds of distinguishable errors:

- Syntax errors
- Exceptions

## Syntax Errors

You probably know what these are by now. A syntax error is just the Python interpreter telling you that your code isn't adhering to proper Python syntax.

```py
this is not valid code, so it will error
```

If I try to run that sentence as if it were valid code I'll get a syntax error:

```text
this is not valid code, so it will error
      ^
SyntaxError: invalid syntax
```

## Exceptions

Even if your code has the right syntax, however, it may still cause an error when an attempt is made to execute it. Errors detected during execution are called "exceptions" and can be handled gracefully by your code. You can even raise your own exceptions when bad things happen in your code.

Python uses a [try-except](https://docs.python.org/3/tutorial/errors.html#handling-exceptions) pattern for handling errors.

```py
try:
  10 / 0
except Exception:
  print("can't divide by zero")
```

The `try` block is executed until an exception is raised or it completes, whichever happens first. In this case, an exception is raised because division by zero is impossible. The `except` block is only executed if an exception is raised in the `try` block.

If we want to access the data from the exception, we use the following syntax:

```py
try:
  10 / 0
except Exception as e:
  print(e)

# prints "division by zero"
```

Wrapping potential errors in `try/except` blocks allows the program to handle the exception gracefully without crashing.

# Try/Except Review

```py
try:
  10 / 0
except Exception as e:
  print(e)

# prints "division by zero"
```

The `try` block is executed until an exception is raised or it completes, whichever happens first. In this case, a "divide by zero" error is raised because division by zero is impossible. The `except` block is only executed if an exception is raised in the `try` block. It then exposes the exception as data (`e` in our case) so that the program can handle the exception gracefully without crashing.

# Raising Your Own Exceptions

Errors are _not_ something to be scared of. Every program that runs in production is expected to manage errors on a constant basis. Our job as developers is to handle the errors gracefully and in a way that aligns with our user's expectations.

## Errors Are NOT Bugs

Click to hide video

When something in our own code happens that isn't the "happy path," we should raise our own exceptions. For example, if someone passes some bad inputs to a function we write, we should not be afraid to raise an exception to let them know they did something wrong.

An _error_ or _exception_ is raised when something bad happens, but as long as our code handles it as users expect it to, it's _not_ a bug. A bug is when code behaves in ways our users don't expect it to.

For example, if a player tries to forge a sword out of a metal bar, we might stop that from happening by using `raise` to prevent a _bug_. If the game doesn't have certain items, such as a gold sword, then players shouldn't be able to craft a sword from gold bars even though gold bars do exist.

```py
def craft_sword(metal_bar):
    if metal_bar == "bronze":
        return "bronze sword"
    if metal_bar == "iron":
        return "iron sword"
    if metal_bar == "steel":
        return "steel sword"
    raise Exception("invalid metal bar")
```

We prevent a bug by _raising an exception_. This exception prevents other developers who use the `craft_sword` function from creating items that don't exist in our game.

`raise` stops the program from executing and forces the exception to be handled.

## Don't Catch Your Own Exceptions

As a rule of thumb, you do not want to catch exceptions you raise within the same function block, for example:

```py
# don't do this
def craft_sword(metal_bar):
    try:
        if metal_bar == "bronze":
            return "bronze sword"
        if metal_bar == "iron":
            return "iron sword"
        if metal_bar == "steel":
            return "steel sword"
        raise Exception("invalid metal bar")
    except Exception as e:
        print(f"An error occurred: {e}")
```

Instead, the caller should handle any potential error by wrapping the function call within a try/except block.

```py
# do this
try:
    craft_sword("gold bar")
except Exception as e:
    print(e)
```

By raising the exception instead of handling it inside `craft_sword`, we let the caller decide how to proceed. The caller might want to log the error, show a message to the player, or crash the program entirely.

# Raising Exceptions Review

Software applications aren't perfect, and user input and network connectivity are far from predictable. Despite intensive debugging and unit testing, applications will still have failure cases.

Loss of network connectivity, missing database rows, out of memory issues, and unexpected user inputs can all prevent an application from performing "normally." It is your job to catch and handle any and all exceptions gracefully so that your app keeps working. When you are able to detect that something is amiss, you should be raising the errors yourself, in addition to the "default" exceptions that the Python interpreter will raise.

```py
raise Exception("something bad happened")
```

# Different Types of Exceptions

We haven't covered classes and objects yet, which is what an `Exception` really is at its core. We'll go more into that in the course on object-oriented programming.

For now, what is important to understand is that there are different types of exceptions, and we can handle them differently depending on the situation. Some exceptions are more specific, like `ZeroDivisionError` (which happens when you divide by zero) or `IndexError` (which happens when you try to access a list element at an invalid index – either too high or too low). Others are more general, like the base `Exception`.

## Syntax

```py
try:
    10/0
except ZeroDivisionError:
    print("0 division")
except Exception as e:
    print(e)

try:
    nums = [0, 1]
    print(nums[2])
except IndexError:
    print("index error")
except Exception as e:
    print(e)
```

Which will print:

```text
0 division
index error
```

## Why Specific Exceptions Come First

When handling exceptions, it's important to catch the **most specific ones** first, because Python stops checking once it finds a matching exception handler. If you catch a more general Exception first, any specific errors will never get handled individually.

For example:

```py
try:
    nums = [0, 1]
    print(nums[2])
except Exception:
    print("An error occurred")
except IndexError:
    print("Index error")
```

In this case, the general Exception will catch the error before the `IndexError` can be reached, and the message "Index error" will never be printed. Always handle the most specific exception first!

## Alias Exception Messages

As seen in the example, you can also access the error using `as`, like this:

```py
except Exception as e:
    print(e)
```

The default behavior of `print` is that it will print the string representation of whatever object is passed to it. In this case, it will print the error message.

# Raising Exceptions Review

As you've noticed, there are many types of exceptions. Many specific exceptions are built into the language like `IndexError` and `ZeroDivisionError`, and (almost) all Exceptions count as the parent `Exception` type. What differentiates exceptions are their types, not their string descriptions. This is important to know when handling errors from imported modules.

If you're interested in the official documentation on all the built-in exceptions you can find a [list here](https://docs.python.org/3/library/exceptions.html).

## Refer to the Following Code for the Question

```py
try:
    raise Exception("zero division")
except ZeroDivisionError as e:
    print("zero")
```

In the code sample, what will happen?
The program will crash with an uncaught exception

---

CH13: Type Hints

# Type Hints

Some functions accept [numbers](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex) as arguments; others accept [strings](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str). Some return [lists](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range); others return [dictionaries](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict), [booleans](https://docs.python.org/3/library/stdtypes.html#boolean-type-bool), or [`None`](https://docs.python.org/3/reference/datamodel.html#none).

When a program is small, you can _usually_ remember the types of your variables. But as programs grow, it's easy to forget:

- Is `level` an `int` or a `str`?
- Does `get_item()` always return an item name (`str`), or sometimes `None` if it can't find one?
- Is `inventory` a list of strings, or a dictionary of item counts?

[Type hints](https://docs.python.org/3/library/typing.html) let us write those expectations directly in our code:

```py
def get_damage(weapon: dict, level: int) -> int:
    return weapon["damage"] + (level * 2)
```

The `weapon: dict`, `level: int`, and `-> int` parts are type hints. They tell humans _and_ code editors what kinds of values the function expects and returns.

Type hints _don't make Python stop being Python_. It's still a [dynamically typed](https://en.wikipedia.org/wiki/Type_system#Dynamic_type_checking_and_runtime_type_information) language, and it won't automatically reject the wrong value just because a type hint says so.

Type hints are for:

- Making code easier to read
- Helping your editor autocomplete and warn you about mistakes
- Making bugs easier to spot before running your code

# Basic Types

To add a type hint to a variable declaration, put a colon after the variable name, then the type. This comes _before_ the equals sign and the value:

```py
character_name: str = "Sir Galahad"
character_level: int = 7
character_health: float = 72.5
has_magic: bool = True
```

_The values work the exact same way they did before._ In fact, when it comes to simple variable declarations like this, you don't actually _need_ the hint. In this example:

```py
character_health = 72.5
```

Because `character_health` is assigned a value of `72.5`, your tooling can _infer_ that it's a `float`. That said, if you also want to _see_ the type name, you can optionally add it.

# Function Parameters

[Function parameters](https://docs.python.org/3/glossary.html#term-parameter) can have type hints too! The syntax is the same as variable type hints: put a colon after the parameter name, then the type.

```py
def greet_player(name: str):
    print(f"Welcome, {name}!")
```

When a function has multiple parameters, each one can have its own type hint:

```py
def add_gold(current_gold: int, found_gold: int):
    return current_gold + found_gold
```

While adding a type hint to a variable declaration like:

```py
character_health: float = 72.5
```

is considered a bit _redundant_ due to type inference, adding type hints to function parameters is _not_ redundant. If you don't add them, your tooling won't know what types the function expects, which makes autocomplete and error checking less effective.

Hover your cursor over the `status` variable. See how the tooltip can show you that it's a string? That's what makes type hinting useful! Note that `name`, `level`, `health`, and `magic` are _all "unknown"_ because Python can't infer function parameter types without hints.

# Return Types

You can _also_ annotate the type that you expect a function to [return](https://docs.python.org/3/reference/simple_stmts.html#the-return-statement). When you know what types go into and come out of a function, you can (probably) use it without having to read every line of the function body. Return types come after the parameter list, before the colon:

```py
def add_gold(current_gold: int, found_gold: int) -> int:
    return current_gold + found_gold
```

The `-> int` means this function is _expected_ to return an integer.

**The syntax is a bit different** from type hints on variables and parameters: we use `->` instead of `:`, and there's no variable name before the type hint. This is because it doesn't really matter what name (if any) the function uses internally for the return value; we just care about the type.

Here's another example:

```py
def get_greeting(player_name: str) -> str:
    return f"Welcome, {player_name}!"
```

The `-> str` means this function is expected to return a string.

# Fixing Type Hints

The whole point of type hints is that they should **match what the code actually does**. If a function returns a string, its return type hint should also be `str`.

```py
def get_quest_reward(quest_name: str, quest_xp: int) -> str:
    return f"You've earned {quest_xp} XP for completing the {quest_name} quest!"
```

An incorrect type hint is confusing _at best_, even if you can still force the Python code to run. Your editor will likely also warn you, with something like a red squiggly line, if a function returns a value that doesn't match its return type hint.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/ooB2NY8-975x250.png)

# List and Set Hints

We've covered hints for **basic types** like `str`, `int`, `float`, and `bool`, but you can also add hints for **container types**: types that _hold other values_. For example:

- [`list`](https://docs.python.org/3/library/stdtypes.html#lists): mutable sequence of values
- [`set`](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset): unordered collection of unique values
- [`dict`](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict): collection of key-value pairs
- [`tuple`](https://docs.python.org/3/library/stdtypes.html#tuples): immutable sequence of values

When we type-hint a container, we specify what kind of container it is _and_ what type of values it contains. For example, a _list_ of _strings_ can be expressed as `list[str]`:

```py
inventory: list[str] = ["Iron Sword", "Healing Potion"]
```

The "contained" type goes in square brackets after the container type. Similarly, for a _set_ of _strings_, we would write `set[str]`:

```py
unique_items: set[str] = {"Iron Sword", "Healing Potion"}
```

# Dictionary Hints

[Dictionaries](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict) are container types too, but they map **keys** to **values**, so their type hints include _both_:

```py
item_counts: dict[str, int] = {
    "Wooden Arrow": 30,
    "Small Amethyst": 2,
}
```

The first type is for the keys; the second is for the values.

```py
dict[key_type, value_type]
```

So `dict[str, int]` means:

- The keys are strings
- The values are integers

Not all types [can be used as dictionary _keys_](https://docs.python.org/3/glossary.html#term-hashable). The key types that you'll see most often are strings and integers. Dictionary _values_, on the other hand, can be any type.

# Tuple Hints

Lists and sets _usually_ hold multiple values of the same type:

```py
inventory: list[str] = ["Black Knight Halberd", "Skull Lantern", "Notched Whip"]
```

But [tuples](https://docs.python.org/3/library/stdtypes.html#tuples) are a **small fixed group of values** where each _position_ has its own meaning. Because they're fixed, it's quite common for those values to be of different types. For example, a loot drop might have an item name and a quantity:

```py
drop: tuple[str, int] = ("Garnet Mark", 2)
```

`tuple[str, int]` means:

- There are two values in the tuple
- The first value is a string
- The second value is an integer

A tuple can have any number of values – though `2` and `3` are the most common. Here's an example representing a character's HP, MP, and stamina:

```py
stats: tuple[int, float, int] = (100, 42.5, 75)
```

The type hint `tuple[int, float, int]` tells us this is a three-value tuple with an integer, a float, and another integer.

# Specific Container Types

It's possible to type-hint a container with _just_ the container type:

```py
items: list = ["Black Firebomb", "Titanite Chunk"]
```

This says `items` is a list, but it doesn't tell us what _kind of values_ go inside! Assuming you know what's inside, best to be specific:

```py
items: list[str] = ["Black Firebomb", "Titanite Chunk"]
```

That said, bare container type hints aren't _wrong_. Sometimes you really _don't know_ what types of values a container will hold, or the specific type hint would be too complicated to be useful. You'll see that occasionally with `dict`s. Just give clear type hints whenever possible!

# Nested Types

We've looked at relatively simple container types like `list[str]`, but they can get more complex when _one container holds another container_. That is, it's possible to have **nested container types**.

A dictionary, for example, could map each character's _name_ to their _list of spells_:

```py
character_spells: dict[str, list[str]] = {
    "Gandalf": ["Fireball", "Light"],
    "Frodo": ["Hide"],
}
```

We read `dict[str, list[str]]` from the outside in:

- It's a dictionary (`dict`)
- Each _key_ is a string (`str`)
- Each _value_ is a list of strings (`list[str]`)

In extreme cases, nested types can get _super_ confusing, but honestly it's less confusing than it would be _without_ the typing. For the time being, just know that type hints _can_ describe containers within containers.

# Optional Values

Sometimes we work with variables that [may or may not](https://docs.python.org/3/library/typing.html#typing.Optional) have an "actual value." For example, a character _might_ have a damage bonus, or they might not. If they _don't_, we can represent that lack of value with [`None`](https://docs.python.org/3/reference/datamodel.html#none).

The [`|` operator](https://docs.python.org/3/library/typing.html#typing.Optional) indicates that a value can be of multiple types:

```py
damage_bonus: int | None
```

That means `damage_bonus` can be either an integer (the bonus amount) _or_ `None`. For another example, a function might return a prepared spell if one is ready, or `None` if no spell is prepared:

```py
def get_prepared_spell(has_spell: bool) -> str | None:
    if has_spell:
        return "Fireball"

    return None
```

# Fix Code With Type Hints

When type hints and code behavior disagree, _one of them is wrong_.

If you know that you've properly typed a function signature, you can use it as the "source of truth," or as a clue for how the implementation should work.

---

CH14: Practice

The built-in [`type()` function](https://docs.python.org/3/library/functions.html#type) can be used to get the type of a variable. For example:

```py
type(1) == int # or type(1) is int
# True

type("1") == str
# True

type(1.0) == float
# True

type("seventy-six") == int
# False
```

In Python, `==` and `is` serve different purposes when comparing objects:

- **`==` is for equality.** It checks if the _values_ of two objects are the same.
- **`is` is for identity.** It checks if two variables point to the _exact same object_ in memory.

When you use `type(num) is int`, it works because there is only one "integer type" object in your Python environment. You are checking if the type of `num` is the same object as the built-in `int` class.

---

CH15: Quiz

# The Zen of Python

Tim Peters, a longtime Pythonista, describes the guiding principles of Python in his famous short piece, [The Zen of Python](https://peps.python.org/pep-0020/).

> Beautiful is better than ugly.
> 
> Explicit is better than implicit.
> 
> Simple is better than complex.
> 
> Complex is better than complicated.
> 
> Flat is better than nested.
> 
> Sparse is better than dense.
> 
> Readability counts.
> 
> Special cases aren't special enough to break the rules.
> 
> Although practicality beats purity.
> 
> Errors should never pass silently.
> 
> Unless explicitly silenced.
> 
> In the face of ambiguity, refuse the temptation to guess.
> 
> There should be one – and preferably only one – obvious way to do it.
> 
> Although that way may not be obvious at first unless you're Dutch.
> 
> Now is better than never.
> 
> Although never is often better than _right_ now.
> 
> If the implementation is hard to explain, it's a bad idea.
> 
> If the implementation is easy to explain, it may be a good idea.
> 
> Namespaces are one honking great idea – let's do more of those!

# Why Python?

Here are some reasons we think Python is a future-proof choice for developers:

- Easy to read and write – Python reads like plain English. Due to its simple syntax, it's a great choice for implementing advanced concepts like AI. This is arguably Python's _best feature_.
- Popular – According to the Stack Overflow Developer Survey, [Python is the 4th most popular](https://survey.stackoverflow.co/2025/technology#1-programming-scripting-and-markup-languages) coding language as of 2025.
- Free – Python, like many languages nowadays, is developed under an open-source license. It's free to install, use, and distribute.
- Portable – Python written for one platform will work on any other platform.
- Interpreted – Code can be executed as soon as it's written. Because it doesn't need to take a long time to compile like Java, C++, or Rust, releasing code to production is often faster.

## Why Not Python?

Python might not be the best choice for a project if:

- The code needs to run fast. Python code executes very slowly, which is why performance-critical applications like PC games aren't written in Python.
- The codebase will become large and complex. Due to its dynamic type system, Python code can be harder to keep clean of bugs.
- The application needs to be distributed directly to non-technical users. They would have to install Python in order to run your code, which would be a huge inconvenience.

# Python 2 vs. Python 3

One thing that's important to keep in mind as you continue your Python journey is that the Python ecosystem is fragmented. Python 3 was [released on December 3, 2008](https://en.wikipedia.org/wiki/History_of_Python#Version_3), but 15+ years later, the web is still full of Python 2 dependencies, scripts and tutorials!

In this course, **we used Python 3** – just like any good citizen should these days.

One of the most obvious breaking changes between Python 2 and 3 is the syntax for printing text to the console.

## Python 2

```py
print "hello world"
```
## Python 3

```py
print("hello world")
```

# Should You Use Python 2 or 3?

![](https://www.python.org/static/community_logos/python-logo-master-v3-TM-flattened.png)

As you've probably guessed, you should **always use Python 3** going forward!

Python 2 and 3 are similar, but Python 3 contains significant changes that are _not_ backward compatible with the 2.x versions.