---
title: "Working With Targets"
date: 2023-10-27T15:38:37+04:00
draft: true
---

**Artifacts** are output files of a buildsystem.

## The concept of a target

**Targets** are files which was compiled from some source files and can be linked with other targets.

A target can be created using one of 3 commands:

- `add_executable()`
- `add_library()`
- `add_custom_target()`

```cmake
add_custom_target(Name [ALL] [command1 [args1...]]
    [COMMAND command2 [args2...] ...]
    [DEPENDS depend depend depend ... ]
    [BYPRODUCTS [files...]]
    [WORKING_DIRECTORY dir]
    [COMMENT comment]
    [JOB_POOL job_pool]
    [VERBATIM] [USES_TERMINAL]
    [COMMAND_EXPAND_LISTS]
    [SOURCES src1 [src2...]])
```

1. _Custom targets_ allow you to specify your own command line that will be
   executed without checking whether the produced output is up to date.
2. _Custom targets_ won't be built until they are added to a _dependency graph_.

### Dependency graph

**Dependency graphs** are graphs in which targets are vertices.
If a target depends on another target, then the directed edge will be between the targets' vertices in this graph.
And the direction of this edge represents what kind of dependency there is between targets.

> All valid dependency graphs are directional acyclic graphs.

- The `target_link_libraries()` is intended to be used with actual libraries
  and allows you to control property propagation.

- The `add_dependencies()`is meant to be used only with top-level targets to set their build order.

### Visualizing dependencies

```cmake
cmake --graphviz=<file> <path-to-source>
```

### Target properties

Use the following commands to manipulate the properties of a target:

```cmake
get_target_property(<var> <target> <property-name>)
set_target_properties(<target1> <target2> ...
                      PROPERTIES <prop1-name> <value1>
                      <prop2-name> <value2> ...)
```

Properties:

- `AUTOUIC_OPTIONS`
- `COMPILE_DEFINITIONS`
- `COMPILE_FEATURES`
- `COMPILE_OPTIONS`
- `INCLUDE_DIRECTORIES`
- `LINK_DEPENDS`
- `LINK_DIRECTORIES`
- `LINK_LIBRARIES`
- `LINK_OPTIONS`
- `POSITION_INDEPENDENT_CODE`
- `PRECOMPILE_HEADERS`
- `SOURCES`

The general command to set a target property:

```cmake
set_property(TARGET <target> PROPERTY <name> <value>)
```

### What are transitive usage requirements?

**Transitive usage requirements** are propagated properties between the source target
(targets that gets used) and destination targets (targets that use other targets).

Propagation keywords work like this when
you use command like `target_<PROPERTY>()` to set some property:

- `PRIVATE` sets the property of the source target.
- `INTERFACE` sets the property of the destination targets.
- `PUBLIC` sets the property of the source and destination targets.

For example

```cmake
target_compile_definitions(<source> <INTERFACE|PUBLIC|PRIVATE>
                          [items1...])
```

How does this work under the hood?

1. When you specify a `PRIVATE` or `PUBLIC` keyword, CMake will store
   provided values in the property of the target matching the command.
2. If a keyword was `INTERFACE` or `PUBLIC`, it will store the value in property
   with an `INTERFACE_` prefix.
3. During the configuration stage, CMake will read the interface
   properties of source targets and append their contents to destination targets.

Full signarute of `target_link_libraries()`

```cmake
target_link_libraries(<target>
                     <PRIVATE|PUBLIC|INTERFACE> <item>...
                    [<PRIVATE|PUBLIC|INTERFACE> <item>...]...)
```

There is propagation keywords controls where
properties from the source target **get stored** in the destination target.

Propagation keywords work like this in the linking case:

- `PRIVATE` appends the source value to the _private_ property of the destination.
- `INTERFACE` appends the source value to the _interface_ property of the destination.
- `PUBLIC` appends to both properties of the destination.

### Dealing with conflicting propagated properties

> To trigger propagatation and compatibility check of a custom property
> you have to explicitly add a custom property to a list of "compatible" properties.

Each target has four lists of "compatible" properties.

