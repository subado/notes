---
title: "The Language"
date: 2023-10-20T21:53:05+04:00
draft: true
---

## Basics

> Everything in a CMake listfile is either a _command_ invocation or a _comment_.

### Comments

```cmake
# they can be placed on an empty line
message("Hi") # or after a command like here.
#[=[
    bracket comment
    #[[
        nested bracket comment
    #]]
#]=]
```

Number of = signs between square breackets can be different.
Zero is also a possible number.

_Good comments_ provide at least one of the following:

- **Information**: They can untangle complexities such as regex patterns or formatting
  strings.
- **Intent**: They can explain the intent of the code when it is unobvious from the
  implementation or interface.
- **Clarification**: They can explain concepts that can't be easily refactored or changed.
- **Warnings of consequences**: They can provide warnings, especially around code
  that can break other things.
- **Amplification**: They can underline the importance of an idea that is hard to express
  in code.
- **Legal clauses**: They can add this necessary evil, which is usually not the domain of a
  programmer.

If you can, avoid adding comments of the following types:

- **Mandated**: These are added for completeness, but they are not really important.
- **Redundant**: These repeat what is already clearly written in the code.
- **Misleading**: These could be outdated or incorrect if they don't follow code changes.
- **Journal**: These note what has been changed and when (use VCS for this instead).
- **Dividers**: These mark sections.

### Command invocations

```cmake
message("hello" world)
| name |  arguments  |
```

1. Commands should be named using _snake_case_
2. Command invocations in CMake are not expressions.
3. Each line of source can contain up to one command
   invocation, followed by an optional single-line comment.

Type of commands:

1. **Scripting**. Change the state of the command processor,
   access variables, and affect other commands and the environment.
2. **Project**. Manipulate the project state and build targets.
3. **CTest**. Available in CTest scripts.

### Command arguments

1. The only data type recognized by CMake is a string.
2. Evaluating means string interpolation.

#### Bracket arguments

These arguments are structured like multi-line comments:

```cmake
message([[
    I
    really
    don't
    want
    to
    pass
]])
message([=[
    UGE [[ЕГЭ]]
]=]
)
```

### Quoted arguments

1. Surrouned with " signs.
2. _Escape sequences_ are expanded.
3. Can contain _variable references_. To insert a variable use: ${name}.
4. Can be multi-lined.

```cmake
message("I was dancing all night
\t And now I'm wanting to expand this variable ${CMAKE_VERSION}")
```

### Unquoted arguments

1. Can contain _escape sequences_ and _variable references_.
2. CMake treat `;` as delimiter.
3. Parentheses `()` are allowed then they are form correct, matching pairs.

```cmake
message(\"first\" \ second  \tthird\t; fourth\n ())
```

## Working with variables

Categories of variables:

- Normal
- Cache
- Environment

1. Variable names are case-sensitive.
2. Are stored in memory as strings.

### Variable references

**Variable evaluation(expansion or interpolation)** is a process of traversing scope stack by CMake
and then replace `${name}` with variable value, or an empty string if no variable is found.

1. The `${}` is used to reference _normal or cache_ variables.
2. The `$ENV{}` is used to reference _environment_ variables.
3. The `$CACHE{}` is used to reference _cache_ variables.

### Using the environment variables

The **enivroment variables** are copy of enivroment which used to start the cmake process.

References to environment variables will be intorpolated during the _generation of the buildsystem_.

_Values are overwritten only for the current execution of CMake._

Syntax:

- `set(ENV{<variable>} <value>)`
- `unset(ENV{<variable>})`

### Using the cache variables

The **cache variables** are persistent variables stored in a CMakeCache.txt.

_Values are overwritten only for the current execution of CMake._

Syntax: `set(<variable> <value> CACHE <type> <docstring> [FORCE])`.

The following `type`s are accepted:

- **BOOL**: A Boolean on/off value.
- **FILEPATH**: A path to a file on a disk.
- **PATH**: A path to a directory on a disk.
- **STRING**: A line of text.
  STRINGS <values>).
- **INTERNAL**: A line of text.The internal entries may be used to store variables
  persistently across runs. Use of this type implicitly adds the FORCE keyword.

The `<doctring>` value is simply a label.

### How to correctly use the variable scope in CMake

Types of scropes:

1. **Function scope**. For when custom functions defined with function() are executed.
2. **Directory scope**. For when a CMakeLists.txt listfile in a nested directory is
   executed from the add_subdirectory() command

