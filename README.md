# Cyclistic Case Study

This project analyzes the usage patterns of Cyclistic's bike-sharing data to derive insights and recommendations for converting casual riders into annual members.

## Phases
### 1. Ask Phase
- Define business objectives and key questions.

### 2. Prepare Phase
- Data collection and cleaning.

### 3. Process Phase
- Data wrangling and transformation.

### 4. Analyze Phase
- Exploratory data analysis and visualization.

### 5. Share Phase
- Communicate findings and insights.

### 6. Act Phase
- Develop actionable recommendations.

## Recommendations
- **Weekend Promotions**: Offer weekend promotions or discounts on annual memberships.
- **Long Ride Incentives**: Introduce incentives for longer rides exclusive to annual members.
- **Targeted Marketing Campaigns**: Use digital media to target casual riders.
- **Referral Programs**: Implement referral programs for current and casual riders.
- **Enhanced User Experience**: Improve bike availability and quality.
- **Feedback and Engagement**: Gather feedback from casual riders.

## Visualization Code
  # Number of rides by user type
rides_by_user_type = all_trips.groupby('member_casual').size().reset_index(name='number_of_rides')

# Plot number of rides by user type
plt.figure(figsize=(10, 5))
sns.barplot(x='member_casual', y='number_of_rides', data=rides_by_user_type, palette='viridis')
plt.title('Number of Rides by User Type')
plt.xlabel('User Type')
plt.ylabel('Number of Rides')
plt.xticks(rotation=0)
plt.tight_layout()
plt.savefig('number_of_rides_by_user_type.png')
plt.show()

# Average ride length by user type and day of the week
avg_ride_length_by_day = all_trips.groupby(['member_casual', 'day_of_week'])['ride_length_m'].mean().reset_index()

# Plot average ride length by user type and day of the week
plt.figure(figsize=(12, 6))
sns.barplot(x='day_of_week', y='ride_length_m', hue='member_casual', data=avg_ride_length_by_day, palette='viridis')
plt.title('Average Ride Length by User Type and Day of the Week')
plt.xlabel('Day of the Week')
plt.ylabel('Average Ride Length (minutes)')
plt.legend(title='User Type')
plt.xticks(rotation=0)
plt.tight_layout()
plt.savefig('avg_ride_length_by_day.png')
plt.show()

# Assign season to months
def assign_season(month):
    if month in [12, 1, 2]:
        return 'Winter'
    elif month in [3, 4, 5]:
        return 'Spring'
    elif month in [6, 7, 8]:
        return 'Summer'
    else:
        return 'Fall'

all_trips['season'] = all_trips['month'].apply(assign_season)

# Number of rides by season, day of the week, and user type
rides_by_season = all_trips.groupby(['season', 'day_of_week', 'member_casual']).size().reset_index(name='number_of_rides')

# Plot number of rides by season, day of the week, and user type
g = sns.catplot(x='day_of_week', y='number_of_rides', hue='member_casual', col='season', data=rides_by_season, kind='bar', height=5, aspect=1, palette='viridis')
g.set_titles('{col_name} Season')
g.set_axis_labels('Day of the Week', 'Number of Rides')
g.fig.suptitle('Number of Rides by Season, Day of the Week, and User Type', y=1.02)
g.despine(left=True)
plt.tight_layout()
g.savefig('number_of_rides_by_season.png')
plt.show()


# Your Python code here
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime

# Set display options
pd.set_option('display.max_columns', None)
sns.set(style="whitegrid")

# Define function to load data
def load_data(file_path):
    return pd.read_csv(file_path)

# Load data from CSV files
files = [f'2021_{str(i).zfill(2)}.csv' for i in range(1, 13)]
all_trips = pd.concat([load_data(f'case study/raw data/{file}') for file in files])

# Display the first few rows of the combined dataframe
all_trips.head()
