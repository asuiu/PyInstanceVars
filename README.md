PyInstanceVars
==============

A function decorator that automatically creates instance variables from function arguments. 

Arguments can be omitted by adding them to the 'omit' list argument of the decorator.
Names are retained on a one-to-one basis (i.e '_arg' -> 'self._arg'). Passing
arguments as raw literals, using a keyword, or as defaults all work. If *args and/or
**kwargs are used by the decorated function, they are not processed and must be handled
explicitly. 

Basic Usage
===========

The simplest way to explain how to use it is with a quick code example. Notice how 'arg2_' is omitted
since it is in the 'omit' list. I created 'arg2' to show that you can easily mix and match auto 
initialization with explicit intialization (which is required for any non-trivial assignment or if
further testing of the argument is required).

```python
>>> from instancevars import *
>>> class TestMe(object):
...     @instancevars(omit=['arg2_'])
...     def __init__(self, _arg1, arg2_, arg3='test'):
...             self.arg2 = arg2_ + 1
...
>>> testme = TestMe(1, 2)
>>> testme._arg1
1
>>> testme.arg2_
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'TestMe' object has no attribute 'arg2_'
>>> testme.arg2
3
>>> testme.arg3
'test'
>>>
```

Why?
====

Because Python initializer functions can get lengthy doing nothing more than one-to-one variable assignment.
Languages such as Scala already have the ability to convert arguments to instance variables using 'val' or 'var'.
Given Python's reputation of being succinct and terse it seems like this should be builtin. Some might believe
this to be 'unpythonic', but I would respectfully disagree. We've listed a decorator and argument names denoting
our intent, so IMHO we've been plenty 'explicit'. Still, I wouldn't recommend using this unless you have several
initializer arguments.

Requirements
============

It was tested under both Python 2.7.x and Python 3.3.x.

There are no library dependencies other than the standard library.

Performance
===========

Not as good as I would like. 

The included rudimentary benchmark indicates 4-5X worse than explicit initialization.
For many scenarios this is probably fine, but there might be some cases where this is just too much overhead.

Credits
=======

The code was originally based on a great comment and snippet from StackOverflow (http://stackoverflow.com/questions/1389180/python-automatically-initialize-instance-variables).
