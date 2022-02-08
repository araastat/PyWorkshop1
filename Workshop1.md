---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.4.2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #region slideshow={"slide_type": "slide"} -->
# Python Data Analysis 1
<!-- #endregion -->

## Why Python?


Python is one of the most popular general-purpose scripting languages in use today. It has over *40K* user-contributed packages for various purposes, including software prototyping and development, GUI development, web programming and scientific programming (about 2400). It is one of the best "glue" languages, that can link together functionalities from different software platforms. 


## Why are we here today? (a.k.a., History of PyData (2014))


* Numpy (starting life as Numeric), c. 1995
* Scipy (2001)
* Matplotlib (early 2000s)
* Sympy (2007)

These formed the core of the Python Data Stack. They were meant to be an open-source replacement for **Matlab**, targeting engineers and *numerical* data. 

Matlab and its clones are great for engineering work and pure numerical problems, but, as we know, data is not just numeric. 

* It can be strings/characters, hash tables, times, images, sounds.  
* It can be stored in 
    * text files (as csv, json, XML, HTML), 
    * relational databases (SQL in its many versions), 
    * NoSQL databases,
    * ...

What we do with data is also not just pure mathematical manipulation and analysis. It includes

* Statistical modeling
* Predictive modeling
* Simulation
* Visualization
* Reporting

More was needed in Python to unify the capabilities for dealing with and analyzing the kinds of data we see today. In particular, we would need:

1. A container for heterogeneous data types that can be easily manipulated
    - allowing for metadata (labeling rows and columns, referencing by labels)
    - allowing for manipulation using array or label or component syntax
2. Easy handling of missing data
    - Simple imputation
    - Extracting complete or partially complete cases
    - Masked arrays available in numpy, but awkward
3. Data munging
    - reshaping data from wide to long and v.v.
    - subsetting
    - split-apply-combine
    - aggregation
4. Easy exploratory analysis
5. Statistical modeling and machine learning


### pandas (Python Data Analysis Library) [web](http://pandas.pydata.org)


<code>pandas</code> changed the way data analysis is performed in Python. It makes importing data, and data munging, much much easier. It is built on top of <code>numpy</code>, but it is much easier to use. This is where we will begin our data analytic journey. We will be using version 0.13.1, released in February.

- Puts R in the bullseye
- Wants to emulate R's capabilites in a more efficient computing environment
- Provide a rich data analysis environment that can be easily integrated into production and web infrastructures


# A Python Primer


### A calculator

```python
from __future__ import division
```

```python
2+2
```

```python
2**3
```

```python
17/3
```

```python
17//3
```

### Lists

```python
x = [1,2,3,4,5,6]
x
```

```python
x = range(6)
x
```

```python
y = [3,10,'Apple',[2,3]]
print(y[0])
print(y[2])
print(y[3])
```

List comprehensions

```python
[u for u in x if u%2 ==0] # Is a number even?
```

```python
[u for u in x if u%2==1] # Is a number odd?
```

### Loops

```python
x = [3,5,8,14]
sum = 0
for i,u in enumerate(x):
    print i
    sum = sum+u

print(sum)
```

```python
a, b = 20,30
print b
```

### Functions

```python
def fib(n):
    a, b = 0, 1
    while a < n:
        print(a)
        a, b = b, a+b
    print()

fib(1000)
```

### More object types


#### Strings

```python
a = 'Every'
b = 'good'
c = 'boy'
d = 'does'
e = 'fine'
```

```python
a+' '+b+' '+c
```

```python
';'.join([a,b,c,d,e])
```

This is an example of object orientation in Python. ' ' is an object that has a function <code>join</code> attached to it. 


The character '.' has special meaning in Python, so **DON'T** use it in variable names. It is used to denote a hierarchical object structure in Python, as we shall soon see.


#### Dicts

```python
me = {"First Name": 'Abhijit', "Last Name": 'Dasgupta','Age':42, 'Sex':'M','Height':175}
print(me['First Name'])
print(me['Age'])
me.values()
```

#### Tuples

