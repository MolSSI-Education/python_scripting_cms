---
title: "Testing Code with pytest"
teaching: 15
exercises: 20
questions:
- "How do I test my functions?"
objectives:
- "Write tests for user-defined functions."
- "Use pytest to run all your tests."
keypoints:
- "The python package pytest looks for functions that start with *test* to run."
- "It is particularly important to write tests for the edge and corner cases of your functions."
- "While writing tests can seem time consuming, they are essential to writing good code.  Testing is particularly important when multiple people are collaborating on a complex code."
---
## The Importance of Testing
Beginning programmers often write a lot of code before testing it.  Testing to makes sure your code is working correctly is an essential part of the programming process.  You may have noticed that in the previous lessons, every time a new section of code was created, it was immediately tested to see if it worked correctly, sometimes by doing something as simple as printing the variable that was just read in or calculated to ensure that the code worked correctly.  

As you write more complicated codes, testing becomes even more important.  You will need to make sure that each part of the code works correctly and that it works with the rest of the code.   Often, situations arise where new code is written and it causes an old part of the code, which was working before, to stop working.  Testing each part of your code individually is called *unit testing* and testing how the parts work together is called *integration testing*.  The best practice is to have unit testing that covers most of your code.  

One way to include unit testing is to write tests that test a single function of your code.  In the last lesson, we wrote three functions in our geometry analysis project, one that opened and processed our xyz file, one that calculated a bond length and one that checks to see if a particular distance corresponded to a bond.  We can now write tests for each of those functions.

## Writing unit tests
Since complicated code may have many, many tests, we are going to learn to use a python package called `pytest` to test our code automatically.  The `pytest` package automatically looks for tests in files whose name start with the word test. If you have a function beginning with the word `test` in a module beginning with the word `test`, `pytest` will run the code in that function. If the function does not result in an error, the test passes.

Before we use `pytest`, we first have to make sure that it's installed. We can install pytest using conda in the command line.

~~~
$ conda install pytest
~~~
{: .language-bash}

You should now be able to run `pytest` from your terminal by typing `pytest`. However, we have not written any tests yet, so this won't be useful.

