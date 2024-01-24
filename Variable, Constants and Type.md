# Variable, Constants and Type

# Variable in Go
- We use the syntax `var <name> <type>` for declaration, if not declared with a specific value, they will be zero-valued.
- We can also use the `:=` syntax **in function** to declare a variable and Go will infer the type. (commonly used)
```go
package main
import (
  "fmt"
  "math/pow"
)

func main(){
  var a string = "hello, world"
  var b int = 10
  c := 3.14 // equal to var c float64 = 3.14
  var e,f int = 4,5
  
  var (
    lmao string = "this is a joke"
    bigggg float64 = math.Pow(2,15)
  )
  
  fmt.Println(a)
  fmt.Println(b)
  fmt.Println(c)
  fmt.Println(e, f)
}
```

*Output*:
```
~/workspace/golearn % go run helloworld.go
hello, world
10
3.14
4 5
```

## Data-type in Go
- bool (true/false)
- string
- int/int8/int16/int32/int64
```
int = platform-dependent size (32/64 bit)
int8 = 8 bit
...
```
- uint/uint8/uint16/uint32/uint64
- uintptr
- byte == uint8
- rune == int32 (represent a Unicode code point)
- float32/float64
- complex64/complex128

> You should understand the difference of types in Go to write effective program.

> In most case, just use `int/uint` unless there is a specific reason.

> In the case you need to represent *very* big number, use `math/big` package.

# Constants
We declare constant with `const <name> <type>`.
```go
package const_in_go
import "fmt"

func main(){
  const Pi = 3.14 // Type will be deduced by compiler
  const ( // Declare multiple const at once!
    No         = !Yes
    Yes        = true
    MaxDegrees = 360
    Unit       = "radian"
   )
  const s string = "A string!" // this is also okay!
}
```

# Type conversions/inference in Go
**Syntax:**
```go
T(v) // converts the value v to type T
i := 42
j := float64(i)
k := uint(j)
```

> PROTIP: If you want to assign a value from one type to another, you **MUST** explicitly convert the assigned value to the same type as the "receiver".
```go
package main
import (
  "fmt"
  "math"
)

func main(){
  var i int = 10
  var j float64 = math.Sqrt(i) // in this case, i need to be float64
  var k uint = uint(j)
  
  fmt.Println(k)
}
```

> Another thing worth noting is: When declaring a variable without specifying the type explicitly, the type will be inferred from the type of the value on the right side
```go
var number int = 10
j := number // j will be int
```
