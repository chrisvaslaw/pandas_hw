# pandas_hw
### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.

# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data_df = pd.read_csv(file_to_load)
purchase_data_df.head(10)

purchase_data_df.describe()

purchase_data_df.count()

# Display the total number of players

player_total = purchase_data_df["Purchase ID"].count()
player_total

# data type of variables

purchase_data_df.dtypes

# Run basic calculations to obtain number of unique items, average price, etc.

unique_values_SN = purchase_data_df["SN"].unique()

unique_values_item = purchase_data_df["Item Name"].unique()

unique_items = len(purchase_data_df["Item ID"].unique())

total_purchases = purchase_data_df["Purchase ID"].count()

total_revenue = purchase_data_df["Price"].sum()

average_price = purchase_data_df["Price"].mean()

summary_data_df = pd.DataFrame([{
    "Unique Items": unique_items,
     "Average Price": average_price,
     "Number of Purchases": total_purchases,
     "Revenue": total_revenue
      }])
summary_data_df

# Sex Counts
sex_counts = purchase_data_df["Gender"].value_counts()
sex_counts

# Gender Demographics
grouped_by_gender = purchase_data_df.groupby('Gender')
total_gender = grouped_by_gender.nunique()["SN"]
percentage = (total_gender / player_total) * 100
percentage.astype(float).map("%{:,.2f}".format)

gender_df = pd.DataFrame({"Total Count": sex_counts, "Percentage of Players": percentage.astype(float).map("{:,.2f}%".format)})
gender_df
gender_df_ascending = gender_df.sort_values("Total Count", ascending=False)
gender_df_ascending

# Purchasing Analysis (Gender)
# purchase count
purchase_count_gender = grouped_by_gender["Purchase ID"].count()
# avg. purchase price
average_price_gender = grouped_by_gender["Price"].mean()
# avg. purchase total
total_price_gender = grouped_by_gender["Price"].sum()
# avg. purchase total per person
average_person_gender = total_price_gender / total_gender

gender_stats_df = pd.DataFrame({"Purchase Count": purchase_count_gender, "Avg. Purchase Price": 
                                average_price_gender, 
                                "Avg. Purchase Total": total_price_gender, 
                                "Avg. Purchase Total Per Person": average_person_gender})
gender_stats_ascending = gender_stats_df.sort_values("Purchase Count", ascending=False)
gender_stats_ascending

# Age Demographics
# Establish bins for ages

print(purchase_data_df["Age"].max())
print(purchase_data_df["Age"].min())
bins = [0, 9, 19, 29, 39, 50]
group_names = ["Under 10", "10 to 19", "20 to 29", "30 to 39", "40 and up"]

# Categorize the existing players using the age bins. Hint: use pd.cut()
purchase_data_df["Age Summary"] = pd.cut(purchase_data_df["Age"], bins, labels=group_names, include_lowest=True)
purchase_data_df.head()

grouped_by_age = purchase_data_df.groupby("Age Summary")
grouped_by_age.max()

grouped_by_age = purchase_data_df.groupby('Age Summary')
total_age = grouped_by_age.nunique()["SN"]
percentage = (total_age / player_total) * 100
percentage.astype(float).map("%{:,.2f}".format)

age_counts = purchase_data_df["Age Summary"].value_counts()
age_counts

age_df = pd.DataFrame({"Total Count": age_counts, "Percentage of Players": percentage.astype(float).map("{:,.2f}%".format)})
age_df
age_df_ascending = age_df.sort_values("Total Count", ascending=False)
age_df_ascending

# Purchasing Analysis (Age)
# total age
total_age = grouped_by_age.nunique()["SN"]
# purchase count
purchase_count_age = grouped_by_age["Purchase ID"].count()
# avg. purchase price
average_price_age = grouped_by_age["Price"].mean()
# avg. purchase total
total_price_age = grouped_by_age["Price"].sum()
# avg. purchase total per person
average_person_age = total_price_age / total_age

age_stats_df = pd.DataFrame({"Purchase Count": purchase_count_age, "Avg. Purchase Price": 
                                average_price_age, 
                                "Avg. Purchase Total": total_price_age, 
                                "Avg. Purchase Total Per Person": average_person_age})
age_stats_df

# Run basic calculations to obtain the results in the table below
# Create a summary data frame to hold the results

grouped_by_SN = purchase_data_df.groupby("SN")
grouped_by_SN.max()

# Top Spenders

# purchase count
purchase_count_SN = grouped_by_SN["Purchase ID"].count()
# avg. purchase price
average_price_SN = grouped_by_SN["Price"].mean()
# avg. purchase total
total_price_SN = grouped_by_SN["Price"].sum()

SN_stats_df = pd.DataFrame({"Purchase Count": purchase_count_SN, "Avg. Purchase Price": 
                                average_price_SN, 
                                "Purchase Value": total_price_SN})

# Sort the total purchase value column in descending order

SN_stats_df_descending = SN_stats_df.sort_values("Purchase Value", ascending=False)
SN_stats_df_descending
SN_stats_df_descending.style.format({"Avg. Purchase Price":"${:,.2f}", 
                                 "Purchase Value":"${:,.2f}"})

# Most Popular Items

purchase_data_df.columns


reduced_purchase_data_df = purchase_data_df.loc[:, ["Item ID", "Item Name", "Price"]]
reduced_purchase_data_df

# Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value

grouped_by_ID = reduced_purchase_data_df.groupby(["Item ID","Item Name"])
grouped_by_ID.max()

# Retrieve the Item ID, Item Name, and Item Price columns
# purchase count
purchase_count_ID = grouped_by_ID["Price"].count()
# purchase price
purchase_value = grouped_by_ID["Price"].sum() 
# total purchase value
total_purchase_ID = purchase_count_ID * purchase_value

# Create a summary data frame to hold the results

ID_stats_df = pd.DataFrame({"Purchase Count": purchase_count_ID, "Purchase Price": 
                                purchase_value, 
                                "Total Purchase Value": total_purchase_ID})
ID_stats_df

# Sort the purchase count column in descending order

ID_stats_df_descending = ID_stats_df.sort_values("Total Purchase Value", ascending=False)
ID_stats_df_descending
ID_stats_df_descending.style.format({"Purchase Price":"${:,.2f}", 
                                 "Total Purchase Value":"${:,.2f}"})

# Most Profitable Items
# Sort the above table by total purchase value in descending order

ID_stats_df_descending.loc[:, ["Total Purchase Value"]].head()
