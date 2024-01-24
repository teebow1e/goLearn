# Pointers Advanced

# *Pointers as Receiver for methods*
- We can declare methods with pointer receiver.
```go
func (v *Vertex) Abs() float64 {} 
```

- The problem with value receiver `(v Vertex)` is the fact that with a value receiver, the method will operates on a copy of the original value and **NOT** returning to apply the change.
- With pointer receiver, the value is modified after applied the modification by the method.

### Example
```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
  // pointer receiver: return 50
  // value receiver: return 5
	v := Vertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs())
}
```

- Another convenient thing is that:
  - Methods with pointer receivers - can take either value/pointer (the opposite still applies)
```go
var v Vertex
v.Scale(5)  // OK
(&v).Scale(10) // OK
```
  - Functions w/ pointer arguments - must take a pointer 
```go
var v Vertex
ScaleFunc(v, 5)  // Compile error!
ScaleFunc(&v, 5) // OK
```

### Should choose to use a pointer receiver.
- The first is so that the method can modify the value that its receiver points to.
- The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.