 [Python tutor](https://pythontutor.com/)
 A code which asks the user to type in a limit. The program then calculates the sum of consecutive numbers (1 + 2 + 3 + ...) until the sum is at least equal to the limit set by the user.  In addition to the result it should also print out the calculation performed:

```python
limit = int(input("Limit:"))
num = 1
i = 1
x = "1"
while i < limit:
    num += 1
    i += num
    x += f" + {num}"
print(f"The consecutive sum: {x} = {i}")
```
`Output: Limit:`<font color="#ff0000"> **18**</font> `The consecutive sum: 1 + 2 + 3 + 4 + 5 + 6 = 21`