When a nested scope is created, CMake simply
fills it with copies of all the variables from the current scope.

Subsequent commands will affect these copies.

But as soon as the execution of the nested scope is completed, all
copies are deleted and the original, parent scope is restored.

Syntax for modify parent scope's variables(work only with variables which scope is one level up):

- `set(MyVariable "New Value" PARENT_SCOPE)`
- `unset(MyVariable PARENT_SCOPE)`

### Using lists

List stored as string in which list elements are delimited with `;`.

```cmake
set(myList "a;list;of;five;elements")
```

## Understanding control structures in CMake

### Conditional blocks

```cmake
if(<condition>)
    <commands>
elseif(<condition>) # optional block, can be repeated
    <commands>
else()  # optional block
    <commands>
endif()
```

Supported logical operators: `NOT`, `AND`, `OR`.

#### The evaluation of a string and a variable

> CMake will try to evaluate _unquoted arguments_ as if they are _variable references_.

Strings are considered **Boolean true** only if they equal any of
the following constants (these comparisons are case insensitive):

- ON, Y, YES, or TRUE
- A non-zero number

_Unquoted arguments_ are considered **Boolean false** only if they equal any of
the following constants (these comparisons are case insensitive):

- OFF, NO, FALSE, N, IGNORE, NOTFOUND
- A string ending with -NOTFOUND
- An empty string
- Zero

Better way to use syntax like this:

```cmake
if ("${FOO}")
```

You can use `if(DEFINED <name>)` to check that variable is defined.

#### Comparing values

Operators: `EQUAL`, `LESS`, `LESS_EQUAL`, `GREATER`, and `GREATER_EQUAL`.

You can compare software versions following the major[.minor[.patch[.tweak]]] format by adding a `VERSION_` prefix to any of the operators.

For lexicographic string comparisons, we need to prepend an operator with the `STR` prefix.

You can use **POSIX regex** matching with this syntax: `<VARIABLE|STRING> MATCHES <regex>`.
Any matched groups are captured in `CMAKE_MATCH_<n>` variables.

#### Simple checks

- If a value is in a list: `<VARIABLE|STRING> IN_LIST <VARIABLE>`.
- If a command is available for invocation: `COMMAND <command-name>`.
- If a CMake policy exists: `POLICY <policy-id>`.
- If a CTest test was added with add_test(): `TEST <test-name>`.
- If a build target is defined: `TARGET <target-name>`.

#### Examining the filesystem

Common commands to work with files(**behavior is well defined only for absolute paths**):

- `EXISTS <path-to-file-or-directory>`: Checks if a file or directory exists
  This resolves symbolic links (it returns true if the target of the symbolic link exists).
- `<file1> IS_NEWER_THAN <file2>`: Checks which file is newer
  This returns true if file1 is newer than (or equal to) file2 or if one of the two
  files doesn't exist.
- `IS_DIRECTORY <path-to-directory>`: Checks if a path is a directory
- `IS_SYMLINK <file-name>`: Checks if a path is a symbolic link
- `IS_ABSOLUTE <path>`: Checks if a path is absolute

### Loops

- The `break()` loop stops the execution of the remaining block and breaks from the enclosing loop.
- The `continue()` loop stops the execution of the current iteration and starts at the top of the next one.

#### While

```cmake
while(<condition>)
    <commands>
endwhile()
```

#### Foreach

```cmake
foreach(<loop_var> RANGE <max>)
    <commands>
endforeach()
```

Max here is inclusive.

```cmake
foreach(<loop_var> RANGE <min> <max> [<step>])
```

```cmake
foreach(<loop_variable> IN [LISTS <lists>] [ITEMS <items>])
```

```cmake
foreach(<loop_var>... IN ZIP_LISTS <lists>)
```

Zipping lists means simply iterating through multiple lists and working
on respective items with the same index

CMake create a `num_<N>` variable for each list provided.

```cmake
set(L1 "one;two;three;four")
set(L2 "1;2;3;4;5")
foreach(num IN ZIP_LISTS L1 L2)
    message("num_0=${num_0}, num_1=${num_1}")
endforeach(
```

You can pass miltiple loop vars with ZIP_LISTS.

```cmake
foreach(word num IN ZIP_LISTS L1 L2)
    message("word=${word}, num=${num}")
```

### Command definitions

