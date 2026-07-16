## 1. Core Architecture & Environment

### Fast and Compiled
Go is a fast, compiled language. Generally speaking, languages that compile directly to machine code produce programs that are faster than interpreted programs (like Python, JavaScript, PHP, or Ruby). While Go programs do not run quite as fast as their compiled counterparts like C, C++, or Rust, Go compiles much faster than they do, ensuring high developer productivity.

### Small Memory Footprint & The Go Runtime
Go programs are highly lightweight. Each compiled Go executable includes a small amount of extra code called the Go Runtime. 
* **Garbage Collector:** One of the main purposes of the Go runtime is automated memory management. It includes a garbage collector that safely frees up memory that is no longer in use.
* **Comparison with Java:** As a general rule, Java programs use significantly more memory than comparable Go programs. Java uses a virtual machine to interpret bytecode at runtime and typically allocates much more data on the heap.
* **Comparison with Rust/C:** Rust and C use slightly less memory than Go because they give the developer manual control to optimize memory, whereas Go handles it automatically via the runtime. In idle memory usage comparisons (e.g., Dexter Darwich's benchmarks), Go and Rust use very little memory compared to Java.

## Idle Memory Usage

![](https://miro.medium.com/max/1400/1*Ggs-bJxobwZmrbfuoWGpFw.png)

In the chart above, [Dexter Darwich compares the memory usage](https://medium.com/@dexterdarwich/comparison-between-java-go-and-rust-fdb21bd5fb7c) of three _very_ simple programs written in Java, Go, and Rust. As you can see, Go and Rust use _very_ little memory when compared to Java.

### Compilation Commands
* `go run <file.go>`: Compiles and runs quickly without saving a binary to the directory. Ideal for scripting/testing.
* `go build`: Compiles code into a single, statically linked executable binary. This allows shipping to production without users needing the Go toolchain.
* `go install`: Compiles and installs the binary globally in the local `GOBIN` directory.
* **GOPATH Warning:** Modern Go relies on Modules. Storing code in the legacy `$GOPATH/src` directory is outdated and should be avoided.

```go
package main // Lets the compiler know this compiles into a standalone program

import "fmt" // Imports the formatting package from the standard library

func main() { // The entry point for the program
	fmt.Println("Starting Textio server...")
}
```

## 2. Program Structure & Packages
Code organization follows a strict hierarchy: **Repository -> Module -> Package -> Source Files**.
* **Module (`go.mod`):** A collection of packages released together. It defines the module path (import prefix), Go version, and dependencies.
    * **Local Development (`replace`):** You can use the `replace` directive in `go.mod` to import local packages before publishing them to a remote repository (e.g., `replace example.com/pkg => ../pkg`). This should be removed for production.
* **Packages:** A directory of Go code compiled together. 
    * `package main`: Tells the compiler to output an executable. Must contain `func main()`. Should *not* export functions.
    * **Library Packages:** Have no entry point. They exist purely to export functionality.
* **Visibility (Exported vs. Unexported):**
    * **Capitalized** identifiers (functions, variables) are Exported (public).
    * **Lowercase** identifiers are Unexported (private to the package).
* **Clean Package Rules:**
    1. **Hide Internal Logic (Encapsulation):** Expose a minimal API; keep complex underlying logic unexported.
    2. **Stable APIs:** Do not change exported function signatures to avoid breaking dependent applications.
    3. **Agnostic Design:** Packages should never have specific knowledge of the applications depending on them.

```go
// In mystrings/mystrings.go
package mystrings

// Reverse is capitalized, making it accessible from other packages
func Reverse(s string) string {
  result := ""
  for _, v := range s {
    result = string(v) + result
  }
  return result
}

// In hellogo/main.go
package main

import (
	"fmt"
	"[example.com/username/mystrings](https://example.com/username/mystrings)" // Module import path
)

func main() {
	fmt.Println(mystrings.Reverse("hello world"))
}
```

## 3. Variables, Types, and Declarations
Variables are block-scoped. Go creates new scopes for: Functions, Loops, If statements, Switch statements, Select statements, and explicit `{}` blocks.

### Declaration Syntax
* **Standard (`var`):** Initializes to the type's "zero value" (`0`, `""`, `false`, `nil`).
* **Short Declaration (`:=`):** The walrus operator declares and assigns in one line. **Yay type inference!** Go infers the type based on the value being assigned (e.g., `42` implies `int`, `3.14` implies `float64`). It should be used wherever possible, but is strictly limited to function scope (cannot be used for global/package variables).

```go
var mySkillIssues int // Defaults to 0
mySkillIssues = 42

mySkillIssues := 42 // Short declaration (Preferred, uses type inference)

mileage, company := 80276, "Toyota" // Same-line multiple declaration
```

### Type Sizes & Memory
Integers, unsigned integers, floats, and complex numbers all have specific type sizes.
* **Signed integers (no decimal):** `int`, `int8`, `int16`, `int32`, `int64`
* **Unsigned integers (non-negative):** `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr`
* **What's the deal with sizes?** The size (8, 16, 32, 64) represents exactly how many *bits* in memory will be used to store the variable. The default `int` and `uint` refer to their respective 32 or 64-bit sizes depending on the environment of the user (32-bit vs 64-bit operating systems).

### Which Type Should I Use?
With so many types for numbers, developers coming from JavaScript or Python may find the choices daunting.
* **Prefer Default Types:** Always use the default types (`int`, `uint`, `float64`, `complex128`) unless you have a strict performance or memory constraint. 
* **The Pain of Specific Types:** If you use a specific type like `uint16`, but try to pass it into a function expecting an `int`, you are forced to write code riddled with type conversions. When Go developers stray from the "default" types, code gets slow and annoying to read quickly.
* **When to deviate?** Only when you are super concerned about performance/memory usage in a resource-constrained application, or need an absurdly large range (`uint64`).

```go
// The problem with deviating from defaults: forced conversions
var myAge uint16 = 25
myAgeInt := int(myAge) 
```

### Type Conversions & Constants
* **Type Conversion:** Explicit casting is strictly required. Casting a float to an integer truncates the decimal portion.
* **Constants (`const`):** Must be determinable at *compile time*. Cannot use `:=` or runtime calls like `time.Now()`. Constants can only be primitive types (strings, ints, bools, floats).

```go
const pi = 3.14159
const firstName = "Lane"
const lastName = "Wagner"
const fullName = firstName + " " + lastName // Computed at compile time is allowed
```

## 4. Strings, Runes, Comments & Formatting

### Comments
Go has two standard styles for leaving notes in your code that will not execute:
```go
// This is a single line comment

/*
  This is a multi-line comment
  neither of these comments will execute as code
*/
```

### String Concatenation Rule
Two strings can be concatenated with the `+` operator. However, Go is strictly typed: **the compiler will not allow you to concatenate a string variable with an int or a float64 directly.**

### Strings & Encoding
* **Byte Slices:** In languages like C, using ASCII encoding, a character is a single byte (7 bits representing 128 characters). In Go, strings are read-only sequences of bytes that can hold arbitrary data using variable-length **UTF-8 encoding**.
* **Runes:** Because a single character (like an emoji or Chinese character) can span multiple bytes, Go provides the `rune` type (an alias for `int32`, a 32-bit integer). 
* **Two Main Takeaways for Runes:**
    1. When you need to work with individual characters in a string, you should use the `rune` type. It breaks strings up into individual characters safely, even if they are more than one byte long.
    2. We can include a wide variety of Unicode characters (emojis, foreign alphabets) in our strings and Go will handle them perfectly.

Note: a string can be converted into a slice of runes as follows:
```go
runes := []rune(word)
```

### Printing & Formatting Strings
Go follows the `printf` tradition from the C language. It is generally less elegant than Python's f-strings.

* **`fmt.Println()` vs `fmt.Printf()` vs `fmt.Sprintf()`:**
    * `fmt.Println()`: Prints to the console (standard out) and appends a new line. When passing multiple variables, it automatically separates them with a space.
    * `fmt.Printf()`: Prints a *formatted* string to standard out.
    * `fmt.Sprintf()`: Returns the formatted string as a value instead of printing it.

```go
// Multiple variables automatic spacing behavior
messageStart := "Happy birthday! You are now"
age := 21
messageEnd := "years old!"
fmt.Println(messageStart, age, messageEnd) 
// Output: Happy birthday! You are now 21 years old! (No need to add manual spaces)
```

**Formatting Verbs:**
* `%v`: Default representation. Used as a catch-all.
* `%s`: String formatting.
* `%d`: Integer formatting.
* `%f`: Float formatting. (`%.2f` rounds the number to 2 decimal places).
* `%t`: Formats a boolean value (`true` or `false`).
* `%T`: Prints the *Type* of the variable instead of its value.

```go
s1 := fmt.Sprintf("I am %v years old", 10)       // Default formatting: I am 10 years old
s2 := fmt.Sprintf("I am %s years old", "many")   // String: I am many years old
s3 := fmt.Sprintf("I am %d years old", 10)       // Integer: I am 10 years old
s4 := fmt.Sprintf("I am %.2f years old", 10.523) // Float (rounded): I am 10.52 years old
s5 := fmt.Sprintf("Type is: %T", 10.523)         // Type: Type is: float64
```

Refer: [`fmt` package's printing related docs](https://pkg.go.dev/fmt#hdr-Printing)
## 5. Control Flow

### Conditionals (`if`)
`if` statements in Go do not use parentheses around the condition. **Strict Syntax Rule:** Unlike other languages, you *must* put the opening curly brace `{` on the same line as the condition and not on a new line.

**The Initial Statement of an If Block:**
An `if` conditional can execute a short initialization statement before the condition. 
* **Why use this?** It serves two valuable purposes: 
    1. It makes the code a bit shorter.
    2. It strictly limits the scope of the initialized variable to the `if` block, preventing you from accidentally using it elsewhere in the parent scope.

```go
// 'length' is defined only within the scope of the if body
if length := getLength(email); length < 10 {
    fmt.Printf("Email must be at least 10 characters, is %d\n", length)
} 
```

### Switch Statements
Switch statements compare a value against multiple options, acting as a concise alternative to long if-else chains.
* **Implicit Break:** Notice that in Go, the `break` statement is not required at the end of a `case` to stop it from falling through to the next case. The break statement is implicit.
* **Fallthrough:** If you *do* want a case to fall through and execute the next case's block, you must explicitly use the `fallthrough` keyword.

```go
func getCreator(os string) string {
    var creator string
    switch os {
    case "linux":
        creator = "Linus Torvalds"
    case "windows":
        creator = "Bill Gates"
    case "macOS":
        fallthrough // Explicitly forces execution into the "mac" case
    case "mac":
        creator = "A Steve"
    default:
        creator = "Unknown"
    }
    return creator
}
```

### Loops (`for`)
Go uses `for` exclusively, utilizing standard operators (Modulo `%`, AND `&&`, OR `||`).
* `continue` stops the current iteration and moves to the next. `break` exits the loop entirely.

```go
// Standard C-like loop with continue
for i := 0; i < 10; i++ {
  if i % 2 == 0 {
    continue // skips even numbers
  }
  fmt.Println(i)
}

// While-equivalent loop
plantHeight := 1
for plantHeight < 5 {
  plantHeight++
}

// Infinite loop with break
for {
  if someCondition {
    break
  }
}
```

## 6. Functions
In Go, the type comes *after* the variable name to make reading left-to-right easier. 
* **Function Signature:** The declaration line defining inputs and outputs (e.g., `func sub(x int, y int) int` is the signature). Multiple arguments of the same type next to each other can be condensed: `func addToDatabase(hp, damage int, name string)`.
* **Pass by Value:** Variables are passed as copies. A function cannot mutate the caller's original data unless a pointer is used.
* **Ignoring Returns:** Use the blank identifier `_` to explicitly discard unwanted return values. In Go, the blank identifier isn't just a convention; it's a real language feature that completely discards the value. Go will throw an error if you have unused variable declarations. 

```go
func getPoint() (x int, y int) {
    return 3, 4
}

x, _ := getPoint() // Captures x, explicitly ignores y
```

### Named Return Values & Naked Returns
Return values may be given names in the signature. They are treated as new variables initialized to zero-values at the top of the function. This acts as excellent documentation for complex functions.
* **Naked Returns:** A return statement without arguments automatically returns the named return values. 
* **Readability Concern:** Naked returns should be used *only* in short functions where the purpose of the returned values is obvious. In longer functions, they harm readability.

```go
func getCoords() (x, y int) {
	// x and y are implicitly initialized with zero values
	return // Naked return, automatically returns x and y
}

func getExplicitCoords() (x, y int) {
    return 5, 6 // Explicit return safely overwrites the zero values
}
```

### Early Returns & Guard Clauses
Go supports returning early from a function. This is a powerful feature that cleans up code by using **Guard Clauses**. Instead of using deeply nested `if/else` chains, guard clauses make the logic one-dimensional, drastically reducing the cognitive load on the reader.

```go
// The bad way: Deeply nested, high cognitive load
func getInsuranceAmount(status insuranceStatus) int {
  amount := 0
  if !status.hasInsurance(){
    amount = 1
  } else {
    if status.isTotaled(){
      amount = 10000
    } else {
      if status.isDented(){
        amount = 160
      } else {
        amount = 0
      }
    }
  }
  return amount
}

// The Go way: One-dimensional Guard Clauses
func getInsuranceAmount(status insuranceStatus) int {
  if !status.hasInsurance(){
    return 1
  }
  if status.isTotaled(){
    return 10000
  }
  if !status.isDented(){
    return 0
  }
  return 160
}
```

### Advanced Function Paradigms
Functions are "first-class" values in Go. They are just another type—like `ints` or `strings`.

```go
// Functions as values
func add(x, y int) int { return x + y }

func aggregate(a, b, c int, arithmetic func(int, int) int) int {
  firstResult := arithmetic(a, b)
  return arithmetic(firstResult, c)
}

func main() {
	sum := aggregate(2, 3, 4, add) // sum is 9
}
```

* **Anonymous Functions & Closures:** Functions defined inline without a name. A closure is a function that references and can mutate variables from outside its own body.

```go
// Closure example
func concatter() func(string) string {
	doc := ""
	return func(word string) string {
		doc += word + " " // References and mutates 'doc' from the outer scope
		return doc
	}
}
```

* **Currying:** Partial application of functions, transforming a multi-argument function into a sequence of single-argument functions.

```go
func multiply(x, y int) int { return x * y }

func selfMath(mathFunc func(int, int) int) func(int) int {
  return func(x int) int {
    return mathFunc(x, x)
  }
}
```

### Defer
The `defer` keyword allows a function to execute automatically *just before* its enclosing function returns.
* Arguments are evaluated immediately, but execution is delayed.
* **LIFO:** If you have multiple `defer` statements, they execute in **Last-In, First-Out (LIFO)** order. 
* **Use case:** It is a great way to guarantee resources (database connections, open files) are closed properly, regardless of which return path the function takes.

```go
func CreateTempFile() {
	f, _ := os.Create("temp-42.txt")
	defer os.Remove(f.Name()) // Executed second
	defer f.Close()           // Executed first

	fmt.Fprintln(f, "How many roads must a man walk down?")
}
```

## 7. Arrays, Slices, and Maps

### Arrays vs. Slices
* **Arrays:** Fixed-size groupings (`var myInts [10]int`).
* **Slices:** A dynamically-sized, flexible wrapper around an underlying array. 99% of sequence handling in Go uses slices.
    * **Creation (`make`):** `make([]int, length, capacity)`. Elements initialize to zero values. Zero value of slice is `nil`.
    * **Length vs. Capacity:** `len()` is active elements. `cap()` is the maximum elements the underlying array can hold before requiring memory reallocation.

```go
var myInts [10]int                     // Array
primes := [6]int{2, 3, 5, 7, 11, 13}   // Initialized Array
madeSlice := make([]int, 5, 10)        // Slice created with make
```

Non-nil slices **always** have an underlying array, though it isn't always specified explicitly. To explicitly create a slice on top of an array:

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
mySlice := primes[1:4]
// mySlice = {3, 5, 7}
```

The syntax is:
```
arrayname[lowIndex:highIndex]
arrayname[lowIndex:]
arrayname[:highIndex]
arrayname[:]
```

Where `lowIndex` is inclusive and `highIndex` is exclusive and `lowIndex`, `highIndex`, or _both_ can be omitted to use the entire array on that side of the colon.

### Under the Hood: Slice Reallocation & References
Slices hold references to an underlying array. If a function takes a slice argument, changes made to elements are visible to the caller (e.g., `func (f *File) Read(buf []byte) (n int, err error)` from *Effective Go*).
If `append()` exceeds capacity, a new array is allocated. *Effective Go* demonstrates this underlying reallocation logic:

```go
func Append(slice, data []byte) []byte {
    l := len(slice)
    if l + len(data) > cap(slice) {
        newSlice := make([]byte, (l+len(data))*2) // Double the capacity
        copy(newSlice, slice)
        slice = newSlice
    }
    slice = slice[0:l+len(data)]
    copy(slice[l:], data)
    return slice
}
```

* **Tricky Slices (Memory Overwrites):** `append()` only creates a new array when capacity is exhausted. If multiple slices point to the same array with remaining capacity, appending to one can maliciously overwrite the data of the other. **Rule:** Always reassign the result to the same variable: `mySlice = append(mySlice, val)`.

```go
// Example of the overwrite danger
i := make([]int, 3, 8) // Length 3, Capacity 8
j := append(i, 4)      // j is [0 0 0 4]
g := append(i, 5)      // g is [0 0 0 5], but it overwrites j's 4!
// i, j, and g all point to the same underlying array memory
```

- Slices can hold other slices, effectively creating a matrix, or a 2D slice.

```go
rows := [][]int{}
rows = append(rows, []int{1, 2, 3})
rows = append(rows, []int{4, 5, 6})
fmt.Println(rows)
// [[1 2 3] [4 5 6]]
```
### Variadic Functions & Range
* **Variadic Functions:** Accept arbitrary final arguments via `...` syntax (`func concat(strs ...string)`). Inside, arguments are a slice.
* **Spread Operator:** Use `slice...` to unpack a slice into a variadic function.

```go
func printStrings(strings ...string) {
	for i := 0; i < len(strings); i++ {
		fmt.Println(strings[i])
	}
}

func main() {
    names := []string{"bob", "sue", "alice"}
    printStrings(names...) // Unpacks the slice into the variadic args
}
```

* **Range:** Iterates over slices/maps. The element returned is a *copy* of the value: `for index, value := range slice {}`.

```go
fruits := []string{"apple", "banana", "grape"}
for i, fruit := range fruits {
    fmt.Println(i, fruit)
}
```

### Maps
Unordered key-value mapping. Passed by reference (functions mutate the original).
* **Creation:** `make(map[string]int)`. Zero value is `nil`.

```go
ages := make(map[string]int)
ages["John"] = 37
delete(ages, "John") // Removing an entry
```

* **Key Constraints:** Keys must be *comparable* types (booleans, numerics, strings, pointers, structs). Slices, maps, and functions *cannot* be keys.
* **Struct Keys:** Highly efficient for multi-dimensional data, avoiding unwieldy nested maps that require constant initialization checks.

    ```go
    // Better than map[string]map[string]int
    type Key struct { Path, Country string }
    hits := make(map[Key]int)
    hits[Key{"/", "vn"}]++
    ```
* **Comma-Ok Idiom:** Accessing a missing key returns the zero-value. Use multiple assignment to distinguish a missing entry.
    ```go
    if seconds, ok := timeZone[tz]; ok {
        return seconds // Found
    }
    ```
* **Sets:** Implement a Set using a map with an empty struct value (`map[string]struct{}{}`) to consume 0 bytes of memory.

    ```go
    attended := map[string]struct{}{
        "Ann": {},
        "Joe": {},
    }
    ```
* **Concurrency:** Maps are **not thread-safe**. Concurrent reads/writes will cause a fatal panic.

## 8. Structs and Pointers

### Structs
Groupings of typed fields. Go's alternative to classes.
* **Nested vs. Embedded:**
    * *Nested:* Requires nested dot access (`lanesTruck.car.brand`).
    * *Embedded:* Provides "data-only inheritance". Embedding `car` inside `truck` promotes the `car` fields to the top level (`lanesTruck.brand`).

```go
type car struct {
  brand string
  model string
}

type truck struct {
  car // Embedded struct
  bedSize int
}

lanesTruck := truck{
  bedSize: 10,
  car: car{ brand: "Toyota", model: "Tundra" },
}
fmt.Println(lanesTruck.brand) // Accessible directly at top level
```

* **Anonymous Structs:** Instantiated inline. Ideal for one-off data shapes (e.g., shaping JSON data in HTTP handlers) to prevent developers from accidentally reusing them.

```go
myCar := struct {
  brand string
  model string
} {
  brand: "Toyota",
  model: "Camry",
}
```

* **Memory Layout:** Fields occupy contiguous memory. Go aligns fields with padding to match sizes. Reordering fields from largest to smallest type size can drastically reduce memory usage. Inspect size via `reflect.TypeOf(stats{}).Size()`.
* **Empty Struct (`struct{}{}`):** A unary value consuming 0 bytes. Used often in maps and channels.

### Pointers & Methods
Pointers store the memory address of another variable.
* **Syntax:** `*` defines a pointer type or dereferences it. `&` generates the pointer address. Zero value is `nil`.

```go
myString := "hello"
myStringPtr := &myString // Address of myString
*myStringPtr = "world"   // Dereference to change original value
```

* **No Arithmetic:** Unlike C, Go has no pointer arithmetic.
* **Struct Field Shorthand:** Go allows `ptr.Field` instead of requiring explicit dereferencing `(*ptr).Field`.
* **Receivers:** Methods attach to structs. *Value receivers* get a copy; *Pointer receivers* allow mutation. Go automatically derives pointers for pointer-receiver methods.

```go
type circle struct { radius int }

// Method with a pointer receiver (can modify the caller's struct)
func (c *circle) grow() {
    c.radius *= 2
}
```

* **Pass by Reference:** Passing pointers as arguments allows functions to mutate caller data.

```go
func increment(x *int) {
    *x++
}

func main() {
    x := 5
    increment(&x) // x is now 6
}
```

* **Pointer Performance:** Pointers are *not* always faster. Local non-pointer variables allocate on the stack (lightning fast). Pointers often cause variables to escape to the heap (slower, requires GC). Use pointers when you explicitly need a shared reference or to avoid copying massive structs.
* Nil Pointers: If a pointer points to nothing (the zero value of the pointer type is `nil`) then dereferencing it will cause a runtime error (a [panic](https://gobyexample.com/panic)) that crashes the program. Generally speaking, whenever you're dealing with pointers you should check if it's `nil` before trying to dereference it.

## 9. Interfaces
Interfaces define behaviors (method signatures). They focus on what a type *does*.
* **Implicit Implementation:** Go has no `implements` keyword. If a type possesses the required methods, it implicitly fulfills the interface. This decouples definition from implementation.

```go
type shape interface {
  area() float64
}

type circle struct { radius float64 }

func (c circle) area() float64 {
  return 3.14 * c.radius * c.radius
}

// circle now implicitly implements shape
```

* **Empty Interface (`interface{}` / `any`):** Implemented by all types.
* **Clean Interface Design:**
    1. **Keep Interfaces Small:** Define minimal required behavior (e.g., standard library's `File` interface requires `io.Closer`, `io.Reader`, etc.).
    2. **No Knowledge of Satisfying Types:** An interface should not be aware of types fulfilling it (e.g., `IsFiretruck()` does not belong on a `Car` interface).
    3. **Name Parameters:** `Copy(sourceFile string, destinationFile string) int` clarifies intent compared to `Copy(string, string) int`.
    4. **Interfaces Are Not Classes:** They do not DRY up struct methods. If five types satisfy `fmt.Stringer`, all five need their own `.String()` method implementation.
* **Type Assertions:** Safely extract concrete types from interfaces.

```go
func printShapeInfo(s shape) {
	c, ok := s.(circle) // Type assertion
	if ok {
		fmt.Printf("s is a circle, radius: %v\n", c.radius)
	}
}
```

* **Type Switches:** Switch statement evaluating underlying types.

```go
func printNumericValue(num interface{}) {
	switch v := num.(type) {
	case int:
		fmt.Printf("It's an int: %T\n", v)
	case string:
		fmt.Printf("It's a string: %T\n", v)
	}
}
```

## 10. Error Handling
Go treats errors as normal values. The built-in `error` is an interface requiring an `Error() string` method.
* **Idiom:** Check errors sequentially: `if err != nil`. A non-nil error denotes failure; return zero-values for other parameters.
    ```go
    // Atoi converts a string to an int. It returns 0 and an error on failure.
    i, err := strconv.Atoi("42b")
    if err != nil {
        fmt.Println("couldn't convert:", err) 
        return
    }
    ```
* **Custom Errors:** Create a struct implementing `Error() string`.

```go
type userError struct {
    name string
}

func (e userError) Error() string {
    return fmt.Sprintf("%v has a problem with their account", e.name)
}
```

* `errors.New`: The Go standard library provides an "errors" package that makes it easy to deal with errors:
```go
	var err error = errors.New("something went wrong")
```
* **Wrapping Context:** Use `fmt.Errorf("error updating user: %w", err)` to embed an error. `%w` preserves the unwrappable error chain for inspection via `errors.Is`. Using `%v` destroys the chain, reducing it to a raw string.
* **Panic & Recover:** `panic()` yeets control up the call stack, crashing the program if unhandled. `recover()` catches it inside a deferred block. **Rule:** Do not use panic for normal control flow (like try/catch). Rely strictly on `error` values. Use `log.Fatal` only for truly unrecoverable states.

```go
func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("recovered from panic:", r)
        }
    }()
    panic("unrecoverable failure")
}
```

## 11. Concurrency
Concurrency is performing multiple tasks simultaneously. Go excels natively via Goroutines and Channels.

### Goroutines
Spawned via `go func()`. Run in the background. If `main` exits, all goroutines are silently killed. To ensure completion, something must block and wait (e.g., reading from a channel or using `sync.WaitGroup`).

### Channels
Typed, thread-safe FIFO queues for goroutine communication.
* **Creation:** `make(chan int)`. Buffered channels: `make(chan int, capacity)` allow holding fixed values before blocking (i.e. sending on a buffered channel only blocks when the buffer is full, and receiving blocks only when the buffer is empty).
* **Operation (`<-`):** Data flows towards the arrow. Sends and receives block until the other side is ready.
* Check if channel is closed: `v, ok := <-ch`. `ok` is false if the channel is empty and closed.

```go
func send(ch chan int) { //Reference type, so channels are passed by reference
    ch <- 99 // Send
}

func main() {
    ch := make(chan int)
    go send(ch)
    fmt.Println(<-ch) // Receive
}
```

* **Signaling with Empty Structs:** Sometimes we don't care about data, only *when* something happens.

```go
func downloadData() chan struct{} {
	downloadDoneCh := make(chan struct{})
	go func() {
		// simulate download time
		downloadDoneCh <- struct{}{} // send a "signal"
	}()
	return downloadDoneCh
}

func processData(downloadDoneCh chan struct{}) {
	<-downloadDoneCh // block until signal is received
	fmt.Println("Data download complete...")
}
```

* **Closing:** Explicitly closed by the sender `close(ch)`.
* **Dave Cheney's Rules on Channels:**
    1. A declared but uninitialized channel is `nil`.
    2. A send to a `nil` channel blocks forever. (Useful in `select` statements to turn off a branch).
    3. A receive from a `nil` channel blocks forever.
    4. A send to a closed channel causes a panic.
    5. A receive from a closed channel returns the zero value immediately (`v, ok := <-ch` handles this).
* **Select Statements:** Listens to multiple channels. Randomly chooses if multiple are ready. A `default` case executes immediately to prevent blocking.

```go
select {
case v := <-ch: //use _ or don't store the value if we want to ignore the value
    // use v
default:
    // receiving from ch would block, execute immediately
}
```

* **Tickers:** `time.Tick()`, `time.After()`, `time.Sleep()` take a `time.Duration` (default nanoseconds; explicitly use `time.Millisecond`).
* **Directional Safety:** `<-chan int` (read-only), `chan<- int` (write-only).
* **Range**: Similar to slices and maps, channels can be ranged over.

```go
for item := range ch {
    // item is the next value received from the channel
}
```

This example will receive values over the channel (blocking at each iteration if nothing new is there) and will exit only when the channel is closed.

### Mutexes (`sync` package)
Mutual Exclusion. Prevents concurrent read/write problems (especially required as Maps are not thread-safe). *Note:* Web Assembly (Wasm) is single-threaded, but code should always be written thread-safe regardless of environment.
* **`sync.Mutex`:** `.Lock()` and `.Unlock()`.
* **`sync.RWMutex`:** Improves read-heavy workloads. Allows unlimited simultaneous readers (`.RLock()`), but strictly blocks all access during an exclusive write (`.Lock()`). Multiple goroutines can safely read from the map simultaneously, as many `RLock()` calls can occur at the same time. However, only one goroutine can hold a `Lock()`, and during this time, all `RLock()` operations are blocked.

```go
func writeLoop(m map[int]int, mu *sync.RWMutex) {
	for i := 0; i < 100; i++ {
		mu.Lock()
		m[i] = i
		mu.Unlock()
	}
}

func readLoop(m map[int]int, mu *sync.RWMutex) {
	mu.RLock()
	for k, v := range m {
		fmt.Println(k, "-", v)
	}
	mu.RUnlock()
}
```

## 12. Generics
Introduced in Go 1.18, Generics use type parameters to reduce code duplication for abstract logic.
* **Type Parameters:** `func split[T any](s []T)`. `any` is a constraint equating to the empty interface.
* **Interface Type Lists (~ Tilde):** Interfaces can list allowed concrete types. The tilde `~int` denotes the *underlying type*, allowing custom types based on int (like `type MyInt int`) to share foundational operators like `<` or `+`.

```go
// Custom Constraint matching built-in and user-defined types based on them
type Ordered interface {
    ~int | ~float64 | ~string
}

func Min[T Ordered](a, b T) T {
    if a < b { return a }
    return b
}
```

* **Custom Interfaces as Constraints:** Ensuring types have specific methods.

```go
type stringer interface {
    String() string
}

func concat[T stringer](vals []T) string {
    result := ""
    for _, val := range vals {
        result += val.String() // Safely utilizes the interface method
    }
    return result
}
```

* **Parametric Constraints:** Interface definitions can accept type parameters.

```go
type store[P product] interface {
	Sell(P)
}
// Enables sellProducts[book](&bookStore, booksSlice)
// and sellProducts[toy](&toyStore, toysSlice) cleanly without broad interfaces.
```

## 13. Enums and Type Systems: A Comparison
Go trades extreme expressiveness for simplicity (The "Grug" mentality). It lacks strict Sum Types, Tagged Unions, or Enums.
* **Error Handling vs. Rust:** Rust uses a `Result` enum (`Ok` or `Err`) coupled with a `match` block, forcing developers at compile-time to handle the error before continuing. Go allows developers to lazily ignore the error and use invalid zero-values.
* **Unions vs. TypeScript:** TypeScript allows union definitions (`type sendingChannel = "email" | "sms"`), making invalid string inputs impossible at compile-time. Go relies on Type Definitions (`type sendingChannel string`), which wrap primitives but cannot strictly block developers from explicitly casting an invalid string (`sendingChannel("slack")`).

```typescript
// TypeScript safely rejects invalid inputs at compile-time
type sendingChannel = "email" | "sms" | "phone";
```
```go
// Go uses type wrapping, but cannot prevent explicit casting of invalid underlying types
type sendingChannel string
convertedSendingCh := sendingChannel("slack")
```

* **Iota:** To simulate Enums, Go uses `iota` inside a `const` block to generate an auto-incrementing integer sequence starting from `0`. It mimics enum relationships but remains just a sequence of numbers.

```go
type sendingChannel int

const (
    Email sendingChannel = iota // 0
    SMS                         // 1
    Phone                       // 2
)
```
### The Custom Type Pattern
To restrict allowed values, define a custom type based on a primitive (e.g., `string`), then declare constants of that type.
```go
// 1. Define custom type
type sendingChannel string

// 2. Declare typed constants
const (
    Email sendingChannel = "email"
    SMS   sendingChannel = "sms"
    Phone sendingChannel = "phone"
)

// 3. Enforce type in signature
func sendNotification(ch sendingChannel, message string) {
    // Implementation
}
```

### Type Safety Mechanics
Custom types provide strict boundaries for **variables**. The compiler catches mismatched explicit variables, preventing accidental misuse in complex systems.
```go
func triggerAlert() {
    sendingCh := "slack" // Inferred as standard string
    
    // ERROR: cannot use sendingCh (type string) as type sendingChannel
    sendNotification(sendingCh, "hello") 
}
```

### Limitations & "Gotchas"
Because the custom type merely wraps a primitive, Go does not enforce exhaustive strictness. Two main loopholes exist:

**1. Implicit Conversion of Untyped Literals**
Go automatically coerces untyped string literals to the custom type if the underlying primitive matches.
```go
func triggerAlert() {
    // NO ERROR: Untyped literal "slack" is implicitly coerced.
    sendNotification("slack", "hello")
}
```

**2. Explicit Type Conversion**
Because the underlying memory representation of the custom type is identical to the primitive, explicit casting bypasses compiler safety checks entirely.
```go
func triggerAlert() {
    sendingCh := "slack" 
    
    // NO ERROR: Explicit casting overrides type boundaries.
    convertedSendingCh := sendingChannel(sendingCh)
    sendNotification(convertedSendingCh, "hello")
}
```

## 14. Go Proverbs (by Rob Pike)
Wise words guiding idiomatic Go architecture (referenced from Gopherfest 2015):
* Don't communicate by sharing memory, share memory by communicating.
* Concurrency is not parallelism.
* Channels orchestrate; mutexes serialize.
* The bigger the interface, the weaker the abstraction.
* Make the zero value useful.
* `interface{}` says nothing.
* Gofmt's style is no one's favorite, yet gofmt is everyone's favorite.
* A little copying is better than a little dependency.
* Syscall must always be guarded with build tags.
* Cgo must always be guarded with build tags.
* Cgo is not Go.
* With the unsafe package there are no guarantees.
* Clear is better than clever.
* Reflection is never clear.
* Errors are values.
* Don't just check errors, handle them gracefully.
* Design the architecture, name the components, document the details.
* Documentation is for users.
* Don't panic.