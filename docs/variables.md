# Variables

## Declaring Variables

There are three types of variables in Sakura: variables, constants, and environment variables. environment variables can not be declared, and are set by either the interpreter or the executor. Both variables and constants can be declared by a script using prefixes. A variable is declared with a `$` prefix, and a constant is declared with a `%` prefix. Constants can not be modified, whereas variables can.

```ska
%myConst = "A constant"
$myVar = "A variable"
```

> Note that assignment operator evaluates to the value of the assignment. For example `$x = 50` evaluates to `50`.

## Using Variables

Both variables and constant are used without their respective prefixes. environment variables on the other hand are used with the `@` prefix. Adding a prefix will result in an error. Undefined variables will result in a value of `NULL`.

```ska
%myConst = "A constant"
$myVar = "A variable"

print(myConst) # Prints "A constant"
print(myVar) # Prints "A variable"
print(undefVar) # Prints "NULL"
```

## Re-assigning Variables

Variables (not constants or environment variables) can be reassigned using the assignment (`=`) operator. Attempting to reassign a constant or an environment variable will result in an error.

```ska
$number = 5
print(number) # Prints "5"
number = 10
print(number) # Prints "10"
```

Variables can also be assigned to other values, including operations performed on itself or function calls. They can not be assigned to the value of other assignment operators.

```ska
$myVar = 10
print(myVar) # Prints "10"

myVar = myVar + 10
print(myVar) # Prints "20"

myVar = list(10, 20, 30)
print(myVar) # Prints "<iterable>"
```

## Variable Scope

All variables have a certain scope to them, the scope of a variable is determined by where it is defined. The bounds of a scope is determined by braces `{}`. Variables can be redefined, even if they exist in their parent scope. This will be the variable used whenever referenced, until it goes out of scope. 

```ska
%x = 5

if x == 5 {
  %y = 7
  print(x + y) # Prints "12"

  $x = 10
  print(x + y) # Prints "17"

  x = 7
  print(x + y) # Prints "14"
}

print(y) # Prints "NULL"
print(x) # Prints "5"
```

## Provided Environment Variables

These are environment variables guaranteed by the interpreter. Refer to your executor's documentation for more environment variables that may be provided.