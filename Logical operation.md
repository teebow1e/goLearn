# Logical operation

## For-loop
1. **Single condition (while loop)**
```go
package main
import "fmt"
func main(){
  i := 1
  for i <= 10{
    fmt.Printf("%d\n", i)
    i += 1
  }
}
```

2. **C-style (initial/condition/after)**
```go
package main
import "fmt"

func main(){
  for j := 1; j <= 10; j++ {
    fmt.Printf("%d\n", j)
  }
}
```

3. **Infinity loop**
```go
package main
import "fmt"

func main(){
  for {
    fmt.Println("okok")
    break // there are also "break" or "continue"
  }
}
```


## If/Else
**Syntax**
```go
if <condition> {
  // do something here
} else if <condition> {
  // some more things
} else {}
```

**Short variable declaration** - Declare statement inside if-else clause
```go
func main() {
    if i := 10; i < 10 {
        fmt.Println("i < 10")
    } else if i == 10 {
        fmt.Println("i is 10")
    } else {
        fmt.Println("i is not 10")
    }

    // This line would result in a compilation error because 'i' is not defined in this scope.
    fmt.Printf("I hope i is still here. %d\n", i)
}

```

## Switch case
**Syntax**
```go
func main(){
  i := ...
  switch i {
    case 1:
      ...
    case 2:
      ...
    case 3:
      ...
    default:
      ...
  }
}
```

**Example**
```go
package main
import "fmt"

func main(){
	var inv int
	fmt.Print("Please enter a number: ")
	fmt.Scanf("%d", &inv)
	
	fmt.Println("Investigate your number..")
	switch inv{
	case 1:
		fmt.Println("Entered 1")
	case 2:
		fmt.Println("2 is entered")
	default:
		fmt.Printf("Not sure what %d is\n", inv)
	}
}
```