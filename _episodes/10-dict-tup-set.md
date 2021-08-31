---
title: Beyond Lists - Tuples, Sets and Dictionaries
teaching: 30
exercises: 10
questions:
- "What other methods can I use to store information?"
- "How can I more efficiently summarise and recall the stored data?"
objectives:
- "Explain what the difference is between a tuple and a list."
- "Be able to create tuples from scratch and from zipping lists."
- "Use `zip()` to create temporary iterators"
- "Understand what a set is and how to define it."
- "Understand what a dictionary is and how to define it."
- "Be able to modify sets and dictionaries."
keypoints:
- "`(value1, value2, value3, ...)` - using parentheses - creates a tuple."
- "Tuples are _iterables_, like lists, and may be indexed and sliced in the same way."
- "Tuples are immutable (their values may not be changed in place) but the values themselves may be mutable (e.g. you can change the contents of a list that is given as a value)."
- "`zip()` can be used to iterate through pairs or higher multiples of values in separate lists. The iterator produced can only be run through once unless converted to a list or tuple."
- "Sets contain the unique and unordered elements of an iterable, created using `set()`. They cannot be indexed or sliced."
- "Dictionaries contain key/value pairs, defined using `{key1:value1, key2:value2, ....}` or `dict()` with key/value pairs given as a list of tuples."
- "Dictionaries can be used to summarise and access information in a more intuitive way than a simple list of values."
---

Lists are containers that can store many values of different types. Other types of container exist, which have different properties. The three main types are _tuples_, _sets_ and _dictionaries_.

## Tuples

Tuples are __immutable__ versions of lists, which can be defined (and are printed) using parentheses rather than square braces (to distinguish them from lists). E.g:

~~~
t_1 = (1, 2, 3, 'abc') # We can define a tuple using parentheses
t_2 = 5, 6  # Or without, using a comma separator
print(t_1,t_2)
~~~
{: .language-python}

~~~
(1, 2, 3, 'abc')
(5, 6)
~~~
{:.output}

We can use indexing and slicing with tuples in the same way as for lists. However, since they are immutable, we cannot assign a new value to an item in a list without redefining the whole list again:

~~~
t_1[1] = 8
~~~
{: .language-python}

~~~
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-13-ac38664d74d9> in <module>
----> 1 t_1[1] = 8

TypeError: 'tuple' object does not support item assignment
~~~
{:.output}

To be more specific, that means that the values of items contained in a tuple cannot change, but it is important to note that if the item is itself a mutable object, the values of that object may still change.  E.g.:

~~~
a = [2, 5, 'apples'] # define a list (which is mutable)
t_3 = [2, 6, a]
print(t_3)
print(t_3[2]) # We can look at a specific item (but not change it)
print(t_3[2][2]) # We can also use nested indices to look at an item in an item
~~~
{: .language-python}

~~~
[2, 6, [2, 5, 'apples']]
[2, 5, 'apples']
apples
~~~
{:.output}

~~~
a[2] = 'pears' # This will work
print(t_3)

t_3[2] = [2, 5, 'apples'] # This won't work to change it back!
~~~
{: .language-python}

~~~
(2, 6, [2, 5, 'pears'])
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-36-34a6582106f0> in <module>
      2 print(t_3)
      3 
----> 4 t_3[2] = [2, 5, 'apples'] # This won't work to change it back!

TypeError: 'tuple' object does not support item assignment
~~~
{:.output}

Tuples are clearly less flexible than lists, but their stability can also be useful in a program. E.g. they are used to define important and unchanging quantities such as the dimensions of pre-defined arrays and lists.

## The zip function

Sometimes it's useful to combine together containers such as lists (or other _iterables_) into corresponding tuples which can themselves be iterated through. This can be done using Python's `zip()` function, which 'zips' together the different containers to produce an _iterator_ of tuples. An _iterator_ is itself not a container such as a list - it will produce it's tuples on demand but can only be iterated through _once_. For example:

~~~
l_1 = ['a', 'b', 'c']
l_2 = [0 , 1, 2]

result = zip(l_1, l_2)
print(result)

print("The first time...")
for pair in result:
    print(pair)
    
print("The second time...")
for pair in result:
    print(pair)
~~~
{: .language-python}

~~~
<zip object at 0x112331eb0>
The first time...
('a', 0)
('b', 1)
('c', 2)
The second time...
~~~
{: .output}

The second time does not work! Thus, `zip()` on its own is mainly useful when a one-off iterator is needed which combines results from two or more collections. For example, we could skip the creation of `result` and simply use:

~~~
for pair in zip(l_1,l_2):
    print(pair)
~~~
{: .language-python}

~~~
('a', 0)
('b', 1)
('c', 2)
~~~
{: .output}

> ## `zip()` in Python 3 vs. Python 2
> There are not too many differences between the outward behaviour of Python 2 and Python 3 - the use of the `print` function is one of them, but the output of `zip()` is also different. In Python 2 `zip()` provides a list of tuples rather than an iterator. You should bear this in mind in case you wish to run (using Python 3) any legacy code that is written in Python 2.
{: .callout}

> ## Converting a zip object into something more permanent
>Zip the lists `l_1` and `l_2` defined above and convert the resulting zip object into a list or tuple which can be used repeatedly.
>
>> ## Solution
>>~~~
>>zipped_list = list(zip(l_1, l_2))
>>zipped_tuple = tuple(zip(l_1, l_2))
>>print(zipped_list)
>>print(zipped_tuple)
>>~~~
>>{: .language-python}
>>
>>~~~
>>[('a', 0), ('b', 1), ('c', 2)]
>>(('a', 0), ('b', 1), ('c', 2))
>>~~~
>>{: .output}
>{: .solution}
{: .challenge}


