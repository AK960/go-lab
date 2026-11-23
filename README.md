# go-lab

## Conventions
### Data Types and Variables
- Declaration
```go
// basic syntax
var <variable_name> <data_type> = <value>
const <const_name> <data_type> = <value> // constant

// examples
var s string = "Moin"
var i int = 100
var f float32 = 1.2 // or float64
var b bool = true

// alternative declaration methods
var ( 
	s string = "foo"
	i int = 123
)
var s,t string = "string1", "string2"
s := "Hello World" // short variable assignment operator := - no need to use var or declare type

/* 
zero values (declared value without initializing it with a value, gets default value)
bool    -> false
int     -> 0
float   -> 0.0
string  -> ""
pointers / functions / interfaces / maps -> nil
*/

// constants 
// - must be declared and initialized at the same time
// - cannot use := operator
const age = 12 // untyped
const name string = "Alex" // typed
// it is not possible to reassign const values


// visibility 
func Format (...) ... // exported, can be used by other packages
func magnitude (...) ... // not exported, only accessible within own package
```
- Output
```go
// printing variables
package main

import (
	"fmt"
	"reflect"
)

func main () {
    var greet string = "Moinsen"
    var user string = "Alex"
    fmt.Println(greet)              // print value
    fmt.Println(greet, user, "!")   // print value and string together

    fmt.Printf("Template string %s", user) // other format specifiers:
    /*
        %v - default format (type agnostic, interprets the type)
        %d - decimal integers
	    %T - type of the value
	    %c - character
	    %q - quoted characters/string
	    %s - plain string
	    %t - true or false
	    %f - floating numbers
	    %.2f - floating numbers up to 2 decimals        
    */
	
	// find type of variable:
	fmt.Printf("Variable has value %v and is type of %T", user, user)
	
	// can also find type of value / literal
	fmt.Printf("Type: %v \n", reflect.TypeOf(user))
}
```
- Input
```go
package main

import "fmt"

func main() {
	var name string
	fmt.Print("Enter your Name:\n")
	fmt.Scanf("%s", &name)

	var muggle bool
	fmt.Printf("Enter the name and if you are muggle:\n")
	fmt.Scanf("%s %t", &name, &muggle)

	var a string
	var b int
	fmt.Print("Enter a string and a number:\n")
	count, err := fmt.Scanf("%s %d", &a, &b) 
	    // causes error when another string is pasted, b will be set to zero-value of an integer, thus 0
	fmt.Println("count:", count)
	fmt.Println("error:", err)
	fmt.Println("a:", a)
	fmt.Println("b:", b)
}
```
- Type casting (converting between types)
```go
// types can be converted but there is no guarantee it remains intact
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// convert int -> float32
	// the other way around we loose the floating values, i.e. 45.67 -> 45
	var i int = 90
	var f = float32(i)
	fmt.Printf("%.2f\n", f)
	
	// string conversion with strconv
	// int -> string with Itoa (returns string)
	var j int = 10
	var s string = strconv.Itoa(j)
	fmt.Printf("%q", s) // yields "10"

	// string -> int with Atoi (returns int and error if any)
	var k string = "100"
	var l string = "299abc" // would throw an eror for invalid type conversion
	t, err := strconv.Atoi(k)
	fmt.Printf("%v, %T \n", t, t) // 100, int
	fmt.Printf("%v, %T \n", err, err) // <nil>, <nil>
}
```

### Operators and Control Flow
#### Operators
There are five groups of operators:
- Comparison
  - Compare operands and yield a boolean value
  - Allow values of the same type
  - Values: `==, !=, <, <=, >, >=`
- Arithmetic
  - Operations like add, subtract, divide, multiply, etc. 
  - Values: `+, -, *, /, %, ++, --`
- Logical
  - Can be used to support control flow
  - Determine logic between variables or values
  - Values: `&&, ||, !`
- Assignment
  - Assign values or add them to structures
  - Values: `=, +=, -=, *=, /=, %=`
- Bitwise
  - Perform bit by bit operation
  - Values: `&, |, ^, >>, <<` (and, or, xor, right shift, left shift)

