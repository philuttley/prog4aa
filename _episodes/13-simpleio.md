---
title: Simple Input/Output
teaching: 20
exercises: 10
questions:
- "How can I write and read data to and from files?"
objectives:
- "Use `File` objects to create and write to new text files and read from existing text files."
- "Be able to use string formatting to write more complex data sets and to parse data that is read from text files"
keypoints:
- "Use `open` with the write (`'w'`), read (`'r'`) and append (`'a'`) methods to write, read and append strings to files."
- "Separate and successive lines can be read in using the `readline()` function or a `for` loop."
- "Remember to close opened files after use, or use `with` to contain operations on a file to an indented block of code."
- "Data of any type must be written to a file as complete strings. String formatting can be used to separate different data values in the string using white spaces, commas or other separators."
- "String formatting methods such as `strip()` and `split()` can be used to remove leading or trailing characters (such as newline commands) and split a string into discrete values according to the locations of the separators."
- "Data values that are read in as strings can be converted back to numerical or integer formats as required using e.g. the `float()` and `int()` commands."
---

## Writing to and reading from files

Python has a built-in `File` object which can be used to represent an open file. By defining such an object, existing files can be read in and new files can be written to. Libraries such as `numpy` and `pandas` have their own powerful functions for these tasks, which you may find easier to use in many cases, but we include a discussion of simple file input/output _(I/O)_ in Python here for completeness. 

This approach can be useful when you need a simple and flexible option to read information from files, or to write output from your programs to files. However, we also need to consider the format of the data which we write, as well as how to _parse_ the lines of the files which we read in.

The `open()` function can be used to assign a filename to an object for reading or writing purposes. For example, let's write to a new file called `smallfile.txt`, using the `write` method which is denoted by the `'w'` argument:

~~~
f = open('smallfile.txt', 'w')
f.write('Some text for this file\n') 
f.write('Some more text for this file\n')
f.close()
~~~
{: .language-python}

It is important that when we are finished writing, we use the  `close()` method to close the file. This is because the operating system will permit only a limited number of files to be open, and keeping too many open at one time can lead to errors with reading and writing.

The resulting file should look like this. Note that the `\n` included inside the string will add a new line, in this case to the file.

~~~
Some text for this file
Some more text for this file
~~~
{: .output}

If we want to write some more to a file that is already closed, we need to re-open it but this time we must use the `append` method (`'a'`) as follows:

~~~
f = open('smallfile.txt', 'a')
f.write('Even more text for this file\n') 
f.close()
~~~
{: .language-python}

This will append the new text at the end of the file. If we used `'w'` instead of `'a'`, we would _overwrite the entire file_ with a file containing the new text.

We can read the file using the `read` method (`'r'`) as follows:

~~~
f = open('smallfile.txt', 'r')
print(f.read())
f.close()
~~~
{: .language-python}

Note that this will output the entire text of the file:

~~~
Some text for this file
Some more text for this file
Even more text for this file
~~~
{: .output}

However, we can also read out an individual line: 
~~~
f = open('smallfile.txt', 'r')
print(f.readline())
f.close()
~~~
{: .language-python}

Running `readline` successively will read successive lines of text. We can also read the lines using a `for` loop:

~~~
f = open('smallfile.txt', 'r')
for line in f:
    print(line)
f.close()
~~~
{: .language-python}

~~~
Some text for this file

Some more text for this file

Even more text for this file
~~~
{: .output}
Note that the gaps arise due to the newline command given in the string, which includes a newline at the end of the text being read in.

A safer way to handle files is to use the `with` keyword, which implicitly closes the file after the corresponding indented block of code has been executed. For example:

~~~
with open('smallfile.txt', 'w') as f:
    f.write('Some text for this file\n') 
    f.write('Some more text for this file\n')
    f.write('Even more text for this file\n') 
    
with open('smallfile.txt', 'r') as f:
    print(f.read())
~~~
{: .language-python}

