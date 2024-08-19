# air_csv_data_analysis 

### IMPORTING THE PACKAGES
import numpy as np 
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import warnings
warnings.filterwarnings("ignore")

### LOADING THE DATASET
df_air = pd.read_csv("air.csv")
df_air.head()

### KNOWING ABOUT THE INFO OF THE DATASET
df.info()

### DESCRIBING THE DATASET
df.describe()

### FINDING THE COLUMNS LIST
df_air.columns.to_list()

###  RETRIEVING THE NULL VALUES
df_air.isnull().sum() # Found null values in the coulumn "Arrival Delay in Minutes"

### FILLING THE NULL VALUES
df_air["Arrival Delay in Minutes"] = df_air["Arrival Delay in Minutes"].fillna(df_air["Arrival Delay in Minutes"].mean()) # The column "Arrival Delay in Minutes is a numeric column, so filled wtih mean of that column. 

### CONFIRMING THAT THE DATASET DOESN'T CONSIST OS ANY NULL VALUE
df_air.isnull().sum()

### DROPPING THE COLUMN ID, AS THIS COLUMN IS NOT USEFUL FOR US FURTHER
df = df_air.drop(columns = ["id"], inplace = True, axis = 1)

### CHECKING THAT WHETHER THE COLUMN ID IS DROPPED OR NOT
df_air.head() # It confirmed that the column is dropped.

### PLOTTING THE BARGRAPH ON COLUMN INFLIGHT WIFI SERVICES
var = df_air["Inflight wifi service"].value_counts()

plt.figure(figsize=(9, 3)) # Setting the figure size
var.plot(kind='bar') # Specifing the kind of graph as bargraph
plt.show() # Trying to show the graph that has been constucted

# The aobve way of making barplot is one way and we have another way to plat the graph of all column at once with that we can save our time.

### BUILDING THE FUCTION TO PLOT TEH BARGRAPHS FOR THE COLUMNS IN THE DATASET
def bar_plot(variable):
    var = df_air[variable]
    var_Value = var.value_counts()
    plt.figure(figsize = (9, 3))
    plt.bar(var_Value.index, var_Value.values) # Specifing that the graph is bar graph.
    plt.xlabel("Passengers Score")
    plt.ylabel("Frequency")
    plt.title(variable)
    plt.show()

### TRYING TO PLOT THE GRAPHS OF THE COLUMNS IN THE DATASET
category1 = ['Inflight wifi service','Departure/Arrival time convenient','Ease of Online booking',
            'Gate location','Food and drink','Online boarding','Seat comfort','Inflight entertainment',
            'On-board service','Leg room service','Baggage handling','Checkin service','Inflight service','Cleanliness']
for c in category1:
    bar_plot(c) # Calling the fuciton bar_plot

### DRAWING THE COUNTPLOT TO GET BETTER UNDERSTANDING
sns.countplot(data = df_air, x = "Gate location")#, color = "blue") # If we want to change the color of the graph we can do it by adding color and specifying the color.

### COUTNING THE VALUES
category = ['Gender','Customer Type','Type of Travel','Class', 'satisfaction']
for c in category:
    print(df_air[c].value_counts())
    print("................")
    print()

### CONSTRUCTING THE FUCNTION TO PLOT THE HISTOGRAM
def plot_hist(variable):
    plt.figure(figsize = (9, 3))
    plt.hist(df_air[variable], bins = 50) # Specifying that the graph must be a histogram
    plt.xlabel(variable)
    plt.ylabel("Frequency")
    plt.title(vaAriable)
    plt.show()

### PLOTTING THE GRAPH USING THE ABOVE FUNCTION
numericVar = ["Age", "Flight Distance", "Departure Delay in Minutes", "Arrival Delay in Minutes"]
for n in numericVar:
    plot_hist(n) # Calling the function

### REPLACING THE COLUMN SATISFACTION WITH NUMERIC VALUES INSTEAD OF CHATOGORICAL VALUES
df_air["satisfaction"].replace({"satisfied": 1, "neutral or dissatisfied": 0}, inplace = True) # This turns the column into numeric by entering the satisfied to 1 and neutral and dissatisfied to 0.
df_air.head()

### USING THE GROUPING FUNCTION TO GROUP THE COULMNS GENDER AND SATISFACTION AND SORTING THEM IN ASCENDING WITH THEIR MEAN VALUES
df_air[["Gender", "satisfaction"]].groupby(["Gender"], as_index = False).mean().sort_values(by = "satisfaction", ascending = False)

### DOING THE TO THE COULMNS AGE AND SATISFACTION
df_sat = df_air[["Age", "satisfaction"]].groupby(["Age"], as_index = False).mean().sort_values(by = "satisfaction", ascending = False)
df_sat.head()

### AS WE STORED THE MEAN VALUES IN DF_SAT, TRYING TO ACCESS THE DATA WITHIN THE STORED VALUE
df_sat[50:]

### USING THE GROUPING FUNCTION TO GROUP THE COULMNS CLEANLINESS AND SATISFACTION AND SORTING THEM IN ASCENDING WITH THEIR MEAN VALUES
df_air[["Cleanliness", "satisfaction"]].groupby(["Cleanliness"], as_index = False).mean().sort_values(by = "satisfaction", ascending = False)

### USING THE GROUPING FUNCTION TO GROUP THE COULMNS INFLIGHT WIFI SERVICES AND SATISFACTION AND SORTING THEM IN ASCENDING WITH THEIR MEAN VALUES
df_air[["Inflight wifi service", "satisfaction"]].groupby(["Inflight wifi service"], as_index = False).mean().sort_values(by = "satisfaction", ascending = False)

### SELECTING THE COLUMNS FROM THE DATASET
numeric_features = df_air.select_dtypes(include = ["int64", "float64"])
num_list = numeric_features.columns.to_list()

### PLOTTING THE BOXPLOT TO GET INSIGHTS OF THE DATA
for cat in num_list:
    sns.boxplot(df_air[cat])
    plt.title(cat)
    plt.show()

### BARPLOTTING
def service_plot(variable):
    var = personal[variable]
    var_Value = var.value_counts()
    plt.figure(figsize = (9, 3))
    plt.bar(var_Value.index, var_Value.values)
    plt.xlabel("Passengers Score")
    plt.ylabel("Frequency")
    plt.title(variable)
    plt.show()

### PLOTTING THE GRAPHS OF SPECIFIED COLUMNS
service = ["On-board service", "Leg room service", "Checkin service", "Inflight service"]
for c in service:
    service_plot(c)

### TRYING TO BUILD THE CORRELAITON MATRIX, WITH THIS MATRIC WE CAN FIND RELATION BETWEEN COLUMNS WHETHER THEY ARE STRONGLY CONNECTED OR WEAKLY
corr = df_air[['Age', 'Flight Distance', 'Inflight wifi service',
       'Departure/Arrival time convenient', 'Ease of Online booking',
       'Gate location','Food and drink','Online boarding','Seat comfort',
               'Inflight entertainment','On-board service','Leg room service',
               'Baggage handling','Checkin service','Inflight service','Cleanliness',
               'Departure Delay in Minutes','Arrival Delay in Minutes','satisfaction']].corr()
plt.figure(figsize = (15, 8))
sns.heatmap(corr, annot = True, cmap = "coolwarm")
plt.show()
