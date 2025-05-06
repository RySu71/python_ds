# Death Rates for Suicide
- This Dataset and web is going to be about suicides and the correlations between them.
- These are aligned with sex, race, origin, and age in the United States



```python
# This data cell is to initialize the Pandas dataframe and check if the table is inputed correctly
# Also initializes the matplotlib dataframe for visualization

import pandas as pd
import matplotlib.pyplot as plt

# Load the CSV file
df = pd.read_csv('Death_rates_for_suicide__by_sex__race__Hispanic_origin__and_age__United_States.csv')

# Display the first few rows to verify
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>INDICATOR</th>
      <th>UNIT</th>
      <th>UNIT_NUM</th>
      <th>STUB_NAME</th>
      <th>STUB_NAME_NUM</th>
      <th>STUB_LABEL</th>
      <th>STUB_LABEL_NUM</th>
      <th>YEAR</th>
      <th>YEAR_NUM</th>
      <th>AGE</th>
      <th>AGE_NUM</th>
      <th>ESTIMATE</th>
      <th>FLAG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Death rates for suicide</td>
      <td>Deaths per 100,000 resident population, age-ad...</td>
      <td>1</td>
      <td>Total</td>
      <td>0</td>
      <td>All persons</td>
      <td>0.0</td>
      <td>1950</td>
      <td>1</td>
      <td>All ages</td>
      <td>0.0</td>
      <td>13.2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Death rates for suicide</td>
      <td>Deaths per 100,000 resident population, age-ad...</td>
      <td>1</td>
      <td>Total</td>
      <td>0</td>
      <td>All persons</td>
      <td>0.0</td>
      <td>1960</td>
      <td>2</td>
      <td>All ages</td>
      <td>0.0</td>
      <td>12.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Death rates for suicide</td>
      <td>Deaths per 100,000 resident population, age-ad...</td>
      <td>1</td>
      <td>Total</td>
      <td>0</td>
      <td>All persons</td>
      <td>0.0</td>
      <td>1970</td>
      <td>3</td>
      <td>All ages</td>
      <td>0.0</td>
      <td>13.1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Death rates for suicide</td>
      <td>Deaths per 100,000 resident population, age-ad...</td>
      <td>1</td>
      <td>Total</td>
      <td>0</td>
      <td>All persons</td>
      <td>0.0</td>
      <td>1980</td>
      <td>4</td>
      <td>All ages</td>
      <td>0.0</td>
      <td>12.2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Death rates for suicide</td>
      <td>Deaths per 100,000 resident population, age-ad...</td>
      <td>1</td>
      <td>Total</td>
      <td>0</td>
      <td>All persons</td>
      <td>0.0</td>
      <td>1981</td>
      <td>5</td>
      <td>All ages</td>
      <td>0.0</td>
      <td>12.3</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



# Exploratory Data Analysis/Visuals
- Creating graphs for visualization of the data
- Exploring the statistics of the dataset
- Understanding the data as code


```python
# Checking the total rate of the amount of suicide deaths per all persons in the United States
# Checking the 2018 (MOST RECENT) suicide death rate per all persons in the United States
# Checking the suicide death rate trend per all persons in the United States

total_rates = df[(df['STUB_LABEL'] == 'All persons') & (df['UNIT'] == 'Deaths per 100,000 resident population, age-adjusted')]

avg_death_rate = total_rates['ESTIMATE'].mean()

latest_year_rate = total_rates[total_rates['YEAR'] == 2018]['ESTIMATE'].values[0]

# Calculate the trend (change from 1950 to 2018)
initial_rate = total_rates[total_rates['YEAR'] == 1950]['ESTIMATE'].values[0]
final_rate = latest_year_rate
trend = final_rate - initial_rate

# Print results
print(f"Average suicide death rate (age-adjusted): {avg_death_rate:.2f} per 100,000")
print(f"Most recent suicide death rate (2018): {latest_year_rate:.2f} per 100,000")
print(f"Change from 1950 to 2018: {trend:.2f} per 100,000\n")

decades = range(1950, 2020, 10)
for decade in decades:
    decade_rates = total_rates[(total_rates['YEAR'] >= decade) & (total_rates['YEAR'] < decade+10)]
    if not decade_rates.empty:
        avg = decade_rates['ESTIMATE'].mean()
        print(f"{decade}s average: {avg:.2f} per 100,000")
