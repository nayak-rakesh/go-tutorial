### User Input From Console
Sometimes we want our program to be dynamic.In such case we take user input from the console instead of hard coding it in the program.
```go
package main

import "fmt"

func main() {
	fmt.Print("Enter First String: ") //Print function is used to display output in same line
	var firstStr string
	fmt.Scanln(&firstStr) // Take input from user
	fmt.Print("Enter Second String: ")
	var secondStr string
	fmt.Scanln(&secondStr)
	fmt.Print(firstStr + " " + secondStr + "\n") // Addition of two string

	fmt.Print("Enter First Number: ") 
	var first float64
	fmt.Scanln(&first) 
	fmt.Print("Enter Second Number: ")
	var second float64
	fmt.Scanln(&second)
	fmt.Printf("%f\n", first+second) // Addition of two floats
}
```
**output:**
```
Enter First String: John
Enter Second String: Smith
John Smith
Enter First Number: 78.388
Enter Second Number: 388.83
467.218000
```
If you want to read the whole line with spaces, you can use `bufio` and `os` packages.In case if you want an integer or float as input you have to explicitly convert it using `ParseInt()` or `ParseFloat()` functions.
```go
package main

import (
	"bufio"
	"fmt"
    "os"
    "strconv"
)

func main() {
	reader := bufio.NewReader(os.Stdin)
	fmt.Print("Enter name: ")
	text, _ := reader.ReadString('\n') // stops reading when new line is encountered
	text = text[:len(text)-1] // removing the new line character from the end
    fmt.Println(text)
    
    fmt.Print("Enter num: ")
	text, _ = reader.ReadString('\n') // stops reading when new line is encountered
	text = text[:len(text)-1]          // removing the new line character from the end
	float, _ := strconv.ParseFloat(text, 64)
	fmt.Println(float + 1.11)
}
```
**output:**
```
Enter name: John Smith
John Smith
Enter num: 2.22
3.33
```
You can use `for` loop to read infinitely from the console or for the specific number of times.