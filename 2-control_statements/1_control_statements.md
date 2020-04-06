### Control Statements
When we run a piece of code, go compiler starts from the top of the code file and start exicuting statements one by one sequentially.Control statements are used to control the flow of the code exicution.We can exicute or skip code blocks depending on the logic provided.We can also exicute a block of code for specified number of times in loop.
#### If statement
If statement takes an expression which evaluates to be `true` or `false`.If the the expression evalutes to `true` then code block inside of if statement gets exicuted if the expression evalutes to `false` then code block inside of if statement gets skiped.We can use `else`, `if else` along with the `if` statement.
```go
package main

import "fmt"

func main() {
    isTrue := true
    x := 10
    name := "John"
    // if statemet
    if isTrue {
        fmt.Println("true")
    }
    // if with else statement
    if x == 10 {
        fmt.Println("x is equal to 10")
    } else {
        fmt.Println("x is not equal to 10")
    }
    // if with else if and else
    if name == "John" {
        fmt.Println("Name is John")
    } else if name == "Sam" {
        fmt.Println("Name is Sam")   
    } else {
        fmt.Println("Name is nither John nor Sam") 
    }
    // if statement with value initialization
    /* we have declared num in the if statement and this variable can be accessible only in the block and it's branches where it's initialized
    */
    if num := 5; num > 5 {
        fmt.Println("num is greater than", num)
    } else if num < 5 {
        fmt.Println("num is less than", num)   
    } else {
        fmt.Println("num is equal to", num) 
    }
}
```
#### For Statement
`for` statement used to exicute a code block for specified amount of times.The basic structure of for statement is 
```go
for i := 0; i <= 5; i++ {
    fmt.Println(i) // prints 0, 1, 2, 3, 4, 5
}
```
We have initialized value of `i` to `0` in the first iteration then we have the conditional statement i.e `i<=5` which evaluates to be `true` or `false`.If it evaluates to be `true` we exicute the code block else we skip it.The third statement is to change the current value of `i` so we won't go into an infinite loop.In the second iteration onwards we evalute the conditional statement `i<=5` and after that `i++` statement.`i:=0` only gets evaluated in the first iteration.The above program can also be written as following
```go
i := 0
for i <= 5 {
    fmt.Println(i) // prints 0, 1, 2, 3, 4, 5
    i++ // (i += 1)
}
```
While increamenting the value of `i`, we don't have to increament in order of 1.We can increament in order of 2 like `i += 2`.We can also decrease the value of `i` like `i--` or `i -= 2`.There is no `while` statement in go.
#### Switch Statement
`switch` statement can be used in place of `if-else` statement.`switch` takes an expression and matchecs with the cases.The type of the expressing should match with the type of the cases.All the cases should be unique i.e should not contain duplicate value.Sometimes `switch` used without an expression and beheaves like `if-else` block. 
```go
i := 10
switch i {
    case 1:
        fmt.Println("i is equal to 1")
    case 2, 3, 4, 5:
        fmt.Println("i is equal to 2, 3 or 4")
    case 6, 7, 8, 9:
        fmt.Println("i is equal to 5, 6, 7, 8, 9")
    default:
        fmt.Println("i is greater than 9")
}
```
**output:**
`i is greater than 9`

```go
i := 4
// switch statement without an expression
switch {
    case i == 1:
        fmt.Println("i is equal to 1")
    case i == 2 || i == 3 || i == 4 :
        fmt.Println("i is equal to 2, 3 or 4")
    case i == 5 || i == 6 || i == 7 || i == 8 || i == 9:
        fmt.Println("i is equal to 5, 6, 7, 8, 9")
    default:
        fmt.Println("i is greater than 9")
}
```
**output:**`i is equal to 2, 3 or 4`  
#### Continue and Break Statement
`continue` statement is used to skip itarations based on conditions and `break` statement is used to  break out of the loop.
```go
for i := 0; i <= 10; i++ {
    if i == 5 {
        continue
    } else if i == 8 {
        break
    }
    fmt.Println(i)
}
```
***output:**
```
0
1
2
3
4
6
7
```