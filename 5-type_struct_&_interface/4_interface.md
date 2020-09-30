### Interface
* Interface is also an user defined type which contains set of methods and method signature.
* When a custom type implements all those methods defined by an interface, we can safely say that the type implements the interface.
```go
type Person interface {
	getName() string
	getAge() int
	getAddress() string
}

type person struct {
	name    string
	age     int
	address string
}

func (p person) getName() string {
	return p.name
}

func (p *person) getAge() int {
	return p.age
}

func (p person) getAddress() string {
	return p.address
}

func main() {
	var p Person
	person1 := person{
		name:    "John",
		age:     25,
		address: "London",
	}
	p = &person1 // as person struct implements all methods of interface we can assign person struct to interface

	fmt.Println("Name of the person one is ", i.getName())
	fmt.Println("Age of the person one is ", i.getAge())
	fmt.Println("Address of the person one is ", i.getAddress())
}
```
**output:**
```
Name of the person one is  John
Age of the person one is  25
Address of the person one is  London
```
* In the above example, we have assigned pointer of struct to the interface variable rather than value of the struct. This is because one of the
methods defined in interface is implemented by the struct with a pointer receiver.
* So if any of the methods of an interface implemented by a type receives a pointer, the interface variable must accept a pointer.
#### Structure Of Interface
* Interface represented as type, value pair in go.
```go
type Person interface {
	getName() string
	getAge() int
	getAddress() string
}

type person struct {
	name    string
	age     int
	address string
}

func (p person) getName() string {
	return p.name
}

func (p person) getAge() int {
	return p.age
}

func (p person) getAddress() string {
	return p.address
}

func main() {
	var p Person
	person1 := person{
		name:    "John",
		age:     25,
		address: "London",
	}
	p = person1
	fmt.Printf("Type: %T\n", p)
	fmt.Printf("Value: %v\n", p)
}
```
**output:**
```
Type: main.person
Value: {John 25 London}
```
#### Empty Interface
* If an interface has zero methods, called an empty interface. It represented by `interface{}`. Because empty interface doesn't have any methods
any type can implement the empty interface.
```go
func emptyInterface(i interface{}) {
	fmt.Printf("Type: %T\n", i)
	fmt.Printf("Value: %v\n", i)
}

func main() {
	str := "hello world"
	emptyInterface(str)

	num := 20
	emptyInterface(num)

	customStruct := struct {
		name string
	}{
		name: "John",
	}
	emptyInterface(customStruct)
}
```
**output:**
```
Type: string
Value: hello world
Type: int
Value: 20
Type: struct { name string }
Value: {John}
```