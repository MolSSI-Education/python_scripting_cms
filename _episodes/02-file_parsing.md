---
title: "File Parsing"
teaching: 20
exercises: 0
questions:
- "Key question (FIXME)"
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

There are many ways in python to read in information from a file.  

```
xyzfile = open("ethanol.out","r")
data=xyzfile.readlines()
```
{: .language-python}

This code opens a file for reading and then uses the `readlines()` function.  Notice the dot notation introduced last lesson; readlines acts on the file handle given right before the dot.  The function creates a list called data where each element of the list is a string that is one line of the file.  This is always how the `readlines()` function works.  

(Maybe add some stuff here about looking at what is in the file and determinign how many lines there are.)

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

Remember that `readlines()` saves each line of the file as a string, so `energy_line` is a string that contains the whole line.  For our analysis, if we are most interested in the energy, we need to split up the line so we can save just the number as a different variable name.

```
words = energy_line.split()
print(words)
```
{: .language-python}

```
['@DF-RHF', 'Final', 'Energy:', '-154.09130176573018']
```
{: .output}

Now we see that we have a list called words, where we have split `energy_line`.  The energy is actually the fourth element of this list, so we can now save it as a new variable.

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

TypeError                                 Traceback (most recent call last)
<ipython-input-52-7bda8dd3f95d> in <module>()
----> 1 energy + 50

TypeError: must be str, not int
```
{: .language-python}

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

Now add a section about keeping up with line numbers.

Then do a section about processing multiple files.

Then do a section about writing the output to a new file.

Maybe a final note about regular expressions.

{% include links.md %}
