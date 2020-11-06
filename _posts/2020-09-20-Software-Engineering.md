---
layout: post
title: Software Engineering for Data Scientists
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Software Engineering]
mathjax: true
summary: Data Processing in Python
---

# Software Engineering

#### Algorithmic Efficiency

Use appropriate data structures. If the order of elements in a collection doesn’t matter, use
set instead of list. In Python, the operation of verifying whether a specific example x belongs
to S is efficient when S is declared as a set and is inefficient when S is declared as a list.

Another important data structure that you can use to make your Python code more efficient
is dict. It is called a dictionary or a hashmap in other languages. It allows you to define a
collection of key-value pairs with very fast lookups for keys.

Using libraries is generally most reliable - you should only write your own code if you are a
researcher or it is truly required. Scientific Python packages like numpy, scipy, and scikit-learn
were built by experienced scientists and engineers with efficiency in mind. They have many
methods implemented in the C programming language for maximum efficiency.

If you need to iterate over a vast collection of elements, use generators that create a function
that returns one element at a time rather than all the elements at once.

Use the cProfile package in Python to find inefficiencies in your code.

Finally, when nothing can be improved in your code from the algorithmic perspective, you
can further boost the speed of your code by using:
• multiprocessing package to run computations in parallel, and
• PyPy, Numba or similar tools to compile your Python code into fast, optimized machine
code.


#### Avoid hardcode paths
If you hardcoded paths others don't have access to, they can't run your code and have to
look in lots of places to manually change paths.

Instead of using relative paths, global path config variables to make your data easily accessible.

#### Write data science code as DAGs

Instead of linearly chaining functions, data science code is bitter written as a set of 
tasks with dependencies between them. Use airflow, luigi.


#### Don't write for loops
For loops are one of the first things you learn in your programming path, but they are slow and
excessively wordy, typically indicating you are unaware of vectorized alternatives.

Numpy, scipy and pandas have vectorized for most things that you think might require for loops.


#### Write Tests
As data, parameters or user input change, your code might break, sometimes without you noticing.


Use assert statements to check for data quality. pandas has equality test.
```
assert df['id'].unique().shape[0] == len(ids) #  have data for all ids
assert df.isna().sum() < 0.9 # catch missing values
assert df.groupby(['g', 'date']).size().max() == 1 # no duplicate values/date?
```
 

#### Save data as binary data format
 
Use parquet or other binary data formats with data schemas, ideally ones that compress data.

#### Don't use Jupyter Notebooks

It does not scale great. Instead of using pycharm or other IDEs.
 
 
#### Document your code, always

Take extra time to document what you did. You will thank yourself and other will do so even more!
 
#### Sources
- https://www.dropbox.com/s/im1s2skkaikzrrs/Chapter8.pdf?dl=0
