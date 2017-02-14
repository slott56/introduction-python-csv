
# Reading (and processing and writing) CSV files.

For a lot of us, automated data processing is often done by transforming and summarizing data that either starts in a spreadsheet or is going to wind up in a spreadsheet.

This notebook is a tutorial on CSV file processing in Python. It doesn't cover the details of installing Python or Jupyter notebooks. You'll have to find that separately. Look at [Intro to Python CSV Processing for Actual Beginners](http://slott-softwarearchitect.blogspot.com/2017/02/intro-to-python-csv-processing-for.html) for details.

Ideally, you've got your own Jupyter Notebook page running. You can follow along here, and create a notebook that reads and writes CSV files.

What we want to do is read a CSV file, do transformations on the data, and write new CSV output. In order to do that, we'll need a file.

# A Sample CSV File

Here's a few lines of data you can copy and paste into a text file. 

1. Click on the browser tab for the **home** directory for your notebooks.
1. Create a new Text file. 
1. Copy the data in the next cell, paste it into your next text file.
1. Click on the name (usually **untitled.txt**) to change this to **"keynote.csv"**.

With this file, you can follow along nicely. The sample data I started with was even larger, but this seems like enough data to get started with.

```
name,size,date,time
../Five Kinds of Functions 3.5.key,5033403,2016-09-29,19:58:58
../Mastering OO Python/Code/inception_doc.key,272,2014-01-31,11:05:10
../Python Callables/Callables.key,8032576,2016-09-29,19:26:22
../Python Cookbook/code/ssl.key,891,2016-07-30,14:25:00
../Python Course/Python 3.key,2285037,2012-06-29,15:49:54
../Python Course/Python2.4.key,360509,2012-02-07,08:40:52
../Python Course/Python2.6.key,1168472,2009-08-26,21:51:58
../Python Namespaces/Namespaces.key,7530051,2016-01-09,08:59:54
../Python Special Method Names/SpecialMethods.key,12217075,2015-04-01,06:41:04
../Python Testing Habits/TestingHabits.key,15240614,2016-06-29,20:43:24
```

# Reading a File

To get a file from outside Python to inside Python there's a two-step dance.

1. Open a connection to the OS.
1. Read the text. 

We'll wrap all of this into a context that makes sure that the OS resources are quietly released when we're done using them.

It looks like this:


```python
with open("keynote.csv") as input_file:
    data = input_file.read()
```

The `open()` function takes a filename as an argument. It makes all the necessary OS connections to read the data. We've wrapped this in a **with** statement to create a processing context and save the working file object in a variable named input_file.

The body of the **with** statement is indented. This touches on two important details of syntax in Python.

- We indent the context to show all of the statements that will work with the input_file object. At the end of the indentation, the file is closed and all of the OS resources are released.
- Each statement is complete on a single line. There are ways to break up lines. But that's a nuance for more advanced programming.

Inside the **with** context we used `input_file.read()` to perform the `read()` method of the `input_file` object. There's a dot to connect the object name with its method name.  There are `()`'s in case we want to provide any argument values. This method will slurp up all the data from the external file. The resulting text is used to create another variable, data.

The screen shot includes **In [4]:**. Because there's a number in the []'s, it means I clicked ⏯ to run the block of code. Your number may not be 4. You can try different things, also. Play around. You can't hurt anything.

"Okay," you say. "How do I see what happened?"

Good question. To see the value of the data variable, it's a really small line of code. Like this:


```python
data
```




    'name,size,date,time\n../Five Kinds of Functions 3.5.key,5033403,2016-09-29,19:58:58\n../Mastering OO Python/Code/inception_doc.key,272,2014-01-31,11:05:10\n../Python Callables/Callables.key,8032576,2016-09-29,19:26:22\n../Python Cookbook/code/ssl.key,891,2016-07-30,14:25:00\n../Python Course/Python 3.key,2285037,2012-06-29,15:49:54\n../Python Course/Python2.4.key,360509,2012-02-07,08:40:52\n../Python Course/Python2.6.key,1168472,2009-08-26,21:51:58\n../Python Namespaces/Namespaces.key,7530051,2016-01-09,08:59:54\n../Python Special Method Names/SpecialMethods.key,12217075,2015-04-01,06:41:04\n../Python Testing Habits/TestingHabits.key,15240614,2016-06-29,20:43:24\n'



Yeah. That's a lot of output.

We wrote a really short expression (one word: `data`) to see the value of the object named data. I clicked ⏯ to run the tiny little block of code. And there it is. Text. Lots of it.

"What's that `\n`?" you might ask. 

That's the Linux line-ending character. Some tools will show each line separately. Text editors are good at this. Some tools just show the characters. If you look at the page where you originally built the file, the `\n` characters don't clutter up the display. They're there. The editor is clever about using them to format the output.

Try this:


```python
print(data)
```

    name,size,date,time
    ../Five Kinds of Functions 3.5.key,5033403,2016-09-29,19:58:58
    ../Mastering OO Python/Code/inception_doc.key,272,2014-01-31,11:05:10
    ../Python Callables/Callables.key,8032576,2016-09-29,19:26:22
    ../Python Cookbook/code/ssl.key,891,2016-07-30,14:25:00
    ../Python Course/Python 3.key,2285037,2012-06-29,15:49:54
    ../Python Course/Python2.4.key,360509,2012-02-07,08:40:52
    ../Python Course/Python2.6.key,1168472,2009-08-26,21:51:58
    ../Python Namespaces/Namespaces.key,7530051,2016-01-09,08:59:54
    ../Python Special Method Names/SpecialMethods.key,12217075,2015-04-01,06:41:04
    ../Python Testing Habits/TestingHabits.key,15240614,2016-06-29,20:43:24
    


The `print()` function creates prettier output. The `\n` characters are used to format the output for us.

# Processing Lines

The `data` object is just a big pile of text. It's awkwardly large. We'd rather have each line as a separate little piece of information.

We want to iterate through the lines of data. We'll use Python's **for** statement to do this. Think of this as meaning "for all" because it handles all of the items in a sequence one at a time.


```python
with open("keynote.csv") as input_file:
    for one_line in input_file:
        print(one_line)
```

    name,size,date,time
    
    ../Five Kinds of Functions 3.5.key,5033403,2016-09-29,19:58:58
    
    ../Mastering OO Python/Code/inception_doc.key,272,2014-01-31,11:05:10
    
    ../Python Callables/Callables.key,8032576,2016-09-29,19:26:22
    
    ../Python Cookbook/code/ssl.key,891,2016-07-30,14:25:00
    
    ../Python Course/Python 3.key,2285037,2012-06-29,15:49:54
    
    ../Python Course/Python2.4.key,360509,2012-02-07,08:40:52
    
    ../Python Course/Python2.6.key,1168472,2009-08-26,21:51:58
    
    ../Python Namespaces/Namespaces.key,7530051,2016-01-09,08:59:54
    
    ../Python Special Method Names/SpecialMethods.key,12217075,2015-04-01,06:41:04
    
    ../Python Testing Habits/TestingHabits.key,15240614,2016-06-29,20:43:24
    


The **with**-statement context should look familiar.  

The **for** statement consumes each individual line of text from the `input_file` object. One at a time, each line is assigned to the `one_line` variable, then, the indented statement(s) are executed. In this case, there's only one indented statement, the `print()` function.

Hint. We can indent many statements to do many things for each line of data. 

Another Hint. Statements must be complete on a single line.

What we need to do now is decompose the data into the original CSV columns. For that, we'll need some help.

# Extracting the CSV Columns

Here's some additional software that we'll need to parse the columns of data properly:


```python
import csv
```

Command number 8 will introduce a library module into our working environment. The module is named **csv** and it has classes and functions that parse CSV-format files.

In a script file, we try to organize all the imports so that they're near the front of the file. In the Jupyter notebook, this isn't really required.

We'll use the `csv.reader()` function to create an object that will iterate through columns of data instead of simple, unstructured text.

Here's the code we'll need:


```python
with open("keynote.csv") as input_file:
    reader = csv.reader(input_file)
    for columns_of_data in reader:
        print(columns_of_data)
```

    ['name', 'size', 'date', 'time']
    ['../Five Kinds of Functions 3.5.key', '5033403', '2016-09-29', '19:58:58']
    ['../Mastering OO Python/Code/inception_doc.key', '272', '2014-01-31', '11:05:10']
    ['../Python Callables/Callables.key', '8032576', '2016-09-29', '19:26:22']
    ['../Python Cookbook/code/ssl.key', '891', '2016-07-30', '14:25:00']
    ['../Python Course/Python 3.key', '2285037', '2012-06-29', '15:49:54']
    ['../Python Course/Python2.4.key', '360509', '2012-02-07', '08:40:52']
    ['../Python Course/Python2.6.key', '1168472', '2009-08-26', '21:51:58']
    ['../Python Namespaces/Namespaces.key', '7530051', '2016-01-09', '08:59:54']
    ['../Python Special Method Names/SpecialMethods.key', '12217075', '2015-04-01', '06:41:04']
    ['../Python Testing Habits/TestingHabits.key', '15240614', '2016-06-29', '20:43:24']


This is starting to look pretty complex. This is a nested set of Russian *matryoshka* dolls. 

- The outermost shell is the **with** statement that manages the open file context. Within this context, we have two statements. 
  - The first statement creates the reader object. The object is assigned the uniomaginative name of `reader`.
  - The second statement uses that reader object. Since it's a **for** statement, it has a nested statement inside of it. In this case, a `print()` statement.

The reader is built using the `csv.reader()` function. We provide an open file, and we get back an object that iterates through data that is properly decomposed into rows and columns.

The **for** statement uses the iterable assigned to the `reader` variable. Each individual row of data is assigned to the `columns_of_data` variable. When we printed them, we saw each row is now surrounded by `[]`'s. 

What does this `[]` syntax mean?

It means that we have a *sequence* of items. 
When reading CSV files, each data item is an individually quoted string value, separated by commas. Other kinds of data are possible here, but we're going to stick with CSV reading; that tends to impose some restrictions.

We can work with an individual column using syntax like `columns_of_data[1]` to refer to the second column, the size. 

Let's play around with a single row for second. Here's how *indexing* works to pick a specific item from a sequence of items:


```python
row = ['../Five Kinds of Functions 3.5.key', '5033403', '2016-09-29', '19:58:58']
```


```python
row[0]
```




    '../Five Kinds of Functions 3.5.key'




```python
row[3]
```




    '19:58:58'



When we use `[0]` we get the first item from the sequence. When we use `[3]` we get the fourth item. 

Indexing starts at zero. 

Here's a way to leverage picking things apart like this. Let's interleave some literal text with the values we read from the CSV file:


```python
with open("keynote.csv") as input_file:
    reader = csv.reader(input_file)
    for the_row in reader:
        print("name=", the_row[0], "size=", the_row[1], "date,time=", the_row[2], the_row[3])
```

    name= name size= size date,time= date time
    name= ../Five Kinds of Functions 3.5.key size= 5033403 date,time= 2016-09-29 19:58:58
    name= ../Mastering OO Python/Code/inception_doc.key size= 272 date,time= 2014-01-31 11:05:10
    name= ../Python Callables/Callables.key size= 8032576 date,time= 2016-09-29 19:26:22
    name= ../Python Cookbook/code/ssl.key size= 891 date,time= 2016-07-30 14:25:00
    name= ../Python Course/Python 3.key size= 2285037 date,time= 2012-06-29 15:49:54
    name= ../Python Course/Python2.4.key size= 360509 date,time= 2012-02-07 08:40:52
    name= ../Python Course/Python2.6.key size= 1168472 date,time= 2009-08-26 21:51:58
    name= ../Python Namespaces/Namespaces.key size= 7530051 date,time= 2016-01-09 08:59:54
    name= ../Python Special Method Names/SpecialMethods.key size= 12217075 date,time= 2015-04-01 06:41:04
    name= ../Python Testing Habits/TestingHabits.key size= 15240614 date,time= 2016-06-29 20:43:24


What have we done? 

1. We've changed the name of the **for** statement variable to `the_row`; it's easier to type.
2. We've expanded the `print()` function arguments to include lots of extra literal text. The `print()` function has seven argument values. Some are literal text. Others are expressions. All of the expressions are expressions to extract specific items from a sequence.

Note the very first line of output.

```
name= name size= size date,time= date time
```

The headings aren't too helpful. We'd like to ignore them.

# Filtering: Skipping the Heading

The heading line isn't helping us much, is it?

We can filter that out by using the **if** statement. We can create a condition expression and provide some processing to be used only when that condition is true.

A condition can be computed using any of the available comparison operators. We'll show an example using "==", but there are all of the usual culprits: >, <, <=, >= and !=.  (Your keyboard has characters like ≤, ≥, and ≠; Python doesn't use these right now.)