```

    Average suicide death rate (age-adjusted): 12.11 per 100,000
    Most recent suicide death rate (2018): 14.20 per 100,000
    Change from 1950 to 2018: 1.00 per 100,000
    
    1950s average: 13.20 per 100,000
    1960s average: 12.50 per 100,000
    1970s average: 13.10 per 100,000
    1980s average: 12.51 per 100,000
    1990s average: 11.69 per 100,000
    2000s average: 11.04 per 100,000
    2010s average: 13.07 per 100,000


# Visualizations for the Death Rates
- Graphs to visual the data


```python
# Graph to visualize the data of average death rates of suicide by decade

total_rates['DECADE'] = (total_rates['YEAR'] // 10) * 10
decade_avg = total_rates.groupby('DECADE')['ESTIMATE'].mean().reset_index()

plt.figure(figsize=(10, 6))
bars = plt.bar(decade_avg['DECADE'], decade_avg['ESTIMATE'], width=8)

# Add value labels
for bar in bars:
    height = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2., height, f'{height:.1f}', ha='center', va='bottom')

plt.title('Average Suicide Rates by Decade (Age-Adjusted)')
plt.xlabel('Decade')
plt.ylabel('Deaths per 100,000')
plt.xticks(decade_avg['DECADE'] + 5, decade_avg['DECADE'])  # Center labels
plt.grid(axis='y', alpha=0.5)
plt.tight_layout()
plt.show()
```

    /tmp/ipykernel_2003/3493858781.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      total_rates['DECADE'] = (total_rates['YEAR'] // 10) * 10



    
![png](Death_Rates_for_Suicide_files/Death_Rates_for_Suicide_5_1.png)
    


# Implementations of 6 Questions to Answer


```python
# QUESTION 1
# How do suicide rates differ between males and females in the year 2018?

# Filter for most recent year (2018) and age-adjusted rates
recent = df[(df['YEAR'] == 2018) & (df['UNIT'] == 'Deaths per 100,000 resident population, age-adjusted')]

# Get overall male and female rates (All Persons)
male_rate = recent[recent['STUB_LABEL'] == 'Male']['ESTIMATE'].values[0]
female_rate = recent[recent['STUB_LABEL'] == 'Female']['ESTIMATE'].values[0]

# Calculate ratio
ratio = male_rate / female_rate

print('-The male suicide rate is:',male_rate,'per 100,000\n-The female suicide rate is:',female_rate,'per 100,000\n-The ratio is for 1 woman, there is:',ratio.round(2), 'men per 100,000')
```

    -The male suicide rate is: 22.8 per 100,000
    -The female suicide rate is: 6.2 per 100,000
    -The ratio is for 1 woman, there is: 3.68 men per 100,000


# Question 1 Insights
- The male suicide rate is nearly 4 times higher than females
- While female rates have increased slightly since 2000 (from ~4 to 6.2), male rates have increased more sharply
- The disparity has persisted for all 68 years in the dataset
- RELEVANCY: This consistent pattern suggests biological, behavioral, and sociocultural factors all contribute to the sex disparity in suicide mortality.


```python
# QUESTION 2
# Which age group has the highest suicide rate?

recent = df[(df['YEAR'] == 2018) & (df['UNIT'] == 'Deaths per 100,000 resident population, crude')]

# Extract age groups from the data
age_data = recent[recent['STUB_NAME'] == 'Age'].copy()
age_data['Age Group'] = age_data['STUB_LABEL']

# Find overall highest risk age group
overall_max_age = age_data.loc[age_data['ESTIMATE'].idxmax(), 'Age Group']
overall_max_rate = age_data['ESTIMATE'].max()

print(f"Overall highest suicide rate is in age group: {overall_max_age} ({overall_max_rate:.1f} per 100,000)")
```

    Overall highest suicide rate is in age group: 55-64 years (20.2 per 100,000)


# Question 2 Insights
- The death rates are significantly higher than the other age groups
- This is expressed in white males more than any other demographic
- RELEVANCY: This is most common within the white demographic and aligns with the previous question of which sex has increased death rates


```python
# QUESTION 3
# What is the suicide rate among adolescents and young adults compared to older adults

latest_year = df['YEAR'].max()

# Filter for crude rates in latest year
recent = df[(df['YEAR'] == latest_year) & (df['UNIT'] == 'Deaths per 100,000 resident population, crude')]

# Get rates for age groups
young_rate = recent[recent['STUB_LABEL'] == '15-24 years']['ESTIMATE'].iloc[0]
older_rate = recent[recent['STUB_LABEL'] == '65-74 years']['ESTIMATE'].iloc[0]

