---
title: "Running code from the Linux Command Line"
teaching: 10
exercises: 5
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

We are now going to move our geometry analysis code out of the jupyter notebook and into a format that can be run from the Linux command line.  Open your favorite text editor and create a new file called "geom_analysis.py" (or choose another filename, just make sure the extension is .py).  Paste in your geometry analysis code from your jupyter notebook and save your file.

Before you can run your code from the command line, you need to make it executable.  At the command prompt in your terminal window, make sure you are in the directory where you saved your geom_analysis.py file and type
```
$ chmod u+x geom_analysis.py
```
This changes the mode (chmod) of the file geom_analysis.py.  It enables the user (u) to execute the file (+x).  

Now all you have to do to run your code is type
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
Good programming practice is to import all of your libraries at the top of your code, so if you imported any other libraries (like numpy) in your code, you should move all the import lines to the top of your code.  

Now that you have imported the `sys` library, you can use its functions.  The library has a function called `sys.argv()` which creates a list of all the arguments the user enters at the command line.  Everything after *python* is an argument, so `sys.argv[0]` is always the name of your script.  We would like our code to accept the name of the xyz file we want to analyze as an argument.  Add this line to your code.
```
filename = sys.argv[1]
```
Then you need to go the part of your code where you read in the data from the xyz file and change the name of the input file to `filename`.  

Save your code and go back to the Terminal window.  Make sure you are in the directory where your code is saved and type
```
$ python geom_analysis.py water.xyz
```
Check that the output of your code is what you expected.

> ## Exercise
> If your code prints to a file, you might also want to specify the name of that file.  Change your code to accept two arguments, the name of the xyz file to analyze and the name of the file you want it to output.  
>
>> ## Answer
>> Add a line to your code like
>> ```
>> outfile = sys.argv[2]
>> ```
>> then in the part of your code where you open your file for writing, change the name of the file to `outfile`
> {: .solution}
{: .challenge}

We could add more here, like creating defaults if the user doesn't enter in an output file name or throwing an error message if they don't enter an xyz file name.

{% include links.md %}
