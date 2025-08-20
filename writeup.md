# Demographic Data Analyzer - Solution Writeup

## Dataset Description
The dataset (`adult.data.csv`) contains demographic information with the following key columns:
- **age**: Age of the person
- **education**: Education level (Bachelors, Masters, Doctorate, HS-grad, etc.)
- **sex**: Gender (Male/Female)
- **race**: Race/ethnicity
- **hours-per-week**: Number of hours worked per week
- **native-country**: Country of origin
- **occupation**: Type of job
- **salary**: Income level (>50K or <=50K)

## Questions Solved and Solutions

### 1. Race Distribution
**Question**: How many people of each race are represented in the dataset?

**Solution**: Used `value_counts()` on the 'race' column to get the count of each racial group.
```python
race_count = df['race'].value_counts()
```
This returns a pandas Series with race names as index labels and their counts as values.

### 2. Average Age of Men
**Question**: What is the average age of men in the dataset?

**Solution**: Filtered the dataset for males and calculated the mean age, rounded to 1 decimal place.
```python
average_age_men = round(df[df['sex'] == 'Male']['age'].mean(), 1)
```

### 3. Bachelor's Degree Percentage
**Question**: What percentage of people have a Bachelor's degree?

**Solution**: Used boolean indexing to find people with Bachelors degrees and calculated the percentage.
```python
percentage_bachelors = round((df['education'] == 'Bachelors').mean() * 100, 1)
```

### 4. Education vs Income Analysis
**Question**: What percentage of people with advanced education make more than 50K? What about those without advanced education?

**Solution**: 
- Defined advanced education as having Bachelors, Masters, or Doctorate degrees
- Created boolean masks to separate higher and lower education groups
- Calculated the percentage earning >50K for each group

```python
higher_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
lower_education = ~higher_education
higher_education_rich = round((df['salary'] == '>50K')[higher_education].mean() * 100, 1)
lower_education_rich = round((df['salary'] == '>50K')[lower_education].mean() * 100, 1)
```

### 5. Minimum Work Hours Analysis
**Question**: What is the minimum number of hours worked per week, and what percentage of people working minimum hours earn >50K?

**Solution**:
- Found the minimum hours using `min()`
- Created a mask for people working minimum hours
- Calculated the percentage of minimum-hour workers earning >50K

```python
min_work_hours = df['hours-per-week'].min()
min_workers_mask = df['hours-per-week'] == min_work_hours
rich_percentage = round((rich_mask & min_workers_mask).sum() / num_min_workers * 100, 1)
```

### 6. Highest Earning Country
**Question**: Which country has the highest percentage of people earning >50K?

**Solution**: Used `groupby()` to group by country and calculated the percentage earning >50K for each country.
```python
country_rich_pct = df.groupby('native-country')['salary'].apply(lambda x: (x == '>50K').mean() * 100)
highest_earning_country = country_rich_pct.idxmax()
```

### 7. Top Occupation in India
**Question**: What is the most popular occupation for high earners in India?

**Solution**: Filtered for people from India earning >50K and found the most common occupation.
```python
india_rich = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]
top_IN_occupation = india_rich['occupation'].value_counts().index[0]
```