Note. We use `==` to mean "test for being equal." We use `=` to mean assign a value to a variable. `==` is a question. `=` is a statement of something to do.

The condition we're interested in is when `the_row[1] == "size"`. That's a heading line. The extra processing we'd like to do is skip that line. 


```python
with open("keynote.csv") as input_file:
    reader = csv.reader(input_file)
    for the_row in reader:
        if the_row[1] == "size":
            continue
        print(the_row)
```

    ['../Five Kinds of Functions 3.5.key', '5033403', '2016-09-29', '19:58:58']
    ['../Mastering OO Python/Code/inception_doc.key', '272', '2014-01-31', '11:05:10']
    ['../Python Callables/Callables.key', '8032576', '2016-09-29', '19:26:22']
    ['../Python Cookbook/code/ssl.key', '891', '2016-07-30', '14:25:00']
    ['../Python Course/Python 3.key', '2285037', '2012-06-29', '15:49:54']
    ['../Python Course/Python2.4.key', '360509', '2012-02-07', '08:40:52']
    ['../Python Course/Python2.6.key', '1168472', '2009-08-26', '21:51:58']
    ['../Python Namespaces/Namespaces.key', '7530051', '2016-01-09', '08:59:54']
    ['../Python Special Method Names/SpecialMethods.key', '12217075', '2015-04-01', '06:41:04']
    ['../Python Testing Habits/TestingHabits.key', '15240614', '2016-06-29', '20:43:24']


