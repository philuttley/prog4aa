---
title: Making Choices
teaching: 30
exercises: 20
questions:
- "How can my programs do different things based on data values?"
objectives:
- "Write conditional statements including `if`, `elif`, and `else` branches."
- "Correctly evaluate expressions containing `and` and `or`."
- "Trace the execution of unnested conditionals and conditionals inside loops."
keypoints:
- "Use `if condition` to start a conditional statement, `elif condition` to
   provide additional tests, and `else` to provide a default."
- "The bodies of the branches of conditional statements must be indented."
- "Use `==` to test for equality."
- "`X and Y` is only true if both `X` and `Y` are true."
- "`X or Y` is true if either `X` or `Y`, or both, are true."
- "Zero, the empty string, and the empty list are considered false;
   all other numbers, strings, and lists are considered true."
- "`True` and `False` represent truth values."
- "Conditions are tested once, in order."
- "`while` loops can be used to continue executing a loop, dependent on a 
conditional statement."
---

Earlier in our lesson, we discovered something suspicious was going on
in our inflammation data by drawing some plots.
How can we use Python to automatically recognize the different features we saw,
and take a different action for each? In this lesson, we'll learn how to write code that
runs only when certain conditions are true.

## Conditionals

We can ask Python to take different actions, depending on a condition, with an `if` statement:

~~~
num = 37
if num > 100:
    print('greater')
else:
    print('not greater')
print('done')
~~~
{: .language-python}

~~~
not greater
done
~~~
{: .output}

The second line of this code uses the keyword `if` to tell Python that we want to make a choice.
If the test that follows the `if` statement is true,
the body of the `if`
(i.e., the set of lines indented underneath it) is executed, and "greater" is printed.
If the test is false,
the body of the `else` is executed instead, and "not greater" is printed.
Only one or the other is ever executed before continuing on with program execution to print "done":

![A flowchart diagram of the if-else construct that tests if variable num is greater than 100](../fig/python-flowchart-conditional.png)

Conditional statements don't have to include an `else`.
If there isn't one,
Python simply does nothing if the test is false:

~~~
num = 53
print('before conditional...')
if num > 100:
    print(num,' is greater than 100')
print('...after conditional')
~~~
{: .language-python}

~~~
before conditional...
...after conditional
~~~
{: .output}

We can also chain several tests together using `elif`,
which is short for "else if".
The following Python code uses `elif` to print the sign of a number.

~~~
num = -3

if num > 0:
    print(num, 'is positive')
elif num == 0:
    print(num, 'is zero')
else:
    print(num, 'is negative')
~~~
{: .language-python}

~~~
-3 is negative
~~~
{: .output}

Note that to test for equality we use a double equals sign `==`
rather than a single equals sign `=` which is used to assign values.

We can also combine tests using `and` and `or`.
`and` is only true if both parts are true:

~~~
if (1 > 0) and (-1 > 0):
    print('both parts are true')
else:
    print('at least one part is false')
~~~
{: .language-python}

~~~
at least one part is false
~~~
{: .output}

while `or` is true if at least one part is true:

~~~
if (1 < 0) or (-1 < 0):
    print('at least one test is true')
~~~
{: .language-python}

~~~
at least one test is true
~~~
{: .output}

> ## `True` and `False`
> `True` and `False` are special words in Python called `booleans`,
> which represent truth values. A statement such as `1 < 0` returns
> the value `False`, while `-1 < 0` returns the value `True`.
{: .callout}

## Conditions are tested once, in order.

Python steps through the branches of the conditional in order, testing each in turn, so ordering matters.

~~~
grade = 85
if grade >= 70:
    print('grade is C')
elif grade >= 80:
    print('grade is B')
elif grade >= 90:
    print('grade is A')
~~~
{: .language-python}
~~~
grade is C
~~~
{: .output}

The Python interpreter does *not* automatically go back and re-evaluate if values used for a condition change within the conditional statement.

~~~
velocity = 10.0
if velocity > 20.0:
    print('moving too fast')
else:
    print('adjusting velocity')
    velocity = 50.0
~~~
{: .language-python}
~~~
adjusting velocity
~~~
{: .output}

We often use conditionals in a loop to "evolve" the values of variables.

~~~
velocity = 10.0
for i in range(5): # execute the loop 5 times
    print(i, ':', velocity)
    if velocity > 20.0:
        print('moving too fast')
        velocity = velocity - 5.0
    else:
        print('moving too slow')
        velocity = velocity + 10.0
print('final velocity:', velocity)
~~~
{: .language-python}
~~~
0 : 10.0
moving too slow
1 : 20.0
moving too slow
2 : 30.0
moving too fast
3 : 25.0
moving too fast
4 : 20.0
moving too slow
final velocity: 30.0
~~~
{: .output}

