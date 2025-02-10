<img alt="Sakura" id="readme-logo" src="/docs/images/logo_text.png"/>

<p id="lang-ver">Official documentation for Sakura language version: <code>0.1.0-beta.4</code></p>

## About

Sakura is a dynamically-typed, interpreted, high-level, Turing-complete, programming language. It's designed for writing installer scripts, built with security, safety, and customization in mind. It is also designed to be easy to use for non-programmers to use, while giving enough control to perform more advanced tasks to users that require it. Sakura's main purpose is to interact with the file system, and provides many features to help make this simpler.

## Advantages of Sakura

### Security

Sakura can restrict access to files or directories for scripts, only allowing a script to execute within the set parameters. This lets you run scripts without being worried about the installer modifying files that it's not supposed to. Sakura also has no networking capabilites, so you don't need to worry about installation scripts stealing data. 

### Safety

One issue that arises with installation scripts is failed installations. On an error, other install scripts can leave things partially installed or in a broken state. Sakura, however, keeps track of all modifications made to the file system, and can rollback changes automatically if an error occurs.

### Customization

Sakura allows executors to add or restrict features dependent on what the script may need to run, such as defining custom functions or environment variables.

### Simplicity

Sakura is simple for both integration into your code, and for users. Interpreters abstract all of the security away so that you don't have to deal with it, and it lets your users write consice and clear code at a high-level, instead of focusing on issues like rollbacks.

## Running Sakura

The main purpose of Sakura is to be run on user devices by other programs (known as *executors*) using a Sakura *interpreter*. Interpreters are what actually parse and run the code, whereas executors instruct interperters to run a certain script within certain parameters. Since a lot of Sakura can be customized by the executor, it's important that you check with the executor your script is intended for when writing code, and use a testing mechanism provided by your executor. Some interpreters do provide a command line tool to run your scripts, so these can be used as well on your local machine.

## Getting Started

Sakura provides the basics of any programming language: variables, functions, and loops, however for a large percentage of scripts, these will not be used. The most basic of Sakura scripts can consist solely of commands to manipulate the file system. A basic script can look something like this:

```ska
MKDIR /some/other
COPY /some/directory TO /some/other/directory
WRITE "Hello World!" TO /some/other/directory/a_file.txt
DELETE /some/directory
```

The full list of commands can be found [here](/commands).
