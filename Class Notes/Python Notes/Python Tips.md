### Printing with f-strings
So called _f-strings_ are another way of formatting printouts in Python. The syntax can initially look a bit confusing, but in the end f-strings are often the simplest way of formatting text. With f-strings the previous example would look like this:

```python
result = 10 * 25
print(f"The result is {result}")
```

A single f-string can contain multiple variables. For example this code

```python
name = "Mark"
age = 37
city = "Palo Alto"
print(f"Hi {name}, you are {age} years old. You live in {city}.")
```

Each `print` command usually prints out a line of its own, complete with a change of line at the end. However, if the `print` command is given an additional argument `end = ""`, it will not print a line change. For example:

```python
print("Hi ", end="")
print("there!")
```

Sample output: Hi there!

 It can be printed an empty line by adding an empty `print` command, or by adding the newline character `\n` into your string.

In the following table you can see the most common arithmetic operators in Python, with examples:

|Operator|Purpose|Example|Result|
|---|---|---|---|
|`+`|Addition|`2 + 4`|`6`|
|`-`|Subtraction|`10 - 2.5`|`7.5`|
|`*`|Multiplication|`-2 * 123`|`-246`|
|`/`|Division (floating point result)|`9 / 2`|`4.5`|
|`//`|Division (integer result)|`9 // 2`|`4`|
|`%`|Modulo|`9 % 2`|`1`|
|`**`|Exponentiation|`2 ** 3`|`8`|
The order of operations is familiar from mathematics: first calculate the exponents, then multiplication and division, and finally addition and subtraction. The order can be changed with parentheses.
For example this bit of code

```python
print(2 + 3 * 3)
print((2 + 3) * 3)
```
Sample output: 11 and 15

Division `/` is an exception as its result is a floating point number, even if the operands are integers. For example `1 / 5` will result in the floating point number `0.2`.

The string produced by the function `input` must then be converted to a numeric data type in the program code. A string can be converted into an integer with the function `int`. The following program asks the user for their year of birth and stores it in the variable `input_str`. The program then creates another variable `year`, which contains the year converted into an integer. After this the calculation `2021-year` is possible, using the user-supplied value.

```python
input_str = input("Which year were you born? ")
year = int(input_str)
print(f"Your age at the end of the year 2021: {2021 - year}" )
```

Usually you do not need to create two separate variables (like `input_str` and `year` above) to read a number value from the user. Instead, reading the input with the `input` function and converting it with the `int` function can be achieved in one go:

```python
year = int(input("Which year were you born? "))
print(f"Your age at the end of the year 2021: {2021 - year}" )
```

The function `int` always rounds down, and not according to the rounding rules in mathematics.

Similarly, a string can be converted into a floating point number with the function `float`. This programs asks the user for their height and weight, and uses these to calculate their BMI:

```python
height = float(input("What is your height? "))
weight = float(input("What is your weight? "))

height = height / 100
bmi = weight / height ** 2

print(f"The BMI is {bmi}")
```

Same way:

```python
sum = sum + number
number = number * 2
```
or,
```python
sum += number
number *= 2
```

### Comparison operators

Very typically conditions consist of comparing two values. Here is a table with the most common comparison operators used in Python:

| Operator | Purpose                  | Example  |
| -------- | ------------------------ | -------- |
| ==       | Equal to                 | `a == b` |
| `!=`     | Not equal to             | `a != b` |
| `>`      | Greater than             | `a > b`  |
| `>=`     | Greater than or equal to | `a >= b` |
| `<`      | Less than                | `a < b`  |
| `<=`     | Less than or equal to    | `a <= b` |
The condition `x >= a and x <= b` is a very common way of checking whether the number x falls within the range of a to b. Python also allows a simplified notation for combining conditions: `a <= x <= b` achieves the same result as the longer version using and. This shorter notation might be more familiar from mathematics, but it is not very widely used in Python programming, possibly because very few other programming languages have a similar shorthand.
### Indentation

Python recognises that a block of code is part of a conditional statement if each line of code in the block is _indented_ the same. That is, there should be a bit of whitespace at the beginning of every line of code within the code block. Each line should have the same amount of whitespace. The syntax of Python specifies, among other things, that the first line of an `if` statement should end in a colon character, and the block of the statement should be indented:

With nested conditional statements it is crucial to get the indentations right. Indentations determine which branches are linked together. For example, an `if` branch and an `else` branch with the same amount of whitespace are determined to be branches of the same conditional statement.

For example:

```python
password = input("Please type in a password: ")

if password == "kittycat":
    print("You knew the password!")
    print("You must be either the intended user...")
    print("...or quite an accomplished hacker.")

print("The program has finished its execution. Thanks and bye!")
```

Notice the colon character following the `if` header. If not colon, the code gives error.

You can use the function `type` to find out the data type of any expression. An example of its use:
```python
print(type("Anna"))
print(type(100))
```