#### Control Flow
- if, else, and else if statement
```go
// basic syntax
if (condition_0) { // Parentheses around conditions are optional
    // exec if condition == True
} else if (condition_1) {
	// exec if condition_1 == True
	// can have n else if statements
} else { 
	// exec if above condition_n == False
}
```
- switch case statement
```go
// basic syntax
switch (expression) {
	case value_1:
		// exec if value_1
		break // is implicit in Go, so no need to specify it explicitly
	case value_2:
		// exec if value_2
		fallthrough // forces execution flow through the case block to the next one (independent if condition is met)
	case value_n:
		// exec if value_n
	default:
		// exec when no match is found
}
```
- for loop
```go
// basic syntax
for initialization; conditions; post {
	// statements
}
// example
for i := 1; i <= 3; i++ { // initialization (optional), condition, post (optional)
	fmt.Println(i*i)
}

// alternative
i := 1
for i <= 5 {
	fmt.Println(i*i)
	i += 1
	if i == 2 {
		continue // skips next steps but starts next iteration
	}
	if i == 3 {
		break // ends loop
	}
}
```

### Arrays, Slices, and Maps
- Arrays
	- Are a collection of similar data elements stored at contiguous memory locations. Each element has an address and the memory interval is the size of the element, i.e., 4 bytes for int.
	- Fixed length, same datatype
	- Has pointer that points to first element, following addresses can be calculated from it
	- Properties length (# of elements), capacity (# of elemnts it can contain)
```go
// basic syntax
var <array_name> [<array_size>] <data_type>

// declaration
var grades [5] int
fmt.Println(grades) // [0 0 0 0 0]

// initialization
var grades [5] int = [5] int {10, 20, 30, 40, 50} // , or:
grades := [5] int {10, 20, 30, 40, 50} // , or:
grades := [...] int {10, 20, 30, 40, 50} // compiler gets implicit length

// length
fmt.Println(len(grades)) // 5, is not indexed (counts 1...5)

// access via index
grades[<index>] // in loop use i < len(array) starting from 0

// alternative loop
for index, element := range grades { // values that store return values from range expression
	fmt.Println(index, "=>", element)
}
/* Output:
0 => 10
1 => 20
...
4 => 50
*/

// multidimensional arrays
grades := [3][2] int {{2, 4}, {4, 16}, {8, 64}} // array with capacity=3 where each elem holds an array with capacity=2, to access:
grades[1][1] // 16
```
- Slices
  - More flexible than arrays
  - Size is not fixed, length can grow and shrink
  - Store elements of the same data type
  - Has three major components: pointer, length, capacity
    - Pointer: points to the first element of the array, that is accessible through that slize. It does not have to be the first element of the array.
    - Length [len()]: number of elements the slice contains
    - Capacity [cap()]: number of elements in the underlying array, counting from the first element of the slice. Mostly used with slices but can be used with arrays to (here however it equals the length of the array)
  - When a slice is created, an array is created first and the compiler returns a slice reference to it
  - Slices get their own indexation as well
    - Changing the value within the slize changes the value within the underlying array as well, since it is only a reference to that value
```go
// basic syntax
<slice_name> := []<data_type>(<values>) // similar to array, size of the slice is not mandatory

// declaring a lice from an array
array := [5]int{1,2,3,4,5}
slice_0 := array[<start_index>:<end_index>] // start index is included, end index not (array[0:3] has 0,1,2)

// declaring a subslice form a slice
slice_1 := slice_0[:] // includes []min:max] by default if left blank

// declaring slice through make function
// it creates an array with the size of the given capacity and returns a slice which refers to it
slice := make([]<data_type>, length, capacity) // capacity is optional
slice := make([]inst, 5, 8)
fmt.Println(slice)      // [0 0 0 0 0]
fmt.Println(len(slize)) // 5
fmt.Println(cap(slize)) // 8

// appending elements to a slice with append()
// if the underlying array is too small, a new one will be allocated under the hood with two times the capacity of the initial array
func append(s []Type, vs ...Type) []Type // (slice of type, new elements) resulting Type of the function
slice := append(slize, 10, 20, 30) 

// appending another_slice to a slice by using ...-Operator
// the operator is used for variadic functions, that can take arbitrary number of arguments
slice = append(alice, another_slize...) 

// deleting from a slize by creating a new slize that does not contain the value to be deleted
arr := [5]int{10, 20, 30, 40, 50}
i := 2
slice_1 := arr[:i]
slize_2 := arr[i+1:]
new_slize := append(slice_1, slize_2...)

// copying elements from one slize to another
// if the slices are not of the same length, only the values that fit the dest_slice capacity will be copied
func copy(dst, src []Type) int // returns int with the number of elements copied
num := copy(dest_slice, src_slice) // must be of same datatype 
```
- Maps
  - Unordered collection of key-value-pairs (associative arrays in python)
  - Values are stored in a map by a key, that works like an index that points to the value associated with that key)
  - Implemented by hash tables with efficient add, get, delete methods
  - Zero value for a map is `nil`
