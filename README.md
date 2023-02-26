# Binary-analysis

## Bivariate Analysis

    
Through bivariate analysis, we try to analyze two variables simultaneously. As opposed to univariate analysis where we check the characteristics of a single variable, in the bivariate analysis we try to determine if there is any relationship between two variables.

&nbsp;

There are essentially 3 major scenarios that we will come across when we perform bivariate analysis.
1. Both variables of interest are qualitative
2. One variable is qualitative and the other is quantitative
3. Both variables are quantitative

&nbsp;

For the purpose of this exercise, we will explore a few most popular techniques to perform bivariate analysis.

The following plots are not limited to the headings they are under. They are the options available to us in the event of a specific situation.
### Numerical  vs. Numerical
    1. Scatterplot
    2. Line plot
    3. Heatmap for correlation
    4. Joint plot
### Categorical vs. Numerical
    1. Bar chart
    2. Violin plot
    3. Categorical box plot
    4. Swarm plot
    
### Two Categorical Variables 
    1. Bar chart
    2. Grouped bar chart
    3. Point plot
### Case Study (Suicide Rates Overview 1985 to 2016)
Our aim is to make general analysis of suicide rates.

Lets start using the above techniques on a practical dataset. The data is about suicide rates from 1985 to 2016.

Source: https://www.kaggle.com/russellyates88/suicide-rates-overview-1985-to-2016


Dataset Feature List:

country

year

sex

age

suicides_no

population

suicides/100k pop

country-year

HDI for year

gdp_for_year

gdp_per_capita

generation
### 1.1 Loading the libraries
import warnings
warnings.filterwarnings('ignore')
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('darkgrid')
### 1.2 Loading the dataset 
data=pd.read_csv('master.csv')
### 1.3 Checking the first 5 rows in the dataset
data.head()
### 1.4 Checking the descriptive statistics of the data
data.describe()
### 1.5 Checking the label of each column in the Dataframe
data.columns
### 1.6 Checking the shape of Dataframe
data.shape
### 1.7 Counting the data types present in the data
data.dtypes.value_counts()
### 1.8 Checking the short summary of the DataFrame
data.info()
### 1.9  Checking for missing values in the dataset
def missing_check(df):
    total = df.isnull().sum().sort_values(ascending=False)   # total number of null values
    percent = (df.isnull().sum()/df.isnull().count()).sort_values(ascending=False)  # percentage of values that are null
    missing_data = pd.concat([total, percent], axis=1, keys=['Total', 'Percent'])  # putting the above two together
    return missing_data # return the dataframe
missing_check(data)
data[['suicides_no','population','suicides/100k pop','gdp_per_capita ($)']].describe() #descriptive stats of continuous columns
### 1.10 Frequency table for Age 
One-Way Tables
my_tab = pd.crosstab(index=data["age"],  # Make a crosstab
                     columns="count")                  # Name the count column
my_tab
### 1.11 Bar plot to check the Number of Suicides in top Countries
### This is an example of Numerical vs Categorical.
data.groupby(by=['country'])['suicides_no'].sum().reset_index().sort_values(['suicides_no']).tail(10).plot(x='country',
                                                                                                           y='suicides_no',
                                                                                                           kind='bar',
                                                                                                           figsize=(15,5))
plt.show()
* Russia has the highest number of suicides followed by the US and Japan.
* Russia, US and Japan have exceptionally high counts of suicides compared to the others in the lot.
### 1.12 Bar plot to check the bottom 10 countries with the lowest number of suicides
data.groupby(by=['country'])['suicides_no'].sum().reset_index().sort_values(['suicides_no'],
                    ascending=True).head(10).plot(x='country',y='suicides_no',kind='bar', figsize=(15,5))

plt.show()
### 1.13 Bar plot for Number of Suicides Vs Age
plt.figure(figsize=(10,5))  # setting the figure size
ax = sns.barplot(x='age', y='suicides_no', data=data, palette='muted')  # barplot
### 1.14 Bar plot for Number of Suicides Vs Sex
plt.figure(figsize=(8,4))
ax = sns.barplot(x="sex", y="suicides_no", data=data)
### 1.15 Bar plot for Number of Suicides Vs generation
plt.figure(figsize=(15,5))

ax = sns.barplot(x='generation', y='suicides_no', data=data)
* Suicides are the highest among the Boomers and lowest among Generation Z
### 1.15 Scatter plot for Number of Suicides Vs population
### This is an example of  Numerical  vs. Numerical
figure = plt.figure(figsize=(15,5))

