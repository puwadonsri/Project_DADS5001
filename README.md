# Project_DADS5001
## Topic : การพัฒนาอสังหาริมทรัพย์กระจุกตัวอยู่ตามแนวรถไฟฟ้า
## Library
```python
#import library
import sys
import numpy as np
import pandas as pd
import matplotlib as mpl
%matplotlib inline
import matplotlib.pyplot as plt
plt.style.use('default') 
import matplotlib.font_manager as font_manager
import os
import plotly.express as px

#install font support thai
!wget -q https://github.com/Phonbopit/sarabun-webfont/raw/master/fonts/thsarabunnew-webfont.ttf
mpl.font_manager.fontManager.addfont('thsarabunnew-webfont.ttf')
mpl.rc('font', family='TH Sarabun New', size=10)
```
## Dataset
###### Residental Project Data 
ข้อมูลรายละเอียดโครงการที่อยู่อาศัยประเภท จัดสรร ผู้พัฒนา พื้นที่โครงการ และสิ่งอำนวนความสะดวกต่าง ๆ จำนวน มีจำนวน 23,744 rows และ 46 columns 

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 23744 entries, 0 to 23743
Data columns (total 46 columns):
 #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
 0   row_number              23739 non-null  float64
 1   project_id              23744 non-null  object 
 2   name_en                 23744 non-null  object 
 3   name_th                 23744 non-null  object 
 4   propertytype_id         23744 non-null  object 
 5   propertytype_name_en    23744 non-null  object 
 6   propertytype_name_th    23744 non-null  object 
 7   price_min               23633 non-null  object 
 8   developer_id            23744 non-null  object 
 9   developer_name_en       11001 non-null  object 
 10  developer_name_th       14957 non-null  object 
 11  latitude                23744 non-null  float64
 12  longitude               23739 non-null  float64
 13  neighborhood_id         19505 non-null  object 
 14  neighborhood_name_en    19506 non-null  object 
 15  neighborhood_name_th    19500 non-null  object 
 16  subdistrict_id          23724 non-null  float64
 17  subdistrict_name_en     23729 non-null  object 
 18  subdistrict_name_th     23727 non-null  object 
 19  district_id             23734 non-null  float64
 20  district_name_en        23734 non-null  object 
 21  district_name_th        23734 non-null  object 
 22  province_id             23735 non-null  float64
 23  province_name_en        23734 non-null  object 
 24  province_name_th        23734 non-null  object 
 25  zipcode                 23707 non-null  float64
 26  count_elevator          1899 non-null   object 
 27  count_elevator_service  610 non-null    object 
 28  count_floor             4735 non-null   object 
 29  count_parking           2017 non-null   object 
 30  count_tower             5 non-null      object 
 31  count_unit              21792 non-null  object 
 32  count_unittype          18590 non-null  float64
 33  facility_clubhouse      6769 non-null   float64
 34  facility_fitness        8906 non-null   float64
 35  facility_meeting        2706 non-null   float64
 36  facility_park           11775 non-null  float64
 37  facility_playground     6172 non-null   float64
 38  facility_pool           9733 non-null   float64
 39  facility_security       16336 non-null  float64
 40  date_created            23734 non-null  object 
 41  date_finish             20918 non-null  object 
 42  date_updated            23734 non-null  object 
 43  source                  23734 non-null  object 
 44  url_project             23734 non-null  object 
 45  day                     23734 non-null  object 
dtypes: float64(15), object(31)
```

###### Residental Unittype Data 
ข้อมูลรายละเอียดแบบบ้านในโครงการที่อยู่อาศัยประเภท จัดสรร มีจำนวน 43,133 rows และ 28 columns
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 43133 entries, 0 to 43132
Data columns (total 28 columns):
 #   Column                Non-Null Count  Dtype  
---  ------                --------------  -----  
 0   row_number            43133 non-null  int64  
 1   unittype_id           43133 non-null  object 
 2   project_id            43133 non-null  object 
 3   name_th               43131 non-null  object 
 4   propertytype_id       43133 non-null  int64  
 5   propertytype_name_en  43133 non-null  object 
 6   propertytype_name_th  43133 non-null  object 
 7   area_total_min_wa     3498 non-null   float64
 8   area_usable_min       37443 non-null  float64
 9   price_min             35737 non-null  float64
 10  count_airconditioner  1780 non-null   float64
 11  count_elevator        0 non-null      float64
 12  count_floor_internal  30175 non-null  float64
 13  count_parking         23963 non-null  float64
 14  count_room_bath       41653 non-null  float64
 15  count_room_bed        41136 non-null  float64
 16  count_room_dinning    13700 non-null  float64
 17  count_room_guest      13082 non-null  float64
 18  count_room_kitchen    20520 non-null  float64
 19  count_room_living     10125 non-null  float64
 20  count_room_maid       2089 non-null   float64
 21  count_room_storage    4104 non-null   float64
 22  count_room_utility    755 non-null    float64
 23  date_created          43133 non-null  object 
 24  date_updated          43133 non-null  object 
 25  source                43133 non-null  object 
 26  url_unittype          43131 non-null  object 
 27  day                   43133 non-null  object 
dtypes: float64(16), int64(2), object(10)
```