> ## Writing data
>
> So far we have seen how to read or write text files, either line by line or in total, 
> but how can we tell our program how the text is actually structured, e.g. as distinct words,
> numbers or some combination of those? First, note that the lines of text are written or read 
> as an entire string. Thus, writing to the file in the desired format is straightforward using 
> the methods we have learned. Let's see with the example of stellar data that we considered 
> previously. Imagine that we have recorded the data for `[Name, Dist_pc, 
> Spec_Type, Mass_Msol, Lum_Lsol]` as lists, i.e.:
>
> ~~~
> Vega = ['Vega', 7.68, 'A0Va', 2.14, 40.12]
> Arcturus = ['Arcturus', 11.26, 'K0III', 1.08, 170]
> Deneb = ['Deneb', 802, 'A2Ia', 19, 1.96e5]
> ~~~
> {: .language-python}
>
> Now use `for` loop(s) to write the data for each star to a separate line of a text file 
> `stars.txt`, using white space to separate the different values on each line. 
>
>> ## Hint
>> If you use `enumerate` to read in the values from each line along with an integer denoting 
>> where you are along the line, you can combine it with a conditional
>> statement so that white space is only added to the string _after_ the first value.
> {: .solution}
>
>> ## Solution
>>
>> ~~~
>> with open('stars.txt', 'w') as f:
>>     for star in [Vega, Deneb, Arcturus]:
>>         for i, val in enumerate(star):
>>             if (i == 0):
>>                 line = str(val) # The first value is used to create `line`
>>             else:
>>                 line = line + " " + str(val)
>>         line = line + '\n'
>>         f.write(line)  
>> ~~~
>> {: .language-python}
>> 
>> Note that if we instead just print the variable `star`, the output will look like the 
>> original Python lists:
> >~~~
>> with open('stars2.txt', 'w') as f:
>>     for star in [Vega, Deneb, Arcturus]:
>>         f.write(str(star) + '\n')       
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> ['Vega', 7.68, 'A0Va', 2.14, 40.12]
>> ['Deneb', 802, 'A2Ia', 19, 196000.0]
>> ['Arcturus', 11.26, 'K0III', 1.08, 170]
>> ~~~
>>
>> This is not ideal when we want our data to be generally readable by other methods, 
>> using a simple multi-column format.
> {: .solution}
{: .challenge}

In the example above, we used white spaces to separate separate data values on a line. It is also common to use commas (_comma separated values_ or _CSV_ format) and sometimes tab (_TSV_ format).

If we want to read data from a file, separating each line into its component data values, we need to account for the fact that each line of data is read in as a whole string and use Python's _string methods_ to separate each string into its components. 

For example, the method `split()` can be used to separate out parts of a string separated by a given _separator_ such as a comma or white space:

~~~
with open('smallfile.txt', 'r') as f:
    for line in f:
        words = line.split(' ')
        print(words)
~~~
{: .language-python}

~~~
['Some', 'text', 'for', 'this', 'file\n']
['Some', 'more', 'text', 'for', 'this', 'file\n']
['Even', 'more', 'text', 'for', 'this', 'file\n']
~~~
{: .output}

The newline command is still present in the strings! We can remove the `\n` by using the `strip()` string method:

~~~
with open('smallfile.txt', 'r') as f:
    for line in f:
        line2 = line.strip('\n')
        words = line.split(' ')
        print(words)
~~~
{: .language-python}

~~~
['Some', 'text', 'for', 'this', 'file']
['Some', 'more', 'text', 'for', 'this', 'file']
['Even', 'more', 'text', 'for', 'this', 'file']
~~~
{: .output}

Note that the default `strip()` with no string specified as its argument will remove both white spaces and newline strings.

We can use indexing to convert values into the resulting lists into whatever data variables we need, using the `float()` or `int()` functions where appropriate to convert the strings output by the read and string methods into numerical values.

> ## Reading data
>
> Now use the methods discussed to read the `stars.txt` file created above and assign all the
> data values as items (with the appropriate data types) in a single nested list, with each row
> corresponding to a star and the columns corresponding to the different variables.
>
>> ## Solution
>> ~~~
>> with open('stars.txt', 'r') as f:
>>     for i, line in enumerate(f):
>>         line2 = line.strip('\n')
>>         data = line2.split(' ')
>>         if i == 0:
>>             star_data = [[data[0],float(data[1]),data[2],float(data[3]),
>>                                                         float(data[4])]]
>>         else:            
>>             star_data.append([data[0],float(data[1]),data[2],float(data[3]),
>>                                                             float(data[4])])
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