We've written a condition of `the_row[1] == "size"`. If this condition is true, we'd like to continue the **for** processing, ignoring all the rest of the statements in the indented body. We've rejected the line with `"size"` in column 1. All the other lines pass through and get printed. 

# A Mapping: String Size to Int Size

It's a pain in the neck to work with the file sizes as strings of characters. A number is more useful. Python has a variety of functions to make string values into useful numbers. We'll look at `int()`, but there's also `float()`, `decimal.Decimal()` and even `fractions.Fraction()`.

Since an actual number looks like a string of characters, it's a little hard to see exactly what's going on. First, we'll format the output. Then we'll see how numbers work with this.

We'll use a `"template".format(values,...)` expression to do the fancy formatting. The template is a string with place-holders. The values are interpolated into the replaement fields in the string. Here's the example:


```python
with open("keynote.csv") as input_file:
    reader = csv.reader(input_file)
    for the_row in reader:
        if the_row[1] == "size":
            continue
        print("{:48s} {:9s} {:s}T{:s}".format(the_row[0], the_row[1], the_row[2], the_row[3]))
```

    ../Five Kinds of Functions 3.5.key               5033403   2016-09-29T19:58:58
    ../Mastering OO Python/Code/inception_doc.key    272       2014-01-31T11:05:10
    ../Python Callables/Callables.key                8032576   2016-09-29T19:26:22
    ../Python Cookbook/code/ssl.key                  891       2016-07-30T14:25:00
    ../Python Course/Python 3.key                    2285037   2012-06-29T15:49:54
    ../Python Course/Python2.4.key                   360509    2012-02-07T08:40:52
    ../Python Course/Python2.6.key                   1168472   2009-08-26T21:51:58
    ../Python Namespaces/Namespaces.key              7530051   2016-01-09T08:59:54
    ../Python Special Method Names/SpecialMethods.key 12217075  2015-04-01T06:41:04
    ../Python Testing Habits/TestingHabits.key       15240614  2016-06-29T20:43:24