```python
constants = (3.1415, 2.2306)
```

Tuples are like lists, but they are *immutable*, i.e., you can't change them on the fly

```python
constants[0]=1
```

### Python packages


Python follows a packaging system, that can be accessed at [PyPi](http://pypi.python.org/pypi). Anaconda provides an easy interface to install packages into your system related to scientific computing.

<!-- #raw -->
> conda install nltk
<!-- #endraw -->

Python itself has a couple of nice methods for installing packages, namely **pip** and **easy_install**. Both are available in your installation and are general purpose tools for installing packages

<!-- #raw -->
> pip install fastcluster
<!-- #endraw -->

<!-- #raw -->
> easy_install fastcluster
<!-- #endraw -->

We will be using several Python packages related to data analysis. However, there are several general packages that are extremely useful and should be in your toolbox for general scripting, file manipulation and the like.

- os (operating system functions)
- shutil (file manipulation)
- re (regular expressions)
- zipfile and tarfile (Dealing with compressed archives)

You should also strongly consider using [IPython](http://www.ipython.org) which provides interactive shells, a web notebook (you're looking at it), and high performance tools for parallel computing


## Some expectation setting


What to expect

- Overview of Python data analysis tools
- Hands-on examples
- A launching pad for your adoption and use of Python for data analyses

What **not** to expect

- A comprehensive, all-encompassing description of Pydata tools
- Covering the full functionality of Python and Pydata


## Useful references


1. [Python for Data Analysis](http://shop.oreilly.com/product/0636920023784.do) by Wes McKinney (also [web](http://pandas.pydata.org))
2. [PyData conference talks](http://www.vimeo.com/pydata)


```python
import warnings
warnings.filterwarnings("ignore", category=DeprecationWarning)
warnings.filterwarnings("ignore", category=UserWarning)
```

# Let's get started with data!!


<quote style="font-family:Times New Roman; font-style:italic; font-size:120%; color: red">R makes users forget how fast a computer really is</quote> <p style="text-align:right">John Myles White, SPDC, October 2013</p>

```python
import numpy as np
import pandas as pd
#from pandas import *
pd.__version__
```

<code>pandas</code> has two main data structures: <code>Series</code> and <code>DataFrame</code>

```python
values = [5,3,4,8,2,9]
vals = pd.Series(values)
vals
```

Each value is now associated with an _index_. The index itself is an object of class <code>Index</code> and can be manipulated directly

```python
vals.index
```

```python
vals.values # Results in a numpy array
```

Let's change the index

```python
vals2=pd.Series(values, index=['a','b','c','d','e','f'])
vals2
```

```python
vals2[['b','d']]
```

```python
vals2[['e','f','g']]
```

```python
vals3 = vals2[['a','b','c','f','g','h']]
vals3
```

#### Missing value manipulation

```python
vals3[vals3.isnull()] # Identification
```

```python
vals3.dropna() # Filtering
```

```python
vals3.fillna(0) # Imputation with the value 0
```

That's a little unsophisticated. Let's try

```python
vals3comp = vals3.fillna(vals3.mean()) # mean value imputation
```

```python
vals3
```

```python
vals3.fillna(method='ffill') # Last value carried forward, also vals3.ffill()
```

### Summarizing data

```python
np.round(vals3.describe(),2)
```

## DataFrame


We can create a DataFrame from 

- a <code>dict</code>
- importing data using <code>pandas</code> import functions
- a set of <code>Series</code> objects

```python
vals.index = pd.Index(['a','b','c','d','e','f'])
vals3 = vals3[['a','b','c','d','e','z']]
dat = pd.DataFrame({'orig':vals,'new':vals3})
```

```python
dat
```

There are two things to note here:

1. We created a dict and then passed it to the DataFrame function, which generates the DataFrame object
2. The two objects were merged based on their indices, and places where the index didn't match were filled with NaN

```python
pd.DataFrame([vals,vals2,vals3]).T
```

```python
dat1=dat.fillna(method='ffill')
dat1
```

```python
dat1.drop_duplicates()
```

```python
v = 'David '
v.strip()
```

```python
help(v.strip)
```

```python
cars = pd.read_csv('data/mtcars.csv') # By far the easiest way to read csv files
```

```python
cars
```

### Slicing and Dicing

```python
cars[:10] # First 10 rows
```

```python
cars.head()
```

```python
cars.index
```

```python
cars.columns
```

```python
cars.iloc[1:3,3:6]
```

```python
cars[1:3][['hp','qsec','am']]
```

```python
cars.loc[:4,'hp':'gear']
```

### Adding on the fly

```python
cars['kmpg'] = cars['mpg']*1.6
cars
```

```python
del cars['kpmg']
cars.head()
```

```python
cars.loc[32]=6
cars.tail()
```

```python
cars.to_csv('data/cars1.csv')
cars.to_excel('data/cars1.xls')
```

## Data munging


### Wide-to-long and v.v.

```python
cars[['mpg','cyl','gear']]
```

```python
cars[['mpg','cyl','gear']].stack()
```

```python
bl[5].cyl
```

### Subsetting

```python
cars=cars[:-1]
cars[(cars['cyl']>4) & (cars['hp']<120)]
```

### Appending

```python
cars1 = cars[:5]
cars2 = cars[7:]
newcars = cars1.append(cars2)
newcars
```

```python
cars3 = cars.loc[:4,'mpg':'cyl']
cars3
```

```python
cars4 = cars.loc[2:6,'qsec':'gear']
cars4
```

```python
cars3.join(cars4, how='outer')
```

## Split-apply-combine


This concept (exemplified by <code>plyr</code> in R) splits a data set by some criterion, applies a function to each piece and then puts the results back together again. 

```python
%pylab inline
```

```python
carsbycyl = cars.groupby(['cyl','gear'])
np.round(carsbycyl.agg(['count','mean','median','std']).loc[:,'disp':'hp'],2)

```

```python
cars.pivot_table(['mpg','wt'],rows=['cyl','gear'], aggfunc=[np.mean,np.median])
```

## Other data imports


### Compressed data files

```python
import os
os.listdir('data/Rlogs')
```

```python
log1 = pd.read_csv('data/Rlogs/2014-02-09.csv.gz',
                   parse_dates={'Timestamp':['date','time']},
                   index_col='Timestamp',
                   compression='gzip')
log1.head()
```

```python
import os
logfiles = os.listdir('data/Rlogs')
inputs = []
for f in logfiles:
    inputs.append(pd.read_csv(os.path.join('data/Rlogs',f),
                              index_col='Timestamp',
                              compression='gzip', 
                              parse_dates={'Timestamp':['date','time']}))
                        

logs = inputs[0].append(inputs[1:])
```

```python
logs.head()
```

```python
logs.groupby(logs.index.day)['r_os'].count().plot()
```

```python
logs['r_os'].value_counts()
```

```python
def comptype(ops):
    ops = str(ops)
    if ops.find('ming')>-1:
        ops = 'Win'
    elif ops.find('darwin')>-1:
        ops='Mac'
    elif ops.find('linux')>-1:
        ops='Linux'
    else:
        ops='Other'    
    return ops

```

```python
logs['r_os'].map(comptype).value_counts()
```

### SQLite

```python
import sqlite3
from pandas.io import sql
cnx = sqlite3.connect('data/mydb.sqlite')
```

```python
sql.read_sql("select * from phenotype where sex = 'MALE' and bmi < 20;",cnx)
```

## Basic Visualization

```python
%pylab inline
import matplotlib.pyplot as plt
```

```python
cars.mpg.plot(kind='kde');
```

```python
plt.plot(cars.wt,cars.mpg,'bo');
```

```python
cars.boxplot('mpg',by=['cyl','gear']);

```

```python
cars.cyl.value_counts().plot(kind='bar')
```

```python
_ = pd.scatter_matrix(cars, figsize=(25,20))
```

```python
from IPython.core.display import HTML
def css_styling():
    styles = open("styles/custom.css", "r").read()
    return HTML(styles)
css_styling()
```
