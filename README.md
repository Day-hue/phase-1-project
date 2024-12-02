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
I load the packages and import the file into a dataframe. I"ll set a default view, specify encoding and let
pandas handle the dtypes

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
I prepare the dataframe for further analysis by replacing the . with a _ and changing to lowercase in the column
names fixing the missing,removing unnecessary columns that aren't needed in my analysis,correct the data types,
drop columns that are missing alot of values, update empty cells with 0 for numerical columns and unknown for categorical columns 

I made new columns too namely: date,injury_severity and region

## Exploration

### accident vs incident
lets plot a bar graph of accident and incident vs accident counts

``` python
# Create a new figure with a specified size (width, height)
plt.figure(figsize=(8, 4))

# Create a bar plot to visualize the counts of accidents by investigation type
plt.bar(accident_counts['investigation_type'], accident_counts['count'], color='skyblue')

# Set the title of the plot to describe what is being visualized
plt.title('Count of Accidents by Investigation Type')

# Label the x-axis to indicate what the categories represent
plt.xlabel('Investigation Type')

# Label the y-axis to indicate what the counts represent
plt.ylabel('Count')

# Rotate the x-axis tick labels by 45 degrees for better readability
plt.xticks(rotation=45)

# Adjust the layout to prevent overlap and ensure everything fits well in the figure and display the figure
plt.tight_layout();
```