###### City Train Station 
ข้อมูลสถานีรถไฟฟ้า ทั้งที่เปิดใช้บริการแล้ว และอยู่ในขั้นตอนของแผนการพัฒนา มีจำนวน 318 rows และ 20 columns
```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 8998 entries, 0 to 8997
Data columns (total 29 columns):
 #   Column                Non-Null Count  Dtype  
---  ------                --------------  -----  
 0   project_id            8680 non-null   object 
 1   name_en               8680 non-null   object 
 2   name_th               8680 non-null   object 
 3   propertytype_name_en  8680 non-null   object 
 4   propertytype_name_th  8680 non-null   object 
 5   price_min             8669 non-null   object 
 6   latitude              8998 non-null   float64
 7   longitude             8998 non-null   float64
 8   province_id           8680 non-null   float64
 9   province_name_en      8680 non-null   object 
 10  province_name_th      8680 non-null   object 
 11  row_number            318 non-null    float64
 12  gid                   318 non-null    float64
 13  line_no               318 non-null    float64
 14  railway               318 non-null    object 
 15  line_typt             318 non-null    object 
 16  line_type             318 non-null    object 
 17  status                318 non-null    object 
 18  line_th               318 non-null    object 
 19  line_en               318 non-null    object 
 20  station_th            318 non-null    object 
 21  station_en            318 non-null    object 
 22  prov_namt             318 non-null    object 
 23  prov_name             318 non-null    object 
 24  amp_namt              318 non-null    object 
 25  amp_name              318 non-null    object 
 26  tam_namt              318 non-null    object 
 27  tam_name              318 non-null    object 
 28  day                   318 non-null    object 
dtypes: float64(6), object(23)
```

## Question & Answer
1.	การพัฒนาอสังหาริมทรัพย์กระจุกอยู่ตามแนวรถไฟฟ้า (Focus พื้นที่กรุงเทพ นนทบุรี ปทุมธานี สมุทรปราการ) 
    -	จากข้อมูลเบื้องต้นพบว่าการการะจายตัวของอสังหาไม่ได้กระจุกตัวอยู่ตามแนวรถไฟฟ้าเสมอไปขึ้นอยู่กับประเภทของอสังหาริมทรัพย์นั้น ๆ ด้วย
2.	ราคาอสังหาริมทรัพย์จะแพงขึ้นตามระยะทางใกล้รถไฟฟ้า
    -	จากข้อมูลเบื้องต้นพบว่าราคาบ้านและคอนโดในพื้นที่กรุงเทพ สมุทรปราการ นนทบุรี และปทุมธานีมีราคาสูงจากเรียงลงมาตามลำดับ

## Challenge 
1.	เนื่องจากข้อมูลใน dataset ที่ต้องการแมทช์กับเส้นทางรถไฟฟ้าอาจยังไม่มีความสัมพันธ์กัน จึงใช้วิธีการ plot ลงบนแผนที่อาจทำให้ข้อมูลมองยาก
2.	จากข้อจำกัดของข้อมูลอาจทำให้การ analysis data ออกมายังไม่ชัดเจน 
3.	เนื่องจากพยายามหาวิธีการที่ให้การดูข้อมูลง่ายขึ้น แต่ยังไม่พบรูปแบบที่น่าพอใจ

## References
1. Dataset : Bestimate By Baania (https://gobestimate.com/data)
2. Mapbox maps : Plotly graphing library (https://plotly.com/python/mapbox-layers/)
3. Font Thai : How to install font (https://colab.research.google.com/drive/1sTdTZx_Cm51mc8OL_QHtehWyO4725sGl#scrollTo=JCcpqQsvSlKs)