```go
// basic syntax
var <map_name> map[<key_type>]<value_type>
var my_map map[string]int // creates a nil-map, when adding values to it we would get a runtime error because you cannot add elements to a nil map, must be initialized first:
<map_name> := map[<key_type>]<value_type>{key-value-pairs}
// declare with make function 
<map_name> := (make(map[<key_type>]<value_type, <init_capacity>) 

codes := map[string]string{"en": "English", "fr": "French"}

// access elements by referencing the key_name
fmt.Println(codes["en"]) // --> English

// getting the value associated with a key (getting a key of a map-element)
// when indexing a map in go, we get two return values; first is the value and second a bool if the key exists. If it does not exist, the first return value will be the zero-value
value, found := map_name[key]

codes := map[string]int{"en": 1, "fr": 2}
value, found := codes["en"]
fmt.Println(value, found) // --> 1 true
value, found := codes["sp"]
fmt.Println(value, found) // --> 0 false; zero is zero-value of int

// adding items to a map by using a new index key and assigning a value to it
codes["it"] = 3

// updating values of a key 
codes["en"] = "British English"

// delete key value pair
delete(map, key_name)
delete(codes, "en")

// looping over a map with range expression (returns key and value)
// for arrays and slices it returns index and value, here key and value
for key, value := range codes {
    fmt.Println(key, value)
}

// truncating (clearing all elements from) a map by re-initializing it with an empty map or looping over it and deleting every key-value-pair
for key, value := range codes {
    delete(codes, key)
}
codes = make(map[string]int)
```

### Functions
- Self-contained units of code which carry out a certain job
- Help us to modularize code
- Functions are reusable and help abstract inner implementations
- Also known as method
- It function does not necessarily return something - if not, no return type would be specified and no return-Statement would end the function

#### Basic Syntax
Naming convention:
- must begin with a letter
- can have any number of additional letters and symbols
- cannot contain spaces
- case-sensitive

**Function parameters** are the names listed in the function's definition. We have input parameters and output / return parameters\
**Function arguments** are the real values passed into the function when calling it

- Basic Functions
```go
// basic syntax
func <function_name>(<params>) <return_value_types> { // function signature
    // body of the function
}

func addNumbers(a int, b int) int {
    return a+b
}

// calling or storing return values
addNumbers(3, 4)
sum := addNumbers(3, 4)

// returning multiple values
func operation(a int, b int) (int, int) {
    sum := a+b
    diff := a-b
    return sum, diff    
}

func operation(a int, b int) (sum int, diff int) { // return parameters can be given names
    sum = a+b // omit ":", already declared in arguments
    diff = a-b
    return // this way, no need to specify them here again
}

sum, diff := operation(5, 3)
_, diff := operation(5, 3) // stores only diff, omits sum
sum, _ := operation(5, 3) // stores only sum, omits diff
```

#### Other function types
- Variadic Functions
  - Accepts variable number of arguments of same type
  - To declare: the type of the final parameter is preceded by an ellipsis "..."
