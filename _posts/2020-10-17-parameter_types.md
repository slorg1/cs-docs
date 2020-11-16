---
title: To be or ~~not~~ how to be... the existential crisis of parameters
---
# {{page.title}}

By the end of the article, you will be able to tell the difference between these declarations and when to use which with discernment.

Python:
```python
def create_new_user(first_name, last_name, avatar=None,):
    ...
```
vs
```python
def create_new_user(first_name=None, last_name=None, avatar=None,):
    ...
```

TypeScript:
```typescript
interface User{
    firstName: string
    lastName: string
    avatar?
}
```
vs
```typescript
interface User{
    firstName: string
    lastName: string
    avatar: string|null
}
```

<div  style="width:316px;margin-left: auto; margin-right: auto">
    <p style="margin-left: auto; margin-right: auto; text-align: center">"Forget Everything You Think You Know" - Ancient One</p>
    <img src="https://media.giphy.com/media/3o6Zt3XF7QywMoCG08/giphy.gif"/>
    <a
        style="font-size:0.7rem"
        href="https://giphy.com/gifs/mistressagency-3o6Zt3XF7QywMoCG08">
        via GIPHY
    </a>
</div>

# A new understanding from the ground up in 2s

## Kinds of parameters
The kinds of parameters discussed in this post are:
* positional parameter: the parameter expects the argument to be given at the matching position
* keyword parameter: the parameter expects the argument to be given using the matching key
* optional/default parameter: the parameter expects the argument to be given using the matching key or omitted

I am glossing over the other kinds of parameters as at a high-level, they are declension of this subset applied to different use cases.

In Python:
```python
def my_function(
    pos_param_1,
    pos_param_2,
    *,
    kw_param_1,
    kw_param_2,
    optional_param_1="1",
    default_param_1=1,
):
    ...
```

In the same way in TS as an object, named parameter (React style function signature)
```typescript
interface Props{
    kw_param_1: string
    kw_param_2: string
    optional_param_1?: string
}
```
In the same way in TS as a function
```typescript
function myFunction(
    pos_param_1: string,
    pos_param_2: string,
    default_param_1=1,
    optional_param_1?: string,
) {
    // nothing to see here
}
```

## Wait a second... parameter or argument?
You may have seen these 2 concepts used interchangeably in the past. They technically express 2 different "states" in the life-cycle of the same "thing".

The parameter is the name of the variable at function or method declaration/definition time.

```python
def my_function(parameter_1, parameter_2):
    ...
```

The argument is the value (or reference) that you pass the function/method at invocation time.

```python
my_function(
    "argument 1",
    "argument 2",
)
```

## What do I mean by semantic meaning?

