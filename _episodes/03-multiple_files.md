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

This will give us a list of all the files which end in `*.out` in the `outfiles` directory. Now if we want to parse every file we just read in, we will use a `for` loop to go through each file.
```
for f in filenames:
    with open(f,'r') as data:
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

file_name = os.path.basename(first_file)
print(file_name)
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
>
>> ## Solution
>> You can use the `str.split` function introduced in the last lesson, and split at the '.' character.
>> ~~~
>> split_filename = file_name.split('.')
>> molecule_name = split_filename[0]
>> print(molecule_name)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Using the solution above, we can modify our loop so that it prints the file name along with each energy value.

~~~
for f in filenames:
    # Get the molecule name
    file_name = os.path.basename(f)
    split_filname = file_name.split('.')
    molecule_name = split_filename[0]

    # Read the data and loop through the data:
    with open(f,'r') as data:
        for line in data:
            if 'Final Energy' in line:
                energy_line = line
                words = energy_line.split()
                energy = float(words[3])
                print(molecule_name, energy)
~~~
{: .language-python}

~~~
propanol -193.12836249728798
pentanol -271.20138119895074
decanol -466.3836241400086
methanol -115.04800861868374
octanol -388.3110864554743
ethanol -154.09130176573018
hexanol -310.2385332251633
heptanol -349.27397687072676
butanol -232.1655798347283
nonanol -427.3465180082815
~~~
{: .output}


## Printing to a File
Finally, it might be useful to print our results in a new file, such that we could share our results with colleagues or or e-mail them to our advisor.  Much like when we read in a file, the first step to writing output to a file is opening that file for writing.  In general to open a file for writing you use the syntax
```
filehandle = open('file_name.txt', 'w+')
```
{: .language-python}

The `w` means open the file for writing.  If you use `w+` that means open the file for writing and if the file does not exist, create it.  You can also use `a` for append to an existing file or `a+`.  The difference between `w+` and `a+` is that `w+` will overwrite the file if it already exists, whereas `a+` will keep what is already there and just add additional text to the file.  

Python can only write strings to files.  Our current print statement is not a string; it prints two python variables.  To convert what we have now to a string, you place a capital **F** in front of the line you want to print and enclose it in single quotes.  Each python variable is placed in braces. Then you can either print the line (as we have done before) or you can use the `filehandle.write()` command to print it to a file.

To make the printing neater, we will separate the file name from the energy using a tab. To insert a tab, we use the special character `\t`.

```
with open('energies.txt','w+') as datafile:     #This opens the file for writing
    for f in filenames:
        # Get the molecule name
        file_name = os.path.basename(f)
        split_filename = file_name.split('.')
        molecule_name = split_filename[0]

        # Read the data and loop through the data
        with open(f,'r') as data:
            for line in data:
                if 'Final Energy' in line:
                    energy_line = line
                    words = energy_line.split()
                    energy = float(words[3])
                    datafile.write(f'{molecule_name} \t {energy} \n')
