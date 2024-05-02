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
