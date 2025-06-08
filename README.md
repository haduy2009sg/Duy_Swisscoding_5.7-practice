# Duy_Swisscoding_5.7-practice
Ở practice này cần lấy thông tin các ứng viên ( được lưu trên nhiều nguồn khác nhau ) thật sự muốn đăng ký khóa học Big Data and Data Science của công ty xong vẫn muốn làm việc tại công ty hoặc là có mong muốn làm vị trí mới trong công ty so với hiện tại. Nhằm giảm chi phí đào tạo và tiết kiệm thời gian của công ty.
### Data Source 
Các nguồn dữ liệu và thông tin tôi có nêu trong: [Data Source](https://github.com/haduy2009sg/Duy_Swisscoding_5.7-practice/blob/554c2dfc9f5d2cbd60a72079b05e80fec53eb500/Source%20Practice%205-7.md)

### Extract Data
#### 1. Enrollies' Data
``` python
import pandas as pd
google_sheet_id = '1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
enrollies_data = pd.read_excel(url, sheet_name='enrollies')
enrollies_data.head(5)
```
|index|enrollee\_id|full\_name|city|gender|
|---|---|---|---|---|
|0|8949|Mike Jones|city\_103|Male|
|1|29725|Laura Jones|city\_40|Male|
|2|11561|David Miller|city\_21|NaN|
|3|33241|Laura Davis|city\_115|NaN|
|4|666|Alex Martinez|city\_162|Male|
#### 2. Enrollies' education
``` python
url_enrollies_education = 'https://assets.swisscoding.edu.vn/company_course/enrollies_education.xlsx'
!curl -o enrollies_education.xlsx $url_enrollies_education
enrollies_education = pd.read_excel('enrollies_education.xlsx')
enrollies_education.head(5)
```
|index|enrollee\_id|enrolled\_university|education\_level|major\_discipline|
|---|---|---|---|---|
|0|8949|no\_enrollment|Graduate|STEM|
|1|29725|no\_enrollment|Graduate|STEM|
|2|11561|Full time course|Graduate|STEM|
|3|33241|NaN|Graduate|Business Degree|
|4|666|no\_enrollment|Masters|STEM|
#### 3. Enrollies' working experience
```python
url_working_experience = 'https://assets.swisscoding.edu.vn/company_course/work_experience.csv'
!curl -o work_experience.csv $url_working_experience
working_experience = pd.read_csv('work_experience.csv')
working_experience.head(5)
```
|index|enrollee\_id|relevent\_experience|experience|company\_size|company\_type|last\_new\_job|
|---|---|---|---|---|---|---|
|0|8949|Has relevent experience|\>20|NaN|NaN|1|
|1|29725|No relevent experience|15|50-99|Pvt Ltd|\>4|
|2|11561|No relevent experience|5|NaN|NaN|never|
|3|33241|No relevent experience|\<1|NaN|Pvt Ltd|never|
|4|666|Has relevent experience|\>20|50-99|Funded Startup|4|
#### 4. Training hours
``` python
from sqlalchemy import create_engine
!pip install pymysql
import pymysql
# <driver>://<login>:<password>@<host>:<port>/<database_name>
engine = create_engine('mysql+pymysql://etl_practice:550814@112.213.86.31:3360/company_course')
#load
training_hours = pd.read_sql_table('training_hours', con=engine)
training_hours.head(5)
```
|index|enrollee\_id|training\_hours|
|---|---|---|
|0|8949|36|
|1|29725|47|
|2|11561|83|
|3|33241|52|
|4|666|8|
#### 5. City development index
``` python
city_development_tables = pd.read_html('https://sca-programming-school.github.io/city_development_index/index.html')
city_development_index = city_development_tables[0]
city_development_index.head(5)
```
|index|City|City Development Index|
|---|---|---|
|0|city\_103|0\.92|
|1|city\_40|0\.7759999999999999|
|2|city\_21|0\.624|
|3|city\_115|0\.789|
|4|city\_162|0\.767|
#### 6. Employment
``` python
# <driver>://<login>:<password>@<host>:<port>/<database_name>
# engine = create_engine('mysql+pymysql://etl_practice:550814@112.213.86.31:3360/company_course')
# -> same database as 4. Trainning hours so no new engine created
employment = pd.read_sql_table('employment', con=engine)
employment.head(5)
```
|index|enrollee\_id|employed|
|---|---|---|
|0|1|0\.0|
|1|2|1\.0|
|2|4|0\.0|
|3|5|0\.0|
|4|7|0\.0|

### Transform
#### 1. Enrollies' Data
``` python
# Fixing data types
## full_name -> String
enrollies_data['full_name'] = enrollies_data['full_name'].astype('string')
## city -> String
enrollies_data['city'] = enrollies_data['city'].astype('string')
# Missing data handling
enrollies_data['gender'] = enrollies_data['gender'].fillna(enrollies_data['gender'].mode()[0])
enrollies_data['gender'] = enrollies_data['gender'].astype('category')
# Handling duplicate
## Check duplicate
enrollies_data.duplicated().sum()
# Consistency
print(enrollies_data['gender'].unique())
```

#### 2. Enrollies' education
``` python
enrollies_education.info()
# Fill missing value ( missing value is pretty big -> fill Unknown)
enrollies_education['enrolled_university'] = enrollies_education['enrolled_university'].fillna('Unknown')
enrollies_education['education_level'] = enrollies_education['education_level'].fillna('Unknown')
enrollies_education['major_discipline'] = enrollies_education['major_discipline'].fillna('Unknown')
# Type
cat_cols = ['enrolled_university', 'education_level', 'major_discipline']
enrollies_education[cat_cols] = enrollies_education[cat_cols].astype('category')
enrollies_education.info()
```

#### 3. Enrollies' working experience
```python
working_experience['experience'] = working_experience['experience'].fillna(working_experience['experience'].mode()[0])
working_experience['company_size'] = working_experience['company_size'].fillna('Unknown')
working_experience['company_type'] = working_experience['company_type'].fillna('Unknown')
working_experience['last_new_job'] = working_experience['last_new_job'].fillna('Unknown')
# fixing data types
cal_cols2 = ['relevent_experience','experience', 'company_size', 'company_type', 'last_new_job']
working_experience[cal_cols2] = working_experience[cal_cols2].astype('category')
working_experience.info()
```

#### 4. Training hours
``` python
display(training_hours.head())
training_hours.info()
# Không có missing data
```

#### 5. City development index
``` python
display(city_development_index.head())
city_development_index.info()
# không có missing data
```

#### 6. Employment
``` python
display(employment.head())
employment.info()
# không có missing data
```

### Load data
``` python
db = 'data_warehouse.db'

target_db_engine = create_engine('sqlite:///data_warehouse.db')

employment.to_sql('Fact_employment', target_db_engine, if_exists='replace', index=False)
city_development_index.to_sql('Dim_city_development_index', target_db_engine, if_exists='replace', index=False)
training_hours.to_sql('Dim_training_hours', target_db_engine, if_exists='replace', index=False)
working_experience.to_sql('Dim_working_experience', target_db_engine, if_exists='replace', index=False)
enrollies_education.to_sql('Dim_enrollies_education', target_db_engine, if_exists='replace', index=False)
enrollies_data.to_sql('Dim_enrollies_data', target_db_engine , if_exists= 'replace', index=False)
```

