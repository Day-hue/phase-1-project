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
### accidents over time
check whether the number of accidents has decreased over time and plot.
``` python
# Group by year and count the number of accidents
yearly_accident_counts = data1['year'].value_counts().reset_index()
yearly_accident_counts.columns = ['year','TotalAccidents']

# Sort by year
accident_counts = yearly_accident_counts.sort_values(by='year')

# Set the style of seaborn
sns.set(style="whitegrid")

# Create a new figure with a specified size (width, height)
plt.figure(figsize=(8, 6))

# Create a line plot to visualize the trend of accidents over the years
sns.lineplot(data=accident_counts, x='year', y='TotalAccidents', marker='o')

# Set the title of the plot to describe what is being visualized
plt.title('Trend of Aviation Accidents Over Time')

# Label the x-axis to indicate what the values represent
plt.xlabel('Year')

# Label the y-axis to indicate what the values represent
plt.ylabel('Total Accidents')

# Rotate the x-axis tick labels by 45 degrees for better readability
plt.xticks(rotation=45)

# Add a grid to the plot for easier visualization of trends and display
plt.grid();
```
But what about the number of fatalities per year? Let’s compute that and
group by date again.
``` 
# Calculate total injuries by aircraft category
injuries_by_category = data1.groupby('aircraft_category').agg({
    'total_fatal_injuries': 'sum',
    'total_serious_injuries': 'sum',
    'total_minor_injuries': 'sum',
    'total_injuries': 'sum'
}).reset_index()

# Display the results
injuries_by_category
```
then plot
``` 
# Plotting total injuries by aircraft category
plt.figure(figsize=(12, 6))
bar_width = 0.2
index = range(len(injuries_by_category))

# Create bars for each type of injury
plt.bar(index, injuries_by_category['total_fatal_injuries'], bar_width, label='Fatal Injuries', color='red')
plt.bar([i + bar_width for i in index], injuries_by_category['total_serious_injuries'], bar_width, label='Serious Injuries', color='orange')
plt.bar([i + 2 * bar_width for i in index], injuries_by_category['total_minor_injuries'], bar_width, label='Minor Injuries', color='yellow')

# Adding labels and title and displaying
plt.xlabel('Aircraft Category')
plt.ylabel('Total Injuries')
plt.title('Total Injuries by Aircraft Category')
plt.xticks([i + bar_width for i in index], injuries_by_category['aircraft_category'], rotation=45)
plt.legend()
plt.tight_layout();
```
### what kind of injuries are there?

we check what ere the most common injuries gotten
we'll count the injuries gotten by the injury type
``` 
# Count the number of injuries by severity
severity_counts = data1['injury_severity'].value_counts().reset_index()
severity_counts.columns = ['injury_severity', 'count']

# Display the counts
severity_counts
```
then we'll plot
``` 
# Create a new figure with a specified size (width, height)
plt.figure(figsize=(8, 6))

# Create a bar plot to visualize the distribution of injury severity
plt.bar(severity_counts['injury_severity'], severity_counts['count'], color='lightblue')

# Set the title of the plot to describe what is being visualized
plt.title('Distribution of Injury Severity')

# Label the x-axis to indicate what the categories represent
plt.xlabel('Injury Severity')

# Label the y-axis to indicate what the counts represent
plt.ylabel('Count')

# Rotate the x-axis tick labels by 45 degrees for better readability
plt.xticks(rotation=45)

# Adjust the layout to prevent overlap and ensure everything fits well in the figure and display it
plt.tight_layout();
```
### what kind of damage did the aircraft get
```
# Count the number of occurrences of each damage type by aircraft category
damage_by_category = data1.groupby(['aircraft_category', 'aircraft_damage']).size().unstack(fill_value=0)
# Create a stacked bar plot to visualize the relationship between aircraft damage and aircraft category
damage_by_category.plot(kind='bar', stacked=True, figsize=(12, 6), color=['lightcoral', 'lightblue', 'lightgreen'])

# Set the title of the plot to describe what is being visualized
plt.title('Aircraft Damage by Aircraft Category')

# Label the x-axis to indicate what the categories represent
plt.xlabel('Aircraft Category')

# Label the y-axis to indicate what the values represent
plt.ylabel('Number of Incidents')

# Rotate the x-axis tick labels by 45 degrees for better readability
plt.xticks(rotation=45)

# Add a legend to the plot to indicate what each color represents in terms of aircraft damage
plt.legend(title='Aircraft Damage')

# Adjust the layout to prevent overlap and ensure everything fits well in the figure and display it
plt.tight_layout();
```
### which makes and models have the highest accident rates
```
# Count the number of accidents by make and model
accidents_by_make_model = data1.groupby(['make', 'model']).size().reset_index(name='accident_count')

# Sort the results by accident count
accidents_by_make_model = accidents_by_make_model.sort_values(by='accident_count', ascending=False)

# Display the top results
accidents_by_make_model.head(10)  # Display top 10 makes/models
```
plot

```
# Select the top 10 makes and models based on accident count
top_makes_models = accidents_by_make_model.head(10)

# Create a new figure with a specified size (width, height)
plt.figure(figsize=(12, 6))

# Create a horizontal bar plot to visualize the top makes and models by accident count
# Concatenate 'make' and 'model' columns to create a single label for each bar
plt.barh(top_makes_models['make'] + ' ' + top_makes_models['model'], top_makes_models['accident_count'], color='skyblue')

# Set the title of the plot to describe what is being visualized
plt.title('Top 10 Makes and Models by Accident Count')

# Label the x-axis to indicate what the values represent
plt.xlabel('Number of Accidents')

# Label the y-axis to indicate what the categories represent
plt.ylabel('Make and Model')

# Adjust the layout to prevent overlap and ensure everything fits well in the figure and display it
plt.tight_layout();
```






