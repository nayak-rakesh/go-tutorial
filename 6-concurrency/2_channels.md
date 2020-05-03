### Channels
* Channels are the medium through which goroutines communicate with each other. Channels are responsible for sharing data between goroutines.
* Data can be of any type including pointers. Each channel has a type associated with it which represents the type of data that can be shared using the channel
```go
package main

import "fmt"

func main() {
	var ch chan int // cd is variable of type channel with zero value (nil)
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
#### Sending and receiving data through channels
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