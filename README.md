echo "# 331final" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/Odalys-SC/331final.git
git push -u origin main
import pandas as pd
import matplotlib.pyplot as plt

# Convert the spreadsheet link to an API link
spreadsheet_id = '1vRJF9p71J7dNvjKD1Th01P42OgCE4h-QnjilpJNy25g'
api_link = f'https://docs.google.com/spreadsheets/d/{spreadsheet_id}/gviz/tq?tqx=out:csv&sheet=Sheet1'

# Read the data from the API link using pandas
df = pd.read_csv(api_link)

# Select relevant columns for analysis
selected_columns = ['Advertisement_Type', 'Newspaper_Investment', 'Facebook_Investment', 'Instagram_Investment',
                    'YouTube_Investment', 'Twitter_Investment', 'LinkedIn_Investment',
                    'Pinterest_Investment', 'Reddit_Investment', 'Snapchat_Investment',
                    'Sales_Revenue', 'Target_Audience', 'Target_Age_Group', 'Profit']
df_selected = df[selected_columns]

# Rename columns (except the last three)
df_selected = df_selected.rename(columns={col: col.split('_')[0] for col in df_selected.columns[:-3]})

with pd.option_context('display.max_columns', None, 'display.precision', 2):
    # Print the selected DataFrame with nicer formatting
    print(df_selected.to_string(index=False))
# Analyze the data
# Calculate total investment and revenue by target audience
total_investment_by_audience = df.groupby('Target_Audience')[['Newspaper_Investment', 'Facebook_Investment', 'Instagram_Investment',
                                                              'YouTube_Investment', 'Twitter_Investment', 'LinkedIn_Investment',
                                                              'Pinterest_Investment', 'Reddit_Investment', 'Snapchat_Investment']].sum()
total_revenue_by_audience = df.groupby('Target_Audience')['Sales_Revenue'].sum()

# Calculate total investment by gender
total_investment_by_gender = df.groupby('Target_Audience')[['Newspaper_Investment', 'Facebook_Investment', 'Instagram_Investment',
                                                             'YouTube_Investment', 'Twitter_Investment', 'LinkedIn_Investment',
                                                             'Pinterest_Investment', 'Reddit_Investment', 'Snapchat_Investment']].sum()

# Plot the data
fig, axs = plt.subplots(2, 2, figsize=(18, 12))

# Total Investment by Gender
total_investment_by_gender.loc[['Women', 'Men']].plot(kind='bar', ax=axs[0, 0])
axs[0, 0].set_title('Total Investment by Gender')
axs[0, 0].set_xlabel('Gender')
axs[0, 0].set_ylabel('Total Investment')

# Total Investment by Target Audience
total_investment_by_audience.plot(kind='bar', stacked=True, ax=axs[0, 1])
axs[0, 1].set_title('Total Investment by Target Audience')
axs[0, 1].set_xlabel('Target Audience')
axs[0, 1].set_ylabel('Total Investment')
axs[0, 1].legend(title='Investment Type', loc='center left', bbox_to_anchor=(1, 0.5))

# Total Revenue by Target Audience
total_revenue_by_audience.plot(kind='bar', ax=axs[1, 0])
axs[1, 0].set_title('Total Revenue by Target Audience')
axs[1, 0].set_xlabel('Target Audience')
axs[1, 0].set_ylabel('Total Revenue')

# Scatter plot of Investment vs. Revenue
axs[1, 1].scatter(df['Total_Investment'], df['Sales_Revenue'])
axs[1, 1].set_title('Relationship between Total Investment and Sales Revenue')
axs[1, 1].set_xlabel('Total Investment')
axs[1, 1].set_ylabel('Sales Revenue')

plt.tight_layout()
plt.show()

# Calculate total revenue per gender per social media platform
selected_columns = ['Facebook_Investment', 'Instagram_Investment',
                    'YouTube_Investment', 'Twitter_Investment', 'LinkedIn_Investment',
                    'Pinterest_Investment', 'Reddit_Investment', 'Snapchat_Investment',
                    'Sales_Revenue', 'Target_Audience']
df_selected = df[selected_columns]
total_revenue_by_gender_platform = df_selected.groupby(['Target_Audience']).sum()

# Plot the data
plt.figure(figsize=(12, 8))
total_revenue_by_gender_platform.drop(columns=['Sales_Revenue']).plot(kind='bar', stacked=True)
plt.title('Total Revenue by Gender and Social Media Platform')
plt.xlabel('Gender')
plt.ylabel('Total Revenue')
plt.legend(title='Social Media Platform', loc='center left', bbox_to_anchor=(1, 0.5))
plt.show()
#Calculate the total revenue per gender
total_revenue_by_platform_gender = df_selected.groupby(['Target_Audience']).sum()

# Determine the platform with the highest revenue for each gender
platforms = ['Facebook_Investment', 'Instagram_Investment', 'YouTube_Investment', 'Twitter_Investment', 'LinkedIn_Investment', 'Pinterest_Investment', 'Reddit_Investment', 'Snapchat_Investment']
max_revenue_platforms = total_revenue_by_platform_gender[platforms].idxmax(axis=1)

# Create a decision tree

platform_decision_tree = pd.DataFrame(max_revenue_platforms, columns=['Recommended Investment'])

# Display the decision tree
print(platform_decision_tree)

max_revenue_platforms = total_revenue_by_platform_gender[platforms].idxmax()

# Create a decision tree
gender_decision_tree = pd.DataFrame(max_revenue_platforms, index=platforms, columns=['Recommended Gender'])

# Display the decision tree
print(gender_decision_tree)

#Redefine df_selected adding 'Profit' column
selected_columns = ['Facebook_Investment', 'Instagram_Investment',
                    'YouTube_Investment', 'Twitter_Investment', 'LinkedIn_Investment',
                    'Pinterest_Investment', 'Reddit_Investment', 'Snapchat_Investment',
                    'Sales_Revenue', 'Target_Audience', 'Profit']
df_selected = df[selected_columns]

#Classify whether the income is a profit, loss, or break-even point
def classify_profit(Profit):
    return 'Loss' if Profit < 0 else 'Break-even' if Profit == 0 else 'Profit'

# Check if 'Profit' column exists in the DataFrame
if 'Profit' in df_selected.columns:
    # Apply the classification function to the 'Profit' column
    df_selected['Profit_Class'] = df_selected['Profit'].apply(classify_profit)

    # Display the updated DataFrame
    with pd.option_context('display.max_columns', None, 'display.precision', 2):
        print(df_selected.to_string(index=False))
else:
    print("Error: 'Profit' column not found in DataFrame.")
