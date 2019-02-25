---
title: "Testing Code"
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

As you write more complicated codes, testing becomes even more important.  You will need to make sure that each part of the code works correctly and that it works with the rest of the code.   Often, situations arise where new code is written and it causes an old part of the code, which was working before, to stop working.  Testing each part of your code individually is called *unit testing* and testing how the parts work together is called *integration testing* testing.  The best practice is to have unit testing that covers most of your code.  

One way to include unit testing is to write tests that test a single function of your code.  In the last lesson, we wrote two functions in our geometry analysis project, one that calculated a bond length and one that checks to see if a particular distance corresponded to a bond.  We can now write tests for each of those functions.

## Writing unit tests
Since complicated code may have many, many tests, we are going to learn to use a python package called `pytest` to test our code automatically.  The `pytest` package automatically looks for tests in files whose name start with the word test.  Open a new file in your favorite text editor.  Name your file test_geom_analysis.py.  Just like any other python module, you have to import `pytest` to use it.  Since we are testing functions we wrote earlier in our geometry analysis project, we also need to import that code.  (In this section, you may have named your code or your functions slightly differently than the examples given here; that's fine, just make sure you use adjust accordingly.)
```
import pytest
import geom_analysis as ga
```
{: .language-python}
Next write a function that tests your `bond_length` function.  
```
def test_bond_length():
    coord1 = [0, 0, 0]
    coord2 = [1, 0, 0]
    expected = 1.0
    observed = ga.bond_length(coord1,coord2)
    assert observed == expected
```
Notice a few things about this function.  The name of our function begins with `test`.  This must be true for your test to run with `pytest`  Since the bond_length function is expecting two inputs, the coordinates of two atoms, we define those variables in our function.  We try to make the tests as simple as possible; in our real code, these coordinates will come from a list we are counting over in a `for` loop.  That is not necessary here; we want to make the test as simple as possible so that we can be sure which part of our code is working or not.  For instance, if we had already tested our `bond_length` function and we knew it was working, if our code then did not give the correct bond lengths for a molecule, we know the problem is in our `for` loop in the way we are passing the information to the function.  This type of information is valuable when you are trying to solve problems in your code.

After we define the two sets of coordinates, we create a variable called `expected` which is the value we expect the test to return.  We then calculate a value called `observed` by calling the `bond_length` function from our code.  The final `assert` statement states what should be true in our test.  If this statement is not true, our test will not pass.

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
============================= test session starts ==============================
collected 2 items                                                              

test_geom_analysis.py::test_bond_length PASSED                           [ 50%]
test_geom_analysis.py::test_bond_check_true PASSED                       [100%]

=========================== 2 passed in 0.16 seconds ===========================
```
{: .output}

> ## Check your Understanding
> Change the `expected` value in your bond_length test so that the test will fail.  Run pytest again so you can see the output of a failed test.
>
>> ## Answer
>> ~~~
>> ============================= test session starts ==============================
>>                                                  
>> test_geom_analysis.py::test_bond_length FAILED                           [ 50%]
>> test_geom_analysis.py::test_bond_check_true PASSED                       [100%]
>>
>> =================================== FAILURES ===================================
>> _______________________________ test_bond_length _______________________________
>>
>>     def test_bond_length():
>>         coord1 = [0, 0, 0]
>>         coord2 = [1, 0, 0]
>>         expected = 2.0
>>         observed = ga.bond_length(coord1,coord2)
>> >       assert observed == expected
>> E       assert 1.0 == 2.0
>>
>> test_geom_analysis.py:9: AssertionError
>> ====================== 1 failed, 1 passed in 0.41 seconds ======================
>> ~~~
>> {: .ouput}
> {: .solution}
{: .challenge}

Don't forget to fix your bond_length test after your activity so that it works again.

## Edge Cases and Corner Cases
It is usually easy to think of an obvious tests that your functions should pass or fail.  However, your code is often likely to fail in more extreme cases.  The situation where the test examines either the beginning or the end of a range is called an *edge case*.  In a simple, one-dimensional problem, the two edge cases should always be tested along with at least one internal point. This ensures that you have good coverage over the range of values.  When two or more edge cases are combined, it is called a *corner case.* If a function is parametrized by two linear and independent variables, a test that is at the extreme of both variables is in a corner.

Let's think some more about our testing of the `bond_check` function.  We are using a very simple (and arbitrary!) definition for a bond, which is that it has a length greater than 0 and less than or equal to 1.5 angstroms.  In the test we already wrote, we supplied a value between 0 and 1.5 angstroms, so the test should return True.  We already tested this behavior.  We would also like to be sure that the test returns False if we give it a value greater than 1.5 angstroms.  But what does our code do if we give exactly 0 as the value?  Exactly 1.5?  What should the function return?  These are the edge cases for this function.  

> ## Check your Understanding
> What should your code return if the bond distance supplied is exactly 0 angstroms.  What about 1.5 angstroms?
>> ## Answer
>> The 0 angstroms test should return False.  The 1.5 angstrom test should return True.  
> {: .solution}
{: .challenge}

> ## Exercise
> Write three additional test cases for the bond_check function:  one to test that a longer bond distance is not a bond and both edge cases described above.  
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
