---
title: "Processing Multiple Files and Writing Files"
teaching: 25
exercises: 10
questions:
- "How do I analyze multiple files at once?"
objectives:
- "Import a python library."
- "Use python library funtions."
- "Process multiple files using a `for` loop."
- "Print output to a new text file."
keypoints:
- "Use the glob function in the python library glob to find all the files you want to analyze."
- "You can have multiple `for` loops nested inside each other."
- "Python can only print strings to files."
- "Don't forget to close files so python will actually write them."
---
## Processing multiple files

In our previous lesson, we parsed values from output files.  While you might have seen the utility of doing such a thing, you might have also wondered why we didn't just search the file and cut and paste the values we wanted into a spreadsheet.  If you only have 1 or 2 files, this might be a very reasonable thing to do.  But what if you had 100 files to analyze?  What if you had 1000?  In such a case the cutting and pasting method would be very tedious and time consuming.  

One of the real powers of writing a program to analyze your data is that you can just as easily analyze 100 files as 1 file.  In this example, we are going to parse the output files for a whole series of aliphatic alcohol compounds and parse the energy value for each one.  The output files are all saved in a folder called outfiles that you should have downloaded in the setup for this lesson.  Make sure the folder is in the same directory as the directory where you are writing and executing your code.

To analyze multiple files, we will need to import a python **library**.  A **library** is a set of modules which contain functions. The functions within a library or module are usually related to one another. Using libraries and in Python reduces the amount of code you have to write. In the last lesson, we imported `os.path`, which was a module that handled filepaths for us.

In this lesson, we will be using the `glob` library, which will help us read in multiple files from our computer.  Within a library there are  modules and functions which do a specific computational task.  Usually a function has some type of input and gives a particular output.  To use a function that is in a library, you often use the dot notation introduced in the previous lesson.  In general
```
import library_name
output = library_name.funtion_name(input)
```
{: .language-python}

We are going to import two libraries.  One is the `os` library which controls functions related to the operating system of your computer. We used this library in the last lesson to handle filepaths.  The other is the `glob` library which contains functions to help us analyze multiple files.  If we are going to analyze multiple files, we first need to specify where those files are located.

>## Exercise
>
> How would you use the `os.path` module to point to the directory where your outfiles are?
>
>> ## Solution
>> ~~~
>> outfile_directory = os.path.join('data', 'outfiles')
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

In order to get all of the files which match a specific pattern, we will use the wildcard character `*`.

```
file_location = os.path.join('data', 'outfiles', '*.out')
print(file_location)
```
{: .language-python}
```
data/outfiles/*.out
```
{: .output}

This specifies that we want to look for all the files in a directory called `data/outfiles` that end in ".out".  The * is the wildcard character which matches any character.  

Next we are going to use a function called `glob` in the library called `glob`.  It is a little confusing since the function and the library have the same name, but we will see other examples where this is not the case later.  The output of the function `glob` is a list of all the filenames that fit the pattern specified in the input.   The input is the file location.
```
import glob
filenames = glob.glob(file_location)
print(filenames)
```
{: .language-python}
```
['data/outfiles/butanol.out', 'data/outfiles/decanol.out', 'data/outfiles/ethanol.out', 'data/outfiles/heptanol.out', 'data/outfiles/hexanol.out', 'data/outfiles/methanol.out', 'data/outfiles/nonanol.out', 'data/outfiles/octanol.out', 'data/outfiles/pentanol.out', 'data/outfiles/propanol.out']
```
{: .output}

Now if we want to parse every file we just read in, we will use a `for` loop to go through each file.
```
for f in filenames:
    outfile = open(f,'r')
    data = outfile.readlines()
    for line in data:
        if 'Final Energy' in line:
            energy_line = line
            words = energy_line.split()
            energy = float(words[3])
            print(energy)
```
{: .language-python}
```
-232.1655798347283
-466.3836241400086
-154.09130176573018
-349.27397687072676
-310.2385332251633
-115.04800861868374
-427.3465180082815
-388.3110864554743
-271.20138119895074
-193.12836249728798
```
{: .output}

Notice that in this code we actually used two `for` loops, one nested inside the other.  The outer `for` loop counts over the filenames we read in earlier.  The inner `for` loop counts over the line in each file, just as we did in our previous file parsing lesson.  

