### Channels
---
* Channels are the medium through which goroutines communicate with each other. Channels are responsible for sharing data between goroutines.
* Data can be of any type including pointers. Each channel has a type associated with it which represents the type of data that can be shared using the channel
```go
package main

import "fmt"

func main() {
	var ch chan int // ch is variable of type channel with zero value (nil)
	fmt.Println(ch)
	ch = make(chan int)
	fmt.Printf("Type of ch: %T\n", ch)

	strChannel := make(chan string) // short declaration
	fmt.Printf("Type of strChannel: %T\n", strChannel)    
}
```
**output:**
```
<nil>
Type of ch: chan int
Type of strChannel: chan string
```
### Sending and receiving data through channels
---
* To write data in to a channel by a goroutine, we have to make sure that there is another goroutine which reads data from the channel.
* In the same manner to read data from a channel by a goroutine, we also have to make sure that there is another goroutine that writes to the channel
* The read from channel expression blocks the execution of the program until some goroutine doesn't write to the channel and vice versa. So we can say that read and write operations are blocking by default.
```go
package main

import "fmt"

func routine(finish chan string) {
	fmt.Println("inside go routine")
	finish <- "goroutine finished" // writes to a channel
}

func main() {
	finish := make(chan string)
	go routine(finish)
	readChannel := <-finish // reads from a channel
	fmt.Printf("Data received from channel: %s\n", readChannel)
	fmt.Println("main routine finished")
}
```
**output:**
```
inside go routine
Data received from channel: goroutine finished
main routine finished
```
* In the above code `goroutine()` takes a channel as parameter and writes to the channel after the end of the function.
* When `routine()` is called it will return automatically and control will return to main routine but as we haven't written to the finish channel the reading from the channel(`readChannel := <-finish`) will block the execution of the main routine.
* Again control will go inside `routine()` and data will be written to the channel which will enable the `readChannel := <-finish` line in the main routine to read from the channel.
* After reading from the channel rest of the main routine will finish executing. We can see that we don't need to use `sync` or `time` packages to stop the execution of main routine. 
```go
func routine(msg string, ch chan string) {
	for i := 0; i <= 2; i++ {
		ch <- msg
	}
	close(ch)
}

func main() {
	ch := make(chan string)
	go routine("hello world", ch)

	for {
		msg, open := <-ch
		if !open {
			break
		}
		fmt.Println(msg)
	}
}
```
**output:**
```
hello world
hello world
hello world
```
* In the above example in the main routine we are trying to read from channel in an infinite loop. In the `routine()` we are sending values through routine
and after we finish sending we close the channel. `open` become false once we closed the channel and we break out of the loop.
* If we don't close the channel and try to read from the channel after `routine()` is finished executing we can go into deadlock. If we try to read from a
closed channel we will get error. So we check if a channel is closed or not and break out of the loop
```go
func routine(msg string, ch chan string) {
	for i := 0; i <= 2; i++ {
		ch <- msg
	}
	close(ch)
}

func main() {
	ch := make(chan string)
	go routine("hello world", ch)

	for msg := range ch {
		fmt.Println(msg)
	}
}
```
* In the above example we iterate over a channel and printing the values as long as channel is closed. This is the better way than the previous example.
```go
func main() {
	ch := make(chan string)
	ch <- "hello world"

	msg := <-ch
	fmt.Println(msg)
}
```
**output:**`fatal error: all goroutines are asleep - deadlock!`  
* In the above example we try to send a value but the line which try to read from the channel doesn't get to execute so the program goes in a deadlock. We can solve the issue by sending from another goroutine or by creating a buffered channel.