```go
// basic syntax
func <func_name>(param_1 type, param_2 type, param_3 ...type) <return_type> {} // function can be called with any number of parameters of <return_type>

func sumNumbers(numbers ...int) int {} // numbers variable will contain a slice of all the arguments
func sumNumbers(str string, numbers ...int) {}
```
- Recursive Functions
  - Recursion is a concept where a function calls itself by direct or indirect means
  - Keeps calling itself until it reaches a base case
  - Used to solve a problem, where the solution is dependent on the smaller instance of the same problem
```go
func factorial(i int) int {
    if i == 1 { return 1 }
    return i * factorial(i-1) // function keeps returning until it reaches i == 1
}
fact := factorial(5) // 5*4*3*2*1 = 120
```
- Anonymous Functions
  - Is a function that is declared without any declared identifier to reference it
  - Used for containing functionality that does not need to be named and is possibly only for short term use
```go
x := func(l int, b int) int {
    return l*b
}
fmt.Printf("%T", x) // type: func(int, int) int
fmt.Println(x(20, 30)) // 600

x := func(l int, b int) int {
    return l*b
}(20, 30) // directly calls anonymous function

fmt.Printf("%T", x) // int, because x stores the result this time
fmt.Println(x(20, 30)) // 600
```
- High Order Functions
  - Function that receives a functions as an input or returns it as an output
  - Compositions: Create smaller functions that take care of certain parts of logic and then compose complex functions out of them
  - Reduced bugs (easier debugging through modularity)
  - Easy to read and understand → better code
```go
# circle properties
func area(r float64) {
    return 3.14 * r * r
}

func perimeter(r float64) {
    return 2 * 3.14 * r
}

func diameter(r float64) {
    return 2 * r
}

func print(radius float64, calcFunction func(r float64) float64) {
    result := calcFunction(radius)
    fmt.Println("Result: ", result)
}

func getFunction(query int) func(r float64) float64 {
    query_to_func := map[int]func(r float64) float64 {
        1: area,
        2: perimeter,
        3: diameter
    }
    return query_to_func[query]
}

func main() {
    var query int
    var radius float64
    
    fmt.Printl("Enter radius:")
    fmt.Scanf("%f", &radius)
    fmt.Printf("Enter \n 1 - area \n 2 - perimeter \n 3 - diameter")
    fmt.Scanf("%d", &query)
    
    print(radius, getFunction(query))
}
```
- Defer Statement
  - delays execution of a function, until surrounding function returns
  - deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns
  - basically, the next function in the control flow is executed before the deferred function

### Pointers
- Used for memory management (RAM) in programs
- For every variable that is declared, a certain amount of memory is allocated at runtime
- A pointer is a variable that holds the memory address of another variable
- The most used operators with pointers are:
  - **Address-of Operator (&)**: the address of a variable can be obtained with the `&` operator, it is in hex
  - **Dereference Operator (*)**: when placed before an address, it returns the value at that address
```go
/* quick note
x := 77         // value of x
&x = 0x0301     // address of x
*0x0301 = 77    // value at the address
*/

// variable is declared and receives an address
x := 1 // address is i.e. 0x0301

// now we can init a pointer, that points on the memory address, where x is stored
var ptr *int := &x // value of ptr is 0x0301; must be of type int because the underlying memory block is allocated based on the size of int

// example
package main

import fmt

func main() {
    i := 10
    fmt.Printf("%T %v\n", &i, &i)       // this would fetch us the address of i
        // output: *int 0xc00018c008
    fmt.Printf("%T %v\n", *(&i), *(&i)) // we can then dereference this address with the dereference operator 
        // output: int 10
}
```
- Declaring and Initializing Pointers
```go
// basic syntax
var <pointer_name> *<data_type>         // option 1: the * is the convention to declare a pointer, it is not the same as the dereference operator!
                                        // data_type regards the type of the variable whose address we want to store
var <pointer_name> = &<variable_name>   // option 2: data_type will be internally determined by the compiler
<pointer_name> := &<variable_name>      // option 3: usage with shorthand declaration operator

// example
i := 10
s := "hello"
f := 1.42
var iptr *int = &i  // option 1
var sptr = &s       // option 2, we initialize the value with the address of another variable 
fptr := &f          // option 3
```
- Dereferencing a Pointer
```go
// basic syntax
*<pointer_name>
*<pointer_name> = <new_value> // assigns a new value to the address
// when placing * before an address, we get the value stored at that address
// since a pointer is also a variable that stores the address of a variable, we can use the * here to get the value at that address

x := 77
var iptr *int := &i
*iptr = 100 // changes x to 100, address remains the same
```
- Passing by Value in Functions
	- Function is called by directly passing the value of the variables as an argument: the parameter is copied into another location of the memory
	- When accessing or modifying the variable within a function, only the copy is accessed or modified, the original value remains the same