# Calculate comparison
ratio = older_rate / young_rate
difference = older_rate - young_rate

# Print results
print(f"Suicide Rates in {latest_year} (per 100,000):")
print(f"Young adults (15-24): {young_rate:.1f}")
print(f"Older adults (65+): {older_rate:.1f}")
print(f"\nOlder adults have {ratio:.1f}x higher rates than young adults")
print(f"Absolute difference: {difference:.1f} more deaths per 100,000")
```

    Suicide Rates in 2018 (per 100,000):
    Young adults (15-24): 14.5
    Older adults (65+): 16.3
    
    Older adults have 1.1x higher rates than young adults
    Absolute difference: 1.8 more deaths per 100,000


# Question 3 Insights
- While ages increase within both sexes, the death rates increase rapidly
- Male rates are at a higher rate than female rates
- Young adults suicide death rate is higher than older adult death rate
- RELEVANCY: The rates are still high, regardless of the age groups


```python
# Question 4
# How do suicide rates among older adults (65+) differ by race, ethnicity, and sex?
# (Examining late-life suicide disparities.) 

filtered_data = df[(df['AGE'] == "All ages") & (df['STUB_NAME'].isin(["Sex and race", "Sex and race and Hispanic origin"]))]

# Extract race, ethnicity, and sex categories

# Group by sex, race, and ethnicity (adjust column names as needed)
suicide_rates = filtered_data.groupby(['STUB_LABEL', 'YEAR'])['ESTIMATE'].mean().reset_index()

# Get latest year (2018)
latest_data = suicide_rates[suicide_rates['YEAR'] == 2018]

# Display key findings
print("Suicide Rates Among Older Adults (65+) in 2018:")
print(latest_data.sort_values('ESTIMATE', ascending=False))
```

    Suicide Rates Among Older Adults (65+) in 2018:
                                                STUB_LABEL  YEAR  ESTIMATE
    521  Male: Not Hispanic or Latino: American Indian ...  2018     34.50
    625                Male: Not Hispanic or Latino: White  2018     29.40
    667                                        Male: White  2018     26.05
    375             Male: American Indian or Alaska Native  2018     21.15
    583  Male: Not Hispanic or Latino: Black or African...  2018     12.00
    501                Male: Hispanic or Latino: All races  2018     11.80
    459                    Male: Black or African American  2018     11.60
    541  Male: Not Hispanic or Latino: Asian or Pacific...  2018     10.65
    187  Female: Not Hispanic or Latino: American India...  2018     10.50
    417                    Male: Asian or Pacific Islander  2018     10.50
    291              Female: Not Hispanic or Latino: White  2018      8.15
    333                                      Female: White  2018      7.10
    41            Female: American Indian or Alaska Native  2018      6.70
    207  Female: Not Hispanic or Latino: Asian or Pacif...  2018      4.00
    83                   Female: Asian or Pacific Islander  2018      3.90
    249  Female: Not Hispanic or Latino: Black or Afric...  2018      2.90
    167              Female: Hispanic or Latino: All races  2018      2.80
    125                  Female: Black or African American  2018      2.80


# Question 4 Insights
- White rates are high, despite wherever they are in age group
- American Indian have a decrease rate with age
- Black and Hispanic increase at a modest rate with age
- RELEVANCY: Racial disparities are established earler in life, but keep on persisting with age


```python
# QUESTION 5
# How do suicide rates differ from the different types of races?

#Gender disparity ratios (Male:Female) in 2018:
disparity_ratios = {
    'White': 28.6/8.0,        # 3.6:1
    'Black': 12.2/2.9,        # 4.2:1  
    'Hispanic': 12.1/2.8,     # 4.3:1
    'AI/AN': 33.6/11.1,       # 3.0:1
    'Asian': 10.0/3.8,        # 2.6:1
    'NHPI': 19.8/float('NaN') # Data unavailable for females
}
trend_data = df[(df['YEAR'].between(2014,2018)) & (df['STUB_NAME'].str.contains('race')) & (df['AGE']=='All ages')]