- `COMPATIBLE_INTERFACE_BOOL`
  The BOOL list will check whether all properties propagated to the destination
  target evaluate to the same Boolean value.
- `COMPATIBLE_INTERFACE_STRING`
  Analogically, STRING will evaluate to a string.
- `COMPATIBLE_INTERFACE_NUMBER_MAX` and `COMPATIBLE_INTERFACE_NUMBER_MIN`
  NUMBER_MAX and NUMBER_MIN are a bit different – propagated values don't have to
  match, but the destination target will just receive the highest or the lowest value instead.

### Meet the pseudo targets

**Pseudo targets** are things that do not represent outputs of
the buildsystem but rather inputs – external dependencies, aliases, and so on.

> Pseudo targets are targets that **don't make it** to the generated buildsystem

#### Imported targets

**Imported targets** are essentially products of managing external dependencies
and you should treat them as immutable despite the fact that you can adjust their
target properties.

The scope of the definition of an IMPORTED target can be global or local to the directory
where it was defined.

#### Alias targets

```cmake
add_executable(<name> ALIAS <target>)
add_library(<name> ALIAS <target>)
```

**Alias targets** are another reference to a target under a different name.

Properties of alias targets are read-only, and you cannot install or export aliases (they
aren't visible in the generated buildsystem).

#### Interface libraries

**Interface libraries** are library that doesn't compile anything, but instead serves
as a utility target, some of which properties are set and can be propagated.

To creare use

```cmake
add_library(<name> INTERFACE <sources>)
```

Interface libraries have two primary uses

1. to represent header-only libraries.
2. bundle a bunch of propagated properties into a single logical unit.

### Build targets

**Buildsystem targets** are targets of buildsystem which files was generated by CMake.

One such buildsystem target is `ALL`, which CMake generates by default to contain all
top-level list file targets, such as executables and libraries (not necessarily custom targets).

To optimize our default build, we can exclude executables or libraries
might not be needed in every build from the `ALL` target like so:

```cmake
add_executable(<name> EXCLUDE_FROM_ALL [<source>...])
add_library(<name> EXCLUDE_FROM_ALL [<source>...])
```

> Custom targets excluded from the ALL by default.

Another implicitly defined build target is `clean`, which simply removes produced
artifacts from the build tree.

## Writing custom commands

**Actual targets** – custom commands.

The 1st version of `add_custom_command()`. It is extended version of
`add_custom_target()`:

```cmake
add_custom_command(OUTPUT output1 [output2 ...]
                   COMMAND command1 [ARGS] [args1...]
                   [COMMAND command2 [ARGS] [args2...] ...]
                   [MAIN_DEPENDENCY depend]
                   [DEPENDS [depends...]]
                   [BYPRODUCTS [files...]]
                   [IMPLICIT_DEPENDS <lang1> depend1
                                    [<lang2> depend2] ...]
                   [WORKING_DIRECTORY dir]
                   [COMMENT comment]
                   [DEPFILE depfile]
                   [JOB_POOL job_pool]
                   [VERBATIM] [APPEND] [USES_TERMINAL]
                   [COMMAND_EXPAND_LISTS])
```

> As soon as you add custom target to the `ALL` target or
> start depending on custom target for other targets, they will be built **every single time**.

There are two ways of adding custim target to dependency graph:

1. Using it's output artifact as a source for an executable (or library).
2. Explicitly adding it to a DEPENDS list for a custom target.

### Using a custom command as a generator

Abstract example:

```cmake
add_custom_command(OUTPUT <generated-source>
    COMMAND <compiler> ARGS <raw-source>
    DEPENDS <raw-source>)

add_executable(main <sources> <generated-source>)
```

### Using a custom command as a target hook

The 2nd version of `add_custom_command()`. It introducesa mechanism
to execute commands before or after building a target:

```cmake
add_custom_command(TARGET <target>
                   PRE_BUILD | PRE_LINK | POST_BUILD
                   COMMAND command1 [ARGS] [args1...]
                   [COMMAND command2 [ARGS] [args2...] ...]
                   [BYPRODUCTS [files...]]
                   [WORKING_DIRECTORY dir]
                   [COMMENT comment]
                   [VERBATIM] [USES_TERMINAL]
                   [COMMAND_EXPAND_LISTS])
```

- `PRE_BUILD` will run before any other rules for this target (Visual Studio generators
  only; for others, it behaves like PRE_LINK).
- `PRE_LINK` binds the command to be run just after all sources have been compiled
  but before the linking (or archiving) the target. It doesn't work for custom targets.
- `POST_BUILD` will run after all other rules have been executed for this target.

## Understanding generator expressions

**Generator expressions(genexes)** - placeholders for information that is only available
after the configuration stage, and it is postponing evaluation to the generation stage.

> Generator expressions are built around _target properties_.

To debug generator expressions use either of these methods:

1. Write it to a file (this specific version of the file() command supports
   generator expressions):
   ```cmake
   file(GENERATE OUTPUT filename CONTENT "$<...>")
   ```
2. Add a custom target and build it explicitly from the command line:
   ```cmake
   add_custom_target(gendbg COMMAND ${CMAKE_COMMAND} -E echo "$<...>")
   ```

### General syntax

```
$<EXPRESSION:arg1,arg2,arg3>
 |   name   |   arguments  |
```

Arguments are optioonal here.

#### Nesting

```
$<EXPRESSION1:$<EXPRESSION2>>
```

```
$<EXPRESSION1:${variable}>
```

#### Conditional expressions

1st form:

```cmake
$<IF:condition,true_string,false_string>
```

2nd form:

```cmake
$<condition:true_string>
```

> Conditions in conditional expressions are explicitly required to evaluate to **Boolean**.

### Types of evaluation

Generator expressions are evaluated to one of two types:

1. **Boolean** is represented by 1 (true) and 0 (false).
2. Everything else is just a **string**.

#### Evaluation to Boolean

There are three categories of expressions that get evaluated to Boolean:

1. **Logical operators**

   - `$<NOT:arg>` negates the Boolean argument.
   - `$<AND:arg1,arg2,arg3...>` returns 1 if all the arguments are 1.
   - `$<OR:arg1,arg2,arg3...>` returns 1 if any of the arguments is 1.
   - `$<BOOL:string_arg>` converts arguments from a string to a Boolean type.

   String conversion will evaluate to 1 if none of these conditions are met:

   - The string is empty.
   - The string is a case-insensitive equivalent of `0`, `FALSE`, `OFF`, `N`, `NO`, `IGNORE`, or
     `NOTFOUND`.
   - The string ends in the `-NOTFOUND` suffix (case-sensitive).

2. **String comparison**
   Comparisons will evaluate to 1 if their condition is met and 0 otherwise:
   - `$<STREQUAL:arg1,arg2>` is a case-sensitive string comparison.
   - `$<EQUAL:arg1,arg2>` converts a string to a number and compares equality.
   - `$<IN_LIST:arg,list>` checks whether the arg element is in the list list
     (case-sensitive).
   - `$<VERSION_EQUAL:v1,v2>`, `$<VERSION_LESS:v1,v2>`, `$<VERSION_GREATER:v1,v2>`,
     `$<VERSION_LESS_EQUAL:v1,v2>`, and `$<VERSION_GREATER_EQUAL:v1,v2>`
     are component-wise version comparisons.
3. **Variable queries**
   They will evaluate to 1 if their condition is met and 0 otherwise.

   - `$<TARGET_EXISTS:arg>` – does the arg target exist?
   - `$<CONFIG:args>` is the current config (Debug, Release, and so on) in args
     (case-insensitive).
   - `$<PLATFORM_ID:args>` is the current platform ID in `args`.
   - `$<LANG_COMPILER_ID:args>` is CMake's `LANG` compiler ID in `args`, where
     `LANG` is one of `C`, `CXX`, `CUDA`, `OBJC`, `OBJCXX`, `Fortran`, or `ISPC`.
   - `$<LANG_COMPILER_VERSION:args>`is CMake's `LANG` compiler version in `args`, where
     `LANG` is one of `C`, `CXX`, `CUDA`, `OBJC`, `OBJCXX`, `Fortran`, or `ISPC`.
   - `$<COMPILE_FEATURES:features>` will return true if features is
     supported by the compiler for this target.
   - `$<COMPILE_LANG_AND_ID:lang,compiler_id1,compiler_id2...>`
     is the language of this `lang` target and is the compiler used for this target present
     in the `compiler_ids` list.
   - `$<COMPILE_LANGUAGE:args>` if a language is used for the compilation of this
     target in `args`.
   - `$<LINK_LANG_AND_ID:lang,compiler_id1,compiler_id2...>`
     works similarly to `COMPILE_LANG_AND_ID` but checks the language used for
     the link step instead. Use this expression to specify link libraries, link options, link
     directories, and link dependencies of a particular language and a linker combination
     in a target.
   - `$<LINK_LANGUAGE:args>` is the language used for the link step in `args`.

#### Evaluation to a string

1.  **Variable queries**
    These expressions will evaluate to a specific value at the generation stage.

    - `$<CONFIG>` – the configuration (Debug and Release) name.
    - `$<PLATFORM_ID>` – the current system's CMake platform ID (Linux, Windows,
      or Darwin). We discussed platform in the previous chapter, in the Scoping the
      environment section.
    - `$<LANG_COMPILER_ID>` – CMake's compiler ID of the `LANG` compiler used,
      where `LANG` is one of `C`, `CXX`, `CUDA`, `OBJC`, `OBJCXX`, `Fortran`, or `ISPC`.
    - `$<LANG_COMPILER_VERSION>` – CMake's compiler version of the `LANG` compiler used,
      where `LANG` is one of `C`, `CXX`, `CUDA`, `OBJC`, `OBJCXX`, `Fortran`, or `ISPC`.
    - `$<COMPILE_LANGUAGE>` – the compiled language of source files when evaluating
      _compile options_.
    - `$<LINK_LANGUAGE>` – the link language of a target when evaluating link options.

