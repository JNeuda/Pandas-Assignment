



import pandas as pd
import numpy as np
import json

#Read json file
purchase_data = "Resources/purchase_data.json"
purchase_data_pd = pd.read_json(purchase_data)

df = pd.DataFrame(purchase_data_pd)

#Player Count
#Total Number of Players

total_players = df["SN"].nunique()
player_count = pd.DataFrame({"Total Players":[total_players]})

#Purchasing Analysis (Total)

#Number of Unique Items
unique_item = df["Item Name"].nunique()

#Average Purchase Price
avg_price = df["Price"].mean()

#Total Number of Purchases
total_purchases = df["SN"].count()

#Total Revenue
total_revenue = df["Price"].sum()

purchasing_analysis = pd.DataFrame({"Number of Unique Items":[unique_item], 
                                    "Average Price":[avg_price], 
                                    "Number of Purchases":[total_purchases],
                                    "Total Revenue":[total_revenue]
                                   })
purchasing_analysis2 = purchasing_analysis[["Number of Unique Items",
                                            "Average Price",
                                            "Number of Purchases",
                                            "Total Revenue"
                                           ]] 
# Use Map to format all the columns
purchasing_analysis2["Average Price"] = purchasing_analysis2["Average Price"].map("${:.2f}".format)
purchasing_analysis2["Total Revenue"] = purchasing_analysis2["Total Revenue"].map("${:.2f}".format)


#Gender Demographics

df_gender = df[["SN","Gender"]]
df_gender_unique = df_gender.drop_duplicates(["SN"]) # got rid of duplicate SN
unique_gender_counts =df_gender_unique["Gender"].value_counts()
gender_demo = pd.DataFrame({"Total Count":unique_gender_counts
                     })
#
percentages =  (gender_demo["Total Count"]/573)*100
gender_demo["Percentage of Players"] = percentages
#
gender_demo = pd.DataFrame({"Total Count":unique_gender_counts,
                            "Percentage of Players":percentages
                     })
# Formatting
gender_demo["Percentage of Players"] = gender_demo["Percentage of Players"].map("{:.2f}".format)
gender_demo


#Purchasing Analysis (Gender)

#The below each broken by gender
grouped_gender_data = df.groupby(['Gender'])

#Purchase Count
purchase_count_gender = grouped_gender_data["SN"].count()

#Average Purchase Price
avg_purch_price = grouped_gender_data["Price"].mean()

#Total Purchase Value
total_purch_value = grouped_gender_data["Price"].sum()

#Normalized Totals
normal_totals = total_purch_value/unique_gender_counts

gender_pa_summary = pd.DataFrame({"Purchase Count":purchase_count_gender,
                            "Average Pruchase Price":avg_purch_price,
                            "Total Purchase Value":total_purch_value,
                            "Normalized Totals":normal_totals,
                     })

#Formatting 
gender_pa_summary["Average Pruchase Price"] = gender_pa_summary["Average Pruchase Price"].map("${:.2f}".format)
gender_pa_summary["Total Purchase Value"] = gender_pa_summary["Total Purchase Value"].map("${:.2f}".format)
gender_pa_summary["Normalized Totals"] = gender_pa_summary["Normalized Totals"].map("${:.2f}".format)

#Reorganizing Columns
gender_pa_summary2 = gender_pa_summary[["Purchase Count",
                                       "Average Pruchase Price",
                                       "Total Purchase Value",
                                       "Normalized Totals"
                                       ]]
gender_pa_summary2


#Age Demographics

#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
bins = [0,10,14,19,24,29,34,39,100]
group_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
df[' '] = pd.cut(df["Age"], bins, labels=group_names)
df_age_group = df.groupby(' ')

#Purchase Count
purchase_count_gender = df_age_group["Age"].count()

#Average Purchase Price
avg_purch_price = df_age_group["Price"].mean()

#Total Purchase Value
total_purch_value = df_age_group["Price"].sum()

#Normalized Totals
unique_age_count =df_age_group[" "].count()
normal_age_totals = total_purch_value/unique_age_count