### Buffered channels
---
* The channels discussed before is called unbuffered channels i.e doesn't have capacity.
* Buffered channel has capacity i.e we can specify how much data we can write to a channel. We can fill the buffered channel without the corresponding receiver and it won't block the code.
* If the channel holds maximum data, the writing will be blocked until a goroutine reads from the channel.
```go
func main() {
	ch := make(chan string, 2)
	ch <- "hello world"

	msg := <-ch
	fmt.Println(msg)
}
```
**output:**```hello world```  
```go
func main() {
	ch := make(chan string, 2)
	ch <- "hello world"
	ch <- "hello world again"

	msg := <-ch
	fmt.Println(msg)
	msg = <-ch
	fmt.Println(msg)
}
```
**output:**
```
hello world
hello world again
```
* In the above examples we can see, we can fill up to the capacity of the channel and it won't block the code. We have to read from channel by range if we want
to read all the values. 
* If we try to write to channel beyond it's capacity without reading from it, the program will go to deadlock. If we want to send more values, we first have to read form the channel which will empty the channel to make space for more values to send to the channel. See from the examples below
```go
func main() {
	ch := make(chan string, 2)
	ch <- "hello world"
	ch <- "hello world again"
	ch <- "deadlock"

	msg := <-ch
	fmt.Println(msg)
	msg = <-ch
	fmt.Println(msg)
}
```
**output:**`fatal error: all goroutines are asleep - deadlock!`  
```go
func main() {
	ch := make(chan string, 2)
	ch <- "hello world"
	ch <- "hello world again"
	<-ch // reading from ch which makes space to send value
	ch <- "no deadlock"

	msg := <-ch
	fmt.Println(msg)
	msg = <-ch
	fmt.Println(msg)
}
```
**output:**
```
hello world again
no deadlock
```
```go
func main() {
	ch1 := make(chan string)
	ch2 := make(chan string)

	go func() {
		for i := 0; i <= 5; i++ {
			ch1 <- "500 millisecond"
			time.Sleep(time.Millisecond * 500)
		}
	}()
	go func() {
		for i := 0; i <= 5; i++ {
			ch2 <- "2 second"
			time.Sleep(time.Second * 2)
		}
	}()

	for i := 0; i <= 5; i++ {
		fmt.Println(<-ch1)
		fmt.Println(<-ch2)
	}
}
```
* If we run the above code we will see that reading from ch1 will be delayed even though it is ready to send value much sooner because program has to wail for 2 second to read from ch2.  
```go
func main() {
	ch1 := make(chan string)
	ch2 := make(chan string)

	go func() {
		for i := 0; i <= 5; i++ {
			ch1 <- "500 millisecond"
			time.Sleep(time.Millisecond * 500)
		}
	}()
	go func() {
		for i := 0; i <= 5; i++ {
			ch2 <- "2 second"
			time.Sleep(time.Second * 2)
		}
	}()

	for {
		select {
		case msg1 := <-ch1:
			fmt.Println(msg1)
		case msg2 := <-ch2:
			fmt.Println(msg2)
		}
	}
}
```
* The above code would print message from ch1 more frequently as it is available early than ch2.

