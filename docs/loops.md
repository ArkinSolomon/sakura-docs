# Loops

## While Loops

While loops consist of the `while` keyword, followed by a boolean expression, followed by code in braces (`{}`), which is called the "body" of the loop. The code runs, re-evaluating the boolean expression after every loop. If the boolean expression evaluates to false, the body will not execute again. If the expression evaluates to false initially, then the boolean expression will never run. For instance:

```ska
%endNum = 10
$myNum = 0

while myNum < endNum {
  myNum = myNum + 1
}
print(myNum) # Prints 10
```

> If you want to loop a certain amount of times, consider using a for-loop along with the [`range()` function](/functions#rangeltstopgt-rangeltstartgt-ltstopgt-step-1-gt-iterable).

## For Loops

For loops consist of the `for` keyword, followed by either a constant or variable declaration (a symbol prefixed by `$` or `%`, similar to how arguments are declared in [functions](`/functions`)), followed by the `in` keyword, followed by an iterable value, followed by code in braces (`{}`), which is called the "body" of the loop. The code runs, iterating over the iterable until the iterable completes, storing its value in the declared variable. Iterables can be provided as a result of a function or a [rest parameters](/functions#Rest-Parameters). If the iterable completes immediately, then the body of the loop never executes.

```ska
for %val in list(1, 2, 3) {
  print(val)# Prints "1", then "2", then "3" all on seperate lines
}
```

> One set of optional parentheses is allowed around the iterable statement for customization purposes. For instance: <code><!--We need two code blocks here so that we get the background--><code class="language-ska">for x in range(10) {}</code></code> can also be <code><code class="language-ska">for (x in range(10)) {}</code></code>

### Looping Over Values

For loops can also loop over strings and directories instead of iterables. If it loops over a string, it loops over each charachter in the string (including spaces and newlines). If it loops over a directory it will loop over each file (or directory) in the directory, storing the path as the value of the declared variable. For instance, here is a simple program that will print out the path for every directory:

```ska
%myDir = PATH /some/directory
printFiles(myDir)

func printFiles(%dir) {
  for %file in dir {
    %isDir = ISDIR $(file)
    
    # If it is a directory print its children too
    if isDir {
      printFiles(file)
    } else {
      print(file)
    }
  }
}
```

> Looping over directories using a for loop does not allow inline path declarations like commands do.

## Breaking or Continuing

Loops can be broken early or continued. Breaking a loop using the `break` keyword will immediately terminate the execution of the loop body and the loop. Continuing a loop will immediately terminate execution of the loop body, and then move to the next iteration of the loop (if the loop can iterate again). 

```ska
for %i in range(1, 9) {
  if i == 3 {
    continue
  } else if i == 5 {
    break
  }
  print(i) # Prints "1" then "2" then "4" on separate lines
}
```

Using `return` in a loop will return the function or script early, terminating execution of the loop body and the function body. For instance:

```ska
print(myNum()) # Prints "5"

func myNum {
  for %i in range(1, 10) {
    if i == 5 {
      return i
    }
  }
}
```