2.  **Target-dependent queries**
    With the following queries, you can evaluate properties of an executable or library target. - `$<TARGET_NAME_IF_EXISTS:target>` – the target name of target if it
    exists; it is an empty string otherwise.
    - `$<TARGET_NAME_IF_EXISTS:target>` – the target name of target if it
      exists; it is an empty string otherwise.
    - `$<TARGET_FILE:target>` – the full path to the target binary file.
    - `$<TARGET_FILE_NAME:target>` – the target filename.
    - `$<TARGET_FILE_BASE_NAME:target>` – the base name of target, or
      `$<TARGET_FILE_NAME:target>` without a prefix and suffix.
    - `$<TARGET_FILE_PREFIX:target>` – the prefix of the target filename (lib).
    - `$<TARGET_FILE_SUFFIX:target>` – the suffix (or extension) of the target
      filename (.so, .exe).
    - `$<TARGET_FILE_DIR:target>` – the directory of the target binary file.
    - `$<TARGET_LINKER_FILE:target>` – the file used when linking to the target
      target. Usually, it is the library that target represents (.a, .lib, .so) on
      platforms with **Dynamically Linked Libraries (DLL)**; for a shared library, it will be
      a .lib import library.
      `TARGET_LINKER_FILE` offers the same family of expressions as the regular
      `TARGET_FILE` expression:
      ```cmake
      $<TARGET_LINKER_FILE_NAME:target>, $<TARGET_LINKER_FILE_BASE_NAME:target>, $<TARGET_LINKER_FILE_PREFIX:target>,
      $<TARGET_LINKER_FILE_SUFFIX:target>, $<TARGET_LINKER_FILE_DIR:target>
      ```
    - `$<TARGET_SONAME_FILE:target>` – the full path to a file with a soname
      (.so.3).
    - `$<TARGET_SONAME_FILE_NAME:target>` – the name of a file with a soname.
    - `$<TARGET_SONAME_FILE_DIR:target>` – the directory of a file with a
      soname.
    - `$<TARGET_PDB_FILE:target>` – the full path to the linker generated program
      database file (.pdb) for target.
      PDB files offer the same expressions as a regular `TARGET_FILE`:
      ```cmake
      $<TARGET_PDB_FILE_BASE_NAME:target>, $<TARGET_PDB_FILE_NAME:target>,
      $<TARGET_PDB_FILE_DIR:target>.
      ```
    - `$<TARGET_BUNDLE_DIR:target>` – the full path to the bundle (Apple–specific
      package) directory (my.app, my.framework, or my.bundle) for target.
    - `$<TARGET_BUNDLE_CONTENT_DIR:target>` – the full path to the
      bundle content directory for target. On macOS, it's my.app/Contents,
      my.framework, or my.bundle/Contents. Other **Software Developent Kits**
      (SDKs) (such as iOS) have a flat bundle structure – my.app, my.framework, or
      my.bundle.
    - `$<TARGET_PROPERTY:target,prop>` – the prop value for target.
    - `$<TARGET_PROPERTY:prop>` – the prop value for target for which the
      expression is being evaluated.
    - `$<INSTALL_PREFIX>` – the install prefix when the target is exported with
      `install(EXPORT)` or when evaluated in `INSTALL_NAME_DIR`; otherwise,
      it is empty.
