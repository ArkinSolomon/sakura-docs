# Operators

## Precedence

Operators are used to perform different tasks. Every operator has a different precedence. If two operators have the same precedence, they are evaluated from left to right. The default order of the precedence for every operator is as follows:

- `!` `+` `-` -- NOT, POSITIVE, NEGATIVE (evaluated first)
- `*` `/` -- MULTIPLICATION, DIVISION 
- `+` `-` -- ADDITION, SUBTRACTION
- `<` `<=` `>` `>=` -- LT, LTE, GT, GTE
- `==` `!=` -- EQUALITY, INEQUALITY
- `&` -- AND
- `|` -- OR
- `=` -- Assignment (evaluated last)

> Operator precedence can be modified using parentheses. For instance `5 + 6 * 5` evaluates to `35`, but `(5 + 6) * 5` evaluates to `55`.

## The Assignment Operator 

The assignment operator, or the `=` operator, is used to assign and re-assign values to variables, or to establish [default parameters](/functions#default-parameters) in function signatures. 

> When iterables are assigned, a copy of the iterable is returned, therefore comparing two iterables may evaluate to false.

## Arithmetic Operators

### `+` (ADDITION/POSITIVE) Operator

The `+` character has multiple uses. It can prefix an expression that evaluates to a number to denote positivity (same as multiplying the result of the expression with 1). If it is placed between two expressions that evaluate to numerical values, it will add them together. If it is placed between two expressions, where at least one evaluates to a string, it will get the string representation of the other value and concatenate the two values together.

```ska
%myNum = 5 + 3 # Evaluates to 8
%myOtherNum = +10 # Evaluates to 10
%myStr = "foo" + "bar" # Evaluates to "foobar"
%myOtherStr = 5 + " pickles" # Evaluates to "5 pickles"
```

> Note that although the positive prefix operator is provided, it performs no modification to its operand, and therefore omitting it has no effect.

### `-` (SUBTRACTION/NEGATIVE) Operator

The `-` character has two uses. If it is used as a prefix to a number, it will invert the sign of it (same as multiplying the result of the expression with -1). If it is placed between two expressions that evaluate to numerical values, it will evaluate the difference of the values.

```ska
%myNum = 10 - 3 # Evaluates to 7
%myOtherNum = -10 # Evaluates to -10
```

### `*` (MULTIPLICATION) Operator

The `*` character can be placed between two numerical values in order to multiply them together. Or it can be placed between a string and a number (the string must be on the left) in order to repeat the string. 

```ska
%myNum = 10 * 5 # Evaluates to 50
%myStr = "hello " * 2 # Evaluates to "hello hello "
```

### `/` (DIVISION) Operator

The `/` character can be placed between two numerical values (as long as it the context does not attempt it to be parsed as a path) to perform division. 

```ska
%myNum = 1 / 2 # Evaluates to 0.5
```

> Note that the slash character may also act as a separation between path parts depending on the context.

## Boolean Operators

### `!` (NOT) Operator

This operator can be prefixed to a boolean expression in order to invert it. So if the boolean expression evaluates to true, it will be inverted to false, and vice versa.

```ska
%myBool = TRUE
%myOtherBool = !myBool # Evaluates to false
```

### `&` (AND) Operator

This operator can be placed between two boolean expressions in order to perform a logical AND, meaning that the overall boolean expression evaluates to true if the left and right values of the operator evaluates to true. 

```ska
%myBool = TRUE & TRUE # Evaluates to true
%myOtherBool = TRUE & FALSE # Evaluates to false
```

### `|` (OR) Operator

This operator can be placed between two boolean expressions in order to perform a logical OR, meaning that the overall boolean expression evaluates to true if the left value or the right value of the operator evaluates to true. 

```ska
%myBool = TRUE | FALSE # Evaluates to true
%myOtherBool = FALSE | FALSE # Evaluates to false
```

> Note that boolean ANDs and ORs are single `&` or `|`, unlike many other programming languages.

### `==` (EQUALITY) Operator

When placed between two values, it will evaluate to true if the left hand of the operator is equivalent to the right hand side. Equivalency can be determined differently depending on the datatypes being compared. If the datatypes are not the same, they are automatically considered to be not equal. If both are `NULL`, then they are equal. For numbers, if the two operands have a difference of less than 1e-12, they are equal. For strings, if they both have the same (case-sensitive) value, they are the same. For paths, they are equivalent if they both point to the same file or directory. Booleans are equal if both operands have the same boolean value. Functions and iterables are equivalent if they point to the same item. 

```ska
%bool1 = undefVar == NULL # Evaluates to true
%bool2 = 5 == 5 # Evaluates to true
%bool3 = "hello" == "HELLO" # Evaluates to false
```

### `!=` (INEQUALITY) Operator

The opposite of the `==` (EQUALITY) operator.

```ska
%x = 5
%y = 4
%myEq = x == y # Evaluates to false
%myNEq = x != y # Evaluates to true
```

### `<` (LT) Operator

This operator can only compare two numerical values, and evaluates to true if the left operand is less than the right operand.

```ska
%myBool = 5 < 10 # Evaluates to true
%myOtherBool = 5 < 5 # Evaluates to false
```

### `<=` (LTE) Operator

This operator can only compare two numerical values, and evaluates to true if the left operand is less than or equal to the right operand.

```ska
%myBool = 5 <= 10 # Evaluates to true
%myOtherBool = 5 <= 5 # Evaluates to true
```

### `>` (GT) Operator

This operator can only compare two numerical values, and evaluates to true if the left operand is greater than the right operand.

```ska
%myBool = 10 > 5 # Evaluates to true
%myOtherBool = 5 > 5 # Evaluates to false
```

### `>=` (GTE) Operator

This operator can only compare two numerical values, and evaluates to true if the left operand is greater than or equal to the right operand.

```ska
%myBool = 10 >= 5 # Evaluates to true
%myOtherBool = 5 >= 5 # Evaluates to true
```