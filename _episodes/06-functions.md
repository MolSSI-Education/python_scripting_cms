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
## Defining a function

Most code is organized into blocks of code which perform a particular task.  These code blocks are called *functions*.  A commercial software package likely has hundreds of thousands or millions of functions.  A function should perform only one computational task.  A function can accept inputs and will return an output.  While many beginning programmers doesn't understand why they would write functions, there are several advantages.  With functions, you can easily reuse pieces of your code in multiple places.  If you wanted to change your code later, often you can do so by only changing one function or calling a different function without having to change the rest of your code. Functions also make your code easier to test, which we will see in a later lesson.  

Let's go back and consider a simple solution for the geometry analysis project.
```
import numpy
xyz_file = numpy.genfromtxt(fname='water.xyz', skip_header=2, dtype='unicode')
symbols = xyz_file[:,0]
coord = (xyz_file[:,1:])
coord = coord.astype(numpy.float)
for numA, atomA in enumerate(coord):
    for numB, atomB in enumerate(coord):
       BL_AB = numpy.sqrt((atomA[0]-atomB[0])**2+(atomA[1]- atomB[1])**2+(atomA[2]-atomB[2])**2)
       BL_AB = round(BL_AB,3)
       print(symbols[numA], "to", symbols[numB], ":", BL_AB)
```
{: .language-python}

Let's change this code so that we write a function to calculate the bond distance. To define a function, you start with the word `def` and then give the name of the function.  In parenthesis are in inputs of the function followed by a colon.  The the statements the function is going to execute are indented on the next lines. The last line of a function shows the return value for the function.  Let's write a function to calculate the bond lengths.
```
def bond_length(atom1, atom2):
    BL = numpy.sqrt((atom1[0]-atom2[0])**2+(atom1[1]-atom2[1])**2+(atom1[2]-atom2[2])**2)
    BL = round(BL,3)
    return BL
```
{: .language-python}

Now we can change our `for` loop to just call the bond length function.
```
for numA, atomA in enumerate(coord):
    for numB, atomB in enumerate(coord):
       BL_AB = bond_length(atomA, atomB)
       print(symbols[numA], "to", symbols[numB], ":", BL_AB)
```
{: .language-python}

> ## Exercise
>
> Write a function called **bond_check** that checks to see if a particular bond length really represents a bond.  For purposes of this exercise, consider a bond to be a distance between 0.5 and 1.5 angstroms. The input of your function should be a bond length.  Make your function return **True** if the bond length really represents a bond.
>> ## Answer
>> ~~~
>> def bond_check(bond_distance):
>>    if bond_distance > 0 and bond_distance < 1.5:
>>        return True
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Now that we have our `bond_check` function, we can use it in our `for` loop to only print the bond lengths that are really bonds.

```
for numA, atomA in enumerate(coord):
    for numB, atomB in enumerate(coord):
       BL_AB = bond_length(atomA, atomB)
       if bond_check(BL_AB) == True:
           print(F'{symbols[numA]} to {symbols[numB]} : {BL_AB}')
```
{: .language-python}
```
O to H1 : 0.969
O to H2 : 0.969
H1 to O : 0.969
H2 to O : 0.969
```
{: .output}

## Testing a function
Whenever you write code, you always want to verify that it is working correctly.  Many beginning programmers write their whole code and then run it, only to discover that it doesn't work correctly.  However, it can be hard to track down exactly where the mistake is.  This is one significant advantage of using functions in your code; since functions only perform one computational task, you can test each function separately and identify exactly where mistake occur.  Let's write a test for our `bond_length` function.

When you are writing a test for a function, the only input you need to supply is the exact input the function is expecting.  Our function is expecting the coordinates of two atoms.  In our code, these coordinates may come from parsing an xyz file, but this doesn't have to be the case in our test; in our test we need only specify the coordinates.



Note that the second test doesn't depend on the first test.




{% include links.md %}
