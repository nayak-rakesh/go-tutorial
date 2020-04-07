### Slice 
Slice is a data structure like array but with flexibility.We can increase the size of a slice by adding elements in it unlike arrays which has fixed length.Slices can be made from an exisitng array, by declaration statement or by `make()` function.Under the hood slices are just references to arrays.
When we crate a slice form a parent array, the slice holds a reference to the parent array so any change in the slice would reflect in the parent array.
```go
package main

import (
	"fmt"
)

func main() {
	intArray := [5]int{1, 2, 3, 4, 5}
	var intSlice []int = intArray[1:4] // from intArray
    // intSlice := intArray[1:4]  (short hand synax for creating slice)
    fmt.Println(intSlice)

    strSlice := []string{"a", "b", "c"} // by declaration statement
    
    // make functon takes type, length and capacity as parameter
    floatSlice := make([]float64, 3, 4) 
}
``` 
#### Some Points To Note
* If a slice gets created from an existing array by `array[start:end]` syntax, the slice created would point(reference) to the element at start index and will hold the elements including at start index upto but excluding the element at end index.The length of the slice would be the elements present in the slice and the capacity would be the total number of elements the parent array have starting from the `start` index
```go
intArray := [5]int{1, 2, 3, 4, 5}
var intSlice []int = intArray[1:4] // intSlice points to element at index 1 i.e 2
// capacity of intSlice would be 4 because total number of elements of intArray starting from 
// start index i.e 1 is 4 [2, 3, 4, 5]
fmt.Printf("Slice: %v, Length: %d, Capacity: %d\n", intSlice, len(intSlice), cap(intSlice))
```
**output:**`Slice: [2 3 4], Length: 3, Capacity: 4`  
* As slice holds reference to the underlaying array any changes made in the slice would reflect in the parent array.
```
[1, 2, 3, 4, 5]
   [2, 3, 4]     (slice points to the start index in this case index 1)
   <------->     length = 3
   <---------->  capacity = 4
```
```go
intArray := [5]int{1, 2, 3, 4, 5}
var intSlice []int = intArray[1:4] 
intSlice[1] = 11
// as slice points to the element at the index 1 of intArray so element at index 1 of slice would
// be equivalent to element at index 2 of the array
fmt.Printf("Slice: %v, Array: %v\n", intSlice, intArray)
```
**output:**`Slice: [2 11 4], Array: [1 2 11 4 5]`  
* As slices are expandable we can add elements at the end of a slice using `append()` function.In the above example `intSlice` has capacity 4 with length 3.If we append a single value to the intSlice, the original array will also receive the change.
```go
intArray := [5]int{1, 2, 3, 4, 5}
var intSlice []int = intArray[1:4]
intSlice = append(intSlice, 6)
fmt.Printf("Slice: %v, Array: %v\n", intSlice, intArray)
```
**output:**`Slice: [2 3 4 6], Array: [1 2 3 4 6]`  
* We saw that the capacity is of a slice is determined from the underlaying array.When we try to change the slice by assigning elements or appending elements within the initial limit, the changes get reflected in the parent array.If we try to append more elements beyond the capacity of the slice, now the slice needs a bigger array to point at so that the capacity can be increased.What happens is a whole new array gets created with double the length, all the elements get copied to the new array and the slice now points to the new array with double capacity.
```go
intArray := [5]int{1, 2, 3, 4, 5}
fmt.Printf("intArray before appending: %v\n", intArray)
var intSlice []int = intArray[1:4]
fmt.Printf("intSlice length before: %d intSlice capacity before: %d\n", len(intSlice), cap(intSlice))
intSlice = append(intSlice, 6)
fmt.Printf("intSlice: %v, intArray: %v\n", intSlice, intArray)
fmt.Printf("intSlice length after: %d intSlice capacity after: %d\n", len(intSlice), cap(intSlice))
fmt.Println("----------------------------------------------------------------")
strArray := []string{"a", "b", "c", "d"}
strSlice := strArray[1:3]
fmt.Printf("strArray before: %v\n", strArray)
fmt.Printf("strSlice: %v, Length before: %d, Capacity before: %d\n", strSlice, len(strSlice), cap(strSlice))
strSlice = append(strSlice, "e", "f")
fmt.Printf("strArray after appending: %v\n", strArray)
fmt.Printf("Slice after: %v, Length after: %d, Capacity after: %d\n", strSlice, len(strSlice), cap(strSlice))
```
**output:**
```
intArray before appending: [1 2 3 4 5]
intSlice length before: 3 intSlice capacity before: 4
intSlice: [2 3 4 6], intArray: [1 2 3 4 6]
intSlice length after: 4 intSlice capacity after: 4
----------------------------------------------------------------
strArray before: [a b c d]
strSlice: [b c], Length before: 2, Capacity before: 3
strArray after appending: [a b c d]
Slice after: [b c e f], Length after: 4, Capacity after: 6
```
* In the first case number of elements that gets appended is in the range of the slice so the slice points to the original array and changes made in slice can be seen in the array.In the second case number of elements that gets appended is out of the range of the slice so a new array gets crated and the slice points to the new array so the parent array remains unchanged after appnding to slice.
* If we try to create a slice with declaration statement insteade of creating from an array, under the hood a array gets created and a slice is returned referencing to the array.When we try to append elements to the slice the similar event happens as discussed before.
* We can create slice with `make(type, len, cap)`.Total number of elements would be equal to length and would be initialized to their zero values.By default the capacity would be equal to length if capacity is not given in the `make()` function.
* We can have multidiminsional slices like we saw in array.Syntax is the same without specifying the length.
* If there is a large array and we need only a part of it in our program, we use slice but it still points to the original array under the hood so the array can not be garbage collected.We can make a copy of slice and dereference the bigger array so it can be garbage collected.
```go
intArray := [8]int{1, 2, 3, 4, 5, 6, 7, 8}
intSlice := intArray[1:5]
sliceCopy := make([]int, len(intSlice))
fmt.Printf("intSlice: %v sliceCopy: %v\n", intSlice, sliceCopy)
copy(sliceCopy, intSlice)
fmt.Println("sliceCopy after copying:", sliceCopy)
```
**output**
```
intSlice: [2 3 4 5] sliceCopy: [0 0 0 0]
sliceCopy after copying: [2 3 4 5]
```
