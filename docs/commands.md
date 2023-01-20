# Commands & Paths

Commands are an essential part of Sakura, and how Sakura interacts with the file system. Commands extend to the end of a line (or to a semi-colon). They can usually not be a part of another expression (excpetions are the `PATH`, `READ`, `EXISTS`, `ISDIR`, and `ISFILE` commands). 

## Paths

### Basics

Commands operate on paths, which reperesents files or directories on a file system. Paths are symbols that are seperated by forward slashes. For example: `/some/directory`. Is parsed to pointing to the (potentially non-existent) file or directory at `/some/directory` within the root. Paths will automatically be parsed to the proper file system reperesentation. For instance, evaluating `/directory/some_file.txt` will be parsed to `C:\directory\some_file.txt` on Windows machines, and `/directory/some_file.txt` on Linux/MacOS machines. If a portion of the path has a space in it, it needs to be wrapped in quotes. For instance: `/"some directory"/"a file.txt"`. These quotes are not permited to have slashes within them.

### The Root

The root of a path is determined by the executor. The executor sets this root. All paths that start with a slash will be prefixed with the root. The root of a path may be changed by providing an expression, or environment variable. See the [PATH command](#PATH) for uses of this. By default, there is an environment variable `@root` which is the same as prefixing a path with a slash. So the path `@root/some/directory` is the same as `/some/directory`.

### Dynamic Path Parts

When using paths, a lot of symbols are not parsed normally, reserved charachters (like parentheses, braces, etc.) are parsed as text. Slashes act as path seperators, in this case. An exception to this rule is the first part of the path, which if it is an enviornment variable, it will be evaluated (and is expected to be a path). For instance evaluating `@root/@root` within an executor which sets the root to `/user/documents/` evaluates to `/user/documents/@root`.

To evaluate symbols normally, use `$()`. This will evaluate the expression and place it in the path literal. The expression must evaluate to a type of string, and can not contain slashes. Paths must either start with a `/`, an expression that evaluates to a path (wrapped in `$()`), or an environment variable of type `PATH`.

## Commands

### PATH

The PATH command evaluates a path, and allows you to store it into a variable.

```ska
%myPath = PATH /some/file
WRITE "Some text!" TO $(myPath)
```

This can be useful as paths can serve as roots, which can save typing.

```ska
%rootDirectory = PATH /some/directory/here
MKDIRS $(rootDirectory)
WRITE "File 1" TO $(rootDirectory)/file_1.txt
WRITE "File 2" TO $(rootDirectory)/file_2.txt 
```

It also can be used for dynamically setting directories (like so).

```ska
%basePath = PATH /folder_1
%basePathExists = EXISTS $(basePath)

if !basePathExists {
  %basePath = PATH /folder_2
  MKDIR $(basePath)
}

WRITE "This file is in the folder: " + basePath TO $(basePath)
```

### DELETE

The delete command deletes a file or directory.

```ska
DELETE /some/directory
DELETE /some/file.txt
```