ax = sns.scatterplot(x=data['population'],y='suicides_no', data=data, size = "suicides_no") # scatter plot
figure = plt.figure(figsize=(50,15))

ax = sns.regplot(x='population',y='suicides_no', data=data ) # regression plot - scatter plot with a regression line
#Here we plotting a line plot.
sns.lineplot(x='population',y='suicides_no', data=data.head() )
### Scatter plot for Number of Suicides/100k Population Vs GDP Per Capita
figure = plt.figure(figsize=(15,7))

sns.scatterplot(x='gdp_per_capita ($)', y='suicides/100k pop', data=data) # scatter plot 
plt.show()
* Looks like higher suicide rates are a bit more prevalent in countries with lower GDP Per Capita.
* However, it doesn't look like there is any significant correlation between the two.
### Checking the correlation among pairs of continuous variables
plt.figure(figsize=(10,5))
sns.heatmap(data.corr(), annot=True, linewidths=.5, fmt= '.1f', center = 1 )  # heatmap
plt.show()
* Instead of doing a simple df.corr(), we can use the heatmap when there are a large number of variables.
* The color helps in picking out the most correlated easily.
* The darker the color higher the correlation.
* None of the attributes seem to have a correlation of real significance.
* Some obvious correlations are that in a larger population, it is very likely that the number of suicides will be more in number.
* Human Development Index - gdp per capita is the only pair with the high correlation. 
### 1.17.1 Bar plot to check Number of suicides by sex and age (three variables used to generate a single plot)
### This is an example of 1 Numerical and 2 Categorical variables.
plt.figure(figsize=(15,5))
sns.barplot(data=data,x='sex',y='suicides_no',hue='age')
plt.show()
### 1.17.2 Bar plot to check Number of suicides by sex and Generation(three variables used to generate a single plot)
plt.figure(figsize=(15,5))
sns.barplot(data=data,x='sex',y='suicides_no',hue='generation')
plt.show()
* Suicides in males apart from being higher has a slight variation in distribution across generations compared to the suicides of females across generation.

* Suicides are high among Boomers, both male and female.
### 1.18 Checking the No.of suicides: Country Vs Sex
suic_sum_m = data['suicides_no'].groupby([data['country'],data['sex']]).sum()  # number of suicides by country and sex
suic_sum_m = suic_sum_m.reset_index().sort_values(by='suicides_no',ascending=False) # sort in descending order
most_cont_m = suic_sum_m.head(10)  # getting the top ten countries in terms of suicides

fig = plt.figure(figsize=(15,5))
plt.title('Count of suicides for 31 years.')

sns.barplot(y='country',x='suicides_no',hue='sex',data=most_cont_m,palette='Set2');

plt.ylabel('Count of suicides')
plt.tight_layout()
*In comparison to other countries with high suicide rates, Japan has a larger proportion of female suicides.
### Average number of suicides across each generation for a given gender along with the confidence intervals - Point Plot
plt.figure(figsize=(15,5))

sns.pointplot(x="generation", y="suicides_no", hue = 'sex',  data=data)
plt.show()
* The graph tells us the average suicides along with the confidence intervals. 
* Suicides among females, in general, don't seem to be fluctuating a lot.
* Average suicides of Gen-Z are almost equally distributed across genders.
### Distribution of population across each generation - Violin plot
plt.figure(figsize=(10,5))
sns.violinplot(x=data.generation, y=data['population'])
plt.show()
* The plot is similar to a box plot but here, we get a density function.
* Distribution of population across every generation is highly skewed.
* Potential a lot of outliers.
* Try it yourself -  Go ahead and check if really there are a lot of outliers in populations across each generation
  (Hint: use a boxplot).
### Checking trends with the Temporal Data 
Temporal data is simply data that represents a state in time, such as the land-use patterns of Hong Kong in 1990,
or total rainfall in Honolulu on July 1, 2009. Temporal data is collected to analyze weather patterns and other environmental variables, monitor traffic conditions, study demographic trends, and so on. This data comes from many sources ranging
from manual data entry to data collected using observational sensors or generated from simulation models. 
### Checking pattern using Trend plot (1985-2015) suides Rate Vs Years
data[['year','suicides_no']].groupby(['year']).sum().plot(figsize=(15,5))

plt.show()
### Checking the pattern using Trend plot (1985-2015) Population Vs Years
data[['year','population']].groupby(['year']).sum().plot(figsize=(15,5))

plt.show()
### Checking the pattern using Trend plot (1985-2015) suicides/100k pop Vs Years
data[['year','suicides/100k pop']].groupby(['year']).sum().plot(figsize=(15,5))

plt.show()

