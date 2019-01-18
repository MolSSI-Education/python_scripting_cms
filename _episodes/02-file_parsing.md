---
title: "File Parsing"
teaching: 20
exercises: 0
questions:
- "How do I sort through all the information in a text file and extract particular pieces of information?"
objectives:
- "Open a file and read in its contents line by line."
- "Search for a particular string in a file."
- "Manipulate strings and change data types."
- "Print to a new file."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
## Reading in a file
One of the most common tasks in research is analyzing data.  Many computational chemistry programs output text files that include a large amount of information including text and data that you need to analyze.  Often, you need to sort through the output file and identify particular pieces of information that are most important to you.  In general, this is called file parsing.

In python, there are many ways in python to read in information from a text file.  The best method to use depends on the type of data and the type of analysis you are performing.  If you have a file with lots of different types of information, text and numbers, with different types of formatting, the most generic way to read in information is the `readlines()` function.  Before you can read in a file, you have to open it and give it a *filehandle*.

```
outfile = open("ethanol.out","r")
data=outfile.readlines()
```
{: .language-python}
This code opens a file for reading (that's the `r` part in the `open` function), assigns it to the filehandle outfile and then uses the `readlines()` function.  Notice the dot notation introduced last lesson; readlines acts on the filehandle given right before the dot.  The function creates a list called data where each element of the list is a string that is one line of the file.  This is always how the `readlines()` function works.  

Exercise: Check that your file was read in correctly by determining how many lines are in the file.

## Searching for a pattern in your file
The file we opened is an output file which calculates the energy (and a lot of other stuff!) for an ethanol molecule.  If you looked through the file in a text editor, you would see that the critical line says "Final Energy".  We want to search through this file and find that line.

```
for line in data:
    if 'Final Energy' in line:
        energy_line = line
        print(energy_line)
```
{: .language-python}

```
@DF-RHF Final Energy:  -154.09130176573018
```
{: .output}

Remember that `readlines()` saves each line of the file as a string, so `energy_line` is a string that contains the whole line.  For our analysis, if we are most interested in the energy, we need to split up the line so we can save just the number as a different variable name. To do this, we use a new function called `split`.  The `split` function takes a string and divides it into its components, which can then be saved as a new list.  If you don't put anything in the parenthesis, it splits based on whitespace.  In the example below, we take the line we found in the `for` loop and split it up into its individual words.

```
words = energy_line.split()
print(words)
```
{: .language-python}

```
['@DF-RHF', 'Final', 'Energy:', '-154.09130176573018']
```
{: .output}

From this `print` statement, we now see that we have a list called words, where we have split `energy_line`.  The energy is actually the fourth element of this list, so we can now save it as a new variable.

```
energy = words[3]
print(energy)
```
{: .language-python}

```
-154.09130176573018
```
{: .output}

If we now try to do a math operation on energy, we get an error message?  Why do you think that is?


```
energy + 50
```
{: .language-python}
```
TypeError                                 Traceback (most recent call last)
<ipython-input-52-7bda8dd3f95d> in <module>()
----> 1 energy + 50

TypeError: must be str, not int
```
{: .output}
Even though `energy` looks like a number to us, it is really a string, so we can not add an integer to it.  We need to change the data type of energy to a float.

Even though `energy` looks like a number to us, it is really a string, so we can not add an integer to it.  We need to change the data type of energy.

```
energy = float(energy)
```
{: .language-python}

Now it will work.  If we thought ahead we could have changed the data type when we assigned the variable originally.

```
energy = float(words[3])
```
{: .language-python}

## Exercise on File parsing (should we move this to the end?)
Use the provided sapt.out file.  In this output file, the program calculates the interaction energy for an ethene-ethyne complex.  The output reports four interaction energy components: electrostatics, induction, exchange, and dispersion.  Parse each of these energies, in kcal/mole, from the output file.  (Hint: study the file in a text editor to help you decide what to search for.) Calculate the total interaction energy by adding the four components together.  Your code's output should look something like this:
```
Electrostatics : -2.25850118 kcal/mole
Exchange : 2.27730198 kcal/mole
Induction : -0.5216933 kcal/mole
Dispersion : -0.9446677 kcal/mole
Total Energy : -1.4475602000000003 kcal/mole
```
(Somehow hide this code block?)
```
#This is one possible solution for the SAPT parsing exercise
saptout = open('SAPT.out','r')
saptlines = saptout.readlines()
important_lines=[]
energies=[]
for line in saptlines:
    if 'Electrostatics    ' in line:
        electro_line = line
        important_lines.append(electro_line)
    if 'Exchange       ' in line:
        exchange_line = line
        important_lines.append(exchange_line)
    if 'Induction      ' in line:
        induction_line = line
        important_lines.append(induction_line)
    if 'Dispersion     ' in line:
        dispersion_line = line
        important_lines.append(dispersion_line)

#print(important_lines)

for line in important_lines:
    words = line.split()
    #print(words)
    energy_type = words[0]
    energy_kcal = float(words[3])
    energies.append(energy_kcal)
    print(energy_type, ':', energy_kcal, 'kcal/mole')

total_energy=energies[0]+energies[1]+energies[2]+energies[3]
print('Total Energy', ':', total_energy, 'kcal/mole')
```
{: .languate-python}
There is a lot of other information in the output file we might be interested in.  For example, We might want to pull out the initial coordinates for the molecule.  If we look through the file in a text editor, we notice that the coordinates begin with a line that says

Center              X                  Y                   Z               Mass

and then the coordinates begin on the next line.  In this case, we don't want to pull something out of this line, as we did in our previous example, but we want to know which line of the file this is so that we can then pull the coordinates from the next few lines.  

When you use a for loop, it is easy to have python keep up with the line numbers using the `enumerate` command.  The general syntax is
```
for linenum, line in enumerate(list_name):
    do things in the loop
```
In this notation, there are now *two* variables you can use in your loop commands, `linenum` (which can be named something else) will keep up with what iteration you are on in the loop, in this case what line you are on in the file. The variable `line` (which could be named something else) functions exactly as it did before, holding the actual information from the list.  Finally, instead of just giving the list name you use `enumerate(list_name)`.  

This block of code searches our file for the line that contains "Center" and reports the line number.
```
for linenum, line in enumerate(data):
    if 'Center' in line:
        print(linenum)
        print(line)
```
## A final note about regular expressions
Sometimes you will need to match something more complex than just a particular word or phrase in your output file.  Sometimes you will need to match a particular word, but only if it is found at the beginning of a line.  Or perhaps you will need to match a particular pattern of data, like a capital letter followed by a number, but you won't know the exact letter and number you are looking for.  These types of matching situations are handled with something called *regular expressions* which is accessed through the python module `re`.  While using regular expressions is outside the scope of this tutorial, they are very useful and you might want to learn more about them in the future.  A tutorial can be found at _______.  

{% include links.md %}