> ## Compound Relations Using `and`, `or`, and Parentheses
>
> Just like with arithmetic, you can and should use parentheses whenever there
> is possible ambiguity.  A good general rule is to *always* use parentheses
> when mixing `and` and `or` in the same condition.  That is, instead of:
>
> ~~~
> if mass[i] <= 2 or mass[i] >= 5 and velocity[i] > 20:
> ~~~
> {: .language-python}
>
> write one of these:
>
> ~~~
> if (mass[i] <= 2 or mass[i] >= 5) and velocity[i] > 20:
> if mass[i] <= 2 or (mass[i] >= 5 and velocity[i] > 20):
> ~~~
> {: .language-python}
>
> so it is perfectly clear to a reader (and to Python) what you really mean.
{: .callout}

> ## Tracing Execution
>
> What does this program print?
>
> ~~~
> pressure = 71.9
> if pressure > 50.0:
>     pressure = 25.0
> elif pressure <= 50.0:
>     pressure = 0.0
> print(pressure)
> ~~~
> {: .language-python}
>
> > ## Solution
> >
> > ~~~
> > 25.0
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Trimming Values
>
> Fill in the blanks so that this program creates a new list
> containing zeroes where the original list's values were negative
> and ones where the original list's values were positive.
>
> ~~~
> original = [-1.5, 0.2, 0.4, 0.0, -1.3, 0.4]
> result = ____
> for value in original:
>     if ____:
>         result.append(0)
>     else:
>         ____
> print(result)
> ~~~
> {: .language-python}
>
> ~~~
> [0, 1, 1, 1, 0, 1]
> ~~~
> {: .output}
> > ## Solution
> >
> > ~~~
> > original = [-1.5, 0.2, 0.4, 0.0, -1.3, 0.4]
> > result = []
> > for value in original:
> >     if value<0.0:
> >         result.append(0)
> >     else:
> >         result.append(1)
> > print(result)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## Initializing
>
> Modify this program so that it finds the largest and smallest values in the list
> no matter what the range of values originally is.
>
> ~~~
> values = [...some test data...]
> smallest, largest = None, None
> for v in values:
>     if ____:
>         smallest, largest = v, v
>     ____:
>         smallest = min(____, v)
>         largest = max(____, v)
> print(smallest, largest)
> ~~~
> {: .language-python}
>
> What are the advantages and disadvantages of using this method
> to find the range of the data?
> > ## Solution
> >
> > ~~~
> > values = [-2,1,65,78,-54,-24,100]
> > smallest, largest = None, None
> > for v in values:
> >     if smallest==None and largest==None:
> >         smallest, largest = v, v
> >     else:
> >         smallest = min(smallest, v)
> >         largest = max(largest, v)
> > print(smallest, largest)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Checking our Data

Now that we've seen how conditionals work,
we can use them to check for the suspicious features we saw in our inflammation data.
We are about to use functions provided by the `numpy` module again.
Therefore, if you're working in a new Python session, make sure to load the
module with:

~~~
import numpy
~~~
{: .language-python}

From the first couple of plots, we saw that maximum daily inflammation exhibits
a strange behavior and raises one unit a day.
Wouldn't it be a good idea to detect such behavior and report it as suspicious?
Let's do that!
However, instead of checking every single day of the study, let's merely check
if maximum inflammation in the beginning (day 0) and in the middle (day 20) of
the study are equal to the corresponding day numbers.