### Concurrency Patterns
---
* Concurrency pattern is a kind of design pattern that deals with efficient use of multi-threading and concurrency.
#### Wait for task pattern
```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	ch := make(chan string)

	go func() {
		task := <-ch // goroutine blocked until message is received
		fmt.Println("Task received: ", task)
	}()

	time.Sleep(time.Duration(rand.Intn(500)) * time.Millisecond)
	ch <- "Reading" // sending message would unblock the previous goroutine that we have started
	fmt.Println("Task has been sent")
	time.Sleep(time.Second)
}
```
**output**
```
Task received:  Reading
Task has been sent
```
* A goroutine has started and waiting for the task to be finished. We can say it's blocked until it received a task.
* It's important to remember that receive happens before send.
#### Wait for result
```go
func main() {
	ch := make(chan string)

	go func() {
		time.Sleep(time.Duration(rand.Intn(500)) * time.Millisecond) // goroutine performing a task
		ch <- "Homework"                                             // goroutine sending result
		fmt.Println("Result has been sent")
	}()

	result := <-ch // waiting for result(blocked)
	fmt.Println("Result received: ", result)
	time.Sleep(time.Second)
}
```
* We are waiting for result from a goroutine so `main` goroutine is blocked until the result is sent.
* It's important to remember that receive happens before send.
#### Wait for finished
```go
func main() {
	ch := make(chan struct{})

	go func() {
		time.Sleep(time.Duration(rand.Intn(500)) * time.Millisecond)
		close(ch)
		fmt.Println("Signal has been sent")
	}()

	_, isData := <-ch
	fmt.Println("Signal received. Is data present:", isData)
	time.Sleep(time.Second)
}
```
**output**
```
Signal has been sent
Signal received. Is data present: false
```
* Using empty struct means we should understand that we would signal without any data. `isData` is a boolean flag to denote if there is data received or not.
* This can be done using waitGroup too.
#### Pooling pattern
```go
func main() {
	ch := make(chan string)
	const gr = 2

	for i := 1; i <= gr; i++ {
		go func(grNum int) {
			for task := range ch {
				fmt.Printf("routine %d : received task %s\n", grNum, task)
			}
			fmt.Printf("goroutine %d received shutdown signal\n", grNum)
		}(i)
	}

	const work = 10
	for w := 1; w <= work; w++ {
		ch <- fmt.Sprintf("%d", w)
	}
	close(ch)
	fmt.Println("Shutdown signal sent")
	time.Sleep(time.Second)
}
```
**output**
```
routine 2 : received task 1
routine 2 : received task 3
routine 1 : received task 2
routine 1 : received task 5
routine 2 : received task 4
routine 1 : received task 6
routine 1 : received task 8
routine 1 : received task 9
routine 1 : received task 10
Shutdown signal sent
goroutine 1 received shutdown signal
routine 2 : received task 7
goroutine 2 received shutdown signal
```
* We created a pool of goroutines in this case two goroutines. Each goroutine receives from the channel via `range`. We can see that those two goroutines are blocking because these two goroutines has to wait until some other goroutine sends to the channel.
* The main goroutine sends  to the channel and then the go scheduler randomly picks up a goroutine and receives from the channel via `range` and start processing the data
* Main goroutine waits to send a task into the channel until the channel is empty and the other two go routines wait for a task to be sent to the channel.
* Once the main goroutine finished sending it won't wait for the other goroutines to finish executing. So to see all the task processing we are waiting for a second in the main goroutine after all the tasks are sent to the channel.
#### Fan out pattern
```go
func main() {
	gr := 5
	ch := make(chan string, gr) // each goroutine can send to the channel as soon as they are finished

	for i := 1; i <= gr; i++ {
		go func(grNum int) {
			time.Sleep(time.Duration(rand.Intn(200)) * time.Millisecond) // emulating a process
			ch <- fmt.Sprintf("result from goroutine %d", grNum)
		}(i)
	}

	for gr > 0 {
		result := <-ch
		fmt.Println("received", result)
		gr--
	}
}
```
**output**
```
received result from goroutine 3
received result from goroutine 1
received result from goroutine 5
received result from goroutine 4
received result from goroutine 2
```
* We created a buffered channel. It has the same capacity as the number of goroutine i.e `10` in above example. The reason is, as soon as a goroutine finished processing then can send to the channel without wait as we have slot for each goroutine hence we can minimize latency in send to the channel.
* The main goroutine is blocked because it has to wait until all the data is retrieved from the channel.
```go
func main() {
	gr := 10 // number of total goroutine
	ch := make(chan string, gr) 

	const active = 3 // number of active goroutine
	sem := make(chan bool, active)

	for i := 1; i <= gr; i++ {
		go func(grNum int) {
			sem <- true
			{
				time.Sleep(time.Duration(rand.Intn(200)) * time.Millisecond)
				ch <- fmt.Sprintf("result from goroutine %d", grNum)
			}
			<-sem
		}(i)
	}

	for gr > 0 {
		result := <-ch
		fmt.Println("received", result)
		gr--
	}
}
```
* In the above example we have total `10` goroutines but we are limiting the number of active goroutine to `3`. The `sem` channel has maximum capacity of `3`.
* When each of the `10` goroutines try to send in to `sem` channel only three among them would be able to do so and will go on execute the next lines of code. 
* Other `7` would be blocked and wait for the active goroutines to pull the data out of `sem` channel making space for remaining goroutines to send to the `sem` channel and execute.
#### Drop Pattern
```go
func main() {
	cap := 3 // number of total goroutine
	ch := make(chan string, cap)

	go func() {
		for task := range ch {
			time.Sleep(time.Duration(rand.Intn(100)) * time.Millisecond) // emulate processing
			fmt.Println("Processed", task)
		}
	}()

	const work = 10
	for w := 1; w <= work; w++ {
		select {
		case ch <- fmt.Sprintf("task %d", w):
			fmt.Printf("task %d has been sent\n", w)
		default:
			fmt.Printf("Task %d has dropped due to unavailability of space\n", w)
		}
	}

	close(ch)
	fmt.Println("Close signal has been sent")
	time.Sleep(time.Second) // required to stop main goroutine finish executing and see the result from other goroutine
}
```
**output**
```
task 1 has been sent
task 2 has been sent
task 3 has been sent
task 4 has been sent
Task 5 has dropped due to unavailability of space
Task 6 has dropped due to unavailability of space
Task 7 has dropped due to unavailability of space
Task 8 has dropped due to unavailability of space
Task 9 has dropped due to unavailability of space
Task 10 has dropped due to unavailability of space
Close signal has been sent
Processed task 1
Processed task 2
Processed task 3
Processed task 4
```
* In the above code, we are dropping the tasks which we can't process anymore. We can think of it as, we are getting a lot of request but our hardware is unable to process all the request so we drop the request which can't process. 
#### Worker pools pattern
* In this pattern we have queue of jobs to be done and number of concurrent workers pulling jobs out of the queue.
```go
func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	go worker(jobs, results)
	go worker(jobs, results)
	go worker(jobs, results)

	for i := 0; i < 20; i++ {
		jobs <- i
	}
	close(jobs)

	for j := 0; j < 20; j++ {
		fmt.Println(<-results)
	}
}

func worker(jobs <-chan int, results chan<- int) {
	for n := range jobs {
		results <- fib(n)
	}
}

func fib(n int) int {
	arr := make([]int, n+1)
	for i := 0; i <= n; i++ {
		arr[i] = -1
	}

	if arr[n] != -1 {
		return arr[n]
	}

	if n == 0 {
		arr[n] = 0
		return arr[n]
	} else if n == 1 {
		arr[n] = 1
		return arr[n]
	} else {
		arr[n] = fib(n-1) + fib(n-2)
		return arr[n]
	}
}
```
* In the above example we are calculating nth fibonacci number concurrently. `worker()` receives `jobs` channel which will be used only to read the jobs from and `results` channel which will be used only to send the data to.
* As we are using buffered channel, it won't block the execution.