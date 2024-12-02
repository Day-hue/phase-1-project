# phase-1-project
Exploring the NTSB Aviation Accident Database
================

My data is from kaggle and its about the NTSB aviation accident . It contains information from 1962 and
later about civil aviation accidents and selected incidents within the
United States, its territories and possessions, and in international
waters.

I want to explore this data set to find out which aircrafts have the lowest risk, how accident
rates have changed over time,the comparison of accident rates on  different aircraft manufacturers
and models, and to see how the data in this dataset correlate.

I notice that the file is a bit messed up. Some missing values and some are empty.hence I decide to clean 
the data.

## Importing data
I load the packages and import the file into a dataframe. I"ll set a default view, specify encoding and let pandas handle the dtypes

``` python
#import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

#for default view
pd.set_option("display.max_columns",50)

#load dataset,specify encoding and let pandas handle the dtypes
data=pd.read_csv('AviationData.csv',encoding='ISO-8859-1',low_memory=False)

#view the first few rows of the dataset
data.head()
```

So, this is how our data set appears straight
away:










