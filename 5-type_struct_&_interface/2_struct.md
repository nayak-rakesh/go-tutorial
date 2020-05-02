### Struct
* Struct is a custom data type defined by user. Struct is generally used to group similar properties and function to a single unit
* Lets say we want to define a struct of type person with properties name, age and address. So we can group all the properties in a single
struct.
```go
package main

import "fmt"

type person struct { // defining a struct
	name    string
	age     int
	address string
}

func main() {
	person1 := person{  // created person1 of struct type person 
		name:    "John",
		age:     25,
		address: "London",
	}

    person2 := person{"Sam", 26, "New York"} // created person2 of struct type person 
    
    var person3 person // initialized with zero values

	fmt.Println("Name of the person one is ", person1.name) // accessing the property
	fmt.Println("Name of the person two is ", person2.name)
}
```
**output:**
```
Name of the person one is  John
Name of the person two is  Sam
```
* If we create a variable of type struct without assigning values or we assign values of only few fields, the fields without values would initialized
to their respective zero values 
* We also can define and create struct in a single statement
```go
func main() {
	person := struct {
		name    string
		age     int
		address string
	}{
		name:    "John",
		age:     25,
		address: "London",
	}

	fmt.Println(person.name)
}
```
**output:**`John`  
