---
layout: post
title:  "Python Collections and Tricks"
date:   2017-5-22 14:13:22
categories:
tags:
image: /assets/article_images/2017-5-22-Python-Collections/campus.JPG
image2: /assets/article_images/2017-5-22-Python-Collections/campus.JPG
---

# Being pythonic

> Introduction of Python unique data structures of high performance and summary of pythonic ways of coding

## Python collections
> Here is [collection ducumentation](https://docs.python.org/2.7/library/collections.html#collections.ChainMap)

Besides ```list```, ```dictionary```, ```tuple```, python provides some high-performance container datatypes in the module ```collections```, which I find extremely useful when dealing with hashtable or stack problems. You can always implement these by using basic ```list``` or ```dictionary```, but it really eases your life if you know their existence and are familiar with their APIs beforehand.

### deque (pronounced "deck", not "D-queue")
Deque is basically a double-ended-queue structure and supports efficient operations like appends and pops from both sides. It has totally same operations as ```list```, but runs in O(1) in operations like pop(0), appendleft(x).

**To initialize**: It takes a iterable and a maxlen. From official document:

>If maxlen is not specified or is None, deques may grow to an arbitrary length. Otherwise, the deque is bounded to the specified maximum length. Once a bounded length deque is full, when new items are added, a corresponding number of items are discarded from the opposite end.

{% highlight python %}
deque = collections.deque([],maxlen=3)
for i in range(4):
    deque.append(i)
print deque # deque([1, 2, 3], maxlen=3)    
{% endhighlight %}

**List of methods from official document**:

- append(x): Add x to the right side of the deque.

- appendleft(x): Add x to the left side of the deque.

- clear():Remove all elements from the deque leaving it with length 0.

- count(x): Count the number of deque elements equal to x.

- extend(iterable): Extend the right side of the deque by appending elements from the iterable argument.

  - if you extend a string, it will treat every char as an element
{% highlight python %}
for i in range(4):
    deque.append(i)
deque.extend("[4,5,6]")
print deque # deque([0, 1, 2, 3, '[', '4', ',', '5', ',', '6', ']'])
{% endhighlight %}

- extendleft(iterable):
Extend the left side of the deque by appending elements from iterable. Note, the series of left appends results in reversing the order of elements in the iterable argument.

- pop(): Remove and return an element from the right side of the deque. If no elements are present, raises an IndexError.

- popleft(): Remove and return an element from the left side of the deque. If no elements are present, raises an IndexError.

- remove(value): Removed the first occurrence of value. If not found, raises a ValueError.

- reverse(): Reverse the elements of the deque in-place and then return None.

- rotate(n): Rotate the deque n steps to the right. If n is negative, rotate to the left. Rotating one step to the right is equivalent to: d.appendleft(d.pop()).

### OrderedDict
If you want to keep the order of the items you insert into a dictionary, ```OrderedDict``` is the one you need.

 {% highlight python %}
>>> # regular unsorted dictionary
>>> d = {'banana': 3, 'apple': 4, 'pear': 1, 'orange': 2}

>>> # dictionary sorted by key
>>> OrderedDict(sorted(d.items(), key=lambda t: t[0]))
OrderedDict([('apple', 4), ('banana', 3), ('orange', 2), ('pear', 1)])

>>> # dictionary sorted by value
>>> OrderedDict(sorted(d.items(), key=lambda t: t[1]))
OrderedDict([('pear', 1), ('orange', 2), ('banana', 3), ('apple', 4)])

>>> # dictionary sorted by length of the key string
>>> OrderedDict(sorted(d.items(), key=lambda t: len(t[0])))
OrderedDict([('pear', 1), ('apple', 4), ('orange', 2), ('banana', 3)])
{% endhighlight %}

### defaultdict
This datatype provides a factory function to store customized key-value pairs into dictionaries.
From doc:

It is easy to group a sequence of key-value pairs into a dictionary of lists:
{% highlight python %}
>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
>>> d = defaultdict(list)
>>> for k, v in s:
...     d[k].append(v)
...
>>> d.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
{% endhighlight %}

This technique is simpler and faster than an equivalent technique using dict.setdefault():

{% highlight python %}
>>> d = {}
>>> for k, v in s:
...     d.setdefault(k, []).append(v)
...
>>> d.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
{% endhighlight %}




## Tricks
(to be filled)
### sorted()

### enumerate()

### zip()

###