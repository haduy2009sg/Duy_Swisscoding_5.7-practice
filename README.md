# Duy_Swisscoding_5.7-practice
Ở practice này cần lấy thông tin các ứng viên ( được lưu trên nhiều nguồn khác nhau ) thật sự muốn đăng ký khóa học Big Data and Data Science của công ty xong vẫn muốn làm việc tại công ty hoặc là có mong muốn làm vị trí mới trong công ty so với hiện tại. Nhằm giảm chi phí đào tạo và tiết kiệm thời gian của công ty.
### Data Source 
Các nguồn dữ liệu và thông tin tôi có nêu trong: [Data Source](https://github.com/haduy2009sg/Duy_Swisscoding_5.7-practice/blob/554c2dfc9f5d2cbd60a72079b05e80fec53eb500/Source%20Practice%205-7.md)

### Extract Data
#### 1. Enrollies' Data
``` python
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
!wget $url_enrollies_education 
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
#### 4. Training hours
#### 5. City development index
#### 6. Employment

