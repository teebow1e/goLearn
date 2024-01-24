# Advanced Types

# **Pointers**
> A pointer **holds** the memory address of a value.

## Syntax
```go
var p *int
// *T == pointer to a value of type T

i := 42
p = &i
// & operator: assign the address of i to p

fmt.Println(p) // 0xc000118000
fmt.Println(*p) // 42
// * operator: read i through pointer p

*p = 21 // set i through pointer p
```

# **Structs**
> Struct is a typed collection of fields. Grouping multiple data types to create your own type.

## Syntax
```go
package main

import "fmt"

// create Person struct
type Person struct {
	name    string
	age     int
	isIdiot bool
}

func main() {
	// Initialize a Person variable
	newPerson := Person{"tuan", 18, true}
	fmt.Println(newPerson)

	// Accessing struct fields using dot
	fmt.Println(newPerson.name, newPerson.age)

	// Initialize using struct literal
	fmt.Println(Person{name: "tee", age: 1337, isIdiot: false})

	// Zero initialization
	fmt.Println(Person{name: "babysh4rk"})

	// Structs are mutable
	fmt.Println("\nNow I'm gonna prove that struct is mutable (able to be changed during runtime)")
	mutablePerson := Person{"n0rmalguy", 38, false}

	fmt.Printf("Before: %v\n", mutablePerson)
	mutablePerson.age = 60
	mutablePerson.isIdiot = true
	fmt.Printf("After: %v\n", mutablePerson)

	// The dot can be used to access fields can also be used with pointers, and it can modify the struct too!
	p := &mutablePerson
	fmt.Println(p.name, p.age)

	fmt.Printf("\nBefore: %v\n", *p)
	p.name = "MutableGuy"
	p.isIdiot = false
	fmt.Printf("After: %v", *p)
}
```

## Printing structs
- `%v` - print struct with only value
- `%+v` - print struct with name + value