We've made a dramatic change to the `print()` statement. Instead of printing a bunch of items, we've formatted a string and printed that. The formatting operation starts with a template; the values are interpolated into that template.

The template is this string object `"{:48s} {:9s} {:s}T{:s}"`. More on the details in a second.

The template string has a method, `format()` that does the interpolation work and creates a new output string.

When we provide values to the `format()` method, the `{}`'s in the template are replaced. There are a bunch of replacement field rules. We've used just a few variations.

Here's the overall pattern for a replacement field:

- Surrounded by {};
- inside each {} is a field name, 
- a :, 
- and a format specification. 

The field names are all omitted. Without names, the argument values are used in the order they're provided. 

If we provide names, it's common to see fields identified by position. We might say `{1:s}` to select field in position 1 to be interpolated here in the string.

The format specifications are mostly a number (which is the number of positions to use) and `s` to insert a string value into the output.

We've spread the name out by using `{:48s}` to take up at least 48 positions. For the size, the `{:9s}` uses at least 9 positions. The date and time don't change size, so we just used `{:s}` rather than count up the characters. We even injected a literal "T" into the output. It looks more ISO-official, doesn't it?

Now let's make two more changes. We want to convert the size to a proper number object. Then we can do math on it, like divide by 1024 to convert to kilobytes (kb) or divide by `1024*1024` to get megabytes (mb).