In CMake two ways of defining commands are exist:

- `macro()`
- `function()`

CMake allows you to access arguments passed in command calls with
the following references:

- `${ARGC}`: The count of arguments
- `${ARGV}`: A list of all arguments
- `${ARG0}`, `${ARG1}`, `${ARG2}`: The value of an argument at a specific index
- `${ARGN}`: A list of anonymous arguments that were passed by a caller after the last
  expected argument

#### Macros

```cmake
macro(<name> [<argument>…])
    <commands>
endmacro()
```

1. Macros works more like a find-and-replace instruction than an actual subroutine call such as function().
2. Arguments passed to macros aren't treated as real variables but
   rather as constant find-and-replace instructions(You cannot `set()` them).

> You should using functions whenever you can, as it will probably save you a lot of headaches.

#### Functions

```cmake
function(<name> [<argument>…])
    <commands>
endfunction()
```

1. Functions have their own scope.
2. Functions follow the rules of the call stack, enabling
   returning to the calling scope with the `return()` command.

CMake sets the following variables for each function:

- `CMAKE_CURRENT_FUNCTION`
- `CMAKE_CURRENT_FUNCTION_LIST_DIR`
- `CMAKE_CURRENT_FUNCTION_LIST_FILE`
- `CMAKE_CURRENT_FUNCTION_LIST_LINE`

#### The procedural paradigm in CMake

A correctly structured piece of code lists the most general steps in the first subroutine, after which it provides the slightly more detailed subroutines, and pushes the most detailed steps to the very end of the file.

Ways to write structured code:

1. Moving command definitions to other files and partitioning scopes across directories.
2. Declaring an **entry-point macro** at the top of the file and calling it at the very end of the file.

#### A word on naming conventions

Rules:

1. Follow a consistent naming style (**snake_case** is an accepted standard in the
   CMake community).
2. Use short but **meaningful** names (for example, avoid func(), f(), and suchlike).
3. **Avoid puns and cleverness** in your naming.
4. Use pronounceable, **searchable names** that don't require mental mapping.

## Useful commands

### `message()`

```cmake
message(<MODE> "text")
```

The recognized modes are as follows:

- `FATAL_ERROR`: This stops processing and generation.
- `SEND_ERROR`: This continues processing, but skips generation.
- `WARNING`: This continues processing.
- `AUTHOR_WARNING`: A CMake warning. This continues processing.
- `DEPRECATION`: This works accordingly if either of the CMAKE_ERROR_DEPRECATED
  or CMAKE_WARN_DEPRECATED variables are enabled.
- `NOTICE` or omitted mode (default): This prints a message to stderr to attract the
  user's attention.
- `STATUS`: This continues processing and is recommended for main messages for
  users.
- `VERBOSE`: This continues processing and should be used for more detailed
  information that usually isn't very necessary.
- `DEBUG`: This continues processing and should contain any fine details that might be
  helpful when there's an issue with a project.
- `TRACE`: This continues processing and is recommended to print messages during
  the project development. Usually, these sorts of messages would be removed before
  publishing the project.

Env vars:

- `CMAKE_MESSAGE_INDENT`
- `CMAKE_MESSAGE_CONTEXT`

### `include()`

```cmake
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>])
```

1. Any changes to variables done in included file will affect the calling scope.
2. `RESULT_VARIABLE` var will be filled with a full path to the
   included file on success or not found (NOTFOUND) on failure.

### `include_guard()`

```cmake
include_guard([DIRECTORY|GLOBAL])
```

If `include_guard()` putted at the top of the included file then it can be included only once.

### `file()`

```cmake
file(READ <filename> <out-var> [...])
file({WRITE | APPEND} <filename> <content>...)
file(DOWNLOAD <url> [<file>] [...])
```

### `execute_process()`

```cmake
execute_process(COMMAND <cmd1> [<arguments>]… [OPTIONS])
```

You can pass more than one `COMMAND <cmd> <arguments>`. Output of the previous command is the input for next.

1. You may use a `TIMEOUT <seconds>` argument to terminate the process
   if it hasn't finished the task within the required limit.
2. You can set the `WORKING_DIRECTORY <directory>` as you need.
3. The exit codes of all tasks can be collected in a list by providing `RESULTS_VARIABLE <variable>` arguments.
4. To collect the output, CMake provides two arguments: `OUTPUT_VARIABLE` and `ERROR_VARIABLE`.
