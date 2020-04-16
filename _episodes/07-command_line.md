---
title: "Running code from the Linux Command Line"
teaching: 15
exercises: 10
questions:
- "How do I move my code from the interactive jupyter notebook to run from the Linux command line?"
objectives:
- "Make code executable from the Linux command line."
- "Use argparse to accept user inputs."
keypoints:
- "You must `import argparse` in your code to accept user arguments."
- "You add must first create an argument parser using `parser = argparse.ArgumentParser`"
- "You add arguments using `parser.add_argument`"
---
## Creating and running a python input file

We are now going to move our geometry analysis code out of the Jupyter notebook and into a format that can be run from the Linux command line.  Open your favorite text editor and create a new file called "geom_analysis.py" (or choose another filename, just make sure the extension is .py).  Paste in your geometry analysis code (the version with your functions) from your jupyter notebook and save your file.

The best practice is to put all your functions at the top of the file, right after your import statements.  Your file will look something like this.
~~~
import numpy
import os

def calculate_distance(atom1_coord, atom2_coord):
    x_distance = atom1_coord[0] - atom2_coord[0]
    y_distance = atom1_coord[1] - atom2_coord[1]
    z_distance = atom1_coord[2] - atom2_coord[2]
    bond_length_12 = numpy.sqrt(x_distance**2+y_distance**2+z_distance**2)
    return bond_length_12

def bond_check(atom_distance, minimum_length=0, maximum_length=1.5):
    if atom_distance > minimum_length and atom_distance <= maximum_length:
        return True
    else:
        return False

def open_xyz(filename):
     xyz_file = numpy.genfromtxt(fname=filename, skip_header=2, dtype='unicode')
     symbols = xyz_file[:,0]
     coord = (xyz_file[:,1:])
     coord = coord.astype(numpy.float)
     return symbols, coord

file_location = os.path.join('data', 'water.xyz')
symbols, coord = open_xyz(file_location)
num_atoms = len(symbols)
for num1 in range(0,num_atoms):
     for num2 in range(0,num_atoms):
         if num1<num2:
             bond_length_12 = calculate_distance(coord[num1], coord[num2])
             if bond_check(bond_length_12) is True:
                 print(F'{symbols[num1]} to {symbols[num2]} : {bond_length_12:.3f}')
~~~
{: .language-python}

Exit your text editor and go back to the command line.  Now all you have to do to run your code is type

~~~
$ python geom_analysis.py
~~~
{: .language-bash}
in your Terminal window.  Your code should either print the output to the screen or write it to a file, depending on what you have it set up to do.  (The code example given prints to the screen.)

## Changing your code to accept user inputs
In your current code, the name of the xyzfile to analyze, "water.xyz", is hardcoded; in order to change it, you have to open your code and change the name of the file that is read in.  If you were going to use this code to analyze geometries in your research, you would probably want to be able to specify the name of the input file when you run the code, so that you don't have to change it every single time. You might want to use the script like this:

~~~
$ python geom_analysis.py water.xyz
~~~
{: .language-bash}

These types of user inputs are called *arguments* and to make our code accept arguments, we have to import a new python library in our code.  


Open your geometry analysis code in your text editor and add this line at the top.

~~~
import argparse
~~~
{: .language-python}