## Sets

Sets are containers of elements which are _unique_ and _unordered_. They have many properties which are analogous to mathematical sets, which we will not go into details about here. A set is especially useful as a way to collect together all unique elements from another collection. 

We can define a set from an iterable such as a list, string or tuple. For example, imagine we have observed a number of stars and want to define the set of all unique stellar types observed:

~~~
s_1 = set(['G5','G3','O2','B2','G3','F5','B2']) # Define using the set() command
print(s_1) # The set contains the unique items in the list
~~~
{: .language-python}

~~~
{'F5', 'G3', 'B2', 'O2', 'G5'}
~~~
{: .output}

The set is printed using curly braces to distinguish it from a list or tuple. Note that the ordering has also changed - it does not matter for a set (note also that unlike a list or tuple, indexing/slicing won't work on a set which is not _subscriptable_).

Sets are themselves mutable and can be modified, but the elements contained in the set must be of immutable type. E.g., imagine that we discover one of the G3 stars is in a binary with a K2 star and we decide to represent the binary by putting them together in a list:

~~~
s_2 = set(['G5','G3','O2','B2',['G3','K2'],'F5','B2'])
~~~
{: .language-python}

~~~
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-43-8b76633952a4> in <module>
----> 1 s_2 = set(['G5','G3','O2','B2',['G3','K2'],'F5','B2'])
      2 print(s_2)

TypeError: unhashable type: 'list'
~~~
{: .output}

Instead, we can use a tuple to represent the binary system:

~~~
s_1 = set(['G5','G3','O2','B2',('G3','K2'),'F5','B2'])
~~~
{: .language-python}

~~~
{'F5', ('G3', 'K2'), 'G3', 'B2', 'O2', 'G5'}
~~~
{: .output}

We can add or remove a new element to a set using the `add()` and `remove()` methods:

~~~
s_1.add('M5')
print(s_2)
s_1.remove('B2')
print(s_2)
~~~
{: .language-python}

~~~
{'F5', 'M5', ('G3', 'K2'), 'G3', 'B2', 'O2', 'G5'}
{'F5', 'M5', ('G3', 'K2'), 'G3', 'O2', 'G5'}
~~~
{: .output}

## Dictionaries

Dictionaries (formally, objects of type `dict` ) are similar to lists except that they are indexed using __keys__.  We can define a dictionary using curly braces `{}` or the `dict()` function to enclose a set of key/value pairs. For example, let's say we want to describe some properties of a star, Vega, including its distance in pc, spectral type and mass and luminosity in Solar units:

~~~
Vega = {'Dist_pc':7.68, 'Spec_Type':'A0Va', 'Mass_Msol':2.14, 'Lum_Lsol':40.12}

print(Vega.keys()) # Remind ourselves of the key names
print(Vega.values()) # Print a list of the values
print(Vega['Spec_Type']) # Look at a specific value
~~~
{: .language-python}

~~~
dict_keys(['Dist_pc', 'Spec_Type', 'Mass_Msol', 'Lum_Lsol'])
dict_values([7.68, 'A0Va', 2.14, 40.12])
A0Va
~~~
{: .output}

Note that the keys do not have to be in the form of strings - they must be immutable so can be floats, integers or tuples too, depending on what is convenient.

Dictionaries offer a powerful way to collect information, for example we can define a dictionary for another star. Here we do this using the `dict()` function. The key/value pairs are passed to it as a list of tuples (note that the order doesn't really matter as long as we know the keys, so it is changed here):

~~~
Arcturus = dict([('Mass_Msol',1.08), ('Lum_Lsol',170), ('Dist_pc',11.26), ('Spec_Type','K0III')])
~~~
{:.language-python}

We can also add a new key/value pair:

~~~
Vega['Name'] = 'Vega'
Arcturus['Name'] = 'Arcturus'
~~~
{:.language-python}

Now we can try something fancy:

~~~
stars = [Vega, Arcturus]

for star in stars:
    print(star['Name'],"is a star of type",star['Spec_Type'],", lying at a distance of",star['Dist_pc']," pc.")
~~~
{:.language-python}

~~~
Vega is a star of type A0Va , lying at a distance of 7.68  pc.
Arcturus is a star of type K0III , lying at a distance of 11.26  pc.
~~~

> ## Dictionaries from lists
>For the star Deneb you have two lists, one contains the keywords and the other the corresponding values:
>~~~ 
>keys = ['Name', 'Dist_pc', 'Spec_Type', 'Mass_Msol', 'Lum_Lsol']
>vals = ['Deneb', 802, 'A2Ia', 19, 1.96e5]
>~~~
>{: .language-python}
Convert these to a dictionary in the most painless way possible. Then, loop over the three stars to output the mass-to-light-ratio of each (mass in Solar units divided by luminosity in Solar units):
>
>> ## Solution
>>~~~
>>Deneb = dict(zip(keys,vals))
>>stars = [Vega, Arcturus, Deneb]
>>for star in stars:
>>    print("The mass-to-light ratio for",star['Name'],"is",star['Mass_Msol']/star['Lum_Lsol']) 
>>~~~
>>{: .language-python}
>>
>>~~~
>>The mass-to-light ratio for Vega is 0.053339980059820546
>>The mass-to-light ratio for Arcturus is 0.006352941176470589
>>The mass-to-light ratio for Deneb is 9.693877551020408e-05
>>~~~
>>{: .output}
>{: .solution}
{: .challenge}


{% include links.md %}
