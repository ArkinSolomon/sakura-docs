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

**Requires no permissions**

The `PATH` command evaluates a path, and allows you to store it into a variable.

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

It also can be used for setting directories dependent on other conditions.

```ska
$basePath = PATH /folder_1
%basePathExists = EXISTS $(basePath)

if !basePathExists {
  basePath = PATH /folder_2
  MKDIR $(basePath)
}

WRITE "new file" TO $(basePath)/new_file.txt
```

### READ

**Requires READ permissions**

The `READ` command reads all the text from a file, and you can store its contents into a variable as a string.

```ska
%content = READ /some/file.txt
print("The contents of file.txt is " + content)
```

### EXISTS

**Requires READ permissions**

The `EXISTS` command can determine if a file or directory exists, and allows it to be stored as a variable with a true boolean value if it exists, or false if it doesn't.

```ska
%exists = EXISTS /some/file.txt
```

### ISDIR

**Requires READ permissions**

Check if a file exists and is a directory. Allows its value to be stored as a variable with a true boolean value if it exists and is a directory, or false if it doesn't.

```ska
%isDir = ISDIR /some/directory
```

### ISFILE

**Requires READ permissions**

Check if a file is a file (as opposed to a directory). Allows its value to be stored as a variable with a true boolean value if it exists and is a file, or false if it doesn't.

```ska
%isFile = ISFILE /some/file.txt
```

> The `PATH`, `READ`, `EXISTS`, `ISDIR`, and `ISFILE` commands return values can only have it's values stored into a variable (or directly returned), they can not be used inline as a value.

### MKDIR

**Requires WRITE permissions**

Create a directory at the specified path. Errors if the directory already exists, or if the parent directory does not exist.

```ska
MKDIR /some/new_folder
```

### MKDIRS

**Requires WRITE permissions**

Create a directory and all parent directories that do not exist. Errors if the directory exists.

```ska
MKDIRS /some/new_folder/and_sub_folders
```

### DELETE

**Requires WRITE permissions**

The `DELETE` command deletes a file or directory.

```ska
DELETE /some/directory
DELETE /some/file.txt
```

### WRITE

**Requires WRITE permissions**

Write some text to a file, completely overwriting any content within it.

```ska
%path = PATH /file.txt
WRITE "Some text!" TO $(path)
%content = READ $(path)
print(content) # Prints "Some text!"
```

### APPEND

**Requires WRITE permissions**

Append some text to a file, adding onto the content that is there already.

```ska
%path = PATH /file.txt
WRITE "Hello" TO $(path)
APPEND " World!" TO $(path)
%content = READ $(path)
print(content) # Prints "Hello World!"
```

### COPY

**Requires READ and WRITE permissions**

The `COPY` command copies a file or directory from one place to another.

```ska
%original = PATH /file.txt
%new = PATH /new_file.txt
COPY $(original) TO $(new)
%originalExists = EXISTS $(original)
%newExists = EXISTS $(new)
print(originalExists) # Prints "true"
print(newExists) # Prints "true"
```

### MOVE

**Requires READ and WRITE permissions**

The `MOVE` command moves a file or directory from one place to another.

```ska
%original = PATH /file.txt
%new = PATH /new_file.txt
MOVE $(original) TO $(new)
%originalExists = EXISTS $(original)
%newExists = EXISTS $(new)
print(originalExists) # Prints "false"
print(newExists) # Prints "true"
```

### RENAME

**Requires READ and WRITE permissions**

Change the name of a file or directory. Slashes are not allowed in the new name. If you are attempting to move the folder use the [`MOVE` command](#move) instead.

```ska
%original = PATH /file.txt
RENAME $(original) TO "renamed_file.txt"
%originalExists = EXISTS $(original)
%newExists = EXISTS /renamed_file.txt
print(originalExists) # Prints "false"
print(newExists) # Prints "true"
```