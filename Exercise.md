# Exercise

## Exercise: Maps
>> Implement `WordCount`. It should return a map of the counts of each “word” in the string `s`. The `wc.Test` function runs a test suite against the provided function and prints success or failure.

```go
package main

import (
	"strings"

	"golang.org/x/tour/wc"
)

func WordCount(s string) map[string]int {
	result := make(map[string]int)
	splited := strings.Fields(s)
	for _, val := range splited {
		_, ok := result[val]
		if !ok {
			result[val] = 1
		} else {
			result[val] += 1
		}
	}
	return result

}

func main() {
	wc.Test(WordCount)
}
```

## Exercise: Stringers

Make the `IPAddr` type implement `fmt.Stringer` to print the address as a dotted quad.


For instance, `IPAddr{1, 2, 3, 4}` should print as `"1.2.3.4"`.

```go
func (ip IPAddr) String() string {
	return fmt.Sprintf("%v.%v.%v.%v", ip[0], ip[1], ip[2], ip[3])
}
```