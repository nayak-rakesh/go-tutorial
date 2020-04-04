### String Foramtting
String formatting can be done be `Printf()` provided by `fmt` package.Go provides many verbs for string formatting.Let's see some examples
```go
package main

import "fmt"

func main() {
    fmt.Printf("%d\n", 65) // prints decimal value(65)
    fmt.Printf("%b\n", 65) // prints binary value(1000001)
    fmt.Printf("%x\n", 65) // prints hexadecimal value(41)
    fmt.Printf("%c\n", 65) // prints character value(A)

    fmt.Printf("%f\n", 123.56) // 123.560000

    fmt.Printf("%e\n", 12345678.0) // 1.234568e+07
    fmt.Printf("%E\n", 12345678.0) // 1.234568E+07

}
```