When we switch to a number, we'll also have to change the formatting to properly display that number. 

Here's the new code.


```python
with open("keynote.csv") as input_file:
    reader = csv.reader(input_file)
    for the_row in reader:
        if the_row[1] == "size":
            continue
        size = int(the_row[1])
        print("{:48s} {:9,d} {:s}T{:s}".format(the_row[0], size, the_row[2], the_row[3]))
```

    ../Five Kinds of Functions 3.5.key               5,033,403 2016-09-29T19:58:58
    ../Mastering OO Python/Code/inception_doc.key          272 2014-01-31T11:05:10
    ../Python Callables/Callables.key                8,032,576 2016-09-29T19:26:22
    ../Python Cookbook/code/ssl.key                        891 2016-07-30T14:25:00
    ../Python Course/Python 3.key                    2,285,037 2012-06-29T15:49:54
    ../Python Course/Python2.4.key                     360,509 2012-02-07T08:40:52
    ../Python Course/Python2.6.key                   1,168,472 2009-08-26T21:51:58
    ../Python Namespaces/Namespaces.key              7,530,051 2016-01-09T08:59:54
    ../Python Special Method Names/SpecialMethods.key 12,217,075 2015-04-01T06:41:04
    ../Python Testing Habits/TestingHabits.key       15,240,614 2016-06-29T20:43:24


What's changed? We've computed a new variable, `size`, using the expression `int(the_row[1])`. The `int()` function will convert its argument value to an integer. No matter what the argument is: floating-point value, complex number, fraction, string. Almost anything.

We changed the format specification to `{:9,d}`. This has a size, `9`. It has a `,` to insert commas into big numbers. And it has a data type of `d` for decimal. Finally, we have changed the argument list to be `the_row[0], size, the_row[2], the_row[3]`.  We've used the integer `size` variable instead of the old string value of the `the_row[1]` expression.

How cool is that? We now have right-aligned numbers, with commas! All we did was convert string to integer, and then use the proper formatting options.

# Strings and Numbers

Strings can be compared for equality. However, they can't be summed to compute an overall size. And when we compare two strings, we'll see an something that might be astonishing.


```python
"11" > "2"
```




    False




```python
11 > 2
```




    True



This is the *lexical ordering* issue. The string values are sorted into alphabetical order. `"11"` comes before `"2"` **alphabetically**. 

On the other hand, the integers are sorted into the expected numeric order: 11 is greater than 2. 

This is why we want to convert the string to a number.  

We'll need to to the same thing for the date and time value.

# A Mapping: Date and Time to Datetime