## Initialize a struct
> References: [https://asankov.dev/blog/2022/01/29/different-ways-to-initialize-go-structs/](https://asankov.dev/blog/2022/01/29/different-ways-to-initialize-go-structs/)

Let's use the above Person struct as an example.
1. Built-in options
- Struct literal
```go
p := Person{name: "tee", age: 18, isIdiot: false}
```
- `new` function
```go
p := new(Person)
fmt.Printf("%T\n", p)
p.name = "haha"
p.age = 1
fmt.Println(p)
```


=> The problem now is: All the fields of the person can be viewed and changed by anyone. Also, we have no validation.
2. Constructor function
```go
func NewPerson(name string, age int) *Person {
  // now we have validation!
  if age < 0 {
    panic("NewPerson: age cannot be a negative number")
  }
  return &Person{name: name, age: age}
}
```
Some downsides of this method:
- The function would be bloated and too long if it has many fields:
```go
func NewPerson(firstName, lastName, addressLine1, addressLine2 string, age int) *Person
```
- Backward-compability: This means if you have a large project, if you change something in the constructor, this can lead to compilation errors across all structs.

# **Array**
> In Go, an *array* is a numbered sequence of elements of a specific length. There is also an alternative called "slices".

## Syntax
```go
package main

import "fmt"

func main() {
	// Initialize with: var <name> [<length>]<type of element inside>
	// Array will be default-initialized with zero values
	// Indexing will start from 0, as normal.
	var arr [5]int
	fmt.Println("zero-initialization:", arr)

	// Basic array operation
	arr[0] = 100
	// arr[3] = "hello" // this line will raise an panic since the type of arr is int
	arr[4] = 1337

	fmt.Println("setter:", arr)
	fmt.Println("getter:", arr[4])

	// Use "len" to retrieve length of an array
	fmt.Println("length of arr:", len(arr))

	// One-line initialization with values
	valArr := [6]string{"abc", "hello", "lorem", "ipsum", "lmao", "kma"}
	fmt.Println("another array:", valArr)

}
```

# **Slices**
## Syntax
```go
package main

import (
	"fmt"
	//"slices"
)

func main() {
	// Slices are presented by the elements it contains, not by the number of elements
	var arr [10]int
	var sli []int
	// We can see that, when not initialize with values,
	// array is initialize with 10 0s, while for slice, it's empty.
	fmt.Println("uninitialized array:", arr)
	fmt.Println("uninitialized slice:", sli)
	fmt.Println("Properties:")
	fmt.Println("sli == nil:", sli == nil)
	fmt.Println("len(sli) == 0:", len(sli) == 0)

	// Slice literal
	b := []string{"hello", "hi", "another"}
	fmt.Println(b, "\n")

	// Creating slices with make()
	// make(<slice with type>, length, capacity)
	// length == number of elements it contains
	// capacity == the maximum amount of elements the slice can contain before the need of reallocation
	newSlice := make([]int, 5, 10)
	fmt.Println("make() slice:", newSlice)

	// Basic operation
	newSlice[0] = 123
	newSlice[1] = 3
	newSlice[2] = 2315
	newSlice[3] = 52
	newSlice[4] = 643
	fmt.Println(newSlice)
	fmt.Println(newSlice[0], newSlice[1], newSlice[4])
	fmt.Println("length:", len(newSlice), "\n")

	// Slices appending
	fmt.Println("before:", newSlice)
	newSlice = append(newSlice, 1337, 6969)
	fmt.Println("after:", newSlice, "\n")

	// Copying slices
	// Need to create destination beforehand
	s := make([]int, len(newSlice), cap(newSlice))
	copy(s, newSlice)
	fmt.Println("copied slice:", s, "\n")

	// Slice a slice
	fmt.Println("original slice:", newSlice)
	fmt.Println(newSlice[2:7])
	fmt.Println(newSlice[:6])
	fmt.Println(newSlice[3:])
}
```

# **Range**
> *Range* iterates over elements in a variety of data structures.
## Syntax
```go
package main

import "fmt"

func main() {
	// Iterate over a slice of int
	nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
	sum := 0
	for _, val := range nums {
		fmt.Printf("%v ", val)
		sum += val
	}
	fmt.Println("\n", sum)
}

// Result:
// 1 2 3 4 5 6 7 8 9
// 45
```
- We can iterate through:
  - array/slice
  - map (a.k.a dictionary)
  - string/rune
- Example for iterate through a string:
```go
package main

import "fmt"

func main() {
	str := "lmao"
	for idx, char := range str {
		// This will print the ASCII code
		fmt.Printf("idx %v: %v\n", idx, char)
	}

	for idx, char := range str {
		// string(int) - convert from ascii to char
		fmt.Printf("idx %v: %v\n", idx, string(char))
	}
}
```

# **Maps**
> Maps, or Dictionary.
## Syntax
```go
package main

import "fmt"

func main() {
	// Let's make a map using make()
	// map[key-type]value-type
	m := make(map[string]string)
	// n := map[string]int{"abed": 1, "lmao": 1337}

	// Setter
	m["key1"] = "word"
	m["key2"] = "sentence"
	m["key3"] = "rune"
	m["key4"] = "string"

	// Getter
	fmt.Println(m)
	fmt.Println(m["key1"])
	fmt.Println(m["key3"])

	fmt.Println(m["doesnotexist"]) // return ""

	// Maps operation

	// delete a key-val pair
	delete(m, "key2")
	fmt.Println(m)

	// delete the whole map
	clear(m)
	fmt.Println(m)

	// verify if a pair exists or not
	_, existence := m["key2"] // second return value indicates existence
	fmt.Println("m[\"key2\"]", "exist?:", existence)

	// Better way to print a map
	n := map[string]int{"number_one": 1, "number_two": 2}
	printMaps(n)
}

func printMaps(m map[string]int) {
	for k, v := range m {
		fmt.Printf("%v: %v\n", k, v)
	}
}
```

# **Interface**
> Very good guide: [https://www.alexedwards.net/blog/interfaces-explained](https://www.alexedwards.net/blog/interfaces-explained)
> *Interfaces* are named collections of method declaration.

## Syntax
```go
type Stringer interface {
    String() string
}
```
- At this point, a type/value **satisfying** this interface if it has a method with the exact signature (declaration) as `String() string`
```go
type Book struct {
    Title  string
    Author string
}

func (b Book) String() string {
    return fmt.Sprintf("Book: %s - %s", b.Title, b.Author)
}
```
- This `Book` type now satisfies the `Stringer` interface because it has the `String() string` method defined.
- So, what is the use case?
```go
package main

import (
    "fmt"
    "strconv"
    "log"
)

// Declare a Book type which satisfies the fmt.Stringer interface.
type Book struct {
    Title  string
    Author string
}

func (b Book) String() string {
    return fmt.Sprintf("Book: %s - %s", b.Title, b.Author)
}

// Declare a Count type which satisfies the fmt.Stringer interface.
type Count int

func (c Count) String() string {
    return strconv.Itoa(int(c))
}

// Declare a WriteLog() function which takes any object that satisfies
// the fmt.Stringer interface as a parameter.
func WriteLog(s fmt.Stringer) {
    log.Print(s.String())
}

func main() {
    // Initialize a Count object and pass it to WriteLog().
    book := Book{"Alice in Wonderland", "Lewis Carrol"}
    WriteLog(book)

    // Initialize a Count object and pass it to WriteLog().
    count := Count(3)
    WriteLog(count)
}
```
- The cool thing here is, we just need one `WriteLog` function that can leverage both `String()` method of different type. (flexibility)

## Empty interface
> The empty interface type essentially *describes no methods*. It has no rules. And because of that, it follows that any and every object satisfies the empty interface.
```go
func main() {
    person := make(map[string]interface{}, 0)

    person["name"] = "Alice"
    person["age"] = 21
    person["height"] = 167.64

    fmt.Printf("%+v", person)
}
```

## Type assertion when working with **empty interface**
- The previous example is very interesting and now our *map* has been empowered. But `21`, `167.64` is `interface` type, not `int` and `float64`.
- In order to manipulate them, we need to convert the type.
```go
func main() {
    person := make(map[string]interface{}, 0)

    person["name"] = "Alice"
    person["age"] = 21
    person["height"] = 167.64

    age, ok := person["age"].(int)
    if !ok {
        log.Fatal("could not assert value to int")
        return
    }
    person["age"] = age + 1

    log.Printf("%+v", person)
}
```

- What actually happen here?
```go
// this statement assert that "i" holds the type "T", and assign it back to "t"
t := i.(T)
// test whether "i" holds the type "T"
t, ok := i.(T)
```

## `Stringer` interface
*It's same as `__str__` from Python.*

## Gist for available standard lib interface
[Here](https://gist.github.com/asukakenji/ac8a05644a2e98f1d5ea8c299541fce9)