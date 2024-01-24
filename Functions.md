# Functions

# **Functions**
## Syntax
```go
func <name> (<parameter_name parameter_type>) (<return_type>) {
  // do something here
  return ...
}
```
**For example,**
```go
package main
import "fmt"

func swap(x,y string) (string, string) {
  return y, x
}

func main(){
  a, b := "hello", "world"
  fmt.Println(swap(a,b))
}
```

## `defer`
A defer statement defers the execution of a function until the surrounding function returns.
```go
package main

import "fmt"

func main() {
	defer fmt.Println("world") // this line will be executed at the end, when everything finished.

	fmt.Println("hello")
}

```

# **Methods**
> Go does not have classes. However, you can define methods on types.
> You can only declare a method with a receiver whose type is defined in the same package as the method. 

## Syntax
```go
// func (<receiver-name> <receiver-Type>) <name>() <return-type> {}
package main

import "fmt"

type Person struct{
  name string
  age int
}

func (p Person) printPerson() string{
  res = fmt.Sprintf("The name is %s and the age is %d", p.name, p.age)
  return res
}

func main(){
  newPer = Person{"noob", 69}
  fmt.Println(newPer.printPerson())
}
```