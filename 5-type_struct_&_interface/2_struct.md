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
	person3.name = "Jenny" // assigning value to a field

	person4 := new(person) // with new keyword
	person4.name = "John"

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
* We also can define and create struct in a single statement. These structs are called anonymous struct.
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
* We also can define pointers to a struct. It's pretty much the same as creating pointers to other data types.
```go
type person struct {
	name    string
	age     int
	address string
}

func main() {
	person1 := &person{
		name:    "John",
		age:     25,
		address: "London",
	}

	fmt.Println("Name of the person one is ", (*person1).name) // explicit dereference
	fmt.Println("Age of the person one is ", person1.age) // implicit dereference
}
```
**output:**
```
Name of the person one is  John
Age of the person one is  25
```
* If fields of a struct doesn't have a name and only have type, those field is called anonymous field. By default the name of the filed is set to
the name of the type.
```go
type person struct {
	string
	int
}

func main() {
	person1 := person{
		string: "John",
		int:    25,
	}

	fmt.Println("Name of the person one is ", person1.string)
	fmt.Println("Name of the person two is ", person1.int)
}
```
* We can also have nested structs in our program.
```go
type person struct {
	name         string
	age          int
	address      address
	familyMember []familyMember
}

type address struct {
	city    string
	country string
}

type familyMember struct {
	name     string
	relation string
}

func main() {
	person1 := person{
		name: "John",
		age:  25,
		address: address{
			city:    "London",
			country: "Englind",
		},
		familyMember: []familyMember{
			familyMember{
				name:     "Robert",
				relation: "father",
			},
			familyMember{
				name:     "Sam",
				relation: "brother",
			},
		},
	}

	fmt.Println("Name of the person one is ", person1.name)
	fmt.Println("City of the person one is ", person1.address.city)
	fmt.Println("Name of father is ", person1.familyMember[0].name)
	fmt.Println("Name of brother is ", person1.familyMember[1].name)
}
```
**output:**
```
Name of the person one is  John
City of the person one is  London
Name of father is  Robert
Name of brother is  Sam
```