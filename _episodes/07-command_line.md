---
title: "Running code from the Linux Command Line"
teaching: 15
exercises: 10
questions:
- "How do I move my code from the interactive jupyter notebook to run from the Linux command line?"
objectives:
- "Make code executable from the Linux command line."
- "Use sys.argv() to accept user inputs."
keypoints:
- "You must `import sys` in your code to accept user arguments."
- "The name of the script itself is always `sys.argv[0]` so the first user input is normally `sys.argv[1]`."
---
## Creating and running a python input file

We are now going to move our geometry analysis code out of the jupyter notebook and into a format that can be run from the Linux command line.  Open your favorite text editor and create a new file called "geom_analysis.py" (or choose another filename, just make sure the extension is .py).  Paste in your geometry analysis code (the version with your functions) from your jupyter notebook and save your file.

The best practice is to put all your functions at the top of the file, right after your import statements.  Your file will look something like this.
```
import numpy

def bond_length(atom1, atom2):
    BL = numpy.sqrt((atom1[0]-atom2[0])**2+(atom1[1]-atom2[1])**2+(atom1[2]-atom2[2])**2)
    BL = round(BL,3)
    return BL

def bond_check(bond_distance):
    if bond_distance > 0 and bond_distance < 1.5:
        return True

xyz_file = numpy.genfromtxt(fname='water.xyz', skip_header=2, dtype='unicode')
symbols = xyz_file[:,0]
coord = (xyz_file[:,1:])
coord = coord.astype(numpy.float)
for numA, atomA in enumerate(coord):
    for numB, atomB in enumerate(coord):
       BL_AB = bond_length(atomA, atomB)
       if bond_check(BL_AB) == True:
           print(F'{symbols[numA]} to {symbols[numB]} : {BL_AB}')
```
{: .language-python}

Exit your text editor and go back to the command line.  Now all you have to do to run your code is type
```
$ python geom_analysis.py
```
in your Terminal window.  Your code should either print the output to the screen or write it to a file, depending on what you have it set up to do.  

## Changing your code to accept user inputs
In your current code, the name of the xyzfile to analyze, "water.xyz", is hardcoded; in order to change it, you have to open your code and change the name of the file that is read in.  If you were going to use this code to analyze geometries in your research, you would probably want to be able to specify the name of the input file when you run the code, so that you don't have to change it every single time.  These types of user inputs are called *arguments* and to make our code accept arguments, we have to import a new python library in our code.  

Open your geometry analysis code in your text editor and add this line at the top.
```
import sys
```

Now that you have imported the `sys` library, you can use its functions.  The library has a function called `sys.argv()` which creates a list of all the arguments the user enters at the command line.  Everything after *python* is an argument, so `sys.argv[0]` is always the name of your script.  We would like our code to accept the name of the xyz file we want to analyze as an argument.  Add this line to your code.
```
filename = sys.argv[1]
```
{: .language-python}

Then you need to go the part of your code where you read in the data from the xyz file and change the name of the file to read to `filename`.  
```
xyz_file = numpy.genfromtxt(fname=filename, skip_header=2, dtype='unicode')
```
{: .language python}

Save your code and go back to the Terminal window.  Make sure you are in the directory where your code is saved and type
```
$ python geom_analysis.py water.xyz
```
Check that the output of your code is what you expected.

What would happen if the user forgot to specify the name of the xyz file?  The way the code is written now, it would give an error message.
```
Traceback (most recent call last):
  File "geom_analysis_2.py", line 13, in <module>
    filename = sys.argv[1]
IndexError: list index out of range
```
{: .error}
The reason it says the list index is out of range is because `sys.argv[1]` does not exist.  Since the user forgot to specify the name of the xyz file, the `sys.argv` array only has one element, `sys.argv[0]`.  It would be better to print an error message and let the user know that they didn't enter the input correctly.  Our code is expecting exactly two inputs: the script name and the xyz file name. The easiest way to add an error message is to check the length of the sys.argv list and print an error message and exit if it does not equal the expected length.  
```
if len(sys.argv) != 2:
        print('Incorrect input! Please try again.')
        exit()
```
{: .language-python}

{% include links.md %}
