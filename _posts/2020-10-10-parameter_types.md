---
title: To be or to be... the existential crisis of the parameters
---
# {{page.title}}

What's the difference between these declarations?

2 in Python:
```
    def create_new_user(first_name, last_name, avatar=None,):
        ...
```
or
```
    def create_new_user(first_name=None, last_name=None, avatar=None,):
        ...
```

2 in TypeScript:
```
interface User{
    firstName: string
    lastname: string
    avatar?
}
```
or
```
interface User{
    firstName: string
    lastname: string
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

# Wait a second... parameter or argument?
You may have seen these 2 concepts used interchangeably in the past. They are technically expressing 2 different "states" in the life-cycle of the same "thing".

The parameter is the name of the variable set at function or method declaration/definition time.

```
def my_function(parameter_1, parameter_2):
    ...
```

The argument is the value that you pass the function/method at invocation time.

```
my_function(
    "argument 1",
    "argument 2",
):
    ...
```

# Kinds of parameters
The kinds of parameters covered by this post:
* positional parameter: the parameter expects the argument to be given at the matching position
* keyword parameter: the parameter expects the argument to be given using the matching key
* optional/default parameter: the parameter expects the argument to be given using the matching key or omitted

At a high-level, the other kinds of parameters are semantic declension of this subset.

In Python:
```
my_function(
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
```
interface Props{
    kw_param_1: string
    kw_param_2: string
    optional_param_1?: string
}
```
In the same way in TS as a function
```
function buildName(
    pos_param_1: string,
    pos_param_2: string,
    default_param_1=1,
    optional_param_1?: string,
) {
    // nothing to see here
}
```

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

In context, it meant that optimizing for execution is important but it is key to remember that code is read by other humans and need to remain human readible.

# Nota __very__ Bene
This post offers a stance on the semantic significant of the different kinds of parameters available in various languages (the same logic applies to members in JS/TS as the same type declination exists).

This stance reflects my personal approach to the matter.
