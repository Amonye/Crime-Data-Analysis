# Crime-Data-Analysis
Crime Data Analysis
#Crime Rate and changes over time 
import pandas as pd
chicago_crime_url = r"C:\Users\hp\Downloads\chicago3.csv"
crime_file = pd.read_csv(chicago_crime_url )
crime_file.head()
crime_file.columns
crime_file.isna().sum()
crime_file.shape
# Read the CSV file into a DataFrame
crime_file = pd.read_csv(chicago_crime_url)

# Function to drop redundant columns
def drop_redundant_columns(data):
    redundant_columns = ['ID', 'Unnamed: 0']  # Add other redundant column names as needed
    data.drop(columns=redundant_columns, inplace=True)

# Call the function to drop redundant columns
drop_redundant_columns(crime_file)

# Display the DataFrame after dropping redundant columns
df = pd.DataFrame(crime_file)
df
crime_file.columns
# Define the file path
chicago_crime_url = r"C:\Users\hp\Downloads\chicago3.csv"

# Read the CSV file into a DataFrame
crime_file = pd.read_csv(chicago_crime_url)

# Function to create new columns for Months, Day, and Season
def create_time_columns(data):
    # Convert 'Date' column to datetime format
    data['Date'] = pd.to_datetime(data['Date'])
    
    # Extract Month, Day, and Season from the 'Date' column
    data['Month'] = data['Date'].dt.month
    data['Day'] = data['Date'].dt.day
    data['Season'] = data['Date'].dt.month.map({1:'Winter', 2:'Winter', 3:'Spring', 
                                                4:'Spring', 5:'Spring', 6:'Summer', 
                                                7:'Summer', 8:'Summer', 9:'Fall', 
                                                10:'Fall', 11:'Fall', 12:'Winter'})

# Call the function to create new columns
create_time_columns(crime_file)

# Display the DataFrame with new columns
crime_file.head()
df.drop(['Case Number'], axis = 1, inplace = True)
# Define the file path
chicago_crime_url = r"C:\Users\hp\Downloads\chicago3.csv"

# Read the CSV file into a DataFrame
crime_file = pd.read_csv(chicago_crime_url)

# Convert 'Date' column to datetime format
crime_file['Date'] = pd.to_datetime(crime_file['Date'])

# Extract Month, Day, and Season from the 'Date' column
crime_file['Month'] = crime_file['Date'].dt.month
crime_file['Day'] = crime_file['Date'].dt.day
crime_file['Season'] = crime_file['Date'].dt.month.map({1:'Winter', 2:'Winter', 3:'Spring', 
                                                        4:'Spring', 5:'Spring', 6:'Summer', 
                                                        7:'Summer', 8:'Summer', 9:'Fall', 
                                                        10:'Fall', 11:'Fall', 12:'Winter'})

# Group by Month and count the number of crimes
monthly_crime_count = crime_file.groupby('Month').size()

# Group by Day and count the number of crimes
daily_crime_count = crime_file.groupby('Day').size()

# Group by Season and count the number of crimes
seasonal_crime_count = crime_file.groupby('Season').size()

# Identify the most frequent crime
most_frequent_crime = crime_file['Primary Type'].mode()[0]

# Identify the least frequent crime
least_frequent_crime = crime_file['Primary Type'].value_counts().idxmin()

# Display the results
print("Frequency of Crime within Months:")
print(monthly_crime_count)
print("\nFrequency of Crime within Days:")
print(daily_crime_count)
print("\nFrequency of Crime within Seasons:")
print(seasonal_crime_count)
print("\nMost Frequent Crime:", most_frequent_crime)
print("Least Frequent Crime:", least_frequent_crime)
# Group by Location Description and count the number of crimes
location_crime_count = crime_file.groupby('Location Description').size()

# Identify the location with the most crime occurrences
most_common_location = location_crime_count.idxmax()

print("Location where crime happens the most:", most_common_location)
import matplotlib.pyplot as plt
crime_count_by_year_type = crime_file.groupby(['Year', 'Primary Type']).size().unstack()

# Plot the data
crime_count_by_year_type.plot(kind='line', marker='o', figsize=(12, 6))
plt.title('Crime Count by Year and Type')
plt.xlabel('Year')
plt.ylabel('Crime Count')
plt.legend(title='Crime Type', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(True)
plt.tight_layout()
plt.show()
# Group the data by year and primary crime type, and count the number of crimes
crime_count_by_year_type = crime_file.groupby(['Year', 'Primary Type']).size().unstack()

# Calculate the total number of crimes for each year
total_crimes_by_year = crime_count_by_year_type.sum(axis=1)

# Calculate the proportion of each crime type for each year
crime_proportion_by_year_type = crime_count_by_year_type.div(total_crimes_by_year, axis=0)

# Calculate the correlation matrix to compare the distribution of crime types across years
crime_correlation = crime_proportion_by_year_type.corr()

# Check if there are significant changes in crime types over the years
# If the correlation between crime distributions for different years is low, it suggests that the types of crime have changed over time
# You can set a threshold for correlation value to determine whether to consider the changes significant
threshold = 0.5  # Example threshold value, it can also be adjusted based on data and requirements

significant_changes = (crime_correlation < threshold).any().any()

if significant_changes:
    print("Yes, the types of crime changed as years go by.")
else:
    print("No, the types of crime did not change significantly over the years.")
