---
title: "Features of Numpy Arrays"
teaching: 40
exercises: 20
questions:
- "What are the differences between numpy arrays and lists?"
- "How can I use NumPy to do calculations?"
objectives:
- "Be able to name the differences between Python lists and numpy arrays."
- "Understand the idea of broadcasting"
keypoints:
- "NumPy arrays which are the same size use element-wise operations when added or subtracted"
- "NumPy uses something called *broadcasting* for arrays which are not the same size to allow arrays to be added or multiplied."
- "NumPy has extensive documentation online - you "
---

[Numpy](https://numpy.org/) is a widely used Python library for scientific computing. It has a number of useful features, including the a data structure called an array which we have already worked with a bit.

## NumPy Arrays vs. Python Lists

Previously, you have worked with the built-in types of `lists`. NumPy arrays seem similar, but offer some distinct advantages. We have also worked with Numpy arrays, but we have not taken advantage of their unique features. 

Numpy arrays take up less space, are faster, and have more mathematical operations associated with them. However, unlike lists, they elements all have to be the same type. This is why we had to read in the file from the Tabular Data lesson using the `dtype` argument.

There are also differences in how lists and numpy arrays behave. Let's look at some of these.

To use the numpy library, we have to import it. When `numpy` is imported, it is often shortened to `np`.

~~~
import numpy as np

# Create some lists
r1 = [0.0, 0.1, 0.0]
r2 = [0.0, 0.0, 0.0]

# Create some numpy arrays
r1_array = np.array(r1)
r2_array = np.array(r2)
~~~
{: .language-python}

In this code block, we've created two lists (`r1` and `r2`). We then created numpy array versions of these lists.

When we print `r1` and `r1_array`, the results look very similar.

~~~
print(r1)
print(r1_array)
~~~
{: .language-python}

~~~
[0.0, 0.1, 0.0]
[0.  0.1 0. ]
~~~
{: .output}

However, one way numpy arrays and lists vary significantly is that you can easily perform element-wise operations on numpy arrays without loops. **This is advantageous because loops in Python are slow. However, `numpy` functions are optimized to run quickly.** If possible, you should avoid loops when working with numpy arrays.

You can add two arrays together, multiply arrays by scalars, or do element-wise multiplcation of arrays.

For example, you can multiply two numpy arrays to get their element-wise product. This means that given two vectors `a = np.array([a0, a1, a2])` and `b = np.array([b0, b1, b2])`, `a * b = [a0*b0, a1*a1, a2*b2]`.

In contrast, if `a` and `b` were lists, you would get an error. 

> ## Check your understanding
> What does the following code result in?
>
> ~~~
> a1 = np.array([2, 1, 0])
> a2 = np.array([1, 3, 5])
> 
> print(a1 * a2)
> ~~~
> {: .language-python}
> 
> What happens if a1 and a2 are lists?
>
>> ## Answer
>> 
>> The code block results in 
>> ~~~
>> [2 3 0]
>> ~~~
>> {: .output}
>> The first element is `a1[0]*a2[0]`, the second element is `a1[1]*a2[1]`, and the third element is `a1[2]*a2[2]`.
>>
>> If a1 and a2 were lists instead, you would get a `TypeError`. `TypeError: can't multiply sequence by non-int of type 'list'`.
>>
> {: .solution}
{: .challenge}

## Returning to Tabular Data

Let's consider our tabular data.

~~~
import os
import numpy

distance_file = os.path.join('data', 'distance_data_headers.csv')
distances = numpy.genfromtxt(fname=distance_file, delimiter=',', dtype='unicode')
headers = distances[0]
data = distances[1:]
data = data.astype(numpy.float)
~~~
{: .language-python}

In a previous lesson, we calculated the mean of each column of the array using the `range` function and a `for` loop. This was good because it reminded us of the `range` function and how `for` loops worked. However, the `numpy.mean` function will let us do that without a for loop.

Checkout the [numpy documentation](https://docs.scipy.org/doc/numpy/reference/generated/numpy.mean.html) for the mean function. Can you figure out what argument we might change to get the mean of each column?

We will use the `axis` argument. If we do not specify `axis`, the mean of the entire array is computed. For the `axis` argument, rows correspond to axis 0, and columns correspond to axis 1. This is similar to slices where you give the indices as `row, column`.

~~~
column_averages = np.mean(data, axis=0)
print(column_averages)
~~~
{: .language-python}

~~~
[5000.5          10.87695093    7.34234496   11.20979133   10.9934435 ]
~~~
{: .output}

> ## Check your understanding
> How would you use the `mean` function and slicing to only get the mean of the columns of data (ie, exluding the frame). Save your answer
> as a variable called `data_averages`.
>> ## Answer
>> We would add slicing to `data` in our mean calculation.
>> ~~~
>> data_averages = np.mean(data[:,1:], axis=0)
>> print(data_averages)
>> ~~~
>> ~~~
>> [10.87695093  7.34234496 11.20979133 10.9934435 ]
>> ~~~
>> {: .output}
>> {: .language-python}
> {: .solution}
{: .challenge}

## Broadcasting

Another special thing about numpy is something called **broadcasting**. Broadcasting occurs when you attempt mathematical operations on arrays that have different shapes. If possible, the smaller array is "broadcast" across the larger array.

For example, using numpy arrays, you can multiply every element in a numpy array by 10

~~~
print(10*r1_array)
~~~
{: .language-python}

~~~
[0. 1. 0.]
~~~
{: .output}

But, if you multiply our variable `r1` (which is a list), by 10, the list will just be repeated 10 times.

~~~
print(r1*10)
~~~
{: .language-python}

~~~
[0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0]
~~~
{: .output}

The multiplication of `r1_array*10` is an example of what is called *broadcasting* in NumPy. You can think of the scalar `10` being stretched during the operation to be the same size and shape as `r1_array`.

Broadcasting looks at the dimensions of each array, and sees if it can match them. For example, if you attempt array addition with an array that has size `(1000, 4)`, and the other has `(1, 4)`, `numpy` will notice that each has four columns, and *broadcast* the smaller array over the larger one.

Imagine you have molecular coordinates stored in a numpy array, and you'd like to translate them by a particular distance vector.

~~~
coordinates = np.array([[0,0,0], [0,1,0],[0,0,1]])
translation_vector = np.array([2,1,0])

print(coordinates)
~~~
{: .language-python}

We would like to move our molecule 2 units in the x direction, 1 unit in the 0 direction, and 0 units in the z direction.

If you were writing a `for` loop to do this, you might write it this way

~~~
new_coordinates = np.zeros([3,3])

for dim in range(len(translation_vector)):
    new_coordinates[:, dim] = coordinates[:, dim] + translation_vector[dim]

print(new_coordinates)
~~~

~~~
[[2. 1. 0.]
 [2. 2. 0.]
 [2. 1. 1.]]
~~~
{: .output}

Broadcasting in `numpy` allows us to achieve that with one command, rather than a `for` loop.

~~~
new_coordinates = coordinates + translation_vector

print(new_coordinates)
~~~
{: .language-python}

> ## Exercise
> Use the features of numpy arrays to subtract the mean of each column from the column data. Leave the `frame` column unchanged. Save your new array as a variable with the name `deviation`
>> ## Solution
>> Recall that we already have a variable with the average of each column (`column_averages`). Using broadcasting, we could subtract this vector from our data array. However, since we want to leave frame unchanged, there is one additional step. Since we don't care about the frame average, we will overwrite it with zero.
>> ~~~
>> column_averages[0] = 0
>> deviation = data - column_averages
>> print(deviation)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

## Optional - Returning to the geometry analysis project

In your geometry analysis project, you had to analyze an xyz file, find the bonds, and print bond lengths.

We can rewrite that project using the features of numpy arrays.  

Recall that a solution given for a function for calculating distances between two points was the following

~~~
def calculate_distance(rA, rB):
    """Calculate distance between points A and B"""
    x_dist = (rA[0] - rB[0]) ** 2
    y_dist = (rA[1] - rB[1]) ** 2
    z_dist = (rA[2] - rB[2]) ** 2
    
    distance = np.sqrt(x_dist + y_dist + z_dist)
    
    return distance
~~~
{: .language-python}

This function would work for both lists and numpy arrays, because it does not assume that rA and rB can do something like element-wise subtraction. 

> ## Exercise
> Using what you've learned about numpy arrays, rewrite the `calculate_distance` function to use the features of numpy arrays.
>> ## Answer
>> If rA and rB are both numpy arrays, we can use element-wise subtraction
>> ~~~
>>  def calculate_distance(rA, rB):
>>     """Calculate the distance between points A and B"""
>>     AB = (rB - rA)**2
>>     distance = np.sqrt(np.sum(AB))
>>     return distance
>> ~~~
>> {: .language-python}
>> You might also have used the `numpy` function `np.linalg.norm` which calculates the magnitude of a given vector.
>> ~~~
>> def calculate_distance(rA, rB):
>>    """Calculate the distance between points A and B"""
>>    dist_vec = (rA-rB)
>>    distance = np.linalg.norm(dist_vec)
>>    return distance
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Redefine your original distance function as `calculate_distance_list`. Using both, we see that both functions give the same answer. 

~~~
print(calculate_distance_list(r1, r2))
print(calculate_distance(r1_array, r2_array))
~~~
{: .language-python}