- Passing by Reference in Functions
	- Here, the address of the variable is passed into the function as parameter
	- All operations the function performs it thus on the value stored at the address of the actual parameter
	- Slices and maps are passed by reference by default
	- Arrays are not passed by reference but by value! Hence, the elements are copied one by one and the original array does not change when a copied element changes
```go
// basic variables
func modify(s *string) {
	*s = "world"
}

func main() {
	a := "hello"
	fmt.Println(a) 	// hello
	modify(&a)		// here, the value that is stored under the address passed to the function is modified
	fmt.Println(a) 	// world
}

// slices
func modify(arr []int) {
	arr[0] = 100
}

func main() {
	slice := []int{10, 20, 30} // remember: slice itself is a reference to an underlying array
	fmt.Println(slice) // 10 20 30 
	modify(slice)
	fmt.Println(slice) // 100 20 30
}

// maps
func modify(m map[string]int) {
	m["K"] = 75
}

func main() {
	ascii := make(map[string]int)
	ascii["A"] = 65
	ascii["F"] = 70
	fmt.Println(ascii) // map[A:65 F:70]
	modify(ascii)
	fmt.Println(ascii) // map[A:65 F:70 M:85]
}
```

### Structs
#### Initialization and Declaration
- User defined data type that is a structure that groups together different data elements
- Provide a way to reference a series of grouped values through a single variable / struct name
```go
// basic syntax init
type <struct_name> struct { // type keyword means user defined type
	// list of fields
}
// basic syntax declaration 
var <variable_name> <struct_name>
<variable_name> := new(<struct_name>) { // returns a pointer to the struct
	<field_name>: <value>,
	<field_name>: <value>, // must have , at the end of last field
}
// simplified (not recommended)
<variable_name> := <struct_name>{<value_1>, <value_2>} // must be in order with initialization to be declared correctly

// example
package main
import fmt
import fmp

// declaration
type Student struct { 
	name string
	rollNo int
	marks []int
	grades map[string]int
}

func main() {
	// initialization (once declared, can be initialized in a number of ways)
	var s Student // creates a local instance of type Student with zero-value for its fields
	fmt.Printf("%+v", s) // {name:  rollNo:0 marks:[] grades:map[]}
	
	stud := new(Student) // stud is a pointer of type student
	fmt.Printf("%+v", stud) // &{name:  rollNo:0 marks:[] grades:map[]}
}
```
- Accessing fields
```go
type Circle struct {
	x int
	y int
	radius int
}

func main() {
	var c Circle
	c.x = 5
	c.y = 5
	c.radius = 9

	circ := new(Circle) {
		x: 5,
		y: 5,
		radius: 9,
	}
}
```
- Passing structs to Functions
```go
type Circle struct {
	x int
	y int
	radius int
	area float64
}

func calcArea(c *Circle) { // must be pointer! see below
	const PI float64 = 3.14
	var area float64
	area = (PI * c.radius * c.radius)
	(*c).area = area
}

func main() {
	// must pass pointer! to change values of a struct, we must pass it by reference, otherwise it will be copied by value and the orig won't be changed
	c := Circle(x:5, y:5, radius:9, area:0)
	fmt.Printf("%+v", c)
	calcArea(&c) // must pass address to struct!
	fmt.Printf("%+v", c)
	// {x:5 y:5 area:0}
	// {x:5 y:5 area:254.34}
}
```
- Comparing structs
	- Same type or same values with equality operators
	- Struct instances that are to be compared must be of same type!
		- We cannot compare instances of `type s1 struct` with instances of `type s2 struct` on
		- This however only only applies for the actual struct!
