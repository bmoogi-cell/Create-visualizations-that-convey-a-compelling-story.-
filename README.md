# 📌 Step 1: Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Enable inline plotting
%matplotlib inline

# 📌 Step 2: Load sample sales data (publicly accessible CSV)
url = "https://raw.githubusercontent.com/datasets/sales-data/master/data/sales.csv"
df = pd.read_csv(url)

# 📌 Step 3: Display the first few rows
print("Sample Data:")
print(df.head())

# 📌 Step 4: Basic info
print("\nDataset Info:")
print(df.info())

# 📌 Step 5: Data cleaning (drop missing or invalid rows)
df.dropna(inplace=True)

# Rename columns if needed
df.columns = df.columns.str.strip().str.title()

# 📌 Step 6: KPI Summary
total_sales = df['Revenue'].sum()
avg_sales = df['Revenue'].mean()
max_sale = df['Revenue'].max()

print("\n📊 KPI Summary:")
print(f"Total Sales: ${total_sales:,.0f}")
print(f"Average Sale Value: ${avg_sales:,.2f}")
print(f"Highest Single Sale: ${max_sale:,.0f}")

# 📌 Step 7: Visualization 1 – Sales by Region
plt.figure(figsize=(8,5))
sns.barplot(data=df, x='Region', y='Revenue', estimator=sum, ci=None, palette='coolwarm')
plt.title("Total Sales by Region", fontsize=14)
plt.xlabel("Region")
plt.ylabel("Total Sales ($)")
plt.show()

# 📌 Step 8: Visualization 2 – Product Sales Distribution
plt.figure(figsize=(8,5))
df.groupby('Product')['Revenue'].sum().plot(kind='pie', autopct='%1.1f%%', startangle=90)
plt.title("Product Sales Share")
plt.ylabel("")
plt.show()

# 📌 Step 9: Visualization 3 – Monthly Sales Trend
df['Orderdate'] = pd.to_datetime(df['Orderdate'])
monthly_sales = df.groupby(df['Orderdate'].dt.to_period('M'))['Revenue'].sum()

plt.figure(figsize=(10,5))
monthly_sales.plot(marker='o')
plt.title("Monthly Sales Trend", fontsize=14)
plt.xlabel("Month")
plt.ylabel("Revenue ($)")
plt.grid(True)
plt.show()

# 📌 Step 10: Insights / Storytelling
print("\n🧭 Story Summary:")
print(f"- The total revenue generated is ${total_sales:,.0f}.")
print("- The top-performing regions show stronger demand patterns.")
print("- Certain products dominate overall sales share.")
print("- Monthly trends show how business performance evolved over time.")
