# Formal Grammar

The following represents Sakura's formal grammar, written in modified Extended Backus-Naur form.

```ebnf
# Formal Grammar for Sakura 
# Language version 0.1.0-beta.2
# 
# Licensed under Apache 2.0 
# Sakura Language (c) 2023 Arkin Solomon
# https://sakura-docs.arkinsolomon.net/
#
# NEWLINE: Refers to the newline character (typically represented by '\n')
# WHITESPACE: Refers to a space ' ' character or a tab character (typically represented by '\t')
#
# *: 0 or more of the preceeding token
# +: 1 or more of the preceeding token
# .: Any UTF-8 character
#
# Whitespace is prohibited except where explicitly stated
# A backslash followed by a double-quote refers to a double-quote character 

program ::= gap* statement gap* terminator*
          | gap* statement gap* terminator* program

bracestatement ::= "{" gap* [bracebody gap*] "}"

bracebody ::= gap* statement gap* terminator*
            | gap* statement gap* terminator* bracebody

statement ::= assignment
            | ifstatement
            | whileloop
            | forloop
            | funcdef
            | funccall
            | command
            | childablecmd
            | returnstatement
            | "break"
            | "continue"

returnstatement ::= "return" gap* expression

ifstatement ::= ifblock gap* elifblock* [gap* elseblock]

ifblock ::= "if" gap+ expression gap* bracestatement 

elifblock ::= "else" gap+ ifblock

elseblock ::= "else" gap* bracestatement

whileloop ::= "while" gap+ expression gap* bracestatement

forloop ::= "for" gap+ constant gap+ "in" gap+ expression gap* bracestatement
          | "for" gap+ variable gap+ "in" gap+ expression gap* bracestatement
          | "for" gap+ "(" gap* constant gap+ "in" gap+ expression gap* ")" gap* bracestatement
          | "for" gap+ "(" gap* variable gap+ "in" gap+ expression gap* ")" gap* bracestatement

funcdef ::= "func" gap+ symbol gap* "(" gap* defargs gap* ")" gap* bracestatement 

defargs ::= constant gap* ["=" gap* expression gap*] ["," gap* defargs]
          | variable gap* ["=" gap* expression gap*] ["," gap* defargs]
          | "..." constant
          | "..." variable

expression ::= symbol
             | envvar
             | literal
             | operation
             | prefixoperation
             | funccall
             | parentheticalexpr
             | assignment

command ::= pathtopathcmd
          | stringtopathcmd
          | pathtostringcmd
          | singlepathcmd

pathtopathcmd ::= pathtopathcmdname WHITESPACE+ path WHITESPACE+ "TO" WHITESPACE+ path

stringtopathcmd ::= stringtopathcmdname WHITESPACE+ statement WHITESPACE+ "TO" WHITESPACE+ path

pathtostringcmd ::= pathtostringcmdname WHITESPACE+ path WHITESPACE+ "TO" WHITESPACE+ statement

singlepathcmd ::= singlepathcmdname WHITESPACE+ path

childablecmd ::= childablecmd WHITESPACE+ path

pathtopathcmdname ::= "MOVE"
                    | "COPY"

stringtopathcmd ::= "WRITE"
                  | "APPEND"

pathtostringcmdname ::= "RENAME"

singlepathcmdname ::= "DELETE"
                    | "MKDIR"
                    | "MKDIRS"

childablecmdname ::= "PATH"
                   | "READ"
                   | "ISDIR"
                   | "ISFILE"

path ::= envvar pathpart* ["/"]
       | pathpart+ ["/"]

pathpart ::= "/" pathtext+
           | "/" stringliteral
           | "/$" parentheticalexpr 

literal ::= stringliteral
          | number

parentheticalexpr ::= "(" gap* expression gap* ")"

operation ::= expression gap* operator gap* expression

prefixoperation ::= prefixoperator gap* expression

operator ::= "+"
           | "-"
           | "*"
           | "/"
           | ""
           | "="
           | ""
           | "="
           | "=="
           | "!="
           | "&"
           | "|"

prefixoperator ::= "!"
                 | "+"
                 | "-"

funccall ::= symbol gap* "(" gap* callargs gap* ")"

callargs ::= expression [gap* "," gap* callargs]

assignment ::= assignable gap* "=" gap* expression
             | assignable gap* "=" gap* childablecmd

assignable ::= constant
             | variable

constant ::= "%" symbol

variable ::= "$" symbol

envvar ::= "@" symbol

symbol ::= alpha alphanumeric*

stringliteral ::= "\"" .* "\""

number ::= numeric* ["." numeric+] ["e" numeric+]

alphanumeric ::= alpha | numeric

alpha ::= "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z" | "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" | "_"

numeric ::= "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" | "0"

pathtext ::= alphanumeric | "!" | "@" | "#" | "$" | "%" | "^" | "&" | "*" | "(" | ")" | "-" | "+" | "=" | "{" | "}" | "[" | "]" | "'" | ":" | "<" | ">" | "," | "." | "?" | "~" | "`"

terminator ::= NEWLINE 
             | ";"

gap ::= NEWLINE
      | WHITESPACE
```