3.  **Escaping**
    Pass a character to a generator expression.

    - `$<ANGLE-R>` – a literal > symbol (which compares strings containing >)
    - `$<COMMA>` – a literal , symbol (which compares strings containing ,)
    - `$<SEMICOLON>` – a literal ; symbol (which prevents a list expansion on an
      argument with ;)

4.  **String transformations**
    Working with strings.

    - `$<JOIN:list,d>` – join a semicolon-separated list using a `d` delimiter.
    - `$<REMOVE_DUPLICATES:list>` – remove duplicates without sorting list.
    - `$<FILTER:list,INCLUDE|EXCLUDE,regex>` – include/exclude items from
      a list using a regex regular expression.
    - `$<LOWER_CASE:string>`, `$<UPPER_CASE:string>` – convert the string to
      another case.
    - `$<GENEX_EVAL:expr>` – evaluate the expr string as a nested expression in
      the context of the current target. This is useful when an evaluation of a nested
      expression returns another expression (they aren't evaluated recursively).
    - `$<TARGET_GENEX_EVAL:target,expr>` – evaluate expr similarly to the
      GENEX_EVAL transformation but in the context of target.

5.  **Output-related expressions**
    _"These expressions generate output, in some cases depending on an input."_

    They return their first arguments if a specific condition is met
    and an empty string otherwise:

    - `$<LINK_ONLY:deps>` – sets implicitly with `target_link_libraries()`
      to store PRIVATE deps link dependencies, which won't be propagated as usage
      requirements
    - `$<INSTALL_INTERFACE:content>` – returns content if used with
      `install(EXPORT)`
    - `$<BUILD_INTERFACE:content>` – returns content if used with an
      `export()` command or by another target in the same buildsystem

    They perform a string transformation on their arguments:

    - `$<MAKE_C_IDENTIFIER:input>` – converts to a C identifier following the same
      behavior as `string(MAKE_C_IDENTIFIER)`.
    - `$<SHELL_PATH:input>` – converts an absolute path (or list of paths) to a shell
      path style matching the target OS. Slashes are converted to backslashes in Windows
      shells and drive letters are converted to POSIX paths in MSYS shells.

    A stray variable query expression:

    - `$<TARGET_OBJECTS:target>` – returns a _list of object files_ from a target
      object library

### Examples to try out

#### Build configurations

```cmake
target_compile_options(main $<$<CONFIG:DEBUG>:-g>)
```

#### System-specific one-liners

```cmake
target_compile_definitions(main PRIVATE $<$<CMAKE_SYSTEM_NAME:LINUX>:LINUX=1>)
```

#### Interface libraries with compiler-specific flags

```cmake
add_library(enable_rtti INTERFACE)
target_compile_options(enable_rtti INTERFACE
    $<$<OR:$<COMPILER_ID:GNU>,$<COMPILER_ID:Clang>>:-rtti>
)
```
