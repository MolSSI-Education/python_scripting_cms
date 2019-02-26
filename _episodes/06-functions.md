---
title: "Writing Functions"
teaching: ??
exercises: ??
questions:
- "How do I write include functions in my code?"
objectives:
- "First objective goes here."
keypoints:
- "First point goes here."

---
## Function

Most code is organized into blocks of code which perform a particular task.  These code blocks are called *functions*.  A commercial software package likely has hundreds of thousands or millions of functions. Functions break up our code into smaller, more easily understandable statements, and also allow our code to be more *modular*, meaning we can take pieces and reuse them. Functions also make your code easier to test, which we will see in a later lesson.

In general, each function should perform only one computational task.

## Defining and running a function

In Python, the following syntax is used to declare a function:

~~~
def function_name(parameters):
  ** function body code **
~~~
{: .language-python}

Functions are defined using the `def` keyword, followed by the name of the function. The function may have parameters which are passed to it, which are in parenthesis following the function name. A function can have no parameters as well. It is important to note here that defining a function does not execute it. Consider the following example with no parameters.

~~~
def print_hello():
  print("Hello!")
~~~
{: .language-python}

We then call the function in the next cell

~~~
print_hello()
~~~
{: .language-python}

~~~
Hello!
~~~
{: .output}

## Writing functions into our geometry analysis project

Let's go back and consider a simple solution for the geometry analysis project.
```
import numpy

xyz_file = numpy.genfromtxt(fname='water.xyz', skip_header=2, dtype='unicode')
symbols = xyz_file[:,0]
coord = (xyz_file[:,1:])
coord = coord.astype(numpy.float)

for numA, atomA in enumerate(coord):
   for numB, atomB in enumerate(coord):
       if numB > numA:
           bond_length_AB = numpy.sqrt((atomA[0]-atomB[0])**2+(atomA[1]- atomB[1])**2+(atomA[2]-atomB[2])**2)

           if bond_length_AB > 0 and bond_length_AB <= 1.5:
               print(symbols[numA], "to", symbols[numB], ":", bond_length_AB)
```
{: .language-python}

To think about where we should write functions in this code, let's think about parts we may want to use again or in other places. One of the first places we might think of is in the bond distance calculation. Perhaps we'd want to calculate a bond distance in some other script. We can reduce the likelihood of errors in our code by defining this in a function (so that if we wanted to change our bond calculation, we would only have to do it in one place.)

Let's change this code so that we write a function to calculate the bond distance. As explained above, to define a function, you start with the word `def` and then give the name of the function.  In parenthesis are in inputs of the function followed by a colon.  The the statements the function is going to execute are indented on the next lines. For this function, we will `return` a value. The last line of a function shows the return value for the function, which we can use to store a variable with the output value.  Let's write a function to calculate the distance between atoms.
```
def calculate_distance(atom1, atom2):
    bond_length = numpy.sqrt((atom1[0]-atom2[0])**2+(atom1[1]-atom2[1])**2+(atom1[2]-atom2[2])**2)

    return bond_length
```
{: .language-python}

Now we can change our `for` loop to just call the distance function we wrote above.
```
for numA, atomA in enumerate(coord):
   for numB, atomB in enumerate(coord):
       if numB > numA:
           bond_length_AB = calculate_distance(atomA, atomB)
           print(symbols[numA], "to", symbols[numB], ":", bond_length_AB)
```
{: .language-python}

Next, let's write another function that checks to see if a particular bond distance represents a bond. This function will be called `bond_check`, and will return `True` if the bond distance is within certain bounds (At first we'll set this to be between 0 and 1.5 angstroms).

~~~
def bond_check(atom_distance):
    if atom_distance > 0 and atom_distance <= 1.5:
        return True
    else:
        return False
~~~
{: .language-python}

This is great! Our function will currently return `True` if our bond distance is less than 1.5 angstrom.

> ## Exercise
>
> Modify the `bond_check` function to take a minimum length and a maximum length as user input.
>> ## Answer
>> ~~~
>> def bond_check(atom_distance, minimum_length, maximum_length):
>>    if atom_distance > minimum_length and bond_distance <= maximum_length:
>>        return True
>>    else:
>>        return False
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

### Function Default arguments
When there are parameters in a function definition, we can set these parameters to default values. For example, if we want the default values in bond check to be 0 and 1.5, we can change the function definition to the following:

~~~
def bond_check(atom_distance, minimum_length=0, maximum_length=1.5):
    if atom_distance > minimum_length and atom_distance <= maximum_length:
        return True
    else:
        return False
~~~
{: .language-python}

Let's try out the function now.

~~~
print(bond_check(1.5))
print(bond_check(1.6))
~~~
{: .language-python}

~~~
True
False
~~~
{: .output}

However, we can overwrite `minimum_length` or `maximum_length`.

