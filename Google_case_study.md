```python
                                                #BELLABEAT DATA ANALYSIS CASE STUDY
```


```python
1.#DATA DESCRIPTION
```


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
plt.style.use('ggplot')
import seaborn as sns
import datetime as dt
```


```python
df=pd.read_csv("C:\Boo\DS projects\Google Capstone Case Study\BellaBeat\Data Set 2\dailyActivity_merged.csv")
```


```python
df.shape                                           #FOR VERIFYING THE TOTAL DATA ON SHEET
```


```python
df.columns                                         #FOR VERIFYING THE COLUMNS PRESENT  
```


```python
df.head()                                          #PROVIDES THE DETAILS OF FIRST 5 ROWS 
```


```python
df.dtypes                                          #FOR VERIFYING THE DATATYPES OF THE EACH COLUMNS
```


```python
2.#DATA CLEANING
```


```python
#Data Format found to be incorrect as Id and Date was wrong datatypes changed from (Int,object) to (Str,Datetime)
```


```python
df['Id']=df['Id'].astype(str)                      #FOR CHANGING THE DATATYPE
```


```python
df['ActivityDate'] = pd.to_datetime(df['ActivityDate'], format='%m/%d/%Y')
df.dtypes                                          #FOR CHANGING THE DATE FORMAT

```


```python
df.columns=df.columns.str.lower()                  #FOR CHANGING THE COLUMN NAME TO LOWER CASE 
df
```


```python
day_of_week=df['activitydate'].dt.day_name()       #FOR ADDING THE DATE COLUMN TO SPECIFTY THE NAME OF THE DAY 
df['day_of_week']=day_of_week
df
```


```python
df['week_number']=df['activitydate'].dt.weekday    #FOR ADDING THE DATE COLUMN TO SPECIFY THE WEEK NUMBER 
df
```


```python
pd.isnull(df).sum()                                #FOR CHECKING NULL VALUES 
```


```python
df.isna().sum()                                    #FOR RECHECKING NULL VALUES 
```


```python
df.duplicated().sum()                              #FOR CHECKING DUPLICATES 
```


```python
df.columns
```


```python
df2=df[['id', 'activitydate', 'totalsteps', 'totaldistance', 'veryactiveminutes', 'fairlyactiveminutes', 
        'lightlyactiveminutes', 'sedentaryminutes', 'calories']].copy()
df2                                                #FOR CREATING SUBSET FOR REQUIRED COLUMNS
```


```python
total_count = df['id'].count()                     #FOR FINDING THE TOTAL COUNTS OF ROW
total_count
```


```python
total_unique_count = df['id'].nunique()            #FOR FINDING THE TOTAL UNIQUE COUNTS
total_unique_count
```


```python
df_unique=df['id'].value_counts()[df['id'].unique()] 
df_unique                                          #FOR FINDING THE TOTAL UNIQUE COLUMN AND THEIR COUNTS
```


```python
id_grp=df2.groupby(['id'])
id_avg_steps=id_grp['totalsteps'].mean().sort_values(ascending=False)
id_avg_steps = id_avg_steps.to_frame()             #FOR SORTING THE ACTIVITY
condition=[                                                 
    (id_avg_steps<= 6000),
    (id_avg_steps>6000)& (id_avg_steps<12000),
    (id_avg_steps>=12000)
]
values=['Sedentary','Active','Pro-Active']
id_avg_steps['activitylevel']=np.select(condition,values)
id_activity_level=id_avg_steps['activitylevel']
df2['activitylevel']=[id_activity_level[c] for c in df2['id']]
```


```python
3.#DATA ANALYSIS
```


```python
#Correlation Calories & Steps
ax=sns.scatterplot(x='totalsteps',y='calories',data=df2,hue='activitylevel')
plt.title('Correlation Calories & Steps')
plt.tight_layout()
plt.show()
```


    
![png](output_25_0.png)
    



```python
#Avg Number of steps per day
day_of_week=['Monday','Tuesday','Wednesday','Thursday','Friday', 'Saturday','Sunday']
fig , ax= plt.subplots(1,1,figsize=(8,5))
day_grp=df.groupby(['day_of_week'])
avg_daily_step=day_grp['totalsteps'].mean()
avg_step=df['totalsteps'].mean()
plt.bar(avg_daily_step.index,avg_daily_step)
ax.set_xticks(range(len(day_of_week)))
ax.set_xticklabels(day_of_week)
ax.axhline(y=avg_daily_step.mean(),color='blue',label='Avg daily steps')
ax.set_ylabel('Steps')
ax.set_xlabel('Day of week')
ax.set_title('Avg Number of steps per day')
plt.legend()
plt.show()
```


    
![png](output_26_0.png)
    



```python
#Percentage of activity level in minutes
very_active_mins=df['veryactiveminutes'].sum()
fairly_active_mins=df['fairlyactiveminutes'].sum()
lightly_active_mins=df['lightlyactiveminutes'].sum()
sedentary_mins=df['sedentaryminutes'].sum()
slices=[very_active_mins,fairly_active_mins,lightly_active_mins,sedentary_mins]
labels=['very_active_mins','fairly_active_mins','lightly_active_mins','sedentary_mins']
explode=[0,0,0,0.1]
plt.pie(slices,labels=labels,explode=explode,autopct='%1.1f%%')
plt.title('% of activity level in minutes')
plt.show()
```


    
![png](output_27_0.png)
    



```python
#Correlation Between Activity Level,Minutes and Calories
day_of_week=[0,1,2,3,4,5,6]
fig, axes =plt.subplots(nrows=2,ncols=2,figsize=(8,9),dpi=70)
sns.scatterplot(data=df2,x='calories',y='sedentaryminutes',hue='activitylevel',ax=axes[0,0],legend=False)
sns.scatterplot(data=df2,x='calories',y='lightlyactiveminutes',hue='activitylevel',ax=axes[0,1],legend=False)
sns.scatterplot(data=df2,x='calories',y='fairlyactiveminutes',hue='activitylevel',ax=axes[1,0],legend=False)
sns.scatterplot(data=df2,x='calories',y='veryactiveminutes',hue='activitylevel',ax=axes[1,1],legend=True)
plt.legend(title='Activity level',bbox_to_anchor=(1.5,2.2))
fig.suptitle('Correlation Between Activity Level,Minutes and Calories',x=0.5,y=0.92,fontsize=24)
plt.show()
```


    
![png](output_28_0.png)
    



```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