df_age_group1 = pd.DataFrame({"Purchase Count":purchase_count_gender,
                            "Average Pruchase Price":avg_purch_price,
                            "Total Purchase Value":total_purch_value,
                            "Normalized Totals":normal_age_totals
                     })
#Formatting 
df_age_group1["Average Pruchase Price"] = df_age_group1["Average Pruchase Price"].map("${:.2f}".format)
df_age_group1["Total Purchase Value"] = df_age_group1["Total Purchase Value"].map("${:.2f}".format)
df_age_group1["Normalized Totals"] = df_age_group1["Normalized Totals"].map("${:.2f}".format)

#Reorganizing Columns
df_age_group2 = df_age_group1[["Purchase Count",
                            "Average Pruchase Price",
                            "Total Purchase Value",
                            "Normalized Totals"
                           ]]
df_age_group2


#Top Spenders

#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#SN
top_spender_data = df.groupby(['SN'])

#Purchase Count
purchase_count_sn = top_spender_data["SN"].count()

#Average Purchase Price
avg_purch_price = top_spender_data["Price"].mean()

#Total Purchase Value
total_purch_value = top_spender_data["Price"].sum()

top_spender_summary = pd.DataFrame({"Purchase Count":purchase_count_sn,
                            "Average Pruchase Price":avg_purch_price,
                            "Total Purchase Value":total_purch_value
                     })

#Formatting 
top_spender_summary["Average Pruchase Price"] = top_spender_summary["Average Pruchase Price"].map("${:.2f}".format)
top_spender_summary["Total Purchase Value"] = top_spender_summary["Total Purchase Value"].map("${:.2f}".format)

#Reorganizing Columns
top_spender_summary2 = top_spender_summary[["Purchase Count",
                                       "Average Pruchase Price",
                                       "Total Purchase Value",
                                       ]]
top_spender_summary3 = top_spender_summary2.sort_values('Total Purchase Value', ascending=False)
top_spender_summary3.head()


#Most Popular Items

#Identify the 5 most popular items by purchase count, then list (in a table):
#Item ID
#Item Name
pop_item_data = df.groupby(['Item ID','Item Name'])

#Purchase Count
purchase_count_sn = pop_item_data["SN"].count()

#Average Purchase Price
avg_purch_price = pop_item_data["Price"].mean()

#Total Purchase Value
total_purch_value = pop_item_data["Price"].sum()

pop_item_summary = pd.DataFrame({"Purchase Count":purchase_count_sn,
                            "Item Price":avg_purch_price,
                            "Total Purchase Value":total_purch_value
                     })

#Formatting 
pop_item_summary["Item Price"] = pop_item_summary["Item Price"].map("${:.2f}".format)
pop_item_summary["Total Purchase Value"] = pop_item_summary["Total Purchase Value"].map("${:.2f}".format)

#Reorganizing Columns
pop_item_summary2 = pop_item_summary[["Purchase Count",
                                       "Item Price",
                                       "Total Purchase Value",
                                       ]]
pop_item_summary3 = pop_item_summary2.sort_values('Purchase Count', ascending=False)
pop_item_summary3.head()


#Most Profitable Items
#Identify the 5 most profitable items by total purchase value, then list (in a table):

most_profitable_data = df.groupby(['Item ID','Item Name'])

#Purchase Count
purchase_count_sn = most_profitable_data["SN"].count()

#Average Purchase Price
avg_purch_price = most_profitable_data["Price"].mean()

#Total Purchase Value
total_purch_value = most_profitable_data["Price"].sum()

most_profitable_summary = pd.DataFrame({"Purchase Count":purchase_count_sn,
                            "Item Price":avg_purch_price,
                            "Total Purchase Value":total_purch_value
                     })

#Formatting 
most_profitable_summary["Item Price"] = most_profitable_summary["Item Price"].map("${:.2f}".format)
most_profitable_summary["Total Purchase Value"] = most_profitable_summary["Total Purchase Value"].map("${:.2f}".format)

#Reorganizing Columns
most_profitable_summary2 = most_profitable_summary[["Purchase Count",
                                       "Item Price",
                                       "Total Purchase Value",
                                       ]]
most_profitable_summary3 = most_profitable_summary2.sort_values('Total Purchase Value', ascending=False)
most_profitable_summary3.head()


