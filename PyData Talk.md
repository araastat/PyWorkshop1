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
# Data analysis using the Python ecosystem

## Before pandas

### Scientific Python
Python has a scientific computing ecosystem that has existed since the mid-90s

+ Numpy (as Numeric since 1995, current incarnation since 2006)
+ Scipy (2001)
+ Matplotlib (early 2000s)
+ Sympy (2007)

This ecosystem is meant to emulate __Matlab__, and is geared to numerical data

### Natural Language Processing
Python has a mature natural language processing library, __NLTK__ (Natural Language ToolKit), for symbolic and statistical natural language processing.

### What was lacking

This ecosystem provided tools for data munging and analysis, but didn't necessarily make it easy. 

+ No container for heterogeneous data types (a la <code>data.frame</code> in R) that can be easily manipulated

    - Lists and dicts are around, but needed to be simpler
    - Metadata (labeling rows and columns, referencing by labels)
    - Manipulation and extraction using either array or label or component syntax
    
+ Easy handling of missing data

    - Masked arrays in numpy are available
    - Simple imputation
    - Easy way to get complete or partially complete cases

+ Easy data munging capabilities

    - reshaping data from wide to long and v.v
    - subsetting
    - split-apply-combine
    - aggregation
    
+ Exploratory data analysis, summaries
+ Statistical modeling and machine learning

    - Was rudimentary c. 2009


<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->

## pandas

__pandas__ (Python data analysis toolbox) was first released in 2008. The current version is 0.12, released in July.

+ Puts R in the bullseye
+ Wants to emulate R's capabilities in a more efficient computing environment
+ Provide a rich data analysis environment that can be easily integrated into production and web infrastructures

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
<quote style="font-family:Times New Roman; font-style:italic; font-size:120%; color: red">R makes users forget how fast a computer really is</quote> <p style="text-align:right">John Myles White, SPDC, October 2013</p>
<!-- #endregion -->

```python jupyter={"outputs_hidden": false} slideshow={"slide_type": "subslide"}
import warnings
warnings.filterwarnings("ignore", category=DeprecationWarning)
warnings.filterwarnings("ignore", category=UserWarning)
```

```python jupyter={"outputs_hidden": false} slideshow={"slide_type": "slide"}
import numpy as np
import pandas as pd
pd.__version__
```

<!-- #region slideshow={"slide_type": "slide"} -->
### <code>pandas</code> has two main data structures: <code>Series</code> and <code>DataFrame</code>
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
## Series
<!-- #endregion -->

```python jupyter={"outputs_hidden": false} slideshow={"slide_type": "subslide"}
values = [5,3,4,8,2,9]
vals = pd.Series(values)
vals
```

<!-- #region slideshow={"slide_type": "subslide"} -->
Each value is now associated with an _index_. The index itself is an object of class <code>Index</code> and can be manipulated directly.
<!-- #endregion -->

```python jupyter={"outputs_hidden": false} slideshow={"slide_type": "subslide"}
vals.index
```

```python jupyter={"outputs_hidden": false} slideshow={"slide_type": "subslide"}
vals.values
```

```python jupyter={"outputs_hidden": false}
vals2 = pd.Series(values, index=['a','b','c','d','e','f'])
vals2
```

```python jupyter={"outputs_hidden": false}
vals2[['b','d']]
```

```python jupyter={"outputs_hidden": false}
vals2[['e','f','g']]
```

```python jupyter={"outputs_hidden": false}
vals3 = vals2[['a','b','c','f','g','h']]
vals3
```

```python jupyter={"outputs_hidden": false}
vals3.isnull()
```

```python jupyter={"outputs_hidden": false}
vals3.dropna()
```

```python jupyter={"outputs_hidden": false}
vals3.fillna(0)
```

```python jupyter={"outputs_hidden": false}
vals3.fillna(vals3.mean())
```

```python jupyter={"outputs_hidden": false}
vals3.fillna(method='ffill')
```

```python jupyter={"outputs_hidden": false}
vals3.describe()
```

## DataFrame

```python jupyter={"outputs_hidden": false}
vals.index=pd.Index(['a','b','c','d','e','f'])
vals3=vals3[['a','c','d','e','z']]

```

```python jupyter={"outputs_hidden": false}
dat = pd.DataFrame({'orig':vals,'new':vals3})
dat
```

```python jupyter={"outputs_hidden": false}
dat.fillna(1)
```

```python jupyter={"outputs_hidden": false}
cars = pd.read_csv('mtcars.csv')
cars[:10]
```

