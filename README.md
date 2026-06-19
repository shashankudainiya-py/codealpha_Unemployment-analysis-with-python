# codealpha_Unemployment-analysis-with-python
# TASK 2: Unemployment Analysis with Python

import pandas as pd
import matplotlib.pyplot as plt

# Load Dataset
df = pd.read_csv("Unemployment in India.csv")

# Display first 5 rows
print("Dataset Preview:")
print(df.head())

# Data Cleaning
df.columns = df.columns.str.strip()  # Remove extra spaces in column names
df.dropna(inplace=True)

# Convert Date column to datetime format
df['Date'] = pd.to_datetime(df['Date'], dayfirst=True)

# Basic Information
print("\nDataset Information:")
print(df.info())

print("\nStatistical Summary:")
print(df.describe())

# Average Unemployment Rate
avg_rate = df['Estimated Unemployment Rate (%)'].mean()
print(f"\nAverage Unemployment Rate: {avg_rate:.2f}%")

# Highest and Lowest Unemployment Rate
highest = df.loc[df['Estimated Unemployment Rate (%)'].idxmax()]
lowest = df.loc[df['Estimated Unemployment Rate (%)'].idxmin()]

print("\nHighest Unemployment Rate:")
print(highest)

print("\nLowest Unemployment Rate:")
print(lowest)

# Monthly Unemployment Trend
monthly_trend = df.groupby(df['Date'].dt.to_period('M'))[
    'Estimated Unemployment Rate (%)'
].mean()

# Plot Monthly Trend
plt.figure(figsize=(12,6))
monthly_trend.plot()
plt.title("Monthly Average Unemployment Rate in India")
plt.xlabel("Month")
plt.ylabel("Unemployment Rate (%)")
plt.grid(True)
plt.show()

# Regional Analysis
region_avg = df.groupby('Region')[
    'Estimated Unemployment Rate (%)'
].mean().sort_values(ascending=False)

print("\nAverage Unemployment Rate by Region:")
print(region_avg)

# Bar Chart for Regional Unemployment
plt.figure(figsize=(12,6))
region_avg.plot(kind='bar')
plt.title("Average Unemployment Rate by Region")
plt.xlabel("Region")
plt.ylabel("Unemployment Rate (%)")
plt.tight_layout()
plt.show()

# COVID-19 Impact Analysis (Year 2020)
covid_data = df[df['Date'].dt.year == 2020]

covid_avg = covid_data['Estimated Unemployment Rate (%)'].mean()
overall_avg = df['Estimated Unemployment Rate (%)'].mean()

print(f"\nOverall Average Unemployment Rate: {overall_avg:.2f}%")
print(f"Average Unemployment Rate During 2020: {covid_avg:.2f}%")

# Conclusion
print("\nKey Insights:")
print("1. Unemployment rates vary across regions.")
print("2. Significant fluctuations are observed during the COVID-19 period.")
print("3. Some regions consistently show higher unemployment rates.")
print("4. Monthly trend analysis helps identify seasonal patterns.")
print("5. The analysis can support economic and social policy decisions.")
