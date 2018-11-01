# Notes

## Printf tricks
---
### Printing the variable type
To determine a type you can use Printf with %T
'''go
year := 2018
fmt.Printf("Type %T for %v\n", year, year)
'''

---
### Printing bibary or hex
%b will print the bit format and %x will return hex format
```go
var green uint8 = 3
fmt.Printf("%08b\n", green)
green++
fmt.Printf("%08b\n", green)
```

---

### Strings
Using backtick (') insdead of double quotes (") indicates a raw string literal and will not evaluate any escape characters (e.g. \n). This also allows you to span multipue lines of code for one string.

```go
fmt.Println(`
    peace be upon you
    upon you be peace`)
```

#### Converting string to number:
```go
countdown, err := strconv.Atoi("10")
if err != nil {
    // oh no, something went wrong
}
fmt.Println(countdown)
```

#### Calculating string length
len(string) - returns the length of string in bytes (not characters so this works for ASCII but not Unicode). Faster but only works for ASCII
utf8.RuneCountInString(string) - will get the actual chacter length of the string (remember utf8 stores 8, 16 or 32 bit characters)

#### Gerneral summary of string
* Escape sequences like \n are ignored in raw string literals (`).
* Strings are immutable. Individual characters can be accessed but not altered.
* Strings use a variable length encoding called UTF-8, where each character consumes 1–4 bytes.
* A byte is an alias for the uint8 type, and rune is an alias for the int32 type.
* The range keyword can decode a UTF-8 encoded string into runes.

---

### Numbers

### Convert a number to a string:
integer to ASCII (note since since number line up in standard ASCII for utf-8. No special encoding is needed for numbers)

```go
strconv.Itoa(number)
```

#### Checking overflow when converting number types
```go
if bh < math.MinInt16 || bh > math.MaxInt16 {
    // handle out of range value
}
```

Sprintf() is just like Printf but it returns a string instead of printing to the screen. You can use this to turn numbers to strings as follows:

```go
countdown := 9
str := fmt.Sprintf("Launch in T minus %v seconds.", countdown)
fmt.Println(str)

```

---


# Got'chas
---
### Floating point accuracy
Floating points suck for comparison because. Can store any number but is not alway accurate. 

```go
piggyBank := 0.1
piggyBank += 0.2
fmt.Println(piggyBank == 0.3)
```

This is false. piggyBank actually equals .300000000000001

So instead we should do:

```go
fmt.Println(math.Abs(piggyBank-0.3) < 0.0001)
```

---
### Dealing with numbers
Use go's ```big``` package

Beware that using these over the primitive types will cause performance and memory hits.

Use in the following cases:
* big.Int is for big integers, when 18 quintillion isn’t enough.
* big.Float is for arbitrary-precision floating-point numbers.
* big.Rat is for fractions like ⅓.

You can also put very big number (over 18 quintillion) into a constant and the underlining type will be big.Int. You can use it in operations but cannot print or asign it unelss it fits into an in.

### Booleans have no numeric counterpart and are stored as ```true``` and ```false```