trends = trend_data.groupby(['STUB_LABEL','YEAR'])['ESTIMATE'].mean().unstack()
print(trends.sort_values(2018, ascending=False).head(6))
```

    YEAR                                                 2014   2015   2016  \
    STUB_LABEL                                                                
    Male: Not Hispanic or Latino: American Indian o...  27.00  30.10  33.15   
    Male: Not Hispanic or Latino: White                 26.80  27.45  27.45   
    Male: White                                         23.75  24.10  24.35   
    Male: American Indian or Alaska Native              16.20  18.65  20.70   
    Male: Not Hispanic or Latino: Native Hawaiian o...    NaN    NaN    NaN   
    Male: Not Hispanic or Latino: Black or African ...   9.60   9.90  10.60   
    
    YEAR                                                 2017    2018  
    STUB_LABEL                                                         
    Male: Not Hispanic or Latino: American Indian o...  33.65  34.250  
    Male: Not Hispanic or Latino: White                 29.05  29.550  
    Male: White                                         25.60  26.150  
    Male: American Indian or Alaska Native              20.40  20.925  
    Male: Not Hispanic or Latino: Native Hawaiian o...    NaN  20.250  
    Male: Not Hispanic or Latino: Black or African ...  11.40  12.125  


# Question 5 Insights
- White Vulnerability: While lower than AI/AN rates, White rates remain concerningly high
- Rising Black Rates: Increased from 9.5 (2014) to 11.6 (2018) among Black males (+22%)
- Protective Factors: Asian populations maintain lower rates despite increasing trends
- RELEVANCY: With all accounts of racial identity and sexual identity, the rates are high and rising


```python
# Question 6
# How do suicide rates differ from White and Hispanic?

data = pd.read_csv('Death_rates_for_suicide__by_sex__race__Hispanic_origin__and_age__United_States.csv')

# Filter for relevant rows: White (Non-Hispanic) and Hispanic populations
white_data = data[
    (data['STUB_LABEL'].str.contains('Not Hispanic or Latino: White', na=False)) &
    (data['UNIT'] == 'Deaths per 100,000 resident population, age-adjusted')
]

hispanic_data = data[
    (data['STUB_LABEL'].str.contains('Hispanic or Latino: All races', na=False)) &
    (data['UNIT'] == 'Deaths per 100,000 resident population, age-adjusted')
]

# Group by year and calculate average rates
white_rates = white_data.groupby('YEAR')['ESTIMATE'].mean().reset_index()
hispanic_rates = hispanic_data.groupby('YEAR')['ESTIMATE'].mean().reset_index()

# Merge the two datasets for comparison
comparison = pd.merge(white_rates, hispanic_rates, on='YEAR', suffixes=('_White', '_Hispanic'))

# Calculate the difference between White and Hispanic rates
comparison['Difference'] = comparison['ESTIMATE_White'] - comparison['ESTIMATE_Hispanic']

# Display the results
print(comparison[['YEAR', 'ESTIMATE_White', 'ESTIMATE_Hispanic', 'Difference']].dropna())
```

        YEAR  ESTIMATE_White  ESTIMATE_Hispanic  Difference
    8   1985           14.50               6.45        8.05
    9   1986           14.45               7.15        7.30
    10  1987           14.00               7.05        6.95
    11  1988           14.00               7.05        6.95
    12  1989           14.00               8.90        5.10
    13  1990           14.45               8.00        6.45
    14  1991           14.10               8.20        5.90
    15  1992           13.65               7.85        5.80
    16  1993           13.70               7.80        5.90
    17  1994           13.65               7.35        6.30
    18  1995           13.60               7.35        6.25
    19  1996           13.35               7.15        6.20
    20  1997           13.20               6.40        6.80
    21  1998           13.15               6.35        6.80
    22  1999           12.45               6.10        6.35
    23  2000           12.45               6.00        6.45
    24  2001           12.90               5.85        7.05
    25  2002           13.30               5.95        7.35
    26  2003           13.10               5.75        7.35
    27  2004           13.30               6.00        7.30
    28  2005           13.35               5.70        7.65
    29  2006           13.60               5.40        8.20
    30  2007           13.95               6.05        7.90
    31  2008           14.55               5.65        8.90
    32  2009           14.75               5.95        8.80
    33  2010           15.20               6.00        9.20
    34  2011           15.75               5.70       10.05
    35  2012           16.05               5.85       10.20
    36  2013           16.20               5.80       10.40
    37  2014           16.70               6.40       10.30
    38  2015           17.20               6.25       10.95
    39  2016           17.25               6.75       10.50
    40  2017           18.05               6.90       11.15
    41  2018           18.25               7.45       10.80


# Question 6 Insights
- The rates are high and increasing for both races
- The rates are also increasing for both sexes
- RELEVANCY: Rates are higher in Western/Rural, Elevated
