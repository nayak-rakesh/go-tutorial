## TYPES
GO is a statically typed language i.e every variable always have a particular type and it can not be changed.The different types available in go are 
1. Numbers 
    1. Integer
        1. `uint8`,`uint16`,`uint32`,`uint64`.These are called unsigned integers.These data types hold only positive numbers with zero.
        2. `int8`,`int16`,`int32`,`int64`.These are called signed integers.These data types hold both positive and negative numbers.
        3. `byte`,`rune`.`byte` is same as `uint8` and `rune` is same as `int32`.
        There are other types such as `uint`,`int`,`uintptr`.These depends on the machine architecture.When working with integer types `int` is generally preferred.
    2. Floats
        1. `float32`,`float64`.Both of these types can be used for storing floating point numbers.If we would use a larger size, we would get higher precision.
        2. `NaN` for not a number(0/0).`+∞` and `+∞`
        It's generally convenient to use `float64`
2. String
    Strings are immutable sequence of bytes.Text strings are UTF-8 encoded sequence of Unicode code points.
3. Boolean
    These are one bit integer used to represent either `true` or `false`.

## OPERATORS
Operators are different characters which are used to carry out different operations on operands.
1. Arithmetic Operators
    These are used to perform arithmetic operations.`+`(integers,floats,strings),`-`(integers,floats),`/`(integers,floats),`*`(integers,floats),`%`(integers)

2. Comparison Operators

| Operator      | Name               | 
| ------------- | ------------------ | 
| ==            | Equal to           | 
| !=            | Not equal to       | 
| <             | Less than          | 
| <=            | Less than or equal      | 
| >             | Greater  than          | 
| >=            | Greater than or equal   | 
* Boolean, integer, float and complex values are comparable
* Strings are ordered lexically byte wise
* Two pointers are equal if they points to the same variable or both are `nil`.
* Two struct values are equal if their corresponding non-blank fields are equal.
* Two array values are equal if their corresponding elements are equal.

3. Logical Operators

| Operator      | Name               | 
| ------------- | ------------------ | 
| &&            | Conditional AND    | 
| \|\|          | Conditional OR     | 
| !             | NOT                |

4. Pointers and Channels