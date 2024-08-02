```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import warnings

```


```python
# Load the cleaned dataset
df = pd.read_csv('cleaned_credit_union_data.csv')

# Correct Zip code datatype
df['Zip code (Mailing address)'] = df['Zip code (Mailing address)'].astype(int).astype(str).str.zfill(5)
```


```python
# Load first few rows and check size of dataframe to confirm successul load.

pd.set_option('display.max_columns', None)
print(f"Dataframe is {df.shape[0]} rows deep and {df.shape[1]} columns wide.")
display(df.head(3))
```


```python
# Correlation Ma
warnings.filterwarnings('ignore')
warnings.simplefilter('ignore')

# Load the cleaned dataset
df = pd.read_csv('cleaned_credit_union_data.csv')

# Define columns for correlation analysis with updated names
all_corr_columns = [
    'Members', 'Total assets', 'Total loans', 'Total deposits', 
    'Return on average assets', 'Net worth ratio (excludes CECL transition provision)', 
    'Loan-to-share ratio', 'Total deposits, 4 quarter growth  (%)', 
    'Total loans,  4 quarter growth  (%)', 'Total assets,  4 quarter growth  (%)',
    'Members, 4 quarter growth  (%)', 'Net worth,  4 quarter growth (excludes CECL transition provision) (%)'
]

# Select relevant columns for correlation analysis
df_all_corr = df[all_corr_columns]

# Rename columns as specified
df_all_corr.rename(columns={
    'Net worth ratio (excludes CECL transition provision)': 'Net worth ratio',
    'Total deposits, 4 quarter growth  (%)': 'Total deposits (Q4 growth %)',
    'Total loans,  4 quarter growth  (%)': 'Total loans (Q4 growth %)',
    'Total assets,  4 quarter growth  (%)': 'Total assets (Q4 growth %)',
    'Members, 4 quarter growth  (%)': 'Members (Q4 growth %)',
    'Net worth,  4 quarter growth (excludes CECL transition provision) (%)': 'Net worth (Q4 growth %)'
}, inplace=True)

# Calculate the correlation matrix
all_corr_matrix = df_all_corr.corr()

# Generate a mask for the upper triangle
mask = np.triu(np.ones_like(all_corr_matrix, dtype=bool))

# Set up the matplotlib figure
fig, ax = plt.subplots(figsize=(12, 10))

# Set color palette
cmap = sns.color_palette("coolwarm", as_cmap=True)

# Draw the heatmap with annotations
sns.heatmap(all_corr_matrix, mask=mask, vmin=-1, vmax=1, center=0, annot=True, fmt=".2f", cmap=cmap, linewidths=.5)

# Add titles and labels
plt.suptitle('Correlation Matrix of Credit Union Metrics', fontsize=12, weight='bold')
plt.title('Source: NCUA Q1 2024 Publication', fontsize=9) 
plt.subplots_adjust(left=0.2, bottom=0.2)  # Adjust these values as needed
plt.savefig('cu_heatmap.png', bbox_inches='tight')
plt.show()

```
