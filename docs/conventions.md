# Conventions

Conventions are provided as a guideline for projects to improve readability and understand code. The documentation follows this convention, except in some demonstrative cases. Conventions can be modified by individuals or groups, consistency is essential. It's important, however, that conventions are not followed blindly, and the readability of code should be prioritized.

## Variables & Constants

Variable names should be short, descriptive, and use camelCase. Boolean variables should prefer to be positively named and start with something along the lines of "is", "has", or does, for instance `isDir` is prefered `isNotDir`. Boolean variables may also be named by a verb with an "s" at the end, for instance `exists`. Variables should be constants whenever possible. Iterable variables should be plural nouns.

In the case of for-loops, if the iterable variable is to be ignored, the iterable variable should be declared as `%_`. In the case of other loop counters, single-letter variables should be prefered, starting with `i`.

> When creating executor environment variables, executors should largely follow the same guidelines. However, in the case of meta-variables (variables used more for debugging rather than programming), they should start with two underscores, For example, see [@__executor](/variables#__executor-gt-string).

## Function Definitions

Functions should be defined at the bottom of the script. Function names should be short, descriptive, and use camelCase. Functions that return boolean values should start with something along the lines of "is", "has", or "does". Unlike boolean variable names, it is discouraged to name then as a verb with an "s" at the end. Functions should be limited to do one thing. Function argument names should follow variable naming conventions.

## Commands & Paths

Commands should be put on their own line. Commands which take paths as arguments should have their paths inlined. However, when the path is used in multiple places, and if it's reasonable, paths should be split into their own variables. The `@root` should never be prefixed to the path.

## Indentation

Indentation of two spaces should be used for code within braces, or for parentheses that stretch over multiple lines.

## Semicolons

Multiple statements on a single line, seperated by a semicolon is discouraged. Since return statements terminate program execution, putting a semicolon after a `return` to prevent it from returning unwanted expressions is discouraged. If this is necessary it's recommended to simply delete the statements after it.

## Braces

Braces should be placed on the same line as a loop statement or function signature, with a space before it.

## Comments

Comments should be used whenever possible to explain clarity of code.

## Guard Statements

Guard statements should be used whenever possible in order to reduce code nesting, instead of if-else-if and nested if-statements. For instance, in the following codeblock, `myFuncGood` should be prefered to `myFuncBad`. 

```ska
func myFuncGood(%x, %y) {
  if (x == NULL | y == NULL) {
    return
  } 
  print(x, y)
}

func myFuncBad(%x, %y) {
  if (x == NULL | y == NULL){
    return
  } else {
    print(x, y)
  }
}
```