~~~
max_inflammation_0 = numpy.max(data, axis=0)[0]
max_inflammation_20 = numpy.max(data, axis=0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20:
    print('Suspicious looking maxima!')
~~~
{: .language-python}

We also saw a different problem in the third dataset;
the minima per day were all zero (looks like a healthy person snuck into our study).
We can also check for this with an `elif` condition:

~~~
elif numpy.sum(numpy.min(data, axis=0)) == 0:
    print('Minima add up to zero!')
~~~
{: .language-python}

And if neither of these conditions are true, we can use `else` to give the all-clear:

~~~
else:
    print('Seems OK!')
~~~
{: .language-python}

Let's test that out:

~~~
data = numpy.loadtxt(fname='inflammation-01.csv', delimiter=',')

max_inflammation_0 = numpy.max(data, axis=0)[0]
max_inflammation_20 = numpy.max(data, axis=0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20:
    print('Suspicious looking maxima!')
elif numpy.sum(numpy.min(data, axis=0)) == 0:
    print('Minima add up to zero!')
else:
    print('Seems OK!')
~~~
{: .language-python}

~~~
Suspicious looking maxima!
~~~
{: .output}

~~~
data = numpy.loadtxt(fname='inflammation-03.csv', delimiter=',')

max_inflammation_0 = numpy.max(data, axis=0)[0]
max_inflammation_20 = numpy.max(data, axis=0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20:
    print('Suspicious looking maxima!')
elif numpy.sum(numpy.min(data, axis=0)) == 0:
    print('Minima add up to zero!')
else:
    print('Seems OK!')
~~~
{: .language-python}

~~~
Minima add up to zero!
~~~
{: .output}

In this way,
we have asked Python to do something different depending on the condition of our data.
Here we printed messages in all cases,
but we could also imagine not using the `else` catch-all
so that messages are only printed when something is wrong,
freeing us from having to manually examine every plot for features we've seen before.

> ## Catching more cases
>
> Note that in the above code example, if the condition to find suspicious maxima is satisfied, 
> we cannot also trigger the condition to confirm whether minima add up to zero. 
> Rewrite the conditional statement from the code above so that both cases can be identified 
> in the same data set.
>
>> ## Solution
>> We can separate out all the conditional statements, with the final check ('Seems OK!')
>> being explicitly conditional on the previous two not being satisfied.
>> ~~~
>> if max_inflammation_0 == 0 and max_inflammation_20 == 20:
>>     print('Suspicious looking maxima!')
>>
>> if numpy.sum(numpy.min(data, axis=0)) == 0:
>>     print('Minima add up to zero!')
>>
>> if (max_inflammation_0 != 0 or max_inflammation_20 != 20) and 
>>   (numpy.sum(numpy.min(data, axis=0)) != 0):
>>     print('Seems OK!')
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


> ## What Is Truth?
>
> `True` and `False` booleans are not the only values in Python that are true and false.
> In fact, *any* value can be used in an `if` or `elif`.
> After reading and running the code below,
> explain what the rule is for which values are considered true and which are considered false.
>
> ~~~
> if '':
>     print('empty string is true')
> if 'word':
>     print('word is true')
> if []:
>     print('empty list is true')
> if [1, 2, 3]:
>     print('non-empty list is true')
> if 0:
>     print('zero is true')
> if 1:
>     print('one is true')
> ~~~
> {: .language-python}
{: .challenge}

> ## That's Not Not What I Meant
>
> Sometimes it is useful to check whether some condition is not true.
> The Boolean operator `not` can do this explicitly.
> After reading and running the code below,
> write some `if` statements that use `not` to test the rule
> that you formulated in the previous challenge.
>
> ~~~
> if not '':
>     print('empty string is not true')
> if not 'word':
>     print('word is not true')
> if not not True:
>     print('not not True is true')
> ~~~
> {: .language-python}
{: .challenge}

> ## Close Enough
>
> Write some conditions that print `True` if the variable `a` is within 10% of the variable `b`
> and `False` otherwise.
> Compare your implementation with your partner's:
> do you get the same answer for all possible pairs of numbers?
>
> > ## Hint
> > There is a [built-in function `abs`][abs-function] that returns the absolute value of
> > a number:
> > ~~~
> > print(abs(-12))
> > ~~~
> > {: .language-python}
> > ~~~
> > 12
> > ~~~
> > {: .output}
> {: .solution}
>
> > ## Solution 1
> > ~~~
> > a = 5
> > b = 5.1
> >
> > if abs(a - b) <= 0.1 * abs(b):
> >     print('True')
> > else:
> >     print('False')
> > ~~~
> > {: .language-python}
> {: .solution}
>
> > ## Solution 2
> > ~~~
> > print(abs(a - b) <= 0.1 * abs(b))
> > ~~~
> > {: .language-python}
> >
> > This works because the Booleans `True` and `False`
> > have string representations which can be printed.
> {: .solution}
{: .challenge}

> ## In-Place Operators
>
> Python (and most other languages in the C family) provides
> [in-place operators]({{ page.root }}/reference/#in-place-operators)
> that work like this:
>
> ~~~
> x = 1  # original value
> x += 1 # add one to x, assigning result back to x
> x *= 3 # multiply x by 3
> print(x)
> ~~~
> {: .language-python}
>
> ~~~
> 6
> ~~~
> {: .output}
>
> Write some code that sums the positive and negative numbers in a list separately,
> using in-place operators.
> Do you think the result is more or less readable
> than writing the same without in-place operators?
>
> > ## Solution
> > ~~~
> > positive_sum = 0
> > negative_sum = 0
> > test_list = [3, 4, 6, 1, -1, -5, 0, 7, -8]
> > for num in test_list:
> >     if num > 0:
> >         positive_sum += num
> >     elif num == 0:
> >         pass
> >     else:
> >         negative_sum += num
> > print(positive_sum, negative_sum)
> > ~~~
> > {: .language-python}
> >
> > Here `pass` means "don't do anything".
> In this particular case, it's not actually needed, since if `num == 0` neither
> > sum needs to change, but it illustrates the use of `elif` and `pass`.
> {: .solution}
{: .challenge}

> ## Sorting a List Into Buckets
>
> In our `data` folder, large data sets are stored in files whose names start with
> "inflammation-" and small data sets -- in files whose names start with "small-". We
> also have some other files that we do not care about at this point. We'd like to break all
> these files into three lists called `large_files`, `small_files`, and `other_files`,
> respectively.
>
> Add code to the template below to do this. Note that the string method
> [`startswith`](https://docs.python.org/3/library/stdtypes.html#str.startswith)
> returns `True` if and only if the string it is called on starts with the string
> passed as an argument, that is:
>
> ~~~
> 'String'.startswith('Str')
> ~~~
> {: .language-python}
> ~~~
> True
> ~~~
> {: .output}
> But
> ~~~
> 'String'.startswith('str')
> ~~~
> {: .language-python}
> ~~~
> False
> ~~~
> {: .output}
>Use the following Python code as your starting point:
> ~~~
> filenames = ['inflammation-01.csv',
>          'myscript.py',
>          'inflammation-02.csv',
>          'small-01.csv',
>          'small-02.csv']
> large_files = []
> small_files = []
> other_files = []
> ~~~
> {: .language-python}
>
> Your solution should:
>
> 1.  loop over the names of the files
> 2.  figure out which group each filename belongs in
> 3.  append the filename to that list
>
> In the end the three lists should be:
>
> ~~~
> large_files = ['inflammation-01.csv', 'inflammation-02.csv']
> small_files = ['small-01.csv', 'small-02.csv']
> other_files = ['myscript.py']
> ~~~
> {: .language-python}
>
> > ## Solution
> > ~~~
> > for filename in filenames:
> >     if filename.startswith('inflammation-'):
> >         large_files.append(filename)
> >     elif filename.startswith('small-'):
> >         small_files.append(filename)
> >     else:
> >         other_files.append(filename)
> >
> > print('large_files:', large_files)
> > print('small_files:', small_files)
> > print('other_files:', other_files)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## Counting Vowels
>
> 1. Write a loop that counts the number of vowels in a character string.
> 2. Test it on a few individual words and full sentences.
> 3. Once you are done, compare your solution to your neighbor's.
>    Did you make the same decisions about how to handle the letter 'y'
>    (which some people think is a vowel, and some do not)?
>
> > ## Solution
> > ~~~
> > vowels = 'aeiouAEIOU'
> > sentence = 'Mary had a little lamb.'
> > count = 0
> > for char in sentence:
> >     if char in vowels:
> >         count += 1
> >
> > print('The number of vowels in this string is ' + str(count))
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## While loops

It's worth noting that in addition to `for` loops which iterate through a set of values to execute multiple iterations of the loop, we can also define a loop based on a conditional statement. These are called `while` loops. They are not commonly used since they run the risk that if the condition is never satisfied, they can run forever! If `while` loops are used they should be handled with care, with careful checks that the condition will be satisfied or that the loop can be escaped through some other means (e.g. setting a maximum value of allowed iterations of the loop).

For example, the following `while` loops have safety escapes built in:

~~~
i = 0

while i < 10:
    print(i)
    i += 1  # Fancy way of saying i = i + 1
else:
    print('i is equal or larger than 10')
~~~
{: .language-python}
~~~
0
1
2
3
4
5
6
7
8
9
i is equal or larger than 10
~~~
{: .output}

The following denotes the kind of `while` loop that might be used together with some other 
function, e.g. in this case a detection algorithm, to loop through some increasing parameter
before giving up the search:

~~~
detected = False
i = 1

while not detected:
    i *= 2

    # We could embed some code here to `detect` what we are looking for, e.g. 
    # a source in an image where i also sets a pixel range searched over
    
    if i == 8:
        print('Halfway')
        continue  # Skips the rest, starts with the next loop
    
    print(i)
    
    if i == 16:
        break  # or detected = True
~~~
{: .language-python}

~~~
2
4
Halfway
16
~~~
{: .output}

Note that `break` included with a `while` in this way can (in some situations) lead to ambiguity about what causes the loop to break. We can make the `while` loop safer if we replace the `break` statement with an additional condition:

~~~
detected = False
i = 1

while not detected and i <= 8:
    i *= 2

    # We could embed some code here to `detect` what we are looking for, e.g. 
    # a source in an image where i also sets a pixel range searched over
    
    if i == 8:
        print('Halfway')
        continue  # Skips the rest, starts with the next loop
    
    print(i)
~~~
{: .language-python}

which produces the same output as the previous example.

[abs-function]: https://docs.python.org/3/library/functions.html#abs

{% include links.md %}
