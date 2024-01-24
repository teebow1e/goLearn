# Concurrency

## **Goroutine**
> A *goroutine* is a lightweight thread of execution.
### Syntax
```go
package main

import (
	"fmt"
	"time"
)

func f(from string) {
	for i := 1; i < 4; i++ {
		fmt.Println(from, ":", i)
	}
}

func main() {
	f("directCall")
	go f("goroutine")
	go func(msg string) {
		fmt.Println(msg)
		time.Sleep(1 * time.Second)
		fmt.Println("sleep done")
	}("going")

	time.Sleep(time.Second)
	fmt.Println("done")
}
```


And we have the result:
```
// main blocking call
directCall : 1
directCall : 2
directCall : 3
// goroutine, everything happens simultaneously
going
goroutine : 1
goroutine : 2
goroutine : 3
sleep done
done
```

## Channels
> *Channels* are the pipes that connect concurrent goroutines. You can send values into channels from one goroutine and receive those values into another goroutine.

```go
package main

import "fmt"

func main() {
	ch := make(chan int)

	go func() { ch <- 42 }()
	//ch <- 42  // blocked: no corresponding receive
	b := <-ch // blocked: no corresponding send

	// Here, we have a deadlock because there is no corresponding send or receive.
	// The program will not proceed beyond this point.
	fmt.Println("This line will not be reached.", b)
}
```

## Unbuffered vs Buffered Channels
* Unbuffered Channel
- Has no capacity by default
- Can only send/receive one value at a time, after that it will block.
- They will only accept sends (chan <-) if there is a corresponding receive (<- chan) (this needs to be a Goroutine - because channels are pipe connecting the Goroutines.) ready to receive the sent value. (deadlock otherwise)

* Buffered Channel
- Has a specified capacity by default
- When a sender sends/receives a value on a buffered channel, it will not block immediately if the channel is not full/empty. It will only block when the buffer is full/empty.

## Channel Synchronization
### Case 1: Waiting for a single Goroutine to finish
- The idea should be creating a channel, when a goroutine finished execution, it will send a value to the channel, and on the main func(), we will wait for the notification from the Goroutine.

### Case 2: Multiple Goroutine


## Passing Channels as Function argument
* We can pass the following values:
- `chan`: bidirectional channel
- `chan<-`: send-only channel
- `<-chan`: receive-only channel

* Example:
```go
package main

import (
	"fmt"
	"time"
)

func sendToChan(channel chan<- string, data string) {
	fmt.Printf("Sending data %s\n", data)
	channel <- data
}

func receiveFromChan(channel <-chan string) {
	fmt.Println("Received data from channel", <-channel)
}

func main() {
	example := make(chan string, 1)
	go sendToChan(example, "hi")
	go receiveFromChan(example)
	time.Sleep(1)
}
```

## `Select`
> `select` is a powerful construct in Go that facilitates concurrent programming by allowing goroutines to communicate and synchronize efficiently. It is commonly used in **scenarios** where you need to wait for multiple channels and respond to the first one that becomes available.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	chan1 := make(chan string, 1)
	chan2 := make(chan string, 1)

	go func() {
		time.Sleep(1 * time.Second)
		chan1 <- "first msg"
	}()
	go func() {
		time.Sleep(1 * time.Second)
		chan2 <- "second msg"
	}()

	for i := 0; i < 2; i++ {
		select {
		case msg1 := <-chan1:
			fmt.Println("received data from chan1", msg1)
		case msg2 := <-chan2:
			fmt.Println("received data from chan2", msg2)
		default:
			// default case if msg1 and msg2 are unavailable
		}
	}

}
```

### Handling timeouts
We specify another case in the `select` statement: `<-time.After(1 * time.Second)` --> Prevent Goroutine leaks
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	chan1 := make(chan string, 1)
	go func() {
		time.Sleep(1 * time.Second)
		chan1 <- "hello world!"
	}()
	select {
	case msg1 := <-chan1:
		fmt.Println("received data from chan1:", msg1)
	// Different timeout second results in different output
	case <-time.After(2 * time.Second):
		fmt.Println("chan1 timeout")
	}
}
```

