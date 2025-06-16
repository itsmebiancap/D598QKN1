```python
import pandas as pd
#help to import the file and give the data a structure
```


```python
df = pd.read_csv("D598 Data Set.csv")
#upload and read the CSV file
```


```python
df = df.drop_duplicates()
#remove the duplicates
```


```python
groupbystate_df = df.groupby("Business State")
#group data by state
```


```python
groupbystate_df = groupbystate_df.apply(lambda x: x.reset_index(drop=True))
#save the newly grouped data by state as a new dataframe
```


```python
#df_new_filtered = df[df["Debt to Equity"] > 0]
#filter out negative debt to income ration values from the Debt to Equity column

# Filter out rows where Debt to Equity is zero
df_new_filtered = groupbystate_df[df["Debt to Equity"] != 0].copy()  
```


```python

# Compute the mean Debt to Equity ratio per Business ID and assign to a new column
df_new_filtered["DTI_Per_Business"] = df_new_filtered.groupby("Business ID")["Debt to Equity"].transform("mean")

# Preview the first few rows to check the results
print(df_new_filtered.head())
```

       Business ID Business State  Total Long-term Debt  Total Equity  \
    0    422282013        Alabama               5314000      24058479   
    1   2775952013        Alabama            1343464000    2858019000   
    2   9445082013        Arizona              65088000      59153000   
    3   7982872013       Arkansas              70366000     115946000   
    4   8839452013       Arkansas             108843000     100538000   
    
       Debt to Equity  Total Liabilities  Total Revenue  Profit Margin  \
    0        0.220878           23698000      137344716       0.510371   
    1        0.470068         3764193000     1256317000       0.297094   
    2        1.100333          110938000      215580000       0.206902   
    3        0.606886          213356000      402813000       0.123482   
    4        1.082606          214408000      555005000       0.073156   
    
       DTI_Per_Business  
    0          0.220878  
    1          0.470068  
    2          1.100333  
    3          0.606886  
    4          1.082606  
    


```python
print(type(df_new_filtered))
```

    <class 'pandas.core.frame.DataFrame'>
    


```python
df_new = df_new_filtered.copy()
#save as a new dataframe
```


```python
final_df = pd.concat([df, df_new])
#concatenate with the original dataset
```


```python
final_df.to_csv("new_completely_processesd_data.csv", index=False)
#save the new dataset as a new CSV file
```


```python
print("Data processing is complete!")
#print to confirm that the process is complete
```

    Data processing is complete!
    


```python
# Create a new project directory in a different location
import os
project_dir = os.path.join("C:\\Users\\greet\\Documents", "my_github_project")
if not os.path.exists(project_dir):
    os.makedirs(project_dir)

# Change to the new project directory
os.chdir(project_dir)
print(f"Current working directory: {os.getcwd()}")

# Initialize a fresh Git repository
!git init
```

    Current working directory: C:\Users\greet\Documents\my_github_project
    Reinitialized existing Git repository in C:/Users/greet/Documents/my_github_project/.git/
    


```python
# Copy your notebook and other project files to the new directory
import shutil

# manually copy your files to this new directory
print(f"Please copy your project files to: {project_dir}")
```

    Please copy your project files to: C:\Users\greet\Documents\my_github_project
    


```python
import matplotlib.pyplot as plt
import seaborn as sns

# Recalculate DTI_Per_Business within df_new_filtered so the column exists
df_new_filtered["DTI_Per_Business"] = df_new_filtered.groupby("Business ID")["Debt to Equity"].transform("mean")

# Create a histogram to show the distribution of DTI per business
plt.figure(figsize=(10, 5))
sns.histplot(df_new_filtered["DTI_Per_Business"], bins=30, kde=True)
plt.title("Distribution of Debt-to-Income Ratio Per Business")
plt.xlabel("Debt-to-Income Ratio")
plt.ylabel("Frequency")
plt.show()

```


    
![png](output_14_0.png)
    



```python
# Calculate average DTI per state
state_dti_avg = df.groupby("Business State")["Debt to Equity"].mean()

plt.figure(figsize=(12, 6)) #display
plt.plot(state_dti_diff.index, state_dti_diff, marker="o", linestyle="-", color="pink") #mgraph it
plt.title("Debt-to-Income Ratio Differences by State") #title
plt.xlabel("State") #x-axis
plt.ylabel("DTI Difference") #y-axis
plt.xticks(rotation=90) #ticks on graph
plt.show() #show graph

```


    
![png](output_15_0.png)
    



```python
#display the column names 
print(df_new_filtered.dtypes)
```

    Business ID               int64
    Business State           object
    Total Long-term Debt      int64
    Total Equity              int64
    Debt to Equity          float64
    Total Liabilities         int64
    Total Revenue             int64
    Profit Margin           float64
    DTI_Per_Business        float64
    dtype: object
    


```python
# Compute average DTI per state
state_dti_avg = df.groupby("Business State")["Debt to Equity"].mean()

# Sort to get top 5 states with highest DTI
top_5_states = state_dti_avg.sort_values(ascending=False).head(5)

# Now plot
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 5))
top_5_states.plot(kind="bar", color="red")
plt.title("Top 5 States with Highest DTI Differences")
plt.xlabel("State")
plt.ylabel("Average Debt-to-Income Ratio")
plt.xticks(rotation=45)
plt.show()

```


    
![png](output_17_0.png)
    



```python
# Calculate average DTI per state
state_dti_avg = df.groupby("Business State")["Debt to Equity"].mean()

# Filter out states with 0 or an empty difference
state_dti_diff_filtered = state_dti_diff[(state_dti_diff != 0) & (~state_dti_diff.isna())]

# Get the 5 states with the smallest (non-zero) differences
lowest_5_states = state_dti_diff_filtered.sort_values().head(5)

if not lowest_5_states.empty: 
    plt.figure(figsize=(12, 6))
    lowest_5_states.plot(kind="barh", color="royalblue")
    plt.title("States with Smallest Debt-to-Income Ratio Differences (Excluding Zeros & Empty)")
    plt.xlabel("DTI Difference")
    plt.ylabel("State")
    plt.show()
else:
    print("No valid data to plot after filtering out zeros and empty values.")

```


    
![png](output_18_0.png)
    