To make this more useful, we need to convert the date and time strings into something we can compare, also.  This is a little more tricky than integer conversion because of the variety of ways days and times can be formatted. 

First, we'll need to use the datetime module. This has all the tools needed for working with dates and times in all of their glorious inconsistent formats.


```python
import datetime
```

The general approach to handling datetime is this:

1. Convert the date column from text to a date
1. Convert the time column from text to a time
1. Combine the date and time into a single date-time stamp 

The reason we want to do this is to simplify comparing two files modification times. If we keep dates separate from times, it's hard to put these three times into order:

- 9/10/2016 23:59:59 
- 9/11/2016 00:00:01
- 9/10/2016 12:00:01

With some thinking, we can get them into order. Mentally, we have to check dates *as well as* times to get the order correct. Creating a combined `datetime` object makes that concept into a concrete combined time-stamp value.

Here's some little examples of how the three step conversion works.


```python
d_text = '2016-04-21'
datetime.datetime.strptime(d_text, '%Y-%m-%d').date()
```




    datetime.date(2016, 4, 21)



We've defined a date that looks like dates in our CSV file: year-month-day and put that into a variable, `d_text`.

We've used a function with the improbably long name of `datetime.datetime.strptime()` to do string parsing of time ("**str**ing **p**arse **time**"). We provide the string data and a template for understanding the string. The template uses ``%Y`` for 4-digit years, ``%m`` for a month number, and ``%d`` for day of the month.