## Closing channels
```go
package main

import "fmt"

func main() {
	jobs := make(chan int)
	done := make(chan bool)

	go func() {
		for {
			job, chanStatus := <-jobs
			if chanStatus {
				fmt.Println("Received a job from Channel:", job, chanStatus)
			} else {
				fmt.Println("Job transfer done.")
				done <- true
				return
			}
		}
	}()

	for x := 1; x < 4; x++ {
		fmt.Println("send jobs:", x)
		jobs <- x
	}
	close(jobs)
	fmt.Println("send all jobs done")
	<-done

	_, chanStatusCheck := <-jobs
	fmt.Println("can receive more job:", chanStatusCheck)
}
```

**Some highlights in this code:**
- `<-chan` - data transferred through channel actually returns 2 values: the value itself, a boolean flag indicating whether the channel is open or closed.
- The boolean flag can be used to distinguish whether the channel is closed/open (in this case, the value has been successfully transfered).
- `close(chan)` - indicate the channel is closed
- `done := make(chan bool)` - the channel synchronization technique used to ensure the Goroutine will be executed and the main goroutine will wait for it.

## Using `range` to iterate through transferred data in a channel
```go
package main

import "fmt"

func main() {
	channel := make(chan string, 2)
	channel <- "hello"
	channel <- "world"
	close(channel)

	for element := range channel {
		fmt.Println(element)
	}
}
```

## Working with Times
### Timers - do something in the future
1. *How to setup a basic timer?*
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	timer := time.NewTimer(2 * time.Second)
	fmt.Println("we can still executing commands during timer.")
	<-timer.C
	fmt.Println("booooooooom")
}
```

2. *Stop a timer*
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	timer := time.NewTimer(2 * time.Second)
	go func() {
		<-timer.C
		fmt.Println("bomb exploded in a Goroutine :)")
	}()
	res := timer.Stop()
	if res {
		fmt.Println("bomb has been defused")
	}
}
```

### Tickers - do something repeatedly
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ticker := time.NewTicker(1 * time.Second) // repeat at the interval of 1 second
	done := make(chan bool, 1)

	go func() {
		for {
			select {
			case <-done:
				return
			case t := <-ticker.C:
				fmt.Println("tick at", t)
			}
		}
	}()
	time.Sleep(20 * time.Second)
	ticker.Stop()
	done <- true
	fmt.Println("ticker done")
}
```


## Worker Pools
> To be implemented.


## WaitGroups
> We use WaitGroups to wait for multiple Goroutines to finish.

> Note: if a WaitGroup is explicitly passed into functions, it should be done by pointer.

### Features
WaitGroups are implemented using the `sync` package, we have these 3 essential methods:
- `Add()` - add the number of Goroutines that need to be waited.
- `Done()` - called **by** the Goroutines when it finishes its work.
- `Wait()` - block the execution of the main Goroutine until all the Goroutines finish their job (`Done()`)

### Example
```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

func worker(id int, wg *sync.WaitGroup, job int) {
	defer wg.Done()
	fmt.Printf("Worker %v started job ID %v\n", id, job)
	time.Sleep(1 * time.Second)
	fmt.Printf("Worker %v finished job ID %v\n", id, job)
}

func main() {
	const numWorkers = 3
	var wg sync.WaitGroup

	for i := 0; i < 3; i++ {
		wg.Add(1)
		go worker(i, &wg, rand.Intn(5)) // notice I use pointer here
	}

	wg.Wait()
	fmt.Println("\n all job have finished.")
}
```

## Mutexes
> Mutex is used to protect shared resources from concurrent access by ensuring that only one goroutine can access the protected section of code at a time.
