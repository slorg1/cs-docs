---
title: Code boo-boos... no mo'
---
# {{page.title}}
We all make mistakes, the key is not to get parallelize by them and to look for ways to promote healthy habits: so they either do not happen again or they are easy to fix when they happen.

Is that not the dream :sparkles:? But who is _actually_ living it?

I will not pretend to know it all (I do not), I only share here a few fundamental tips that ~~I have learnt the hard way~~ helped me over the years.


# Keep it DRY...
...and it shall not get infected :stuck_out_tongue:

DRY is an old enough concept, it stands _Do not Repeat Yourself_, more [here](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). We have all heard it one way or the other.

It is easy enough to centralize your code (in classes, objects, methods, functions etc.) and reuse it but is that _really_ the essence of DRY?

Building upon the basis of [KISS](#KISS), it is easy to understand that to keep our projects simple, it helps to keep them small (or at least not with scattered different versions of the same code).

DRY is a far reaching mindset, not a small set of precise rules. It is easy to apply it at the low/descreet level. We all recognize a simple pattern when it happens in a piece of code (method) or across a couple of pieces of code. What is harder and when it really shines is when applying it as a systemic thought process: whether across large chunk of software as a systematic architecture or in business or in your day to day personal life.

We will discuss here how to apply it at the descreet level and learn to recognize gains and gotchas to understand the Software Engineering mindset. Building upon our SE understanding, we will skip to the 10k-foot view to touch upon the generic mindset.

## Making Methods
### A grand classic: utility classes
As an example, we can think of string manipulation. How many time do we write the same or similar code at API boundaries? We all know the best practices and we cleanse data like we mean it.
Instead of replicating these lines of code:

```python
id = str(my_value).lower().strip() # ... whatever else you want to do
```
We will use a library or framework that will do a solid 80%+ of the work. Then, for the remaining 20%, applying our pragmatic KISS learnings. We will write the code as we go and centralize the code as we start writing it multiple times (3+ as a good rule of thumb) and we gain an understanding of the pattern.

```python
class StringUtils:
    """
        Utility centralizing common string manipulations.
    """

    @staticmethod
    def to_stripped_lowercase_string(input_value):
        """
            Given any input value, returns a string lowercased and without 
            whitespaces at the beginning and at the end.
        """
        return str(input_value).lower().strip() 
```
Now, we do quick search through the project for anything that resembles that e.g. grep for `.lower().strip()` and replace it nicely with `StringUtils.input_value(my_value)`.

 :heartpulse: Hurray! Look at us! We centralized code and we did not repeat ourself!

But wait, this will turn any value into a string... right?

:scream_cat:**GOTCHA included**:scream_cat:
You will notice that in our specific case, we made a key assumption and we passed it on to the utility class:
we could assume (or know) that `my_value` could not be `None`.

So, what? It works! No, crash. :see_no_evil:

True, the code will work and will certainly not crash as we force-convert the value to a string: `.lower().strip()` will always work against a string: peachy!

:point_right: It also means that `None` will turn to `"None"` *ooops*  :broken_heart:

You could argue 2 things:
- [x] This is on purpose: we are all set!
- [x] This was an original bug and we can fix it in 1 place and everyone gets the benefit   :revolving_hearts:
{: style='list-style-type: none'}

But, wait? What should the method return in case of `None`?
Well, that entirely depends on your application and the context in which it is used.

Here are 2 examples highlighting key assumptions:

Honour the `None`
```python
    ...

    @staticmethod
    def to_stripped_lowercase_string(input_value):
        """
            Given any input value, returns a string lowercased and without 
            whitespaces at the beginning and at the end.
        """
        return str(input_value).lower().strip() if input_value != "" else ""
```

Always returns a string
```python
    ...

    @staticmethod
    def to_stripped_lowercase_string(input_value):
        """
            Given any input value, returns a string lowercased and without 
            whitespaces at the beginning and at the end.
            Returns `None` is the input is `None`
        """
        return str(input_value).lower().strip() if input_value is not None else None
```
In any case. what did we gain:
1. a centralized way of converting data
2. a centralized way of changing how the data is converted
3. we **abstracted** the way data is converted. Now we do not think about **HOW** to lowercase and strip whitespace from a string, we think about **WHAT** what want to do: the transformation

> ### Takeaways
> - When centralizing code, we abstract the details of implementation (**HOW**) and focus on the outcome (**WHAT**)
> - Centralizing code allows us to share bugs and to share code fixes :smiley:
> - Be careful, centralizing code also centralizes the assumptions

### "Flushing the buffer"
This is a technic that I call "flushing the buffer".
It is used when iterating over iterables, accumulating data and operations needs to happen at each boundaries.

```python
my_dict = {}
values = (("a", 1,), ("a", 2), ("a", 30,), ("b", 32,), ("b", 40),)
current_label = None

for label, v, in values:

    if current_label != label:
        # for the example we assume that the labels cannot be None
        if current_label != None: 
            my_dict[current_label] = sum_of_values
        current_label = label
        sum_of_values = 0

    sum_of_values += v

# we need to deal with "b" now...
if current_label != None:
    my_dict[current_label] = sum_of_values
```
But that is not DRY: we repeated ourselves.
How about now?
```python
import itertools

# same initializations cut for conciseness

for label, v, in itertools.chain(
                             values, 
                             (None, 0,), # flush the buffer
                             ):

    if current_label != label:
        if current_label != None:
            my_dict[current_label] = sum_of_values
        current_label = label
        sum_of_values = 0 # this is our buffer

    sum_of_values += v
```
While this allows for no code duplication, it is a little _harder_ to read than the previous version.
> ### Takeaways
> - Centralizing code can make it harder to read
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMTY0NjIzOTcsMTUzMDc0MjY0MSwtOD
UxNTExMTQ2LC0xODA1MjgxODQ0LC04MzcwNTM1MzUsNTkyMDMy
MjkwLC0yNzYxOTk2MjksLTEwNjQyNjMxNywtMTY2MzQ4NjQwNS
wtMjc2NDg2MDc1XX0=
-->