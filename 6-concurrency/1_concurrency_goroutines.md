### Concurrency
* Concurrency is running processes in alternate intervals. Lets say we have two processes are running concurrently. Go executes the first 
process and then pauses the process and start executing the second process. Again it pauses second process and executes the first process 
from where it has left before.
* The above process is called context switching. Go switches the execution context between processes.
* A parallel execution is running process in multiple cores of a processor without pausing any process.
* Go support concurrency by using goroutines and channels.
### Goroutine
* Goroutines are functions or methods that runs concurrently with other functions.
* Goroutines are light and doesn't take much memory and has very less stack size in memory. That's the reason it is preferred over threads
* Goroutines are created by using the keyword `go` in front of a function or method 
* The important thing to remember is `main()` has it's own routine. Let's call it main routine. And other routines run inside this main routine
* To get results from other goroutines, we have to make sure that the main routine should run until all the goroutines finished execution. 
* Goroutines returns immediately when called. Then the control goes to the next line for execution. Let's say we have only one goroutine and one print statement.
* When control starts the single goroutine it returns immediately and goes to next line(the print statement).Then the print statement executes and as there is no lines after that, the main goroutine exits and the single goroutine doesn't get any chance to produce result.
```go
package main

import "fmt"

func routine() {
	fmt.Println("inside go routine")
}

func main() {
	go routine()
	fmt.Println("main goroutine ends")
}
```
**output:**`main goroutine ends`  
* We need to find a way to make the main goroutine wait until the other gorutines finished executing. We can use `time` package to make the main goroutine sleep for specified time or `sync` package to make the main goroutine wait until all the launched goroutines are finished executing.
```go
package main

import (
	"fmt"
	"time"
)

func routine() {
	fmt.Println("inside go routine")
}

func main() {
	go routine() // returns immediately
	time.Sleep(1 * time.Second) // control comes here but sleeps for one second so routine() gets called
	fmt.Println("main goroutine ends") // executes after one second 
}
```
**output:**
```
inside go routine
main goroutine ends
```
```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func routine(index string) {
	defer wg.Done() // calles when function finishes execution
	fmt.Printf("inside go routine %s\n", index)
}

func main() {
	wg.Add(2) // count of number of goroutines
	go routine("one")
	go routine("two")
	wg.Wait() // waits for the program to finish execution of all goroutines

	fmt.Println("main goroutine ends")
}
```
**output:**
```
inside go routine two
inside go routine one
main goroutine ends
```
* In the above example `Add()` creates a counter from the number passed as argument, which represents number of goroutines.
* `Done()` is called at the end of each goroutine and it decrements the counter started by `Add()`
* `Wait()` blocks the execution of the program until the counter is zero i.e all the goroutines finished executing.
