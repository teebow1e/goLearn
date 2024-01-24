# Introduction

# **Hello World - Syntax of a Go program**
## Introduction
```go
package main

import (
  "fmt"
  "math/rand"
)

func main(){
  fmt.Println("hello, world!")
  fmt.Println(math.Pi)
}
```

## Break-down
- Every Go program is made up of package.
  - The program starts in the `main` package.
  - This program also using modules from packages called "fmt" and "math/rand".
- The reason why there is `math.Pi` but not `math.pi` is because `Pi` is an exported name - which means it can be accessed outside a package (when imported).
  - Exported name starts with uppercase letter.