```python jupyter={"outputs_hidden": false}
cars.cyl
```

```python jupyter={"outputs_hidden": false}
cars.ix[[5,7]]
```

```python jupyter={"outputs_hidden": false}
cars['kmpg']=cars['mpg']*1.6
cars[:4]
```

```python jupyter={"outputs_hidden": false}
del cars['kmpg']
cars[:4]
```

```python jupyter={"outputs_hidden": false}
cars.mpg[:10]
```

```python jupyter={"outputs_hidden": false}
cars3=cars.stack()
cars3[:20]
```

```python jupyter={"outputs_hidden": false}
cars3.unstack()[:5]
```

```python jupyter={"outputs_hidden": false}
grouped=cars.groupby(['cyl','gear','carb'])
grouped['mpg'].mean()
```

```python jupyter={"outputs_hidden": false}
stats = ['count','mean','median']
grouped.agg(stats)[['mpg','disp']]
```

```python jupyter={"outputs_hidden": false}
grouped.first()
```

```python jupyter={"outputs_hidden": false}
cars[cars.cyl==4]
```

```python jupyter={"outputs_hidden": false}
tips = pd.read_csv('tips.csv')
tips[:5]
```

```python jupyter={"outputs_hidden": false}
tips['tip_pct'] = tips['tip']/tips['total_bill']*100
```

```python jupyter={"outputs_hidden": false}
groupedtips = tips.groupby(['sex','smoker'])
groupedtips['tip_pct'].agg('mean')
```

```python jupyter={"outputs_hidden": false}
result=tips.groupby('smoker')['tip_pct'].describe()
```

```python jupyter={"outputs_hidden": false}
result.unstack('smoker')
```

```python jupyter={"outputs_hidden": false}
states = ['Ohio','New York','Vermont','Oregon','Washington','Nevada']
group_key=['East']*3 + ['West']*3
data = pd.Series(np.random.randn(6), index=states)
data[['Vermont','Washington']]=np.nan
data
```

```python jupyter={"outputs_hidden": false}
data.groupby(group_key).mean()
```

```python jupyter={"outputs_hidden": false}
fill_mean=lambda g: g.fillna(g.mean())

data.groupby(group_key).apply(fill_mean)
```

```python jupyter={"outputs_hidden": false}
tips.pivot_table(rows=['sex','smoker'])
```

```python jupyter={"outputs_hidden": false}
tips.pivot_table(['tip_pct','size'],rows=['sex','day'], cols='smoker')
```

## Statistical modeling


### statsmodels

Methods provided include

+ Linear regression
+ Generalized linear models
+ ANOVA
+ Nonparametric methods
+ Few others

```python jupyter={"outputs_hidden": false}
import statsmodels as sm
import statsmodels.formula.api as smf
cars[:5]
```

```python jupyter={"outputs_hidden": false}
mod1 = smf.ols('mpg~disp+hp+C(cyl)-1', data=cars) # change to category
mod1.fit().summary()
```

## Machine learning

### Scikits-learn

Methods include:

+ Cluster analysis
+ Dimension reduction
+ Generalized linear models
+ Support Vector Machines
+ Nearest neighbors
+ Decision Trees
+ Ensemble methods
+ Discriminant analysis
+ Cross-validation
+ Transformations

```python jupyter={"outputs_hidden": false}
import sklearn as learn
from sklearn.ensemble import RandomForestRegressor
X = cars.values[:,1:]
y = cars.values[:,0]

rf = RandomForestRegressor(n_estimators=100)
rf = rf.fit(X, y)
ypred=rf.predict(X)
```

```python jupyter={"outputs_hidden": false}
import matplotlib as mpl
import matplotlib.pyplot as plt
plt.plot(y,ypred,'.')
```

```python jupyter={"outputs_hidden": false}
import seaborn as sns
sns.set(palette="Purples_r")
mpl.rc("figure", figsize=(5, 5))

d = pd.DataFrame({'y':y,'ypred':ypred})
```

```python jupyter={"outputs_hidden": false}
sns.lmplot('y','ypred',d)
```

```python jupyter={"outputs_hidden": false}
sns.lmplot("total_bill","tip",tips, col="sex",color="time")
```

```python jupyter={"outputs_hidden": false}
from IPython.core.display import HTML
def css_styling():
    styles = open("styles/custom.css", "r").read()
    return HTML(styles)
css_styling()
```

```python jupyter={"outputs_hidden": false}

```