We are importing a library called [https://docs.python.org/3/library/argparse.html](argparse) which can be used to easily make scripts with command line arguments. `Argparse` has the ability to allow us to easily write documentation for our scripts as well.

We tell argparse that we want to add a command line interface. The syntax for this is

~~~
parser = argparse.ArgumentParser(description="This script analyzes a user given xyz file and outputs the length of the bonds.")
~~~
{: .language-python}

We've included a description of the script for our users using `description=`. This description does not need to explain what the arguments are, that will be done automatically for us in the next steps.

Next, we have to tell `argparse` what arguments it should expect. In general, the syntax for this is

~~~
parser.add_argument("argument_name", help="Your help message for this argument.")
~~~
{: .language-python}

Let's add one for the xyz file the user should specify.

~~~
parser.add_argument("xyz_file", help="The filepath for the xyz file to analyze.")
~~~

Next, we have to get the arguments.

~~~
args = parser.parse_args()
~~~
{: .language-python}

Our arguments are in the `args` variable. We can get the value of an argument by using `args.argument_name`, so to get the xyz file the user puts in, we use `args.xyz_file`. Notice that what follows after the dot is the same thing we but in quotation marks when using `add_argument.`

~~~
xyzfilename = args.xyz_file
symbols, coord = open_xyz(xyzfilename)
~~~
{: .language python}

Save your code and go back to the Terminal window.  Make sure you are in the directory where your code is saved and type

~~~
$ python geom_analysis.py --help
~~~
{: .language-bash}

This will print a help message. The `argparse` library has written this help message for us based on the descriptions and arguments we added.

~~~
usage: analyze.py [-h] xyz_file

This script analyzes a user given xyz file and outputs the length of the
bonds.

positional arguments:
  xyz_file    The filepath for the xyz file to analyze.

optional arguments:
  -h, --help  show this help message and exit
~~~
{: .output}

Now try running your script with an xyz file.

~~~
$ python analyze.py data/water.xyz
~~~
{: .language-bash}

Check that the output of your code is what you expected.

What would happen if the user forgot to specify the name of the xyz file?  

~~~
usage: analyze.py [-h] xyz_file
analyze.py: error: the following arguments are required: xyz_file
~~~
{: .error}

Argparse handles this for us and prints an error message. It tells us that we must specifiy an xyz file.

Try out your program with other XYZ files in your `data` folder.

## The "main" part of our script
We need to add one more thing to our code.  When you write a code that includes function definitions and a main script, you need to tell python which part is the main script. (This becomes very important later when we are talking about testing.) *After* your import statements and function definitions and  *before* use `argparse`
```
if __name__ == "__main__":
```
{: .language-python}

Since this is an `if` statement, you now need to indent each line of your main script below this if statement.  Be very careful with your indentation! Don't use a mixture of tabs and spaces! A good way to indent multiple lines in many text editors is to highlight the lines you would like to indent, then press `tab`.  

Save your code and run it again.  It should work exactly as before.  If you now get an error message, it is probably due to inconsistent indentation.  

~~~
import os
import numpy
import argparse

def calculate_distance(atom1_coord, atom2_coord):
    x_distance = atom1_coord[0] - atom2_coord[0]
    y_distance = atom1_coord[1] - atom2_coord[1]
    z_distance = atom1_coord[2] - atom2_coord[2]
    bond_length_12 = numpy.sqrt(x_distance**2+y_distance**2+z_distance**2)
    return bond_length_12

def bond_check(atom_distance, minimum_length=0, maximum_length=1.5):
    if atom_distance > minimum_length and atom_distance <= maximum_length:
        return True
    else:
        return False

def open_xyz(filename):
     xyz_file = numpy.genfromtxt(fname=filename, skip_header=2, dtype='unicode')
     symbols = xyz_file[:,0]
     coord = (xyz_file[:,1:])
     coord = coord.astype(numpy.float)
     return symbols, coord


if __name__ == "__main__":

    ## Get the arguments.
    parser = argparse.ArgumentParser(description="This script analyzes a user given xyz file and outputs the length of the bonds.")
    parser.add_argument("xyz_file", help="The filepath for the xyz file to analyze.")

    args = parser.parse_args()

    symbols, coord = open_xyz(args.xyz_file)
    num_atoms = len(symbols)

    for num1 in range(0,num_atoms):
         for num2 in range(0,num_atoms):
             if num1<num2:
                 bond_length_12 = calculate_distance(coord[num1], coord[num2])
                 if bond_check(bond_length_12, minimum_length=args.minimum_length, maximum_length=args.maximum_length) is True:
                     print(F'{symbols[num1]} to {symbols[num2]} : {bond_length_12:.3f}')
~~~
{: .language-python}

## Extension - Optional Arguments
What's another argument we might want to include? We also might want to let the user specify a minimum and maximum bond length on the command line. We would want these to be optional, just like they are in our function.

We can add optional arguments by putting a dash (`-`) or two dashes (`--`) in front of the argument name when we add an argument. Add this line below where you added the fist argument. Note that all `add_argument` lines should be above the line with `parse_args`.

~~~
parser.add_argument('-minimum_length', help='The minimum distance to consider atoms bonded.', type=float, default=0)
~~~
{: .language-python}

We've added two new things to our argument as well. We have told `argparse` that the argument people will pass for this will be a decimal number (float) and that the default value will be zero. If a user does not use `-minimum_length`, the value will be zero.

Now we will change our code to use this value. Find the line where you call `bond_check` and change it to use the value from `argparse`.

~~~
if bond_check(bond_length_12, minimum_length=args.minimum_length) is True:
~~~
{: .language-python}

~~~
$ python analyze.py data/water.xyz -minimum_length 1
~~~
{: .language-bash}

Now we can override the minimum length when running our script. If we don't specify it, it will default to using zero.

We can do the same thing for our maximum bond length.

~~~
parser.add_argument('-maximum_length', help='The maximium distance to consider atoms bonded.', type=float, default=1.5)
~~~
{: .language-python}

And, don't forget to update your `bond_check` function.
~~~
if bond_check(bond_length_12, minimum_length=args.minimum_length, maximum_length=args.maximum_length) is True:
~~~
{: .language-python}

Our final program looks like this:

~~~
import os
import numpy
import argparse

def calculate_distance(atom1_coord, atom2_coord):
    x_distance = atom1_coord[0] - atom2_coord[0]
    y_distance = atom1_coord[1] - atom2_coord[1]
    z_distance = atom1_coord[2] - atom2_coord[2]
    bond_length_12 = numpy.sqrt(x_distance**2+y_distance**2+z_distance**2)
    return bond_length_12

def bond_check(atom_distance, minimum_length=0, maximum_length=1.5):
    if atom_distance > minimum_length and atom_distance <= maximum_length:
        return True
    else:
        return False

def open_xyz(filename):
     xyz_file = numpy.genfromtxt(fname=filename, skip_header=2, dtype='unicode')
     symbols = xyz_file[:,0]
     coord = (xyz_file[:,1:])
     coord = coord.astype(numpy.float)
     return symbols, coord


if __name__ == "__main__":

    ## Get the arguments.
    parser = argparse.ArgumentParser(description="This script analyzes a user given xyz file and outputs the length of the bonds.")
    parser.add_argument("xyz_file", help="The filepath for the xyz file to analyze.")

    parser.add_argument('-minimum_length', help='The minimum distance to consider atoms bonded.', type=float, default=0)
    parser.add_argument('-maximum_length', help='The maximium distance to consider atoms bonded.', type=float, default=1.5)

    args = parser.parse_args()

    symbols, coord = open_xyz(args.xyz_file)
    num_atoms = len(symbols)

    for num1 in range(0,num_atoms):
         for num2 in range(0,num_atoms):
             if num1<num2:
                 bond_length_12 = calculate_distance(coord[num1], coord[num2])
                 if bond_check(bond_length_12, minimum_length=args.minimum_length, maximum_length=args.maximum_length) is True:
                     print(F'{symbols[num1]} to {symbols[num2]} : {bond_length_12:.3f}')
~~~
{: .language-python}

> ## Project Assignment
>
> For this homework assignment, you will return to your Project from Lesson 3.
>
> Create a command line script using `argparse` which can take in an `mdout` file from Amber,  pull out total energy for each time step, and write a new file containing these values. The script should take a file name (`03_Prod.mdout`) OR pattern (`*.mdout`) and output files with the names `filename_Etot.txt` **in the same directory as the files**. Modify your week 1 homework to do this. In contrast to week 1, truncate the last two values from your script (these are an average value and a fluctuation value) from the file you write.
>
> You can download a directory containing more mdout files [here](../data/mdout.zip) 
>
> Call your script `analyze_mdout.py`. You should be able to call the script in the following > ways:
>
> ~~~
> $ python analyze_mdout.py 03_Prod.mdout
> ~~~
> {: .language-bash}
>
> or
>
> ~~~
> $ python analyze_mdout.py '*.out'
> ~~~
> {: .language-bash}
>
> When you call help, you should get the following output:
> ~~~
> $ python analyze_mdout.py --help
> ~~~
> {: .language-bash}
>
> ~~~
> usage: This script parses amber mdout files to extract the total energy. You can also use it to create plots.
>       [-h] [--make_plots] path
>
> positional arguments:
>  path          The filepath to the file(s) to be analyzed. To analyze
>                multiple files, you can use the `*` pattern.
>
> optional arguments:
>  -h, --help    show this help message and exit
> ~~~
> {: .output}
> **Extension**
> Add an optional argument to your script to create plots of the data with matplotlib.
>
>> ## Solution
>> ~~~
>> import os
>> import glob
>> import argparse
>> import matplotlib.pyplot as plt
>>
>>
>> if __name__ == "__main__":
>>
>>     parser = argparse.ArgumentParser("This script parses amber mdout files to extract the total energy. You can also use it to create plots.")
>>     parser.add_argument("path", help="The filepath to the file(s) to be analyzed. To analyze multiple files, you can use the `*` pattern.", type=str)
>>     parser.add_argument("--make_plots", help="Create a line plot of the values.", action='store_true')
>>
>>     args = parser.parse_args()
>>
>>     filenames = args.path
>>     dir_name = os.path.dirname(filenames)
>>
>>     files = glob.glob(filenames)
>>
>>     for file in files:
>>         base_name = os.path.basename(file).split('.')[0]
>>
>>         # Open the file
>>         f = open(file,'r')
>>
>>         # Read the data in the file.
>>         data = f.readlines()
>>
>>         # Close the file.
>>         f.close()
>>
>>         etot = []
>>         # Loop through the lines
>>         for line in data:
>>             split_line = line.split()
>>             if 'Etot' in line:
>>                 etot.append(float(split_line[2]))
>>
>>         # Get rid of values we don't need.
>>         values = etot[:-2]
>>         # Open a file for writing
>>         outfile_location = os.path.join(dir_name, F'{base_name}_Etot.txt')
>>         outfile = open(outfile_location, 'w+')
>>
>>         for value in values:
>>             outfile.write(f'{value}\n')
>>
>>         outfile.close()
>>
>>         if args.make_plots:
>>             plt.figure()
>>             plot_name = os.path.join(dir_name, F'{base_name}_Etot.png')
>>             plt.plot(values, label=base_name)
>>             plt.legend()
>>             plt.savefig(plot_name, dpi=300)
>> ~~~
>> {: .language-python}
>>
> {: .solution}
{: .challenge}

{% include links.md %}
