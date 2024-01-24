# IO in Go

# IO in Go
*This sections mostly refer to the [fmt](https://pkg.go.dev/fmt) package in Go.*

> The ‘fmt’ package is responsible for *handling input and output operations with standard functionalities such as reading input from the console and printing output to the console. It’s also used for more advanced operations like formatting strings and data.

## Basic functions
1. Printing
- `fmt.Println` - print a string and append a newline
- `fmt.Print` - print a string and not append a newline
- `fmt.Printf` - similar as C's `printf`

2. Receiving input
- `fmt.Scanln` - scan until hits a newline
- `fmt.Scanf` - read input according to specified format

3. Building formatted strings
- We use `fmt.Sprintf` for building formatted string, then use `fmt.Println` to print to console.
```go
package main
import (
  "fmt"
)

func main(){
  name := "Perez"
  age := 33
  s := fmt.Sprintf("My name is %s and I am %d years old.", name, age)
  fmt.Println(s)
}
```

4. Parse data from a formatted string
```go
package main
import "fmt"

func main(){
  // assume this is received from input
  inputString := "Name: Perez - Age: 30"
  
  var name string
  var age int
  
  // This function will return 2 values: numbers of items parsed and error (if there is)
  n, _ := fmt.Sscanf(inputString, "Name: %s - Age: %d", &name, &age)
  
  fmt.Printf("Parsed %d values: Name=%s, Age=%d", n, name, age)
}
```

## Formatted-string verbs
1. General
- `%v`: the value in a default format (general purpose)
- `%T`: a Go-syntax representation of the type of the value
- `%t`: the word true or false (for booleans)
2. String
- `%s`: string
3. Number
- `%d`: integer
- `%b`: the binary representation
- `%x`	 base 16, with lower-case letters for a-f
- `%X`	 base 16, with lower-case letters for a-f
4. Floats
- `%g` - general purpose of formatting floating-point number
  - `%e` for large exponents
  - `%f` otherwise
- `%e`, `%E` - scientic notation (example: `1e10`)
- `%f` - normal float
5. Precision and Width
- Width: the total number of present number
- Precision: how many numbers after the period
```
%f     default width, default precision
%9f    width 9, default precision
%.2f   default width, precision 2
%9.2f  width 9, precision 2
%9.f   width 9, precision 0
```
6. Flags
- `+` - always print sign for numbers
- `#` - alternate format (add 0x to hex)