The output our code currently generates is not that useful.  It doesn't show us which file each energy value came from.  

We want to print the name of the molecule with the energy. We can use `os.path.basename`, which is another function in `os.path` to get just the name of the file.

~~~
first_file = filenames[0]
print(first_file)

first_molecule = os.path.basename(first_file)
print(first_molecule)
~~~
{: .language-python}

~~~
data/outfiles/propanol.out
propanol.out
~~~
{: .output}

> ## Exercise
>
> How would you extract the molecule name from the example above?
> ~~~
> test_file = filenames[0]
> print(test_file)
> ~~~
> {: .language-python}
>
>> ## Solution
>> You can use the `str.split` function introduced in the last lesson, and split at the '.' character.
>> ~~~
>> split_filename = first_molecule.split('.')
>> molecule_name = split_filename[0]
>> print(molecule_name)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


Modify the code such that it prints the file name along with each energy value.  Each filename will be of the form "outfile/filename".  See if you can remove "outfile/" when you print your results.
>
>> ## Solution
>> ~~~
>> for f in filenames:
>>    outfile = open(f,'r')
>>    data=outfile.readlines()
>>    for line in data:
>>        if 'Final Energy' in line:
>>            energy_line = line
>>            words = energy_line.split()
>>            energy = float(words[3])
>>            print(f[9:], energy)
>> ~~~
>> {: .language-python}
>> ~~~
>> butanol.out -232.1655798347283
>> decanol.out -466.3836241400086
>> ethanol.out -154.09130176573018
>> heptanol.out -349.27397687072676
>> hexanol.out -310.2385332251633
>> methanol.out -115.04800861868374
>> nonanol.out -427.3465180082815
>> octanol.out -388.3110864554743
>> pentanol.out -271.20138119895074
>> propanol.out -193.12836249728798
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

## Printing to a File
Finally, it might be useful to print our results in a new file, such that we could share our results with colleagues or or e-mail them to our advisor.  Much like when we read in a file, the first step to writing output to a file is opening that file for writing.  In general to open a file for writing you use the syntax
```
filehandle = open('file_name.txt', 'w+')
```
{: .language-python}

The `w` means open the file for writing.  If you use `w+` that means open the file for writing and if the file does not exist, create it.  You can also use `a` for append to an existing file or `a+`.  The difference between `w+` and `a+` is that `w+` will overwrite the file if it already exists, whereas `a+` will keep what is already there and just add additional text to the file.  

Python can only write strings to files.  Our current print statement is not a string; it prints two python variables.  To convert what we have now to a string, you place a capital **F** in front of the line you want to print and enclose it in single quotes.  Each python variable is placed in braces. Then you can either print the line (as we have done before) or you can use the `filehandle.write()` command to print it to a file.

```
datafile = open('energies.txt','w+')  #This opens the file for writing
for f in filenames:
    outfile = open(f,'r')
    data=outfile.readlines()
    for line in data:
        if 'Final Energy' in line:
            energy_line = line
            words = energy_line.split()
            energy = float(words[3])
            datafile.write(F'{f[9:]} {energy}\n')  #Prints to file
datafile.close()
```
{: .language-python}

After you run this command, look in the directory where you ran your code and find the "energies.txt" file.  Open it in a text editor and look at the file.

In the file writing line, notice the `\n` at the end of the line.  This is the newline character.  Without it, the text in our file would just be all smushed together on one line.  Also, the `filehandle.close()` command is very important.  Think about a computer as someone who has a very good memory, but is very slow at writing.  Therefore, when you tell the computer to write a line, it remembers what you want it to write, but it doesn't actually write the new file until you tell it you are finished.   The `datafile.close()` command tells the computer you are finished giving it lines to write and that it should go ahead and write the file now.  If you are trying to write a file and the file keeps coming up empty, it is probably because you forgot to close the file.  

## A final note about string formatting
The F'string' notation that you can use with the print or the write command lets you format strings in many ways.  You could include other words or whole sentences.  For example, we could change the file writing line to
```
datafile.write(F'For the file {f[9:]} the energy is {energy} in kcal/mole.')
```
{: .language-python}
where anything in the braces is a python variable and it will print the value of that variable.  

{% include links.md %}