The result is an object which includes the parsed date. We use the ``date()`` method to extract just the date from that object. (Think of the object the comes back as a kind of buffet table of information. We don't want the whole table.)

The **Out[20]** line shows the Python understanding. It's a ``datetime.date`` object, with the given year, month, and day. Success!

Here's the step for processing time.


```python
t_text = '11:00:02'
datetime.datetime.strptime(t_text, '%H:%M:%S').time()
```




    datetime.time(11, 0, 2)



We've defined a time that looks like dates in our CSV file: hour:minute:second and put that into a variable, `t_text`.

We've use the `datetime.datetime.strptime()` function to do string parsing of time. We provide the string and we provide a template for understanding the string. The template uses `%H` for hours, `%M` for minutes, and `%S` for seconds.

The result is an object which includes the parsed time. We use the `time()` method to extract just the time from that object.

The **Out[21]** line shows the Python understanding. It's a ``datetime.time`` object, with the given hour, minute, and second. Success!

Here's how we can parse and combine:


```python
d = datetime.datetime.strptime(d_text, '%Y-%m-%d').date()
t = datetime.datetime.strptime(t_text, '%H:%M:%S').time()
datetime.datetime.combine(d, t)
```




    datetime.datetime(2016, 4, 21, 11, 0, 2)



We've shown the two `datetime.datetime.strptime()` functions to create the date portion and the time portion of the overall datetime object. Then we used the `datetime.datetime.combine()` function to assemble a complete datetime object from the pieces.

This parallels the way we converted size from string to integer. Okay. It's a lot longer. But it follows the essential template. We can define a new function to handle this. But that's for a later presentation. For now, we'll focus on extracting useful data from the CSV file.


```python
with open("keynote.csv") as input_file:
    reader = csv.reader(input_file)
    for the_row in reader:
        if the_row[1] == "size":
            continue
        size = int(the_row[1])
        d = datetime.datetime.strptime(the_row[2], '%Y-%m-%d').date()
        t = datetime.datetime.strptime(the_row[3], '%H:%M:%S').time()
        modified = datetime.datetime.combine(d, t)
        print("{:48s} {:9,d} {:%c}".format(the_row[0], size, modified))
```

    ../Five Kinds of Functions 3.5.key               5,033,403 Thu Sep 29 19:58:58 2016
    ../Mastering OO Python/Code/inception_doc.key          272 Fri Jan 31 11:05:10 2014
    ../Python Callables/Callables.key                8,032,576 Thu Sep 29 19:26:22 2016
    ../Python Cookbook/code/ssl.key                        891 Sat Jul 30 14:25:00 2016
    ../Python Course/Python 3.key                    2,285,037 Fri Jun 29 15:49:54 2012
    ../Python Course/Python2.4.key                     360,509 Tue Feb  7 08:40:52 2012
    ../Python Course/Python2.6.key                   1,168,472 Wed Aug 26 21:51:58 2009
    ../Python Namespaces/Namespaces.key              7,530,051 Sat Jan  9 08:59:54 2016
    ../Python Special Method Names/SpecialMethods.key 12,217,075 Wed Apr  1 06:41:04 2015
    ../Python Testing Habits/TestingHabits.key       15,240,614 Wed Jun 29 20:43:24 2016


We've extracted a date and assigned it to the `d` variable. This is computed from `the_row[2]`, using the expected date format. We've extracted a time and assigned it to the `t` variable. This comes from `the_row[3]`. We combined these to a single datetime object, and called this `modified`.

We've also changed the format template. To format the datetime object we've used `{:%c}`. This applies that datetime's internal `%c` formatting rule to the data.  This rule provides Linux-like date-time values.

We've turned the string of digits into an integer that we can work with. We've turned two strings of digits and punctuation into a single datetime object that we can work with. 

# Some Additional Filters

Now that we have usable data, what are some things we can do?

Filter by size? `size > 1024*1024` for example, finds files over 1M. We can use `size < 500*1024` to find files under 500K.

Filter by date? `modified >= datetime.datetime(2016, 1, 1)` will find files modified after the start of 2016.

Within a date range? `datetime.datetime(2016, 1, 1) <= modified < datetime.datetime(2017, 1, 1)` finds files modified within the confines of 2016. 

"Why can't I use commas in the numbers?" It's difficult for programming languages to deal with too much punctuation. You can use _'s in the numbers. `1_000_000` will work in Python 3.6. It won't work in older Python variants.

"Why are the dates so complex?" We can use slightly more clever imports to make the dates slightly simpler. Instead of simply using `import datetime`, we can use `from datetime import datetime` to make the names shorter. I'm not sure this is the best way to learn, however, so I suggest sticking with the full, formal name for a while.


# Writing CSV Output

Here's the other side of the coin. We read a CSV file. Now we want to write a CSV file. Maybe with a subset of file names. Maybe with the dates cleaned up. This is similar to reading a CSV file.

Here's the template for reading and writing CSV files.


```python
with open("keynote.csv") as input_file, open("temp.tmp", "w", newline="") as output_file:
    reader = csv.reader(input_file)
    writer = csv.writer(output_file)
    writer.writerow(["name", "size", "modified"])
    for row in reader:
        writer.writerow(row)
```

We've expanded the **with** context to include two open files.

- The `open("keynote.csv")` opens a file that can only be read. This is assigned to the `input_file` variable. There's a mode of `"r"` that's assumed.

- The `open("temp.tmp", "w")` opens a file that can only be written. The explicit mode of `"w"` must be provided so that we can write. We've added `newline=""` to be sure that the newline rules are dictated by the CSV writer instead of using the OS defaults. This is assigned to the output_file variable.

Indented inside this **with** statement is the processing we want to do on our two open files. There are four steps.

1. Create a `csv.reader` object that can parse the `input_file`. Save this in the `reader` variable.

2. Create a `csv.writer` object that can write to the `output_file` in CSV format. This will properly handle commas and quoting of values. This is assigned to the `writer` variable. 

3. Use `writer.writerow()` to put a heading on the output file. Note how we specified the heading column names: we used `[]`'s around the column names, and we provided a list of quoted literal strings.

4. The rest of the processing is inside the **for** statement. In this example, we don't do anything useful. We didn't even copy and paste the code to compute sizes and dates. And we left out any filtering that might be part of the application. We cut it down to just the `writer.writerow()` method of the `writer` object. This will create proper text for each row and write that text to the `output_file`.

When the **for** statement is done, the **with** statement is also done. The files are properly closed and turned over to the OS.

# Conclusion

This demo shows the basics of getting data from a CSV file, doing mappings from the input data to more useful internal data. It shows how to filter lines in a really simple way (by rejecting them.) And it shows how to write the resulting file. 