Open a new file in your favorite text editor.  Name your file `test_geom_analysis.py`. Since we are testing functions we wrote earlier in our geometry analysis project, we need to import that code.  (In this section, you may have named your code or your functions slightly differently than the examples given here; that's fine, just make sure you use adjust accordingly.)

~~~
import geom_analysis as ga
~~~
{: .language-python}

A few things are very important to note for this `import` statement.
1. We import `geom_analysis`. This is because the module we are importing is called `geom_analysis.py`. 
2. `geom_analysis.py` must be in the same directory as our test file.

We are importing our module (`geom_analysis.py`) just like we have been importing other python packages and libraries. The functions we defined outside of our `__main__` function are now accessible to us using the syntax `ga.function_name`, just like with other libraries we've used.

What we would like to do now is to write a test for those functions. Next write a function that tests your `calculate_distance` function.  

~~~
def test_calculate_distance():
    coord1 = [0, 0, 0]
    coord2 = [1, 0, 0]
    expected = 1.0
    observed = ga.calculate_distance(coord1,coord2)
    assert observed == expected
~~~
{: .language-python}

Notice a few things about this function.  The name of our function begins with `test`.  This must be true for your test to run with `pytest`  Since the `calculate_distance` function is expecting two inputs, the coordinates of two atoms, we define those variables in our function.  We try to make the tests as simple as possible; in our real code, these coordinates will come from a list we are counting over in a `for` loop.  That is not necessary here; we want to make the test as simple as possible so that we can be sure which part of our code is working or not.  For instance, if we had already tested our `calculate_distance` function and we knew it was working, if our code then did not give the correct bond distances for a molecule, we know the problem is in our `for` loop in the way we are passing the information to the function.  This type of information is valuable when you are trying to solve problems in your code.

After we define the two sets of coordinates, we create a variable called `expected` which is the value we expect the test to return.  We then calculate a value called `observed` by calling the `calculate_distance` function from our code.  The final `assert` statement states what should be true in our test.  If this statement is not true, our test will not pass.'=

Navigate back to your terminal and run your test

~~~
$ pytest
~~~

You should see an output similar to the following:

~~~
============================= test session starts ==============================
platform darwin -- Python 3.6.8, pytest-5.4.1, py-1.8.1, pluggy-0.12.0
rootdir: /Users/jessica/Desktop/cms-workshop
collected 1 item

test_geom_analysis.py .                                                  [100%]

============================== 1 passed in 0.11s ===============================
~~~
{: .output}

Use of Python `assert` here makes the code throw an error if the following statement is `False`. Notice that if we change our test to not have the `assert` statement, the test will pass. This is because the function did not result in an error.

> ## Exercise
> Write a function to test your `bond_check` function.  (The name of your function may be slightly different!)  
>
>> ## Answer
>> ~~~
>> def test_bond_check():
>>     bond_distance = 1.2
>>     expected = True
>>     observed = ga.bond_check(bond_distance)
>>     assert observed == expected
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Once you have both your functions written, save your file of tests and return to the command line.  Run `pytest`.
```
$ pytest -v
```
The "-v" flag stands for verbose.  It tells `pytest` to print more information when it runs the tests.  The output should look like this.
```
=========================== test session starts ============================
collected 2 items                                                          

test_geom_analysis.py::test_calculate_distance PASSED                [ 50%]
test_geom_analysis.py::test_bond_check PASSED                        [100%]

========================= 2 passed in 0.12 seconds =========================
```
{: .output}

> ## Check your Understanding
> Change the `expected` value in your calculate_distance test so that the test will fail.  Run pytest again so you can see the output of a failed test.
>
>> ## Answer
>> ~~~
>> =========================== test session starts ============================
>>
>> collected 2 items                                                          
>>
>> test_geom_analysis.py::test_calculate_distance FAILED                [ 50%]
>> test_geom_analysis.py::test_bond_check PASSED                        [100%]
>>
>> ================================= FAILURES =================================
>> _________________________ test_calculate_distance __________________________
>>
>>     def test_calculate_distance():
>>         coord1 = [0, 0, 0]
>>         coord2 = [1, 0, 0]
>>         expected = 2.0
>>         observed = ga.calculate_distance(coord1,coord2)
>> >       assert observed == expected
>> E       assert 1.0 == 2.0
>>
>> test_geom_analysis.py:9: AssertionError
>> ==================== 1 failed, 1 passed in 0.21 seconds ====================
>> ~~~
>> {: .error}
> {: .solution}
{: .challenge}

Don't forget to fix your `calculate_distance` test after your activity so that it works again.

## Edge Cases and Corner Cases
It is usually easy to think of obvious tests that your functions should pass or fail.  However, your code is often likely to fail in more extreme cases.  The situation where the test examines either the beginning or the end of a range is called an *edge case*.  In a simple, one-dimensional problem, the two edge cases should always be tested along with at least one internal point. This ensures that you have good coverage over the range of values.  When two or more edge cases are combined, it is called a *corner case.* If a function is parametrized by two linear and independent variables, a test that is at the extreme of both variables is in a corner.

Let's think some more about our testing of the `bond_check` function.  We are using a very simple (and arbitrary!) definition for a bond, which is that it has a length greater than 0 and less than or equal to 1.5 angstroms.  In the test we already wrote, we supplied a value between 0 and 1.5 angstroms, so the test should return True.  We already tested this behavior.  We would also like to be sure that the test returns False if we give it a value greater than 1.5 angstroms.  But what does our code do if we give exactly 0 as the value?  Exactly 1.5?  What should the function return?  These are the edge cases for this function.  

> ## Check your Understanding
> What should your code return if the bond distance supplied is exactly 0 angstroms.  What about 1.5 angstroms?
>> ## Answer
>> The 0 angstroms test should return False.  The 1.5 angstrom test should return True.  
> {: .solution}
{: .challenge}

> ## Exercise
> Write three additional test cases for the bond_check function:  one to test that a longer bond distance returns False and both edge cases described above.  
>> ## Answer
>> Note that the original bond_check test has been renamed to bond_check_true.  
>> ~~~
>> def test_bond_check_true():
>>     bond_distance = 1.2
>>     expected = True
>>     observed = ga.bond_check(bond_distance)
>>     assert observed == expected
>>
>> def test_bond_check_false():
>>     bond_distance = 2.0
>>     expected = False
>>     observed = ga.bond_check(bond_distance)
>>     assert observed == expected
>>
>> def test_bond_check_0():
>>     bond_distance = 0
>>     expected = False
>>     observed = ga.bond_check(bond_distance)
>>     assert observed == expected
>>
>> def test_bond_check_1_5():
>>     bond_distance = 1.5
>>     expected = True
>>     observed = ga.bond_check(bond_distance)
>>     assert observed == expected
>> ~~~
>> {: .language-python}
>>
>> All of these tests should pass.  If they do not, it is probably due to a mistake in how your `bond_check` function is defined.  Check your function and adjust accordingly.  A common mistake is that your bond_check function only returns True and does not have an else statement to return False if the bond length doesn't check out.  
> {: .solution}
{: .challenge}

> ## Time Check
> If you are running a  two day workshop, you will likely stop here and not use the next section.
{: .callout}

## Extension - Raising Errors

One concept we have not yet discussed in programming is raising errors. You have probably already seen errors in Python if you have been programming for a while. One error you might have seen is `FileNotFoundError` which happens when you try to open a file that Python can't find (if you constructed your file path incorrectly, for example).

Python will raise errors when it has problems, but you can actually raise errors (also called Exceptions in Python) in your own code too. You can raise an error when one wouldn't usually occur, or you can intercept errors and print your own error message.

Let's talk about a few instances where we might want to check some things or raise errors in your code. 

In general, the syntax to raise an error is:

~~~
raise ErrorType("Your error message here")
~~~
{: .language-python}

You can see a list of built in errors and exceptions that you can raise [here](https://docs.python.org/3/library/exceptions.html).

Consider your `bond_check` function. We've talked about already that if someone passes you a negative distance, it should not be a bond. However, we might decide that if someone gives us a negative distance, we think something is wrong with the code and want to halt execution and raise an error. We can raise a `ValueError` if we get a negative value. The Python documentation says that a value error is "Raised when an operation or function receives an argument that has the right type but an inappropriate value"

~~~
def bond_check(atom_distance, minimum_length=0, maximum_length=1.5):
    
    if atom_distance < 0:
        raise ValueError(F'Invalid atom distance {atom_distance}. Distance can not be less than 0!')

    if atom_distance > minimum_length and atom_distance <= maximum_length:
        return True
    else:
        return False
~~~
{: .language-python}

You can also test that this error is raised in your test.

Consider a test we might write:

~~~
def test_bond_check_negative():
    distance = -1
    expected = False
    calculated = ga.bond_check(distance)
    assert expected == calculated
~~~
{: .language}

With our new exception, this test will fail. We can use a feature of pytest to make sure the correct error is raised. To do this, you must add `import pytest` and modify your test.

~~~ 
import pytest

def test_bond_check_negative():
    distance = -1
    expected = False
    with pytest.raises(ValueError)
        calculated = ga.bond_check(distance)
~~~
{: .language-python}

Now we have raised an error, and tested that this error was actually raised.

> ## Exercise
> Write an exception into your `open_xyz` function where you check the file extension of the file name. Raise a `ValueError` if the file extension is not `.xyz`. Write a test to go with your new value error.
>
>> ## Solution
>> ~~~
>> def open_xyz(filename):
>>     fpath, extension = os.path.splitext(filename)
>>
>>     if extension.lower() != '.xyz':
>>         raise ValueError("Incorrect file type! File must be type xyz")
>> 
>>     xyz_file = numpy.genfromtxt(fname=filename, skip_header=2, dtype='unicode')
>>     symbols = xyz_file[:,0]
>>     coord = (xyz_file[:,1:])
>>     coord = coord.astype(numpy.float)
>>     return symbols, coord
>> ~~~
>> {: .language-python}
>> 
>> And a test
>> def test_open_xyz_error():
>>     fname = 'hello.txt'
>> 
>>     with pytest.raises(ValueError):
>>         ga.open_xyz(fname)
>> {: .language-python}
> {: .solution}
{: .challenge}

