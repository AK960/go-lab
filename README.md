# go-lab

## Conventions
### Data Types and Variables
- Declaration
```go
// Basic syntax
var <variable_name> <data_type> = <value>
const <const_name> <data_type> = <value> // constant

// Examples
var s string = "Moin"
var i int = 100
var f float32 = 1.2 // or float64
var b bool = true

// Alternative declaration methods
var ( 
	s string = "foo"
	i int = 123
)
var s,t string = "string1", "string2"
s := "Hello World" // Short variable assignment operator := - no need to use var or declare type

/* 
Zero values (declared value without initializing it with a value, gets default value)
bool    -> false
int     -> 0
float   -> 0.0
string  -> ""
pointers / functions / interfaces / maps -> nil
*/

// Constants 
// - must be declared and initialized at the same time
// - cannot use := operator
const age = 12 // untyped
const name string = "Alex" // typed
// it is not possible to reassign const values


// Visibility 
func Format (...) ... // exported, can be used by other packages
func magnitude (...) ... // not exported, only accessible within own package
```
- Output
```go
// Printing variables
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

    fmt.Printf("Template string %s", user) // Other format specifiers:
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
	
	// Find type of variable:
	fmt.Printf("Variable has value %v and is type of %T", user, user)
	
	// Can also find type of value / literal
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
// Types can be converted but there is no guarantee it remains intact
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// Convert int -> float32
	// The other way around we loose the floating values, i.e. 45.67 -> 45
	var i int = 90
	var f = float32(i)
	fmt.Printf("%.2f\n", f)
	
	// String conversion with strconv
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
- switch case statement
- Usage:
```go
// Basic syntax
if (condition_0) { // Parentheses around conditions are optional
    // exec if condition == True
} else if (condition_1) {
    // exec if condition_1 == True
	// can have n else if statements
} else { 
	// exec if above condition_n == False
}


```