```go 
type s1 struct {
	x int
}

type s2 struct {
	x int
}

func main() {
	c := s1{x: 5}
	c1 := s2{x: 5}
	if c == c1 { // invalid operations! c == c1 (mismatched types s1 and s2)
		fmt.Println("yes") 
	}
	
	c := s1{x: 5}
	c1 := s1{x: 6}
	c2 := s1{x: 5}
	if c != c1 { // true
		fmt.Println("c and c1 have different values") 
	} 
	if c == c2 { // true
		fmt.Println("c is same as c2")
	}
}
```

#### Methods
- Similar to function with one simple addition: a method augments a function by adding an extra parameter section immediately after the `func` keyword that accepts a single argument
	- This is called a `receiver`
	- A receiver is something that a method is associated with: whenever there is a strong relationship between a function and a struct, it makes sense to use a method
- Hence: a method is a function that has a defined receiver and can be associated with a named type
- Faustregel: Braucht auch nur eine Methode einen Pointer-Receiver, nimm ihn für alle Methoden des Typs. Das ist Go-Konvention und vermeidet eine Falle bei Interfaces.
```go
// basic syntax
func (<receiver>) <method_name> (<parameters>) <return_type> {
	// code
}

// any instance of the circle struct will have the methods declared below
type Circle struct {
	radius float64
	area   float64
}

// for methods, there is also a difference if we use references or values as receivers:
// - if the receiver is a pointer, we work on the original struct
// - if the receiver is a value, we work on a copy of the original struct and may need to return a value

// pointer receiver: modifies the original struct
func (c *Circle) calcArea() {
	c.area = 3.14 * c.radius * c.radius
}

// value receiver: only reads the struct and returns a result,
// the original stays untouched -> ideal for getters / calculations
func (c Circle) circumference() float64 {
	return 2 * 3.14 * c.radius
}

// value receiver that tries to modify: only the copy is changed!
func (c Circle) scale(factor float64) {
	c.radius = c.radius * factor // no effect on the caller
}

// fix: return the modified copy instead
func (c Circle) scaled(factor float64) Circle {
	c.radius = c.radius * factor
	return c
}

func main() {
	c := Circle{radius: 5}

	c.calcArea()
	fmt.Printf("%+v\n", c) // {radius:5 area:78.5}

	fmt.Println(c.circumference()) // 31.4

	c.scale(2)
	fmt.Printf("%+v\n", c) // {radius:5 area:78.5} -> unchanged!

	big := c.scaled(2)
	fmt.Printf("%+v\n", big) // {radius:10 area:78.5} -> area is stale now
}
```
##### Value or pointer receiver?

| Use a pointer receiver `(c *T)` when... | Use a value receiver `(c T)` when... |
|---|---|
| the method needs to modify the struct | the method only reads the struct |
| the struct is large (copying would be expensive) | the struct is small and immutable (e.g. `time.Time`, coordinates) |
| the struct contains something that must not be copied, e.g. `sync.Mutex` | the type should behave like a plain value |

##### Pitfall: pointer receivers and interfaces
- When calling a method, Go is forgiving: `c.calcArea()` works even though `c` is a `Circle` and not a `*Circle`. Go silently turns it into `(&c).calcArea()`
- When assigning to an interface, this automatic conversion does not happen:
	- An interface stores a *copy* of the value you put in
	- A pointer method called on that copy would only change the copy, not your original `c`, which would be confusing
	- So Go simply doesn't allow it and reports a compile error
- Result:
	- `*Circle` has all methods (value and pointer receivers)
	- `Circle` only has the methods with value receivers
```go
type AreaCalculator interface {
	calcArea()
}

func main() {
	c := Circle{radius: 5}

	c.calcArea() // ok: Go automatically uses &c

	var a AreaCalculator = c  // compile error:
	                          // Circle does not implement AreaCalculator
	                          // (method calcArea has pointer receiver)

	var b AreaCalculator = &c // ok: *Circle has all methods
	b.calcArea()
}
```

















	