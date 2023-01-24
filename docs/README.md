<img alt="Sakura" id="readme-logo" src="/images/logo_text.png"/>

<p id="lang-ver">Official documentation for Sakura language version: <code>0.1.0-beta.1</code></p>

## About

Sakura is a dynamically-typed, interpreted, high-level, Turing-complete, programming language. It's designed for security, customization, and safety in mind. It is also designed to be easy to use for non-programmers, while giving enough control to perform more advanced tasks to users that require it. Sakura's main purpose is to interact with the file system, and provides features to make this simple.

## Running Sakura

The main purpose of Sakura is to be run on user devices by other programs (known as *executors*) using a Sakura *interpreter*. Interpreters are what actually parse and run the code, whereas executors instruct interperters to run a certain script within certain parameters. Since a lot of Sakura can be customized by the executor, it's important that you check with the executor your script is intended for when writing code, and use a testing mechanism provided by your executor. Some interpreters, however, do provide a command line tool to run your scripts, so these can be used as well on your local machine.

## Getting Started

Sakura provides the basics of any programming language: variables, functions, and loops, however for a large percentage of scripts, these will not be used. The most basic of Sakura scripts can consist solely of commands to manipulate the file system. A basic script can look something like this:

```ska
MKDIR /some/other
COPY /some/directory TO /some/other/directory
WRITE "Hello World!" TO /some/other/directory/a_file.txt
DELETE /some/directory
```

The full list of commands can be found [here](/commands).