~~~
print(bond_check(1.6, maximum_length=1.6))
~~~
{: .language-python}


Now that we have our `bond_check` function, we can use it in our `for` loop to only print the bond lengths that are really bonds.

```
for numA, atomA in enumerate(coord):
   for numB, atomB in enumerate(coord):
       if numB > numA:
           bond_length_AB = atom_distance(atomA, atomB)
           if bond_check(bond_length_AB):
               print(F'{symbols[numA]} to {symbols[numB]} : {bond_length_AB}')
```
{: .language-python}
```
O to H1 : 0.969
O to H2 : 0.969
```
{: .output}

> ## Exercise
>
>We might also want to write a function that opens and processes our `xyz` file for us. Write a function called open_xyz which takes an xyz file as a parameter and returns the symbols and coordinates.
>
> **Hint**: You can return two values from a function by typing `return variable1, variable2`.
>> ## Answer
>> ~~~
>> def open_xyz(filename):
>>      xyz_file = numpy.genfromtxt(fname=filename, skip_header=2, dtype='unicode')
>>      symbols = xyz_file[:,0]
>>      coord = (xyz_file[:,1:])
>>      coord = coord.astype(numpy.float)
>>      return symbols, coord
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


We've now written three functions. Using these functions, our script to print bonded atoms now looks like this:

~~~
import numpy

symbols, coord = open_xyz('water.xyz')

for numA, atomA in enumerate(coord):
   for numB, atomB in enumerate(coord):
       if numB > numA:
           bond_length_AB = calculate_distance(atomA, atomB)

           if bond_check(bond_length_AB):
               print(symbols[numA], "to", symbols[numB], ":", bond_length_AB)
~~~
{: .language-python}

You can probably think of a further extension to use functions here. What if we wanted to print the bonds for another `xyz` file besides water? One option would be to copy and paste the two two `for` loops we've written. However, the smarter move is to put them in a function.

 We could encapsulate the `for` loops into a function as well. Let's do this, then perform the same analysis using both the `water.xyz` file and the `benzene.xyz` file.

First, we will define a new function `print_bonds`, which takes bond symbols and coordinates from an `xyz` file as an input.

~~~
def print_bonds(atom_symbols, atom_coordinates):
    for numA, atomA in enumerate(atom_coordinates):
        for numB, atomB in enumerate(atom_coordinates):
            if numB > numA:
                bond_length_AB = calculate_distance(atomA, atomB)

                if bond_check(bond_length_AB):
                    print(atom_symbols[numA], "to", atom_symbols[numB], ":", bond_length_AB)
~~~

If you were to put all the functions we wrote into a single cell, it looks like this

~~~
import numpy

def calculate_distance(atom1, atom2):
    bond_length = numpy.sqrt((atom1[0]-atom2[0])**2+(atom1[1]-atom2[1])**2+(atom1[2]-atom2[2])**2)

    return bond_length

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

def print_bonds(atom_symbols, atom_coordinates):
    for numA, atomA in enumerate(atom_coordinates):
        for numB, atomB in enumerate(atom_coordinates):
            if numB > numA:
                bond_length_AB = cacluate_distance(atomA, atomB)

                if bond_check(bond_length_AB):
                    print(atom_symbols[numA], "to", atom_symbols[numB], ":", bond_length_AB)
~~~
{: .language-python}

We can now open an arbitrary `xyz` file and print the bonded atoms. For example, to do this for water and benze, we could execute a cell like this:

~~~
water_symbols, water_coords = open_xyz('water.xyz')

benzene_symbols, benzene_coords = open_xyz('benzene.xyz')

print("Printing bonds for water")
print_bonds(water_symbols, water_coords)

print("Printing bonds for Benzene")
print_bonds(benzene_symbols, benzene_coords)
~~~
{: .language-python}

~~~
Printing bonds for water
O to H1 : 0.9690005374652793
O to H2 : 0.9690003348647513
Printing bonds for Benzene
C to C : 1.3960247132483008
C to C : 1.3960247132483008
C to H : 1.0830000000000002
C to C : 1.396
C to H : 1.0833318974349455
C to C : 1.3960247132483008
C to H : 1.0833318974349455
C to C : 1.3960247132483008
C to H : 1.0830000000000002
C to C : 1.396
C to H : 1.0833318974349455
C to H : 1.0833318974349455
~~~
{: .output}

> ## Extension
>
> In earlier lessons, we used `glob` to process multiple files. How could you use `glob` to print bonds for all the xyz files?
>
>> ## Solution
>> ~~~
>>import glob
>>
>>xyz_files = glob.glob("data/*.xyz")
>>
>>for xyz_file in xyz_files:
>>    atom_symbols, atom_coords = open_xyz(xyz_file)
>>    print("Printing bonds for ", xyz_file)
>>    print_bonds(atom_symbols, atom_coords)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


{% include links.md %}
