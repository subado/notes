---
title: "Classes"
date: 2023-12-13T17:09:24+04:00
draft: true
---

**Classes** provide a means of bundling data and functionality together.

Creating a new class creates a _new type of object_, allowing new **instances** of that type to be made.

Class instances can have

- **methods** (defined by its class) for modifying its state,
- **attributes** attached to it for maintaining its state.

All member functions are **virtual**.
The method function is declared with an explicit first argument representing the object.

Classes themselves are objects.

Most built-in operators with special syntax (arithmetic operators, subscripting etc.)
can be redefined for class instances.

## 9.1. A Word About Names and Objects

Objects have individuality, and multiple names (in multiple scopes) can be bound to the same object.
This is known as aliasing in other languages.

Aliases behave like pointers in some respects, and this allows kind of passing by reference in Python.

## 9.2. Python Scopes and Namespaces

A **namespace** is a mapping from names to objects.

The set of built-in names, the global names in a module,
the local names in a function invocation, the set of attributes
of an object form a namespace.

There is absolutely no relation between names in different namespaces

The module's attributes and the global names defined in the module share the same namespace.

Namespaces are created at different moments and have different lifetimes.

A **scope** is a textual region of a Python program where a namespace is directly accessible.
**Directly accessible** means that an unqualified reference to a name attempts to find the name in the namespace.

At any time during execution, there are 3 or 4 nested scopes whose namespaces are directly accessible:

- the innermost scope, which is searched first, contains the local names

- the scopes of any enclosing functions, which are searched starting with the nearest enclosing scope, contain non-local, but also non-global names

- the next-to-last scope contains the current module’s global names

- the outermost scope (searched last) is the namespace containing built-in names

`nonlocal` and `global` allow you to tell Python that you want to rebind variable outside of the innermost scope.

## 9.3 A First Look at Classes

### Class Definition Syntax

```python
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

When a class definition is left normally (via the end), a class object is created.

### Class Objects

Class objects support two kinds of operations:

- **attribute references**

  ```python
  ClassName.AttributeName
  ```

  Valid attribute names are all the names that were in the
  class's namespace when the class object was created.

- **instantiation**

  ```python
  var = ClassName([(args)*]) # args will be passed to __init__()
  ```

  The instantiation operation creates an empty object but if
  a class defined an `__init__()`, then this method auomatically
  will be invoked for the newly create class instance.

### Instance Objects

There are two kinds of valid attribute names:

- **data attributes**

  Data attributes spring into existence when they are first assigned to.

- **methods**

  A **method** is a function that "belongs to" an object.

  All attributes of a class that are function objects define corresponding methods of its instances.

### Method Objects

The **method object** is an abstract object
created by packing (pointers to)
the instance object and the function object.

The method object's argument list constructed from the instance object(getting rid of `self`).

The `variable` is a instance of `ClassName`.
`ClassName.Function(variable)` = `variable.Function()`

### Class and Instance Variables

**Instance variables** are for data unique to each instance.
**Class variables** are for attributes and methods shared by all instances of the class.

## 4 Random Remarks

1. If the same attribute name occurs in both an instance and in a class, then attribute lookup prioritizes the instance.

2. Nothing in Python makes it possible to enforce data hiding — it is all based upon convention

3. Clients should use data attributes with care to not rewrite a method with some value.

4. There is no shorthand for referencing data attributes (or other methods!) from within methods.

5. By convention, the first argument of a method is called `self`.

6. Any function object that is a class attribute defines a method for instances of that class.

7. Each value is an object, and therefore has a class (also called its type). It is stored as `object.__class__`.

## 5 Inheritance

```python
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```

When the class object is constructed, the base class is remembered.

If a requested attribute is not found in the class, the search proceeds to look in the base class.

A method of a base class that calls another method defined in the
same base class may end up calling a method of a derived class that overrides it.

A way to call the base class method directly:

```python
BaseClassName.methodname(self, arguments)
```

Python has two built-in functions that work with inheritance:

- `isinstance(obj, ClassName)`

  return `True` when `obj.__class__` is `ClassName` or
  some class derived from `ClassName`.

- `issubclass(ClassName1, ClassName2)`

  return `True` when `ClassName1` is a subclass of `ClassName2`

### Multiple Inheritance

```python
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

Dynamic ordering is necessary because all cases of multiple inheritance exhibit one or more diamond relationships.

The list of the ancestors of a class C, including the class itself,
ordered from the nearest ancestor to the furthest,
is called the class precedence list or the **linearization** of C.

The **Method Resolution Order (MRO)** is the set of rules that construct the linearization.

> The linearization of C is the sum of C plus the merge of
> the linearizations of the parents and the list of the parents.

The merge is computed according to the following prescription:

> The **head** of the list is its first element.
> The **tail** is the rest of the list.
>
> Take the head of the first list;
> if this head is not in the tail of any of the other lists,
> then add it to the linearization of C and remove it from the lists in the merge,
> otherwise look at the head of the next list and take it, if it is a good head.
> Then repeat the operation until all the class are removed or it is impossible to find good heads.
> In this case, it is impossible to construct the merge, Python 2.3 will raise an exception.

The MRO determines the resolution order of attributes and methods.

The good MRO should respects local precedence ordering and monotonicity and
your good class hierarchy also should respect this dudes.

A **monotonic** MRO is meaning that a class can be subclassed without
affecting the precedence order of its parents.

A breaking of **local precedence** ordering means that
the order in the local precedence list isn't preserved in the linearization.

## 6 Metaclasses

**Metaclasses** are classes' classes due to the fact that classes are objects.

By default, classes are constructed using `type(name, bases, namespace)`, which is like default metaclass.
Where:

- `name`: name of the class
- `bases`: tuple of the parent class (for inheritance, can be empty)
- `namespace`: dictionary containing attributes names and values

Syntax to using custom metaclasses in Python 3:

```python
class ClassName(metaclass=MetaclassName, kwarg1=value1, kwargN=valueN):
    ...
```

**Metaclasses**:

- intercept a class creation
- modify the class
- return the modified class

You can hook on `__new__`, `__init__` and `__call__`.

### ABC

Python provide `abc.py` module for defining ABCs.

The **ABC** stands for **abstract base class**.

This module provides the metaclass `ABCMeta` for defining ABCs and
a helper class `ABC` to alternatively define ABCs through inheritance.

The `@abstractmethod` is very useful due to the fact that it can only be used in classes
that have `ABCMeta` (or one of their bases), and all classes that have a method defined
with this decorator cannot be instanced.

The ABC is the best way for duck-typing.

The `__subclasshook__` check whether subclass is considered a subclass of this ABC.
Must be defined as a class method.

The `ABC.register(subclass)` register subclass as a "virtual subclass" of this ABC.

## 7 Dataclasses

The **dataclasses** is a Python module that provides a decorator
and functions for automatically adding generated special methods
such as `__init__()` and `__repr__()` to user-defined classes.

It is very useful for classes that only have variables and doesn't have methods.
The module allows you to write classes like C-structures.
