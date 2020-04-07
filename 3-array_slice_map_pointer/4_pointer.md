### Pointer
Pointers are also a kind of variable which stores memory address of another variable.When we create a variable and assign a value to it, compiler stores the value in a memory location and label it with the variable name.Using pointers, we can get the memory address of the variable and store it in another variable.
```go
package main

import (
	"fmt"
)

func main() {
	num := 10
	var numPtr *int = &num
	fmt.Printf("Type of numPtr is %T\n", numPtr)
    fmt.Println("Address of num is", numPtr)
    
    // new() function is also used to create pointers
	str := "a"
	strPtr := new(string) // points to an empty string
	fmt.Printf("Type of strPtr: %T, address in strPtr: %v value of strPtr: %s\n", strPtr, strPtr, *strPtr)
	strPtr = &str
	fmt.Println("After assigning strPtr")
	fmt.Printf("Type of strPtr: %T, address in strPtr: %v value of strPtr: %s\n", strPtr, strPtr, *strPtr)

}
```
**output:**
```
Type of numPtr is *int
Address of num is 0xc00001a0e0
Type of strPtr: *string, address in strPtr: 0xc000010210 value of strPtr: 
After assigning strPtr
Type of strPtr: *string, address in strPtr: 0xc000010200 value of strPtr: a
```
* In go `&` operator is used to get the memory location of a variable(`&varName`).`*` with type is used to declare a pointer variable(`*int` -> pointer of a variable of type int).In the above example `numPtr` is a pointer of type `*int` means `numPtr` stores memory address of a variable of `int` type, in this case `num`,
* To get the value stored in the memory location we also use `*` operator.This process is called dereferencing of pointer.(`fmt.Println(*numPtr)` prints the value which is stored in the memory location the pointer points to)
* We can declare a pointer without assigning a value to it.In such case the pointer gets it's zero value i.e `nil` 
* We can also change the value stored in the memory location through pointers(`*numPtr=11` will change the value of the variable `num`)
```go
num := 10
var numPtr *int
fmt.Println("Initial value of pointer numPtr is", numPtr)
numPtr = &num
fmt.Println("Assigned value of pointer numPtr is", numPtr)
fmt.Printf("Value stored in location %v is %d\n", numPtr, *numPtr) // retrieves value from memory location
*numPtr = 11 // changing the value in the memory location will change the value of variable num
fmt.Println("New value of variable num is,", num)
```
**output:**
```
Initial value of pointer numPtr is <nil>
Assigned value of pointer numPtr is 0xc00010c000
Value stored in location 0xc00010c000 is 10
New value of variable num is, 11
```
* Pointer arithmetic is not possible in go.Pointers can be passed as argument to a function which we will see in future.
* As pointers refer to the memory location of a variable, any change using pointer would change the value of the original variable.