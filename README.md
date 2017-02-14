

```python
355/113
```




    3.1415929203539825



First. The notebook was originally called "**untitled**" which seemed less than ideal. So I clicked on the name and changed it to "**csv_westling**". 

Second. There was a box labeled **In [ ]:**. I entered some Python code to the right of this label. Then I clicked the run cell icon. (It's similar to this emoji --  ⏯ -- but not exactly.)

The **In [ ]:** changed to **In [1]:**. A second box appeared labeled **Out [1]:**. This annotates our dialog with Python: each input and Python's response is tracked. It's pretty nice. We can change our input and rerun the cell. We can add new cells with different things to run. We can run all of the cells. Lots of things are possible based on this idea of a cell with our command. When we run a cell, Python processes the command and we see the output.

For many expressions, a value is displayed.  For some expressions, however, nothing is displayed. For complete statements, nothing is displayed. This means we'll often have to throw the name of a variable in to see the value of that variable.


```python
pi = 355/113
```

See? No output. The variable was created by an assignment statement, and nothing else was shown. 


```python
pi
```




    3.1415929203539825



That's how we examine the state of our process: we display the values of variables.

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




    'name,size,date,time\n../Five Kinds of Functions 3.5.key,5033403,2016-09-29,19:58:58\n../Mastering OO Python/Code/inception_doc.key,272,2014-01-31,11:05:10\n../Python Callables/Callables.key,8032576,2016-09-29,19:26:22\n../Python Cookbook/code/ssl.key,891,2016-07-30,14:25:00\n../Python Course/Python 3.key,2285037,2012-06-29,15:49:54\n../Python Course/Python2.4.key,360509,2012-02-07,08:40:52\n../Python Course/Python2.6.key,1168472,2009-08-26,21:51:58\n../Python Namespaces/Namespaces.key,7530051,2016-01-09,08:59:54\n../Python Special Method Names/SpecialMethods.key,12217075,2015-04-01,06:41:04\n../Python Testing Habits/TestingHabits.key,15240614,2016-06-29,20:43:24\n../Python2v3/00_01_Introduction.key,3439648,2016-12-17,16:20:21\n../Python2v3/00_02_Quick_Overview.key,12710870,2016-12-17,16:19:56\n../Python2v3/00_03_Official_Howto.key,14264411,2016-12-17,16:21:01\n../Python2v3/00_04_Differences.key,11018272,2016-12-21,13:18:50\n../Python2v3/01_01_Syntax_Comparison.key,3078962,2016-12-17,16:22:48\n../Python2v3/01_02_Type_Comparison.key,2964145,2016-12-17,16:23:43\n../Python2v3/01_03_Builtins_Comparison.key,2969941,2016-12-17,16:24:54\n../Python2v3/01_04_Other_Semantics.key,2931235,2016-12-17,16:28:18\n../Python2v3/02_01_Strategies.key,19123686,2016-12-18,11:19:04\n../Python2v3/02_02_Convert.key,9369086,2016-12-18,11:54:12\n../Python2v3/02_03_Coordinate.key,9410504,2016-12-18,12:49:55\n../Python2v3/02_04_Coexist_six.key,10806912,2016-12-18,13:20:25\n../Python2v3/02_05_Coexist_2to3.key,9448602,2016-12-18,13:54:06\n../Python2v3/02_06_Tooling.key,6146437,2016-12-18,14:09:29\n../Python2v3/03_01_Six.key,2863992,2016-12-22,16:03:29\n../Python2v3/03_02_Syntax.key,2853481,2016-12-23,11:50:56\n../Python2v3/03_03_Classes.key,2832664,2016-12-22,21:42:10\n../Python2v3/03_04_Builtins.key,2885234,2016-12-18,18:03:48\n../Python2v3/03_05_Library.key,2838517,2016-12-23,08:15:10\n../Python2v3/04_01_Future.key,2902153,2016-12-23,10:46:44\n../Python2v3/04_02_Syntax.key,2868553,2016-12-23,11:59:19\n../Python2v3/04_03_Classes.key,2852716,2016-12-23,13:32:15\n../Python2v3/04_04_Builtins.key,2858460,2016-12-24,09:02:44\n../Python2v3/04_05_Library.key,2823998,2016-12-24,13:13:40\n../Python2v3/05_01_2to3.key,2921014,2016-12-24,13:46:55\n../Python2v3/05_02_Syntax_Classes.key,2918938,2016-12-24,12:17:07\n../Python2v3/05_03_Builtins_Library.key,2923791,2016-12-24,13:15:04\n../Python2v3/05_04_Wrap_Up.key,6134784,2016-12-24,15:10:26\n../Python2v3/BLUE TEMPLATE.key,32352638,2016-12-12,19:12:33\n../Python2v3/Orange_Icons.key,125356246,2016-12-17,15:44:57\n../SOLID python/01_01_d2_what_could_go_wrong copy.key,8627939,2016-04-18,14:24:31\n../SOLID python/01_01_what_could_go_wrong_sfl.key,11957033,2016-02-21,20:27:18\n../SOLID python/01_02_d2_what_could_go_wrong.key,6826203,2016-04-18,14:31:32\n../SOLID python/01_03_overview.key,14541373,2016-04-18,16:14:45\n../SOLID python/02_01_ISP.key,4938428,2016-04-18,16:47:58\n../SOLID python/02_02_ISP_Deck_Shoe.key,4300448,2016-04-18,17:34:44\n../SOLID python/02_03_wrap_v_extend.key,5823388,2016-04-18,17:57:51\n../SOLID python/02_04_extend.key,7499702,2016-04-18,18:20:30\n../SOLID python/03_01_LSP.key,3637628,2016-04-18,18:47:52\n../SOLID python/03_02_interface_variations.key,6872805,2016-04-22,10:48:08\n../SOLID python/03_03_cards.key,3676603,2016-04-19,12:31:08\n../SOLID python/03_04_defaults.key,3637506,2016-04-19,14:17:42\n../SOLID python/03_05_isinstance.key,3599543,2016-04-19,15:07:53\n../SOLID python/04_01_OCP.key,3658747,2016-04-19,15:17:38\n../SOLID python/04_02_bug_fix.key,10695446,2016-04-19,15:25:32\n../SOLID python/04_03_implementation.key,3644860,2016-04-20,12:31:41\n../SOLID python/04_04_implementation2.key,3671805,2016-04-20,12:43:27\n../SOLID python/05_01_dip.key,3716795,2016-04-19,17:16:15\n../SOLID python/05_02_injection.key,5287175,2016-04-19,17:20:48\n../SOLID python/05_03_testing.key,4836941,2016-04-19,16:54:12\n../SOLID python/06_01_srp.key,4194075,2016-04-21,14:07:21\n../SOLID python/06_02_grasp2.key,3631958,2016-04-21,14:00:02\n../SOLID python/06_03_grasp3.key,3656579,2016-04-21,15:23:59\n../SOLID python/07_01_process.key,5852212,2016-04-20,16:03:14\n../SOLID python/07_02_testing.key,6486325,2016-04-20,15:57:53\n../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key,25699644,2016-02-10,18:12:13\n'



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
    ../Python2v3/00_01_Introduction.key,3439648,2016-12-17,16:20:21
    ../Python2v3/00_02_Quick_Overview.key,12710870,2016-12-17,16:19:56
    ../Python2v3/00_03_Official_Howto.key,14264411,2016-12-17,16:21:01
    ../Python2v3/00_04_Differences.key,11018272,2016-12-21,13:18:50
    ../Python2v3/01_01_Syntax_Comparison.key,3078962,2016-12-17,16:22:48
    ../Python2v3/01_02_Type_Comparison.key,2964145,2016-12-17,16:23:43
    ../Python2v3/01_03_Builtins_Comparison.key,2969941,2016-12-17,16:24:54
    ../Python2v3/01_04_Other_Semantics.key,2931235,2016-12-17,16:28:18
    ../Python2v3/02_01_Strategies.key,19123686,2016-12-18,11:19:04
    ../Python2v3/02_02_Convert.key,9369086,2016-12-18,11:54:12
    ../Python2v3/02_03_Coordinate.key,9410504,2016-12-18,12:49:55
    ../Python2v3/02_04_Coexist_six.key,10806912,2016-12-18,13:20:25
    ../Python2v3/02_05_Coexist_2to3.key,9448602,2016-12-18,13:54:06
    ../Python2v3/02_06_Tooling.key,6146437,2016-12-18,14:09:29
    ../Python2v3/03_01_Six.key,2863992,2016-12-22,16:03:29
    ../Python2v3/03_02_Syntax.key,2853481,2016-12-23,11:50:56
    ../Python2v3/03_03_Classes.key,2832664,2016-12-22,21:42:10
    ../Python2v3/03_04_Builtins.key,2885234,2016-12-18,18:03:48
    ../Python2v3/03_05_Library.key,2838517,2016-12-23,08:15:10
    ../Python2v3/04_01_Future.key,2902153,2016-12-23,10:46:44
    ../Python2v3/04_02_Syntax.key,2868553,2016-12-23,11:59:19
    ../Python2v3/04_03_Classes.key,2852716,2016-12-23,13:32:15
    ../Python2v3/04_04_Builtins.key,2858460,2016-12-24,09:02:44
    ../Python2v3/04_05_Library.key,2823998,2016-12-24,13:13:40
    ../Python2v3/05_01_2to3.key,2921014,2016-12-24,13:46:55
    ../Python2v3/05_02_Syntax_Classes.key,2918938,2016-12-24,12:17:07
    ../Python2v3/05_03_Builtins_Library.key,2923791,2016-12-24,13:15:04
    ../Python2v3/05_04_Wrap_Up.key,6134784,2016-12-24,15:10:26
    ../Python2v3/BLUE TEMPLATE.key,32352638,2016-12-12,19:12:33
    ../Python2v3/Orange_Icons.key,125356246,2016-12-17,15:44:57
    ../SOLID python/01_01_d2_what_could_go_wrong copy.key,8627939,2016-04-18,14:24:31
    ../SOLID python/01_01_what_could_go_wrong_sfl.key,11957033,2016-02-21,20:27:18
    ../SOLID python/01_02_d2_what_could_go_wrong.key,6826203,2016-04-18,14:31:32
    ../SOLID python/01_03_overview.key,14541373,2016-04-18,16:14:45
    ../SOLID python/02_01_ISP.key,4938428,2016-04-18,16:47:58
    ../SOLID python/02_02_ISP_Deck_Shoe.key,4300448,2016-04-18,17:34:44
    ../SOLID python/02_03_wrap_v_extend.key,5823388,2016-04-18,17:57:51
    ../SOLID python/02_04_extend.key,7499702,2016-04-18,18:20:30
    ../SOLID python/03_01_LSP.key,3637628,2016-04-18,18:47:52
    ../SOLID python/03_02_interface_variations.key,6872805,2016-04-22,10:48:08
    ../SOLID python/03_03_cards.key,3676603,2016-04-19,12:31:08
    ../SOLID python/03_04_defaults.key,3637506,2016-04-19,14:17:42
    ../SOLID python/03_05_isinstance.key,3599543,2016-04-19,15:07:53
    ../SOLID python/04_01_OCP.key,3658747,2016-04-19,15:17:38
    ../SOLID python/04_02_bug_fix.key,10695446,2016-04-19,15:25:32
    ../SOLID python/04_03_implementation.key,3644860,2016-04-20,12:31:41
    ../SOLID python/04_04_implementation2.key,3671805,2016-04-20,12:43:27
    ../SOLID python/05_01_dip.key,3716795,2016-04-19,17:16:15
    ../SOLID python/05_02_injection.key,5287175,2016-04-19,17:20:48
    ../SOLID python/05_03_testing.key,4836941,2016-04-19,16:54:12
    ../SOLID python/06_01_srp.key,4194075,2016-04-21,14:07:21
    ../SOLID python/06_02_grasp2.key,3631958,2016-04-21,14:00:02
    ../SOLID python/06_03_grasp3.key,3656579,2016-04-21,15:23:59
    ../SOLID python/07_01_process.key,5852212,2016-04-20,16:03:14
    ../SOLID python/07_02_testing.key,6486325,2016-04-20,15:57:53
    ../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key,25699644,2016-02-10,18:12:13
    


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
    
    ../Python2v3/00_01_Introduction.key,3439648,2016-12-17,16:20:21
    
    ../Python2v3/00_02_Quick_Overview.key,12710870,2016-12-17,16:19:56
    
    ../Python2v3/00_03_Official_Howto.key,14264411,2016-12-17,16:21:01
    
    ../Python2v3/00_04_Differences.key,11018272,2016-12-21,13:18:50
    
    ../Python2v3/01_01_Syntax_Comparison.key,3078962,2016-12-17,16:22:48
    
    ../Python2v3/01_02_Type_Comparison.key,2964145,2016-12-17,16:23:43
    
    ../Python2v3/01_03_Builtins_Comparison.key,2969941,2016-12-17,16:24:54
    
    ../Python2v3/01_04_Other_Semantics.key,2931235,2016-12-17,16:28:18
    
    ../Python2v3/02_01_Strategies.key,19123686,2016-12-18,11:19:04
    
    ../Python2v3/02_02_Convert.key,9369086,2016-12-18,11:54:12
    
    ../Python2v3/02_03_Coordinate.key,9410504,2016-12-18,12:49:55
    
    ../Python2v3/02_04_Coexist_six.key,10806912,2016-12-18,13:20:25
    
    ../Python2v3/02_05_Coexist_2to3.key,9448602,2016-12-18,13:54:06
    
    ../Python2v3/02_06_Tooling.key,6146437,2016-12-18,14:09:29
    
    ../Python2v3/03_01_Six.key,2863992,2016-12-22,16:03:29
    
    ../Python2v3/03_02_Syntax.key,2853481,2016-12-23,11:50:56
    
    ../Python2v3/03_03_Classes.key,2832664,2016-12-22,21:42:10
    
    ../Python2v3/03_04_Builtins.key,2885234,2016-12-18,18:03:48
    
    ../Python2v3/03_05_Library.key,2838517,2016-12-23,08:15:10
    
    ../Python2v3/04_01_Future.key,2902153,2016-12-23,10:46:44
    
    ../Python2v3/04_02_Syntax.key,2868553,2016-12-23,11:59:19
    
    ../Python2v3/04_03_Classes.key,2852716,2016-12-23,13:32:15
    
    ../Python2v3/04_04_Builtins.key,2858460,2016-12-24,09:02:44
    
    ../Python2v3/04_05_Library.key,2823998,2016-12-24,13:13:40
    
    ../Python2v3/05_01_2to3.key,2921014,2016-12-24,13:46:55
    
    ../Python2v3/05_02_Syntax_Classes.key,2918938,2016-12-24,12:17:07
    
    ../Python2v3/05_03_Builtins_Library.key,2923791,2016-12-24,13:15:04
    
    ../Python2v3/05_04_Wrap_Up.key,6134784,2016-12-24,15:10:26
    
    ../Python2v3/BLUE TEMPLATE.key,32352638,2016-12-12,19:12:33
    
    ../Python2v3/Orange_Icons.key,125356246,2016-12-17,15:44:57
    
    ../SOLID python/01_01_d2_what_could_go_wrong copy.key,8627939,2016-04-18,14:24:31
    
    ../SOLID python/01_01_what_could_go_wrong_sfl.key,11957033,2016-02-21,20:27:18
    
    ../SOLID python/01_02_d2_what_could_go_wrong.key,6826203,2016-04-18,14:31:32
    
    ../SOLID python/01_03_overview.key,14541373,2016-04-18,16:14:45
    
    ../SOLID python/02_01_ISP.key,4938428,2016-04-18,16:47:58
    
    ../SOLID python/02_02_ISP_Deck_Shoe.key,4300448,2016-04-18,17:34:44
    
    ../SOLID python/02_03_wrap_v_extend.key,5823388,2016-04-18,17:57:51
    
    ../SOLID python/02_04_extend.key,7499702,2016-04-18,18:20:30
    
    ../SOLID python/03_01_LSP.key,3637628,2016-04-18,18:47:52
    
    ../SOLID python/03_02_interface_variations.key,6872805,2016-04-22,10:48:08
    
    ../SOLID python/03_03_cards.key,3676603,2016-04-19,12:31:08
    
    ../SOLID python/03_04_defaults.key,3637506,2016-04-19,14:17:42
    
    ../SOLID python/03_05_isinstance.key,3599543,2016-04-19,15:07:53
    
    ../SOLID python/04_01_OCP.key,3658747,2016-04-19,15:17:38
    
    ../SOLID python/04_02_bug_fix.key,10695446,2016-04-19,15:25:32
    
    ../SOLID python/04_03_implementation.key,3644860,2016-04-20,12:31:41
    
    ../SOLID python/04_04_implementation2.key,3671805,2016-04-20,12:43:27
    
    ../SOLID python/05_01_dip.key,3716795,2016-04-19,17:16:15
    
    ../SOLID python/05_02_injection.key,5287175,2016-04-19,17:20:48
    
    ../SOLID python/05_03_testing.key,4836941,2016-04-19,16:54:12
    
    ../SOLID python/06_01_srp.key,4194075,2016-04-21,14:07:21
    
    ../SOLID python/06_02_grasp2.key,3631958,2016-04-21,14:00:02
    
    ../SOLID python/06_03_grasp3.key,3656579,2016-04-21,15:23:59
    
    ../SOLID python/07_01_process.key,5852212,2016-04-20,16:03:14
    
    ../SOLID python/07_02_testing.key,6486325,2016-04-20,15:57:53
    
    ../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key,25699644,2016-02-10,18:12:13
    


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
    ['../Python2v3/00_01_Introduction.key', '3439648', '2016-12-17', '16:20:21']
    ['../Python2v3/00_02_Quick_Overview.key', '12710870', '2016-12-17', '16:19:56']
    ['../Python2v3/00_03_Official_Howto.key', '14264411', '2016-12-17', '16:21:01']
    ['../Python2v3/00_04_Differences.key', '11018272', '2016-12-21', '13:18:50']
    ['../Python2v3/01_01_Syntax_Comparison.key', '3078962', '2016-12-17', '16:22:48']
    ['../Python2v3/01_02_Type_Comparison.key', '2964145', '2016-12-17', '16:23:43']
    ['../Python2v3/01_03_Builtins_Comparison.key', '2969941', '2016-12-17', '16:24:54']
    ['../Python2v3/01_04_Other_Semantics.key', '2931235', '2016-12-17', '16:28:18']
    ['../Python2v3/02_01_Strategies.key', '19123686', '2016-12-18', '11:19:04']
    ['../Python2v3/02_02_Convert.key', '9369086', '2016-12-18', '11:54:12']
    ['../Python2v3/02_03_Coordinate.key', '9410504', '2016-12-18', '12:49:55']
    ['../Python2v3/02_04_Coexist_six.key', '10806912', '2016-12-18', '13:20:25']
    ['../Python2v3/02_05_Coexist_2to3.key', '9448602', '2016-12-18', '13:54:06']
    ['../Python2v3/02_06_Tooling.key', '6146437', '2016-12-18', '14:09:29']
    ['../Python2v3/03_01_Six.key', '2863992', '2016-12-22', '16:03:29']
    ['../Python2v3/03_02_Syntax.key', '2853481', '2016-12-23', '11:50:56']
    ['../Python2v3/03_03_Classes.key', '2832664', '2016-12-22', '21:42:10']
    ['../Python2v3/03_04_Builtins.key', '2885234', '2016-12-18', '18:03:48']
    ['../Python2v3/03_05_Library.key', '2838517', '2016-12-23', '08:15:10']
    ['../Python2v3/04_01_Future.key', '2902153', '2016-12-23', '10:46:44']
    ['../Python2v3/04_02_Syntax.key', '2868553', '2016-12-23', '11:59:19']
    ['../Python2v3/04_03_Classes.key', '2852716', '2016-12-23', '13:32:15']
    ['../Python2v3/04_04_Builtins.key', '2858460', '2016-12-24', '09:02:44']
    ['../Python2v3/04_05_Library.key', '2823998', '2016-12-24', '13:13:40']
    ['../Python2v3/05_01_2to3.key', '2921014', '2016-12-24', '13:46:55']
    ['../Python2v3/05_02_Syntax_Classes.key', '2918938', '2016-12-24', '12:17:07']
    ['../Python2v3/05_03_Builtins_Library.key', '2923791', '2016-12-24', '13:15:04']
    ['../Python2v3/05_04_Wrap_Up.key', '6134784', '2016-12-24', '15:10:26']
    ['../Python2v3/BLUE TEMPLATE.key', '32352638', '2016-12-12', '19:12:33']
    ['../Python2v3/Orange_Icons.key', '125356246', '2016-12-17', '15:44:57']
    ['../SOLID python/01_01_d2_what_could_go_wrong copy.key', '8627939', '2016-04-18', '14:24:31']
    ['../SOLID python/01_01_what_could_go_wrong_sfl.key', '11957033', '2016-02-21', '20:27:18']
    ['../SOLID python/01_02_d2_what_could_go_wrong.key', '6826203', '2016-04-18', '14:31:32']
    ['../SOLID python/01_03_overview.key', '14541373', '2016-04-18', '16:14:45']
    ['../SOLID python/02_01_ISP.key', '4938428', '2016-04-18', '16:47:58']
    ['../SOLID python/02_02_ISP_Deck_Shoe.key', '4300448', '2016-04-18', '17:34:44']
    ['../SOLID python/02_03_wrap_v_extend.key', '5823388', '2016-04-18', '17:57:51']
    ['../SOLID python/02_04_extend.key', '7499702', '2016-04-18', '18:20:30']
    ['../SOLID python/03_01_LSP.key', '3637628', '2016-04-18', '18:47:52']
    ['../SOLID python/03_02_interface_variations.key', '6872805', '2016-04-22', '10:48:08']
    ['../SOLID python/03_03_cards.key', '3676603', '2016-04-19', '12:31:08']
    ['../SOLID python/03_04_defaults.key', '3637506', '2016-04-19', '14:17:42']
    ['../SOLID python/03_05_isinstance.key', '3599543', '2016-04-19', '15:07:53']
    ['../SOLID python/04_01_OCP.key', '3658747', '2016-04-19', '15:17:38']
    ['../SOLID python/04_02_bug_fix.key', '10695446', '2016-04-19', '15:25:32']
    ['../SOLID python/04_03_implementation.key', '3644860', '2016-04-20', '12:31:41']
    ['../SOLID python/04_04_implementation2.key', '3671805', '2016-04-20', '12:43:27']
    ['../SOLID python/05_01_dip.key', '3716795', '2016-04-19', '17:16:15']
    ['../SOLID python/05_02_injection.key', '5287175', '2016-04-19', '17:20:48']
    ['../SOLID python/05_03_testing.key', '4836941', '2016-04-19', '16:54:12']
    ['../SOLID python/06_01_srp.key', '4194075', '2016-04-21', '14:07:21']
    ['../SOLID python/06_02_grasp2.key', '3631958', '2016-04-21', '14:00:02']
    ['../SOLID python/06_03_grasp3.key', '3656579', '2016-04-21', '15:23:59']
    ['../SOLID python/07_01_process.key', '5852212', '2016-04-20', '16:03:14']
    ['../SOLID python/07_02_testing.key', '6486325', '2016-04-20', '15:57:53']
    ['../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key', '25699644', '2016-02-10', '18:12:13']


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
    name= ../Python2v3/00_01_Introduction.key size= 3439648 date,time= 2016-12-17 16:20:21
    name= ../Python2v3/00_02_Quick_Overview.key size= 12710870 date,time= 2016-12-17 16:19:56
    name= ../Python2v3/00_03_Official_Howto.key size= 14264411 date,time= 2016-12-17 16:21:01
    name= ../Python2v3/00_04_Differences.key size= 11018272 date,time= 2016-12-21 13:18:50
    name= ../Python2v3/01_01_Syntax_Comparison.key size= 3078962 date,time= 2016-12-17 16:22:48
    name= ../Python2v3/01_02_Type_Comparison.key size= 2964145 date,time= 2016-12-17 16:23:43
    name= ../Python2v3/01_03_Builtins_Comparison.key size= 2969941 date,time= 2016-12-17 16:24:54
    name= ../Python2v3/01_04_Other_Semantics.key size= 2931235 date,time= 2016-12-17 16:28:18
    name= ../Python2v3/02_01_Strategies.key size= 19123686 date,time= 2016-12-18 11:19:04
    name= ../Python2v3/02_02_Convert.key size= 9369086 date,time= 2016-12-18 11:54:12
    name= ../Python2v3/02_03_Coordinate.key size= 9410504 date,time= 2016-12-18 12:49:55
    name= ../Python2v3/02_04_Coexist_six.key size= 10806912 date,time= 2016-12-18 13:20:25
    name= ../Python2v3/02_05_Coexist_2to3.key size= 9448602 date,time= 2016-12-18 13:54:06
    name= ../Python2v3/02_06_Tooling.key size= 6146437 date,time= 2016-12-18 14:09:29
    name= ../Python2v3/03_01_Six.key size= 2863992 date,time= 2016-12-22 16:03:29
    name= ../Python2v3/03_02_Syntax.key size= 2853481 date,time= 2016-12-23 11:50:56
    name= ../Python2v3/03_03_Classes.key size= 2832664 date,time= 2016-12-22 21:42:10
    name= ../Python2v3/03_04_Builtins.key size= 2885234 date,time= 2016-12-18 18:03:48
    name= ../Python2v3/03_05_Library.key size= 2838517 date,time= 2016-12-23 08:15:10
    name= ../Python2v3/04_01_Future.key size= 2902153 date,time= 2016-12-23 10:46:44
    name= ../Python2v3/04_02_Syntax.key size= 2868553 date,time= 2016-12-23 11:59:19
    name= ../Python2v3/04_03_Classes.key size= 2852716 date,time= 2016-12-23 13:32:15
    name= ../Python2v3/04_04_Builtins.key size= 2858460 date,time= 2016-12-24 09:02:44
    name= ../Python2v3/04_05_Library.key size= 2823998 date,time= 2016-12-24 13:13:40
    name= ../Python2v3/05_01_2to3.key size= 2921014 date,time= 2016-12-24 13:46:55
    name= ../Python2v3/05_02_Syntax_Classes.key size= 2918938 date,time= 2016-12-24 12:17:07
    name= ../Python2v3/05_03_Builtins_Library.key size= 2923791 date,time= 2016-12-24 13:15:04
    name= ../Python2v3/05_04_Wrap_Up.key size= 6134784 date,time= 2016-12-24 15:10:26
    name= ../Python2v3/BLUE TEMPLATE.key size= 32352638 date,time= 2016-12-12 19:12:33
    name= ../Python2v3/Orange_Icons.key size= 125356246 date,time= 2016-12-17 15:44:57
    name= ../SOLID python/01_01_d2_what_could_go_wrong copy.key size= 8627939 date,time= 2016-04-18 14:24:31
    name= ../SOLID python/01_01_what_could_go_wrong_sfl.key size= 11957033 date,time= 2016-02-21 20:27:18
    name= ../SOLID python/01_02_d2_what_could_go_wrong.key size= 6826203 date,time= 2016-04-18 14:31:32
    name= ../SOLID python/01_03_overview.key size= 14541373 date,time= 2016-04-18 16:14:45
    name= ../SOLID python/02_01_ISP.key size= 4938428 date,time= 2016-04-18 16:47:58
    name= ../SOLID python/02_02_ISP_Deck_Shoe.key size= 4300448 date,time= 2016-04-18 17:34:44
    name= ../SOLID python/02_03_wrap_v_extend.key size= 5823388 date,time= 2016-04-18 17:57:51
    name= ../SOLID python/02_04_extend.key size= 7499702 date,time= 2016-04-18 18:20:30
    name= ../SOLID python/03_01_LSP.key size= 3637628 date,time= 2016-04-18 18:47:52
    name= ../SOLID python/03_02_interface_variations.key size= 6872805 date,time= 2016-04-22 10:48:08
    name= ../SOLID python/03_03_cards.key size= 3676603 date,time= 2016-04-19 12:31:08
    name= ../SOLID python/03_04_defaults.key size= 3637506 date,time= 2016-04-19 14:17:42
    name= ../SOLID python/03_05_isinstance.key size= 3599543 date,time= 2016-04-19 15:07:53
    name= ../SOLID python/04_01_OCP.key size= 3658747 date,time= 2016-04-19 15:17:38
    name= ../SOLID python/04_02_bug_fix.key size= 10695446 date,time= 2016-04-19 15:25:32
    name= ../SOLID python/04_03_implementation.key size= 3644860 date,time= 2016-04-20 12:31:41
    name= ../SOLID python/04_04_implementation2.key size= 3671805 date,time= 2016-04-20 12:43:27
    name= ../SOLID python/05_01_dip.key size= 3716795 date,time= 2016-04-19 17:16:15
    name= ../SOLID python/05_02_injection.key size= 5287175 date,time= 2016-04-19 17:20:48
    name= ../SOLID python/05_03_testing.key size= 4836941 date,time= 2016-04-19 16:54:12
    name= ../SOLID python/06_01_srp.key size= 4194075 date,time= 2016-04-21 14:07:21
    name= ../SOLID python/06_02_grasp2.key size= 3631958 date,time= 2016-04-21 14:00:02
    name= ../SOLID python/06_03_grasp3.key size= 3656579 date,time= 2016-04-21 15:23:59
    name= ../SOLID python/07_01_process.key size= 5852212 date,time= 2016-04-20 16:03:14
    name= ../SOLID python/07_02_testing.key size= 6486325 date,time= 2016-04-20 15:57:53
    name= ../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key size= 25699644 date,time= 2016-02-10 18:12:13


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

We can filter that out by using the if statement. We can create a condition expression and provide some processing to be used only when that condition is true.

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
    ['../Python2v3/00_01_Introduction.key', '3439648', '2016-12-17', '16:20:21']
    ['../Python2v3/00_02_Quick_Overview.key', '12710870', '2016-12-17', '16:19:56']
    ['../Python2v3/00_03_Official_Howto.key', '14264411', '2016-12-17', '16:21:01']
    ['../Python2v3/00_04_Differences.key', '11018272', '2016-12-21', '13:18:50']
    ['../Python2v3/01_01_Syntax_Comparison.key', '3078962', '2016-12-17', '16:22:48']
    ['../Python2v3/01_02_Type_Comparison.key', '2964145', '2016-12-17', '16:23:43']
    ['../Python2v3/01_03_Builtins_Comparison.key', '2969941', '2016-12-17', '16:24:54']
    ['../Python2v3/01_04_Other_Semantics.key', '2931235', '2016-12-17', '16:28:18']
    ['../Python2v3/02_01_Strategies.key', '19123686', '2016-12-18', '11:19:04']
    ['../Python2v3/02_02_Convert.key', '9369086', '2016-12-18', '11:54:12']
    ['../Python2v3/02_03_Coordinate.key', '9410504', '2016-12-18', '12:49:55']
    ['../Python2v3/02_04_Coexist_six.key', '10806912', '2016-12-18', '13:20:25']
    ['../Python2v3/02_05_Coexist_2to3.key', '9448602', '2016-12-18', '13:54:06']
    ['../Python2v3/02_06_Tooling.key', '6146437', '2016-12-18', '14:09:29']
    ['../Python2v3/03_01_Six.key', '2863992', '2016-12-22', '16:03:29']
    ['../Python2v3/03_02_Syntax.key', '2853481', '2016-12-23', '11:50:56']
    ['../Python2v3/03_03_Classes.key', '2832664', '2016-12-22', '21:42:10']
    ['../Python2v3/03_04_Builtins.key', '2885234', '2016-12-18', '18:03:48']
    ['../Python2v3/03_05_Library.key', '2838517', '2016-12-23', '08:15:10']
    ['../Python2v3/04_01_Future.key', '2902153', '2016-12-23', '10:46:44']
    ['../Python2v3/04_02_Syntax.key', '2868553', '2016-12-23', '11:59:19']
    ['../Python2v3/04_03_Classes.key', '2852716', '2016-12-23', '13:32:15']
    ['../Python2v3/04_04_Builtins.key', '2858460', '2016-12-24', '09:02:44']
    ['../Python2v3/04_05_Library.key', '2823998', '2016-12-24', '13:13:40']
    ['../Python2v3/05_01_2to3.key', '2921014', '2016-12-24', '13:46:55']
    ['../Python2v3/05_02_Syntax_Classes.key', '2918938', '2016-12-24', '12:17:07']
    ['../Python2v3/05_03_Builtins_Library.key', '2923791', '2016-12-24', '13:15:04']
    ['../Python2v3/05_04_Wrap_Up.key', '6134784', '2016-12-24', '15:10:26']
    ['../Python2v3/BLUE TEMPLATE.key', '32352638', '2016-12-12', '19:12:33']
    ['../Python2v3/Orange_Icons.key', '125356246', '2016-12-17', '15:44:57']
    ['../SOLID python/01_01_d2_what_could_go_wrong copy.key', '8627939', '2016-04-18', '14:24:31']
    ['../SOLID python/01_01_what_could_go_wrong_sfl.key', '11957033', '2016-02-21', '20:27:18']
    ['../SOLID python/01_02_d2_what_could_go_wrong.key', '6826203', '2016-04-18', '14:31:32']
    ['../SOLID python/01_03_overview.key', '14541373', '2016-04-18', '16:14:45']
    ['../SOLID python/02_01_ISP.key', '4938428', '2016-04-18', '16:47:58']
    ['../SOLID python/02_02_ISP_Deck_Shoe.key', '4300448', '2016-04-18', '17:34:44']
    ['../SOLID python/02_03_wrap_v_extend.key', '5823388', '2016-04-18', '17:57:51']
    ['../SOLID python/02_04_extend.key', '7499702', '2016-04-18', '18:20:30']
    ['../SOLID python/03_01_LSP.key', '3637628', '2016-04-18', '18:47:52']
    ['../SOLID python/03_02_interface_variations.key', '6872805', '2016-04-22', '10:48:08']
    ['../SOLID python/03_03_cards.key', '3676603', '2016-04-19', '12:31:08']
    ['../SOLID python/03_04_defaults.key', '3637506', '2016-04-19', '14:17:42']
    ['../SOLID python/03_05_isinstance.key', '3599543', '2016-04-19', '15:07:53']
    ['../SOLID python/04_01_OCP.key', '3658747', '2016-04-19', '15:17:38']
    ['../SOLID python/04_02_bug_fix.key', '10695446', '2016-04-19', '15:25:32']
    ['../SOLID python/04_03_implementation.key', '3644860', '2016-04-20', '12:31:41']
    ['../SOLID python/04_04_implementation2.key', '3671805', '2016-04-20', '12:43:27']
    ['../SOLID python/05_01_dip.key', '3716795', '2016-04-19', '17:16:15']
    ['../SOLID python/05_02_injection.key', '5287175', '2016-04-19', '17:20:48']
    ['../SOLID python/05_03_testing.key', '4836941', '2016-04-19', '16:54:12']
    ['../SOLID python/06_01_srp.key', '4194075', '2016-04-21', '14:07:21']
    ['../SOLID python/06_02_grasp2.key', '3631958', '2016-04-21', '14:00:02']
    ['../SOLID python/06_03_grasp3.key', '3656579', '2016-04-21', '15:23:59']
    ['../SOLID python/07_01_process.key', '5852212', '2016-04-20', '16:03:14']
    ['../SOLID python/07_02_testing.key', '6486325', '2016-04-20', '15:57:53']
    ['../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key', '25699644', '2016-02-10', '18:12:13']


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
    ../Python2v3/00_01_Introduction.key              3439648   2016-12-17T16:20:21
    ../Python2v3/00_02_Quick_Overview.key            12710870  2016-12-17T16:19:56
    ../Python2v3/00_03_Official_Howto.key            14264411  2016-12-17T16:21:01
    ../Python2v3/00_04_Differences.key               11018272  2016-12-21T13:18:50
    ../Python2v3/01_01_Syntax_Comparison.key         3078962   2016-12-17T16:22:48
    ../Python2v3/01_02_Type_Comparison.key           2964145   2016-12-17T16:23:43
    ../Python2v3/01_03_Builtins_Comparison.key       2969941   2016-12-17T16:24:54
    ../Python2v3/01_04_Other_Semantics.key           2931235   2016-12-17T16:28:18
    ../Python2v3/02_01_Strategies.key                19123686  2016-12-18T11:19:04
    ../Python2v3/02_02_Convert.key                   9369086   2016-12-18T11:54:12
    ../Python2v3/02_03_Coordinate.key                9410504   2016-12-18T12:49:55
    ../Python2v3/02_04_Coexist_six.key               10806912  2016-12-18T13:20:25
    ../Python2v3/02_05_Coexist_2to3.key              9448602   2016-12-18T13:54:06
    ../Python2v3/02_06_Tooling.key                   6146437   2016-12-18T14:09:29
    ../Python2v3/03_01_Six.key                       2863992   2016-12-22T16:03:29
    ../Python2v3/03_02_Syntax.key                    2853481   2016-12-23T11:50:56
    ../Python2v3/03_03_Classes.key                   2832664   2016-12-22T21:42:10
    ../Python2v3/03_04_Builtins.key                  2885234   2016-12-18T18:03:48
    ../Python2v3/03_05_Library.key                   2838517   2016-12-23T08:15:10
    ../Python2v3/04_01_Future.key                    2902153   2016-12-23T10:46:44
    ../Python2v3/04_02_Syntax.key                    2868553   2016-12-23T11:59:19
    ../Python2v3/04_03_Classes.key                   2852716   2016-12-23T13:32:15
    ../Python2v3/04_04_Builtins.key                  2858460   2016-12-24T09:02:44
    ../Python2v3/04_05_Library.key                   2823998   2016-12-24T13:13:40
    ../Python2v3/05_01_2to3.key                      2921014   2016-12-24T13:46:55
    ../Python2v3/05_02_Syntax_Classes.key            2918938   2016-12-24T12:17:07
    ../Python2v3/05_03_Builtins_Library.key          2923791   2016-12-24T13:15:04
    ../Python2v3/05_04_Wrap_Up.key                   6134784   2016-12-24T15:10:26
    ../Python2v3/BLUE TEMPLATE.key                   32352638  2016-12-12T19:12:33
    ../Python2v3/Orange_Icons.key                    125356246 2016-12-17T15:44:57
    ../SOLID python/01_01_d2_what_could_go_wrong copy.key 8627939   2016-04-18T14:24:31
    ../SOLID python/01_01_what_could_go_wrong_sfl.key 11957033  2016-02-21T20:27:18
    ../SOLID python/01_02_d2_what_could_go_wrong.key 6826203   2016-04-18T14:31:32
    ../SOLID python/01_03_overview.key               14541373  2016-04-18T16:14:45
    ../SOLID python/02_01_ISP.key                    4938428   2016-04-18T16:47:58
    ../SOLID python/02_02_ISP_Deck_Shoe.key          4300448   2016-04-18T17:34:44
    ../SOLID python/02_03_wrap_v_extend.key          5823388   2016-04-18T17:57:51
    ../SOLID python/02_04_extend.key                 7499702   2016-04-18T18:20:30
    ../SOLID python/03_01_LSP.key                    3637628   2016-04-18T18:47:52
    ../SOLID python/03_02_interface_variations.key   6872805   2016-04-22T10:48:08
    ../SOLID python/03_03_cards.key                  3676603   2016-04-19T12:31:08
    ../SOLID python/03_04_defaults.key               3637506   2016-04-19T14:17:42
    ../SOLID python/03_05_isinstance.key             3599543   2016-04-19T15:07:53
    ../SOLID python/04_01_OCP.key                    3658747   2016-04-19T15:17:38
    ../SOLID python/04_02_bug_fix.key                10695446  2016-04-19T15:25:32
    ../SOLID python/04_03_implementation.key         3644860   2016-04-20T12:31:41
    ../SOLID python/04_04_implementation2.key        3671805   2016-04-20T12:43:27
    ../SOLID python/05_01_dip.key                    3716795   2016-04-19T17:16:15
    ../SOLID python/05_02_injection.key              5287175   2016-04-19T17:20:48
    ../SOLID python/05_03_testing.key                4836941   2016-04-19T16:54:12
    ../SOLID python/06_01_srp.key                    4194075   2016-04-21T14:07:21
    ../SOLID python/06_02_grasp2.key                 3631958   2016-04-21T14:00:02
    ../SOLID python/06_03_grasp3.key                 3656579   2016-04-21T15:23:59
    ../SOLID python/07_01_process.key                5852212   2016-04-20T16:03:14
    ../SOLID python/07_02_testing.key                6486325   2016-04-20T15:57:53
    ../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key 25699644  2016-02-10T18:12:13


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
    ../Python2v3/00_01_Introduction.key              3,439,648 2016-12-17T16:20:21
    ../Python2v3/00_02_Quick_Overview.key            12,710,870 2016-12-17T16:19:56
    ../Python2v3/00_03_Official_Howto.key            14,264,411 2016-12-17T16:21:01
    ../Python2v3/00_04_Differences.key               11,018,272 2016-12-21T13:18:50
    ../Python2v3/01_01_Syntax_Comparison.key         3,078,962 2016-12-17T16:22:48
    ../Python2v3/01_02_Type_Comparison.key           2,964,145 2016-12-17T16:23:43
    ../Python2v3/01_03_Builtins_Comparison.key       2,969,941 2016-12-17T16:24:54
    ../Python2v3/01_04_Other_Semantics.key           2,931,235 2016-12-17T16:28:18
    ../Python2v3/02_01_Strategies.key                19,123,686 2016-12-18T11:19:04
    ../Python2v3/02_02_Convert.key                   9,369,086 2016-12-18T11:54:12
    ../Python2v3/02_03_Coordinate.key                9,410,504 2016-12-18T12:49:55
    ../Python2v3/02_04_Coexist_six.key               10,806,912 2016-12-18T13:20:25
    ../Python2v3/02_05_Coexist_2to3.key              9,448,602 2016-12-18T13:54:06
    ../Python2v3/02_06_Tooling.key                   6,146,437 2016-12-18T14:09:29
    ../Python2v3/03_01_Six.key                       2,863,992 2016-12-22T16:03:29
    ../Python2v3/03_02_Syntax.key                    2,853,481 2016-12-23T11:50:56
    ../Python2v3/03_03_Classes.key                   2,832,664 2016-12-22T21:42:10
    ../Python2v3/03_04_Builtins.key                  2,885,234 2016-12-18T18:03:48
    ../Python2v3/03_05_Library.key                   2,838,517 2016-12-23T08:15:10
    ../Python2v3/04_01_Future.key                    2,902,153 2016-12-23T10:46:44
    ../Python2v3/04_02_Syntax.key                    2,868,553 2016-12-23T11:59:19
    ../Python2v3/04_03_Classes.key                   2,852,716 2016-12-23T13:32:15
    ../Python2v3/04_04_Builtins.key                  2,858,460 2016-12-24T09:02:44
    ../Python2v3/04_05_Library.key                   2,823,998 2016-12-24T13:13:40
    ../Python2v3/05_01_2to3.key                      2,921,014 2016-12-24T13:46:55
    ../Python2v3/05_02_Syntax_Classes.key            2,918,938 2016-12-24T12:17:07
    ../Python2v3/05_03_Builtins_Library.key          2,923,791 2016-12-24T13:15:04
    ../Python2v3/05_04_Wrap_Up.key                   6,134,784 2016-12-24T15:10:26
    ../Python2v3/BLUE TEMPLATE.key                   32,352,638 2016-12-12T19:12:33
    ../Python2v3/Orange_Icons.key                    125,356,246 2016-12-17T15:44:57
    ../SOLID python/01_01_d2_what_could_go_wrong copy.key 8,627,939 2016-04-18T14:24:31
    ../SOLID python/01_01_what_could_go_wrong_sfl.key 11,957,033 2016-02-21T20:27:18
    ../SOLID python/01_02_d2_what_could_go_wrong.key 6,826,203 2016-04-18T14:31:32
    ../SOLID python/01_03_overview.key               14,541,373 2016-04-18T16:14:45
    ../SOLID python/02_01_ISP.key                    4,938,428 2016-04-18T16:47:58
    ../SOLID python/02_02_ISP_Deck_Shoe.key          4,300,448 2016-04-18T17:34:44
    ../SOLID python/02_03_wrap_v_extend.key          5,823,388 2016-04-18T17:57:51
    ../SOLID python/02_04_extend.key                 7,499,702 2016-04-18T18:20:30
    ../SOLID python/03_01_LSP.key                    3,637,628 2016-04-18T18:47:52
    ../SOLID python/03_02_interface_variations.key   6,872,805 2016-04-22T10:48:08
    ../SOLID python/03_03_cards.key                  3,676,603 2016-04-19T12:31:08
    ../SOLID python/03_04_defaults.key               3,637,506 2016-04-19T14:17:42
    ../SOLID python/03_05_isinstance.key             3,599,543 2016-04-19T15:07:53
    ../SOLID python/04_01_OCP.key                    3,658,747 2016-04-19T15:17:38
    ../SOLID python/04_02_bug_fix.key                10,695,446 2016-04-19T15:25:32
    ../SOLID python/04_03_implementation.key         3,644,860 2016-04-20T12:31:41
    ../SOLID python/04_04_implementation2.key        3,671,805 2016-04-20T12:43:27
    ../SOLID python/05_01_dip.key                    3,716,795 2016-04-19T17:16:15
    ../SOLID python/05_02_injection.key              5,287,175 2016-04-19T17:20:48
    ../SOLID python/05_03_testing.key                4,836,941 2016-04-19T16:54:12
    ../SOLID python/06_01_srp.key                    4,194,075 2016-04-21T14:07:21
    ../SOLID python/06_02_grasp2.key                 3,631,958 2016-04-21T14:00:02
    ../SOLID python/06_03_grasp3.key                 3,656,579 2016-04-21T15:23:59
    ../SOLID python/07_01_process.key                5,852,212 2016-04-20T16:03:14
    ../SOLID python/07_02_testing.key                6,486,325 2016-04-20T15:57:53
    ../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key 25,699,644 2016-02-10T18:12:13


What's changed? We've computed a new variable, `size`, using the expression `int(the_row[1])`. The `int()` function will convert it's argument value to an integer. No matter what the argument is: floating-point value, complex number, fraction, string. Almost anything.

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
    ../Python2v3/00_01_Introduction.key              3,439,648 Sat Dec 17 16:20:21 2016
    ../Python2v3/00_02_Quick_Overview.key            12,710,870 Sat Dec 17 16:19:56 2016
    ../Python2v3/00_03_Official_Howto.key            14,264,411 Sat Dec 17 16:21:01 2016
    ../Python2v3/00_04_Differences.key               11,018,272 Wed Dec 21 13:18:50 2016
    ../Python2v3/01_01_Syntax_Comparison.key         3,078,962 Sat Dec 17 16:22:48 2016
    ../Python2v3/01_02_Type_Comparison.key           2,964,145 Sat Dec 17 16:23:43 2016
    ../Python2v3/01_03_Builtins_Comparison.key       2,969,941 Sat Dec 17 16:24:54 2016
    ../Python2v3/01_04_Other_Semantics.key           2,931,235 Sat Dec 17 16:28:18 2016
    ../Python2v3/02_01_Strategies.key                19,123,686 Sun Dec 18 11:19:04 2016
    ../Python2v3/02_02_Convert.key                   9,369,086 Sun Dec 18 11:54:12 2016
    ../Python2v3/02_03_Coordinate.key                9,410,504 Sun Dec 18 12:49:55 2016
    ../Python2v3/02_04_Coexist_six.key               10,806,912 Sun Dec 18 13:20:25 2016
    ../Python2v3/02_05_Coexist_2to3.key              9,448,602 Sun Dec 18 13:54:06 2016
    ../Python2v3/02_06_Tooling.key                   6,146,437 Sun Dec 18 14:09:29 2016
    ../Python2v3/03_01_Six.key                       2,863,992 Thu Dec 22 16:03:29 2016
    ../Python2v3/03_02_Syntax.key                    2,853,481 Fri Dec 23 11:50:56 2016
    ../Python2v3/03_03_Classes.key                   2,832,664 Thu Dec 22 21:42:10 2016
    ../Python2v3/03_04_Builtins.key                  2,885,234 Sun Dec 18 18:03:48 2016
    ../Python2v3/03_05_Library.key                   2,838,517 Fri Dec 23 08:15:10 2016
    ../Python2v3/04_01_Future.key                    2,902,153 Fri Dec 23 10:46:44 2016
    ../Python2v3/04_02_Syntax.key                    2,868,553 Fri Dec 23 11:59:19 2016
    ../Python2v3/04_03_Classes.key                   2,852,716 Fri Dec 23 13:32:15 2016
    ../Python2v3/04_04_Builtins.key                  2,858,460 Sat Dec 24 09:02:44 2016
    ../Python2v3/04_05_Library.key                   2,823,998 Sat Dec 24 13:13:40 2016
    ../Python2v3/05_01_2to3.key                      2,921,014 Sat Dec 24 13:46:55 2016
    ../Python2v3/05_02_Syntax_Classes.key            2,918,938 Sat Dec 24 12:17:07 2016
    ../Python2v3/05_03_Builtins_Library.key          2,923,791 Sat Dec 24 13:15:04 2016
    ../Python2v3/05_04_Wrap_Up.key                   6,134,784 Sat Dec 24 15:10:26 2016
    ../Python2v3/BLUE TEMPLATE.key                   32,352,638 Mon Dec 12 19:12:33 2016
    ../Python2v3/Orange_Icons.key                    125,356,246 Sat Dec 17 15:44:57 2016
    ../SOLID python/01_01_d2_what_could_go_wrong copy.key 8,627,939 Mon Apr 18 14:24:31 2016
    ../SOLID python/01_01_what_could_go_wrong_sfl.key 11,957,033 Sun Feb 21 20:27:18 2016
    ../SOLID python/01_02_d2_what_could_go_wrong.key 6,826,203 Mon Apr 18 14:31:32 2016
    ../SOLID python/01_03_overview.key               14,541,373 Mon Apr 18 16:14:45 2016
    ../SOLID python/02_01_ISP.key                    4,938,428 Mon Apr 18 16:47:58 2016
    ../SOLID python/02_02_ISP_Deck_Shoe.key          4,300,448 Mon Apr 18 17:34:44 2016
    ../SOLID python/02_03_wrap_v_extend.key          5,823,388 Mon Apr 18 17:57:51 2016
    ../SOLID python/02_04_extend.key                 7,499,702 Mon Apr 18 18:20:30 2016
    ../SOLID python/03_01_LSP.key                    3,637,628 Mon Apr 18 18:47:52 2016
    ../SOLID python/03_02_interface_variations.key   6,872,805 Fri Apr 22 10:48:08 2016
    ../SOLID python/03_03_cards.key                  3,676,603 Tue Apr 19 12:31:08 2016
    ../SOLID python/03_04_defaults.key               3,637,506 Tue Apr 19 14:17:42 2016
    ../SOLID python/03_05_isinstance.key             3,599,543 Tue Apr 19 15:07:53 2016
    ../SOLID python/04_01_OCP.key                    3,658,747 Tue Apr 19 15:17:38 2016
    ../SOLID python/04_02_bug_fix.key                10,695,446 Tue Apr 19 15:25:32 2016
    ../SOLID python/04_03_implementation.key         3,644,860 Wed Apr 20 12:31:41 2016
    ../SOLID python/04_04_implementation2.key        3,671,805 Wed Apr 20 12:43:27 2016
    ../SOLID python/05_01_dip.key                    3,716,795 Tue Apr 19 17:16:15 2016
    ../SOLID python/05_02_injection.key              5,287,175 Tue Apr 19 17:20:48 2016
    ../SOLID python/05_03_testing.key                4,836,941 Tue Apr 19 16:54:12 2016
    ../SOLID python/06_01_srp.key                    4,194,075 Thu Apr 21 14:07:21 2016
    ../SOLID python/06_02_grasp2.key                 3,631,958 Thu Apr 21 14:00:02 2016
    ../SOLID python/06_03_grasp3.key                 3,656,579 Thu Apr 21 15:23:59 2016
    ../SOLID python/07_01_process.key                5,852,212 Wed Apr 20 16:03:14 2016
    ../SOLID python/07_02_testing.key                6,486,325 Wed Apr 20 15:57:53 2016
    ../SOLID python/GREEN KEYNOTE TEMPLATE_1360_768.key 25,699,644 Wed Feb 10 18:12:13 2016


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
