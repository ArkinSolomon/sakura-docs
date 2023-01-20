# Variables

## Declaring Variables

There are three types of variables in Sakura: variables, constants, and environment variables. environment variables can not be declared, and are set by either the interpreter or the executor. Both variables and constants can be declared by a script using prefixes. A variable is declared with a `$` prefix, and a constant is declared with a `%` prefix. Constants can not be modified, whereas variables can. Variable names can only contain alpha-numeric charachters, and underscores, and must start with a letter or underscore. Variable names are case-sensitive.

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
print(type(@root)) # Prints "path"
print(undefVar) # Prints "NULL"
```

## Datatypes

There are 7 different datatypes in Sakura. You can perform type checking using the [`type()`](/functions#typeltvaluegt-gt-string) function.

### String

Strings are text values defined using double quotes. 

```ska
%myStr = "String"
```

### Number

Numbers are integers or decimals defined by literals in arabic numerals using a . as a decimal, or scientific notation.

```ska
%myInt = 50
%myDec = 3.14
%myBigNum = 9e9
%myNegativeNum = -10
```

> Internally all numbers are represented as floating points, so they may become slightly innacurate after performing arithmetic.

### Boolean

Booleans are true or false values defined by either boolean literals (`TRUE` or `FALSE`) or boolean expressions. 

```ska
%myTruthyVal = TRUE
%myFalsyVal = !myTruthyVal # Evaluates to false
%equalityCheck = "foo" == "bar" # Evaluates to false
```

### Null

`NULL` is not only a type but a value as well. Any value with this type is automatically equivalent to `NULL`. Null values can be defined using the `NULL` keyword.

```ska
%myVal = NULL
```

### Function

A function name holds the value of this type. Function typed-variables can only be defined by [defining a function](/functions#Defining-Functions). Built-in functions also are of this type.

```ska
type(myFunc) # Returns "function"

func myFunc {}
```

### Iterable

Iterables are values which can be iterated over by a for-loop. There is no concept of arrays or lists in Sakura. When a for-loop iterates over an iterable, it starts at the beginning of the iterable and iterates until the end. Breaking a loop early does not retain the iterable's state. [Rest parameters](/functions#Rest-Parameters) are of type iterable. Iterables can also be created using the [`range()`](/functions#rangeltstopgt-rangeltstartgt-ltstopgt-step-1-gt-iterable) or [`list()`](/functions#listvalues-gt-iterable) functions.

```ska
%myRange = range(10)
%myList = list(10, 20, 30)

func myFunc(...%rest) {
  type(rest) # Returns "iterable"
}
```

### Path

Paths are types which point to a potentially non-existent file. They are frequently used within [commands](/commands), and are created using the `PATH` command. See the [PATH command](/commands#PATH) for more information.

```ska
%myPath = PATH /some/directory
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

Since Sakura is dynamically typed, variables can change the types of their values. For instance: 

```ska
$myVar = "A string!"
print(type(myVar)) # Prints "string"
myVar = 10
print(type(myVar)) # Prints "number"
```

## Variable Scope

All variables have a certain scope to them, the scope of a variable is determined by where it is defined. The bounds of a scope is determined by braces `{}`. Variables can be redefined, even if they exist in their parent scope. This will be the variable used whenever referenced, until it goes out of scope. For instance:

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

If you want to declare a variable to be re-assigned within a scope, and be available outside of the scope, simply set its value to `NULL`. For instance:

```ska
$var = NULL
%const = 50

if const > 10 {
  var = "Const is greater than 10"
} else {
  var = "Const is less than or equal to 10"
}

print(var) # Prints "Const is greater than 10"
```

## Environment Variables

These are environment variables guaranteed by the interpreter. Refer to your executor's documentation for more environment variables that may be provided.

### @isMacOS -> *boolean*

True if the script is running on a machine running MacOS, otherwise false.

### @isWindows -> *boolean*

True if the script is running on a machine running Windows, otherwise false.

### @isLinux -> *boolean*

True if the script is running on a machine running Linux, otherwise false.

### @isOtherOS -> *boolean*

True if the script is running on a machine that is not running MacOS, Windows, or Linux, otherwise false.

### @root -> *path*

The path which points to the root. [More information on the root](/commands#The-Root) can be found on the [commands reference](/commands).

### @__executor -> *string*

The identifier for the executor, "unknown" if not specified. 

### @__interpreter -> *string*

The identifier for the interpreter. 

### @__lang_version -> *string*

The version of the language.

### @__interpreter_version -> *string*

The version of the interpreter.