```
{: .language-python}

After you run this command, look in the directory where you ran your code and find the "energies.txt" file.  Open it in a text editor and look at the file.

In the file writing line, notice the `\n` at the end of the line.  This is the newline character.  Without it, the text in our file would just be all smushed together on one line.  ~Also, the `filehandle.close()` command is very important.  Think about a computer as someone who has a very good memory, but is very slow at writing.  Therefore, when you tell the computer to write a line, it remembers what you want it to write, but it doesn't actually write the new file until you tell it you are finished.   The `datafile.close()` command tells the computer you are finished giving it lines to write and that it should go ahead and write the file now.  If you are trying to write a file and the file keeps coming up empty, it is probably because you forgot to close the file.~ All of this now will not be neccessary with 'context-manager' (the use of `with open('energies.txt','w+') as datafile:`).  Context-manager will automatically take care of closing the file with or without any error during the process.

## A final note about string formatting
Also, notice that `f'{molecule_name} \t {energy} \n'` was use as a new string format. This is call f-string and was introduced for python 3.5+. An excellent tutorial is [here](https://realpython.com/python-f-strings/).

The f-string notation that you can use with the print or the write command lets you format strings in many ways.  You could include other words or whole sentences.  For example, we could change the file writing line to
```
datafile.write(f'For the file {molecule_name} the energy is {energy} in kcal/mol.')
```
{: .language-python}
where anything in the braces is a python variable and it will print the value of that variable.  

## Project Assignment

This is a project assignment which you can complete to test your skills. This project should be used when this material is used in a long workshop, or if you are working through this material independently.

> ## File parsing homework
> In the lesson materials, there is a file called `03_Prod.mdout`. This is a file output by the Amber molecular dynamics simulation program.
>
> If you open the file and look at it, you will see sections which look like this:
>
> ~~~
>  NSTEP =      100   TIME(PS) =      20.200  TEMP(K) =   296.43  PRESS =  -300.8
>  Etot   =     -4585.1049  EKtot   =      1129.2368  EPtot      =     -5714.3417
>  BOND   =         1.5160  ANGLE   =         6.9846  DIHED      =        11.7108
>  1-4 NB =         4.3175  1-4 EEL =        49.9911  VDWAALS    =       882.6741
>  EELEC  =     -6671.5358  EHBOND  =         0.0000  RESTRAINT  =         0.0000
>  EKCMT  =       583.7810  VIRIAL  =       786.8823  VOLUME     =     31270.0410
>                                                     Density    =         0.6104
>  Ewald error estimate:   0.3214E-03
>  ------------------------------------------------------------------------------
> ~~~
>
> Your assignment is to parse this file, and write a new file containing a list of the > total energies. Name your file `Etot.txt`. When you open it, it should look like this:
>
> ~~~
> -4585.1049
> -4573.5326
> -4548.1223
> -4525.341
> -4542.8995
> -4550.9376
> -4543.8652
> -4570.4109
> -4550.4225
> -4585.2078
> ...
> ~~~
>
> `...` indicates that you will have many more rows. We've only shown the first 10 here.
>
> If you are unsure of where to start with this assignment, check the hint section!
>
>> ## Hints
>> It helps when you are writing code to break up what you have to do into steps. Overall, we want to get information from the file. How do we do that?
>>
>> If you think about the steps you will need to do this assignment you might come up with a list that is like this, you might have a list like
>>
>> 1. Open the file for reading
>> 2. Read the data in the file
>> 3. Loop through the lines in the file.
>> 4. Get the information from the line we are interested in.
>> 5. Write the information to a file.
>>
>> It can be helpful when you code to write out these steps and work on it in pieces. Try to write the code using these steps. Note that as you write the code, you may come up with other steps! First, thing about what you have to do for step 1, and write the code for that. Next, think about how you would do step 2 and write the code for that. You can troubleshoot each step using print statments. The steps build on each other, so you can work on getting each piece written before moving on to the next step.
> {: .solution}
>
>> ## Solution
>> The following is one potential solution, though you may have come up with another. Just make sure that your text file looks like the solution given in the problem statememt.
>>
>> This solution will walk through writing the code step-by-step. If you do not wish to walk through step-by-step, simply skip to the end of the solution.
>>
>> Let's start with the steps we thought of in the hint to write this code in pieces.
>>
>> ~~~
>> # Open the file for reading
>>
>> # Read the data in the file.
>>
>> # Loop through the lines in the file.
>>
>> # Get information from the line.
>> ~~~
>> {: .language-python}
>>
>> For part one, you will realize that you need to build a file path before opening. We also know that if we open a file, we will need to close it. After we build a file path, and open the file, our code will look like this.
>>
>> ~~~
>> import os
>>
>> # Open the file for reading.
>>
>> ## Build the file filepath
>> filename = os.path.join('data', '03_Prod.mdout')
>>
>> # Open the file
>> f = open(filename,'r')
>>
>> # Close the file.
>> f.close()
>>
>> # Read the data in the file.
>>
>> # Loop through the lines in the file.
>>
>> # Get information from the line.
>>
>> # Write to a file
>> ~~~
>> {: .language-python}
>>
>> This code will not appear to do anything, but it building a filepath, opening a file, then closing the file. Running it will allow us to see if we've correctly built our filepath and to see that we were able to open and close the file.
>>
>> Our second step was reading the data in the file. Recall that we did this earlier with a function called `readlines`. Recall from `readlines` that the file will need to be open to do this, so we will add this before we use the `f.close()` command.
>>
>> ~~~
>> import os
>>
>> # Open the file for reading.
>>
>> ## Build the file filepath
>> filename = os.path.join('data', '03_Prod.mdout')
>>
>> # Open the file
>> f = open(filename,'r')
>>
>> # Read the data in the file.
>> data = f.readlines()
>>
>> # Close the file.
>> f.close()
>>
>> # Loop through the lines in the file.
>>
>> # Get information from the line.
>>
>> # Write to a file.
>> ~~~
>> {: .language-python}
>>
>> We know that readlines gives us a list where every line is an element of a list. We need to loop through the lines in the file, and we will do this using a `for` loop.
>>
>> We will need to extract information from lines, so let's go ahead and split the lines. Let's just print this first.
>>
>> ~~~
>> import os
>>
>> # Open the file for reading.
>>
>> ## Build the file filepath
>> filename = os.path.join('data', '03_Prod.mdout')
>>
>> # Open the file
>> f = open(filename,'r')
>>
>> # Read the data in the file.
>> data = f.readlines()
>>
>> # Close the file.
>> f.close()
>>
>> # Loop through the lines
>> for line in data:
>>     split_line = line.split()
>>     print(split_line)
>> ~~~
>> {: .language-python}
>>
>> When you examine this output, you will discover that **if** the line contains our keyword (`Etot`), then the value associated will be element `2` in `split_line` (remember that counting starts at 0). Let's print that.
>> ~~~
>> import os
>>
>> # Open the file for reading.
>>
>> ## Build the file filepath
>> filename = os.path.join('data', '03_Prod.mdout')
>>
>> # Open the file
>> f = open(filename,'r')
>>
>> # Read the data in the file.
>> data = f.readlines()
>>
>> # Close the file.
>> f.close()
>>
>> # Loop through the lines
>> for line in data:
>>     split_line = line.split()
>>     if 'Etot' in line:
>>         print(split_line[2])
>> ~~~
>> {: .language-python}
>>
>> Now all that is left to do is to write this information to a file. We will need to open the file before the loop, write to it inside the loop, and finally close it outside of the loop. Our final solution is
>>
>> ~~~
>> import os
>>
>> # Open the file for reading.
>>
>> ## Build the file filepath
>> filename = os.path.join('data', '03_Prod.mdout')
>>
>> # Open the file
>> f = open(filename,'r')
>>
>> # Read the data in the file.
>> data = f.readlines()
>>
>> # Close the file.
>> f.close()
>>
>> # Open a file for writing
>> f_write = open('Etot.txt', 'w+')
>>
>> # Loop through the lines
>> for line in data:
>>     split_line = line.split()
>>     if 'Etot' in line:
>>         print(split_line[2])
>>         f_write.write(f'{split_line[2]}\n')
>>
>> f_write.close()
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

{% include links.md %}
