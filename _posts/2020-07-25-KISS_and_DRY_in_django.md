---
title: Code booboos... no more
---
# {{page.title}}
We all make mistakes and the key is not to worry about them: it is to promote healthy habits so they either do not happen again or they are easy to fix when they happen.

Is that not the dream :sparkles:? But who is _actually_ living it?

I will not pretend to know it all (I do not), I only share here a few fundamental tips that I have ~~learnt the hard way~~ helped me over the years.

# KISS it better

# Keep it DRY...
...and it shall not get infected :stuck_out_tongue:

DRY is an old enough concept. It stands _Do not Repeat Yourself_ and we have all heard it one way or the other. It is easy to centralize your code in classes, objects, methods, functions etc. and reuse it but is that _really_ the essence of DRY?
Building upon the basis of [KISS](#KISS) it is easy to understand that to keep our projects simple it helps to keep them small (or at least not ...)
DRY is more of a mindset than a precise set of rule. It is easy to isolate it at the low level. We call all recognize a simple pattern when it happens in a piece of code (method) or across a couple of pieces of code.
##
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
True, the code will work and will certainly not crash as we force-convert the value as a string. What this means is that `.lower().strip()` will always work against a string: peachy!

:point_right: It also means that `None` will turn to `"None"` *ooops*  :broken_heart:

You could argue 2 things:
- [x] This is on purpose: we are all set!
- [x] This was an original bug and we can fix it in 1 place and everyone gets the benefit   :revolving_hearts:
{: style='list-style-type: none'}

In any case. what did we gain:
1. a centralized way of converting data
2. we abstracted the way data is converted: now we do not think about **how** to lowercase and strip whitespace from a string, we think about the transformation
3. a centralized way of changing how the data is converted

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3NjQ4NjA3NV19
-->