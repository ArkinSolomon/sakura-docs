# Branching

## Boolean Expressions 

To understand branching, boolean expressions must be understood. Boolean expressions can be as simple as using the `TRUE` or `FALSE` boolean literals. They can also be symbols or comparison between symbols. Boolean expressions can also be combined or modified with [boolean operators](/operators#boolean-operators).

## If Statements

If statements consist of an `if` keyword, followed by a boolean expression (the condition of this block), followed by braces (`{}`). The code within in braces will execute if and only if the boolean expression passed evaluates to true. Parentheses are not necessary around the boolean expression. For example:

```ska
%x = 5
%y = 5
%z = 10

if x == y {
  print("this will run")
}

if x + y == z {
  print("this will run")
}

if x > z {
  print("this will not run")
}
```

### Else-if Blocks

Putting an `else if` after the previous block, followed by another boolean expression (the condition of this block), followed by braces, the code within the braces will execute if the boolean expression of all previous else-if blocks, and the if block,  does not execute, and if the boolean expression provided evaluates to true. Any amount of else-if blocks can be chained together. The first else-if block to have a truthy boolean expression will run. For example:

```ska
%val = 10

if val < 5 {
  print("this will not run")
} else if val < 50 {
  print("this will run")
} else if val < 500 {
  print("this will not run)
}
```

### Else Blocks

Putting an `else` after the last else-if, or if block, followed by braces, the code within the braces will execute if the conditions of all previous blocks are false. An else block must be the last block of an if statement. Only one else block is permitted per if-statement. For example:

```ska
%val = 50
if val < 20 {
  print("this will not run")
} else if val < 40 {
  print("this will not run")
} else {
  print("this will run")
}
```