The semantic meaning is the meaning (from Greek _sēmantikos_ 'significant') "something" has based on its relationship with others. The dictionary reference for the definition is [here](https://www.merriam-webster.com/dictionary/semantics).

The salient point here is: what does the declaration of a variable mean beyond the syntactic and computational correctness?

***Ready?***
<div  style="width:60%;margin-left: auto; margin-right: auto">
    <p style="margin-left: auto; margin-right: auto; text-align: center">"You have to let it all go. fear, doubt, and disbelief. Free your mind." - Morpheus</p>
    <img src="https://media.giphy.com/media/nb8FKHnLX6BiM/giphy.gif"/>
    <a
        style="font-size:0.7rem"
        href="https://giphy.com/gifs/hollywoodsuite-film-matrix-the-nb8FKHnLX6BiM">
        via GIPHY
    </a>
</div>


# Semantic meaning of parameters

## Why does it even matter?

A thought that was shared to me by Tony Deigh (CTO of Jobcase) that stuck in my mind: "Code is read more than it is executed".

I keep it as a reminder that correctness (it does what it is supposed to do) and optimization for execution are important and it is also key to remember that code is read by other humans (mostly). As a result, it needs to remain human-readable.

## Breaking it down

Taking this Python as an exhaustive example (the TS equivalent is the same):

```python
def my_function(
    pos_param_1,
    *,
    kw_param_1,
    optional_param_1="1",
    default_param_1=1,
):
    ...
```
Let's break down what the developer of this function is telling:
* `pos_param_1`: this parameter is **required** and its placement in the sequence in the parameter's list is **likely important**
* `kw_param_1`: this parameter is **required** and its placement in the sequence in the parameter's list is **meaningless**
* `optional_param_1`: this parameter is **optional**, its placement in the sequence in the parameter's list is **meaningless** and the function will work just as well if it is not supplied
* `default_param_1`: this parameter is **optional**, its placement in the sequence in the parameter's list is **meaningless** and the function will work just as well if it is not supplied

**NB:**
* the optional and default parameter are **semantically identical**. i.e. to the reader of the code, they convey the same information and they behave the same way
* semantically, whether the optional and default parameter are defaulted to `None` or a different value (here respectively `"1"` and `1`) is identical. `None` is a value and as such, it does not convey a "requirement". i.e. that the default of a parameter is `None` does not mean that the parameter is required and needs a non-`None` value. It means *" `None` is one of the values I can have, all good"*.

Take the TS class (named parameters) example:
```typescript
interface Props{
    kw_param_1: string;
    kw_param_2: string | null;
    optional_param_1?: string;
    optional_param_2: string | undefined;
}
```

Let's break down what the developer of this class is telling us:
* `kw_param_1`: this parameter is **required** and its placement in the sequence in the parameter's list is **meaningless**
* `kw_param_2`: this parameter is **required** and its placement in the sequence in the parameter's list is **meaningless**
* `optional_param_1`: this parameter is **optional** and the function will work just as well if it is not supplied
* `optional_param_2`: this parameter is **optional** and the function will work just as well if it is not supplied

**NB:**
* `optional_param_1` and `optional_param_2` are 2 ways of expressing the same thing.
* `kw_param_2` and `optional_param_2` express different intent: omitted/`undefined` means "I will work just as well if it is not supplied", `null` means "I need a value and the value may be `null` which dictates a specific behaviour".

## Takeaways

The code is certainly instructions for the **machine** but it is also the embodiment of a **developer**'s thoughts and approach to a solution. It is a message left by the author for the future. Whether it is during the code review process, the maintenance process or simply leveraging the existing modules. **WE**, the developers, (including the original author) will have to make sense of the code as fast and as unequivocally as possible the next time it is read. Our best-case scenario is to be able to use or to update the functionality built without having to deep dive into code every time.

Additionally, depending on the programming language used, the semantics meaning of all variables can be used by the interpreter or compiler to optimize the code further.

# Acknowledgement

Before closing on the subject, I would like to bring up the key counter-arguments that I balance constantly in my mind assessing which strategy works best.

The argument goes as follows:

> I use keyword arguments _**systematically**_ to make my code more maintainable

I have seen 2 variations of how it comes to life.

### Some exclusively use it at invocation time and declare the parameters with a clear semantic meaning.
```python
def my_function(
    pos_param_1,
    optional_param_1="1",
    default_param_1=1,
):
    ...

my_function(
    pos_param_1="argument 1",
    optional_param_1="argument 2",
)
```
**pros**
* it is easier to add a parameter:
    * the task of ordering arguments/parameters is no longer applicable
* it is easier to remember the name (and meaning, if properly named) of arguments/parameters.

**con**
* the arguments lose most semantic meaning: without reading the declaration of the function you cannot tell if an argument is needed or not: it hard to distinguish a bug (missing argument) from an intended declaration
* you still need to add a new positional argument for all invocation

### Some exclusively use it at invocation time and declaration time.
```python
def my_function(
    pos_param_1="",
    optional_param_1="1",
    default_param_1=1,
    new_param=None,
):
    ...

my_function(
    pos_param_1="argument 1",
    optional_param_1="argument 2",
)
```
**pros**
* easier to add a parameter:
    * you do not need to add the argument to all invocations
    * the task of ordering arguments/parameters is no longer applicable

**con**
* it is hard (if at all possible) to set sensible defaults
* the code in the function needs to handle the possible combinations (permutations do not matter:stuck_out_tongue_winking_eye:) of supplied vs not supplied arguments: lead to lots of defensive programming or hoping for the best and dealing with consequences
* the arguments lose all semantic meaning: without reading the body of the function you cannot tell if an argument is required or not

Now, you are in the know.
<p align="center">
    <strong>
        Choose your path, do not settle
    </strong>
</p>

# Nota __very__ Bene
This post offers a stance on the semantic significance of the different kinds of parameters available in various languages.<br />
This stance reflects only my personal approach to the matter.
