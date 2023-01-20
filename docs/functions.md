# Functions

Functions are a crucial part of any programming and are also available in Sakura. Sakura provides some built in functions, and your executor may provide additional functions. It is also possible for you to define your own functions. See [built in functions](#built-in-functions) for reference on all built in functions.

## Calling Functions

You can call a function using it's named followed by parentheses, with any parameters within the parentheses. Having less parameters than provided will be passed as `NULL`, and having more parameters than the functions accepts will be ignored. For instance, the [`type()`](#typevalue) function, which requires one parameter only throws an exception if no parameter are provided. Adding more parameters does not change the output of the function. It is convention that all function definitions follow this method of parameter processing.

```ska
type() # Errors
type(5) # Returns "number"
type(5, "this parameter will be ignored") # Returns "number"
```

Some functions (like the [`print()`](#printvalues) function) take an infinite amount of arguments, which are passed by just passing more and more values.

```ska
print("Hello", "there", "person") # Prints "Hello there person".
```

## Defining Functions

### Basic Functions

Functions can be defined in Sakura only in the globabl scope. Functions that are defined are available anywhere. It is convention to define functions at the bottom of the file. This means that you can not define functions within other functions or statements. Functions are defined using the `func` keyword. Following the keyword is the function name, and parentheses with any parameters inside, using the `$` for variables, and `%` as constants. For instance:

```ska
add(10, 15) # Returns 25
printHello() # Prints "Hello!"

func add(%left, %right) {
  return left + right
}

func printHello() {
  print("Hello!")
}
```

Defining an argument as a variable or a constant determines whether or not the value can be reassigned. For instance:

```ska
func myFunc($val1, %val2) {
  val1 = 5 # This is allowed
  val2 = 10 # This is not allowed
}
```

Functions without arguments may omit the parentheses if desired, however they still require the parentheses when called. For instance: 

```ska
printHello() # Prints "Hello!"

func printHello {
    print("Hello!")
}
```

### Returning Values

The return statement can be used in a function to get a return value. Recall [the `add()` function from above](#basic-functions) which uses the statement. Calling the function evaluates to the value provided after `return`. Function execution terminates immediately after hitting a `return`. For instance: 

```ska
%sum = add(10, 15) 
print(sum) # Prints "25"

print(add(10, 15)) # Prints "25"
```

Omitting a value from the statement, or not providing one at all, implicitly returns `NULL`. For instance:

```ska
print(myNullFunc1()) # Prints "NULL"
print(myNullFunc2()) # Prints "NULL"

func myNullFunc1 {
  %myVar = 5 + 3
}

func myNullFunc2 {
  %myVar = 5 + 3
  return 
}
```

The return statement may consider the next line as it's value in certain cases. This behaviour can be stopped with a semi-colon.

```ska
myFunc() # Returns "string"

func myFunc {
  return # Inserting a semi-colon here would cause this to return NULL
  type("some value")
}
```

> The return statement also can be used in the top level to return value a value from the script. Using it within a function elimates this behaviour, and only exits the function. If you would like to return a value immediately from a function, see the [`exit()`](#exitcode-value) function instead.
 
### Default Parameters

Sakura supports default parameters, these are defined by simply putting an equals sign and then a default value after a parameter in the function definition. Default values are only provided when the user does not pass a value. A user may pass null as the value to a parameter, in this case the value of the parameter will be `NULL`. For instance: 

```ska
myFunc() # Prints "NULL John"
myFunc("Hello") # Prints "Hello John"
myFunc("Goodbye", "Jimmy") # Prints "Goodbye Jimmy"
myFunc("Greetings", NULL) # Prints "Greetings NULL"

func myFunc(%val, %def = "John"){
  print(val, def)
}
```

Functions can also have other expressions as the default value. These are re-evaluated in the global scope every time the function is called. For instance:

```ska
$i = 10
myFunc()

func myFunc(%n = i) {
  if i <= 0 {
    return
  }

  i = n - 1;
  myFunc()
}

print(i) # Prints "0"
```

### Rest Parameters

Sakura also supports custom-defined rest-parameters. These can be defined by prepending three periods before an parameter. The value of the parameter will be an iterable consisting of all of the values. The rest parameter must be the last parameter in a parameter list. 

```ska
print(total(5, 1, 2, 2)) # Prints "10"

func total($initial, ...%values){
  for %val in values {
    initial = initial + val
  }

  return initial
}
```

## Built in Functions

There are several built in functions that are provided by the interpreter, however these may be overriden by your executor. This documentation is for the default behaviour of these functions.

### print(\[...values\])

Prints the string representation of every parameter passed to it to standard output, joined by spaces. If no arguments are provided, it simply prints a blank line. Each print statement creates a new line. Returns `NULL`.

```ska
print("Str1", 2, x, "another string") # Prints "Str1 2 <function> another string"

func x {}
```

> It's recommended for executors to overwrite this function and change its output, while keeping the same string concatenation functionality.

### list(\[...values\])

Returns all of the values as an iterable that can be looped over.

```ska
%myList = list("A string", 7)

for %val in myList {
  print(val) # Prints "A string" and then "7" on a new line
}
```

### range(\<stop\>), range(\<start\>, \<stop\>, \[step = 1\])

If one parameter is provided, return an iterable from 0 to `stop` (exclusive) in increments of 1. If more than two parameters are provided, loop from `start` (inclusive) to `stop` (exclusive) in increments of `step`.

```ska
$sum = 0
for %i in range(10) {
    sum = sum + i
}
print(sum) # Prints "45"
```

### type(\<value\>)

Returns the type of the value as a string.

```ska
type("foobar") # Returns "string"
type(10) # Returns "number"
type(FALSE) # Returns "boolean"
type(NULL) # Returns "null"
type(print) # Returns "function"
type(range(10)) # Returns "iterable"
type(@root) # Returns "path"
```

### exit(\[code\], \[value\])

Immediately terminate script execution. If no parameters are provided, the script terminates execution with a return value of `NULL`, and an exit code of zero. 

If only `code` is provided, and `code` is zero, then the script terminates execution with a return value of `NULL`. If only `code` is provided, and `code` is not zero, the script terminates in error with the reason "unknown".

If both `code` and `value` are provided, and `code` is zero, then the script terminates with the return value `value`. Otherwise if both `code` and `value` are provided, and `code` is not zero, then the script terminates in error with the reason as the string representation of `value`.

```ska
exit() # Terminate with exit code 0 and a return value of NULL
exit(1) # Terminate with exit code 1 with the reason "unknown"
exit(0, 15) # Terminate with exit code 0 and a return value of the number 15
exit(30, "Error :(") # Terminate with exit code 30 with the reason "Error :("
```

> If `code` is provided, it will be converted to a byte value internally, so executing with a decimal value, or a value outside of the range -127 to 127 (inclusive) may produce undefined behavior.

## Creating Executor Functions

Executors can create functions and the script will consider them as built in functions. These functions can not be overridden by the user. It's recommended that you follow [conventions](/conventions). For specifics creating and overriding functions, see your interpreters documentation.
