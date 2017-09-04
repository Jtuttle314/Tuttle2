
# PyCity Schools Analysis
### OBSERVED TREND 1: Math and reading scores remain consistent across 9th - 12th grade, though reading scores are higher on average compared to math.
### OBSERVED TREND 2: Less school spending per student appears to yield stronger average scores. Especially in math
### OBSERVED TREND 3: District schools have a stronger average math and reading scores, but Charter schools have a higher passing rate. 


```python
# import modules
import pandas as pd
import numpy as np
```


```python
# read csv files

schools = "schools_complete.csv"
df_schools = pd.read_csv(schools)
df_schools.columns = ['School ID','school','type','students','budget']

students = "students_complete.csv"
df_students = pd.read_csv(students)

# merge csv files
District_info = pd.merge(df_students, df_schools, on="school")
```

# District Summary


```python
#District Summary

#set variables

Total_Schools = df_schools["school"].count()
Total_Students = df_students["name"].count()
Total_Budget = df_schools["budget"].sum()
Average_Math = df_students["math_score"].mean()
Average_Reading = df_students["reading_score"].mean()
Percent_Passing_Math = (df_students[df_students["math_score"] > 70]["math_score"].count())/(df_students["math_score"].count())*100
Percent_Passing_Reading = (df_students[df_students["reading_score"] > 70]["reading_score"].count())/(df_students["reading_score"].count())*100
Percent_Overall_Passing_Rate = (Percent_Passing_Math + Percent_Passing_Reading)/2

#set data frame
District_Summary_raw = pd.DataFrame(
    {"Total Schools": [Total_Schools],
     "Total Students": [Total_Students],
     "Total Budget": [Total_Budget],
     "Average Math": [Average_Math],
     "Average Reading": [Average_Reading],
     "%_Passing Math": [Percent_Passing_Math],
     "%_Passing Reading": [Percent_Passing_Reading],
     "%_Overall Passing Rate": [Percent_Overall_Passing_Rate]})  

District_Summary = District_Summary_raw[["Total Schools","Total Students", "Total Budget", "Average Math", "Average Reading", "%_Passing Math", "%_Passing Reading", "%_Overall Passing Rate"]]


Formatted_District_Summary = District_Summary.style.format({"Total Schools":"{:,}",
                                                            "Total Students":"{:,}",
                                                            "Total Budget":"${:,.2f}",
                                                            "Average Math":"{:.2f}", 
                                                            "Average Reading":"{:.2f}",
                                                            "%_Passing Math":"{:.2f}%", 
                                                            "%_Passing Reading":"{:.2f}%", 
                                                            "%_Overall Passing Rate":"{:.2f}%"})

Formatted_District_Summary

```




<style  type="text/css" >
</style>  
<table id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Schools</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Math</th> 
        <th class="col_heading level0 col4" >Average Reading</th> 
        <th class="col_heading level0 col5" >%_Passing Math</th> 
        <th class="col_heading level0 col6" >%_Passing Reading</th> 
        <th class="col_heading level0 col7" >%_Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864" class="row_heading level0 row0" >0</th> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col0" class="data row0 col0" >15</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col1" class="data row0 col1" >39,170</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col2" class="data row0 col2" >$24,649,428.00</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col3" class="data row0 col3" >78.99</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col4" class="data row0 col4" >81.88</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col5" class="data row0 col5" >72.39%</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col6" class="data row0 col6" >82.97%</td> 
        <td id="T_1f02159a_90c0_11e7_aed8_6036dd4a2864row0_col7" class="data row0 col7" >77.68%</td> 
    </tr></tbody> 
</table> 



# School Summary


```python
#School Summary
School = df_schools.school
School_Type = df_schools.type
Total_Students = df_schools.students
Total_School_Budget = df_schools.budget
Per_Student_Budget = df_schools.groupby("School ID").budget.sum()/df_schools.groupby("School ID").students.sum()
Average_Math_Score = District_info.groupby("School ID")["math_score"].sum()/District_info.students
Average_Reading_Score = District_info.groupby("School ID")["reading_score"].sum()/District_info.students
Percent_Passing_Math = ((District_info[District_info["math_score"] > 70].groupby("School ID")["math_score"].count())/District_info.groupby("School ID")["math_score"].count())*100   
Percent_Passing_Reading = ((District_info[District_info["reading_score"] > 70].groupby("School ID")["reading_score"].count())/District_info.groupby("School ID")["math_score"].count())*100
Percent_Overall_Passing_Rate = (Percent_Passing_Math + Percent_Passing_Reading)/2

School_Summary_raw = pd.DataFrame(
    {"School": df_schools.school,
    "School Type": df_schools.type,
    "Total Students": df_schools.students,
    "Total School Budget": df_schools.budget,
    "Per Student Budget": Per_Student_Budget,
    "Average Math Score": Average_Math_Score,
    "Average Reading Score": Average_Reading_Score,
    "% Passing Math": Percent_Passing_Math,
    "% Passing Reading": Percent_Passing_Reading,
    "% Overall Passing Rate": Percent_Overall_Passing_Rate,
    })

School_Summary= School_Summary_raw[["School", "School Type", "Total Students", "Total School Budget", "Per Student Budget", "Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate", ]]

School_Summary = School_Summary.dropna().set_index("School")

Formatted_School_Summary = School_Summary.style.format({"Total School Budget":"${:,.2f}",
                                                          "Per Student Budget":"${:,.2f}",
                                                          "Average Math Score":"{:.2f}",
                                                          "Average Reading Score":"{:.2f}",
                                                          "% Passing Math":"{:.2f}%",
                                                          "% Passing Reading":"{:.2f}%",
                                                          "% Overall Passing Rate":"{:.2f}%", })                                                 
Formatted_School_Summary.index.name = None

Formatted_School_Summary
```




<style  type="text/css" >
</style>  
<table id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row0" >Huang High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col0" class="data row0 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col1" class="data row0 col1" >2917</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col2" class="data row0 col2" >$1,910,635.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col3" class="data row0 col3" >$655.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col4" class="data row0 col4" >76.63</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col5" class="data row0 col5" >81.18</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col6" class="data row0 col6" >63.32%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col7" class="data row0 col7" >78.81%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row0_col8" class="data row0 col8" >71.07%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row1" >Figueroa High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col0" class="data row1 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col1" class="data row1 col1" >2949</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col2" class="data row1 col2" >$1,884,411.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col3" class="data row1 col3" >$639.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col4" class="data row1 col4" >77.55</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col5" class="data row1 col5" >82.05</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col6" class="data row1 col6" >63.75%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col7" class="data row1 col7" >78.43%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row1_col8" class="data row1 col8" >71.09%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row2" >Shelton High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col1" class="data row2 col1" >1761</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col2" class="data row2 col2" >$1,056,600.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col3" class="data row2 col3" >$600.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col4" class="data row2 col4" >50.32</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col5" class="data row2 col5" >50.55</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col6" class="data row2 col6" >89.89%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col7" class="data row2 col7" >92.62%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row2_col8" class="data row2 col8" >91.25%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row3" >Hernandez High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col0" class="data row3 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col1" class="data row3 col1" >4635</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col2" class="data row3 col2" >$3,022,020.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col3" class="data row3 col3" >$652.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col4" class="data row3 col4" >122.81</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col5" class="data row3 col5" >128.60</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col6" class="data row3 col6" >64.75%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col7" class="data row3 col7" >78.19%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row3_col8" class="data row3 col8" >71.47%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col1" class="data row4 col1" >1468</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col2" class="data row4 col2" >$917,500.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col3" class="data row4 col3" >$625.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col4" class="data row4 col4" >41.95</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col5" class="data row4 col5" >42.18</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col6" class="data row4 col6" >89.71%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col7" class="data row4 col7" >93.39%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row4_col8" class="data row4 col8" >91.55%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row5" >Wilson High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col0" class="data row5 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col1" class="data row5 col1" >2283</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col2" class="data row5 col2" >$1,319,574.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col3" class="data row5 col3" >$578.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col4" class="data row5 col4" >65.17</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col5" class="data row5 col5" >65.73</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col6" class="data row5 col6" >90.93%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col7" class="data row5 col7" >93.25%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row5_col8" class="data row5 col8" >92.09%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row6" >Cabrera High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col1" class="data row6 col1" >1858</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col2" class="data row6 col2" >$1,081,356.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col3" class="data row6 col3" >$582.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col4" class="data row6 col4" >52.91</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col5" class="data row6 col5" >53.49</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col6" class="data row6 col6" >89.56%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col7" class="data row6 col7" >93.86%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row6_col8" class="data row6 col8" >91.71%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row7" >Bailey High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col0" class="data row7 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col1" class="data row7 col1" >4976</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col2" class="data row7 col2" >$3,124,928.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col3" class="data row7 col3" >$628.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col4" class="data row7 col4" >131.43</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col5" class="data row7 col5" >138.23</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col6" class="data row7 col6" >64.63%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col7" class="data row7 col7" >79.30%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row7_col8" class="data row7 col8" >71.97%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row8" >Holden High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col0" class="data row8 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col1" class="data row8 col1" >427</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col2" class="data row8 col2" >$248,087.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col3" class="data row8 col3" >$581.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col4" class="data row8 col4" >12.27</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col5" class="data row8 col5" >12.27</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col6" class="data row8 col6" >90.63%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col7" class="data row8 col7" >92.74%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row8_col8" class="data row8 col8" >91.69%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col0" class="data row9 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col1" class="data row9 col1" >962</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col2" class="data row9 col2" >$585,858.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col3" class="data row9 col3" >$609.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col4" class="data row9 col4" >27.65</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col5" class="data row9 col5" >27.72</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col6" class="data row9 col6" >91.68%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col7" class="data row9 col7" >92.20%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row9_col8" class="data row9 col8" >91.94%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row10" >Wright High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col0" class="data row10 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col1" class="data row10 col1" >1800</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col2" class="data row10 col2" >$1,049,400.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col3" class="data row10 col3" >$583.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col4" class="data row10 col4" >51.64</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col5" class="data row10 col5" >51.81</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col6" class="data row10 col6" >90.28%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col7" class="data row10 col7" >93.44%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row10_col8" class="data row10 col8" >91.86%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row11" >Rodriguez High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col0" class="data row11 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col1" class="data row11 col1" >3999</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col2" class="data row11 col2" >$2,547,363.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col3" class="data row11 col3" >$637.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col4" class="data row11 col4" >105.35</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col5" class="data row11 col5" >110.70</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col6" class="data row11 col6" >64.07%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col7" class="data row11 col7" >77.74%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row11_col8" class="data row11 col8" >70.91%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row12" >Johnson High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col0" class="data row12 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col1" class="data row12 col1" >4761</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col2" class="data row12 col2" >$3,094,650.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col3" class="data row12 col3" >$650.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col4" class="data row12 col4" >125.79</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col5" class="data row12 col5" >132.15</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col6" class="data row12 col6" >63.85%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col7" class="data row12 col7" >78.28%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row12_col8" class="data row12 col8" >71.07%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row13" >Ford High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col0" class="data row13 col0" >District</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col1" class="data row13 col1" >2739</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col2" class="data row13 col2" >$1,763,916.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col3" class="data row13 col3" >$644.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col4" class="data row13 col4" >72.40</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col5" class="data row13 col5" >75.82</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col6" class="data row13 col6" >65.75%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col7" class="data row13 col7" >77.51%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row13_col8" class="data row13 col8" >71.63%</td> 
    </tr>    <tr> 
        <th id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864" class="row_heading level0 row14" >Thomas High School</th> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col0" class="data row14 col0" >Charter</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col1" class="data row14 col1" >1635</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col2" class="data row14 col2" >$1,043,130.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col3" class="data row14 col3" >$638.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col4" class="data row14 col4" >46.76</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col5" class="data row14 col5" >47.00</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col6" class="data row14 col6" >90.21%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col7" class="data row14 col7" >92.91%</td> 
        <td id="T_1f256bd2_90c0_11e7_9451_6036dd4a2864row14_col8" class="data row14 col8" >91.56%</td> 
    </tr></tbody> 
</table> 



# Top Performing Schools (By Passing Rate)


```python
#Top Performing Schools (By Passing Rate)
School_Summary_Top = School_Summary.sort_values("% Overall Passing Rate",ascending=False).head()

Formatted_School_Summary_Top = School_Summary_Top.style.format({"Total School Budget":"${:,.2f}",
                                                              "Per Student Budget":"${:,.2f}",
                                                              "Average Math Score":"{:.2f}",
                                                              "Average Reading Score":"{:.2f}",
                                                              "% Passing Math":"{:.2f}%",
                                                              "% Passing Reading":"{:.2f}%",
                                                              "% Overall Passing Rate":"{:.2f}%", })                                                 
Formatted_School_Summary_Top.index.name = None

Formatted_School_Summary_Top
```




<style  type="text/css" >
</style>  
<table id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864" class="row_heading level0 row0" >Wilson High School</th> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col0" class="data row0 col0" >Charter</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col1" class="data row0 col1" >2283</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col2" class="data row0 col2" >$1,319,574.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col3" class="data row0 col3" >$578.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col4" class="data row0 col4" >65.17</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col5" class="data row0 col5" >65.73</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col6" class="data row0 col6" >90.93%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col7" class="data row0 col7" >93.25%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row0_col8" class="data row0 col8" >92.09%</td> 
    </tr>    <tr> 
        <th id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864" class="row_heading level0 row1" >Pena High School</th> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col1" class="data row1 col1" >962</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col2" class="data row1 col2" >$585,858.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col3" class="data row1 col3" >$609.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col4" class="data row1 col4" >27.65</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col5" class="data row1 col5" >27.72</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col6" class="data row1 col6" >91.68%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col7" class="data row1 col7" >92.20%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row1_col8" class="data row1 col8" >91.94%</td> 
    </tr>    <tr> 
        <th id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864" class="row_heading level0 row2" >Wright High School</th> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col1" class="data row2 col1" >1800</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col2" class="data row2 col2" >$1,049,400.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col3" class="data row2 col3" >$583.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col4" class="data row2 col4" >51.64</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col5" class="data row2 col5" >51.81</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col6" class="data row2 col6" >90.28%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col7" class="data row2 col7" >93.44%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row2_col8" class="data row2 col8" >91.86%</td> 
    </tr>    <tr> 
        <th id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864" class="row_heading level0 row3" >Cabrera High School</th> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col0" class="data row3 col0" >Charter</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col1" class="data row3 col1" >1858</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col2" class="data row3 col2" >$1,081,356.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col3" class="data row3 col3" >$582.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col4" class="data row3 col4" >52.91</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col5" class="data row3 col5" >53.49</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col6" class="data row3 col6" >89.56%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col7" class="data row3 col7" >93.86%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row3_col8" class="data row3 col8" >91.71%</td> 
    </tr>    <tr> 
        <th id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864" class="row_heading level0 row4" >Holden High School</th> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col1" class="data row4 col1" >427</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col2" class="data row4 col2" >$248,087.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col3" class="data row4 col3" >$581.00</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col4" class="data row4 col4" >12.27</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col5" class="data row4 col5" >12.27</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col6" class="data row4 col6" >90.63%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col7" class="data row4 col7" >92.74%</td> 
        <td id="T_1f2e0cdc_90c0_11e7_9acf_6036dd4a2864row4_col8" class="data row4 col8" >91.69%</td> 
    </tr></tbody> 
</table> 



# Bottom Performing Schools (By Passing Rate)


```python
#Bottom Performing Schools (By Passing Rate)
School_Summary_Bottom = School_Summary.sort_values("% Overall Passing Rate",ascending=True).head()

Formatted_School_Summary_Bottom = School_Summary_Bottom.style.format({"Total School Budget":"${:,.2f}",
                                                              "Per Student Budget":"${:,.2f}",
                                                              "Average Math Score":"{:.2f}",
                                                              "Average Reading Score":"{:.2f}",
                                                              "% Passing Math":"{:.2f}%",
                                                              "% Passing Reading":"{:.2f}%",
                                                              "% Overall Passing Rate":"{:.2f}%", })                                                 
Formatted_School_Summary_Bottom.index.name = None

Formatted_School_Summary_Bottom
```




<style  type="text/css" >
</style>  
<table id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864" class="row_heading level0 row0" >Rodriguez High School</th> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col0" class="data row0 col0" >District</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col1" class="data row0 col1" >3999</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col2" class="data row0 col2" >$2,547,363.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col3" class="data row0 col3" >$637.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col4" class="data row0 col4" >105.35</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col5" class="data row0 col5" >110.70</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col6" class="data row0 col6" >64.07%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col7" class="data row0 col7" >77.74%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row0_col8" class="data row0 col8" >70.91%</td> 
    </tr>    <tr> 
        <th id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864" class="row_heading level0 row1" >Huang High School</th> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col0" class="data row1 col0" >District</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col1" class="data row1 col1" >2917</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col2" class="data row1 col2" >$1,910,635.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col3" class="data row1 col3" >$655.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col4" class="data row1 col4" >76.63</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col5" class="data row1 col5" >81.18</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col6" class="data row1 col6" >63.32%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col7" class="data row1 col7" >78.81%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row1_col8" class="data row1 col8" >71.07%</td> 
    </tr>    <tr> 
        <th id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864" class="row_heading level0 row2" >Johnson High School</th> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col0" class="data row2 col0" >District</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col1" class="data row2 col1" >4761</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col2" class="data row2 col2" >$3,094,650.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col3" class="data row2 col3" >$650.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col4" class="data row2 col4" >125.79</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col5" class="data row2 col5" >132.15</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col6" class="data row2 col6" >63.85%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col7" class="data row2 col7" >78.28%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row2_col8" class="data row2 col8" >71.07%</td> 
    </tr>    <tr> 
        <th id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864" class="row_heading level0 row3" >Figueroa High School</th> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col0" class="data row3 col0" >District</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col1" class="data row3 col1" >2949</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col2" class="data row3 col2" >$1,884,411.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col3" class="data row3 col3" >$639.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col4" class="data row3 col4" >77.55</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col5" class="data row3 col5" >82.05</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col6" class="data row3 col6" >63.75%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col7" class="data row3 col7" >78.43%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row3_col8" class="data row3 col8" >71.09%</td> 
    </tr>    <tr> 
        <th id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864" class="row_heading level0 row4" >Hernandez High School</th> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col0" class="data row4 col0" >District</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col1" class="data row4 col1" >4635</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col2" class="data row4 col2" >$3,022,020.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col3" class="data row4 col3" >$652.00</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col4" class="data row4 col4" >122.81</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col5" class="data row4 col5" >128.60</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col6" class="data row4 col6" >64.75%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col7" class="data row4 col7" >78.19%</td> 
        <td id="T_1f3fc35e_90c0_11e7_a054_6036dd4a2864row4_col8" class="data row4 col8" >71.47%</td> 
    </tr></tbody> 
</table> 



# Math Scores by Grade


```python
#Math scores

#by grade
Ninth_scores = df_students[(df_students["grade"] == "9th")]
Tenth_scores = df_students[(df_students["grade"] == "10th")] 
Eleventh_scores = df_students[(df_students["grade"] == "11th")] 
Twelfth_scores = df_students[(df_students["grade"] == "12th")]

#by school
Ninth = Ninth_scores.groupby([df_students.school]).mean()["math_score"]
Tenth = Tenth_scores.groupby([df_students.school]).mean()["math_score"]
Eleventh = Eleventh_scores.groupby([df_students.school]).mean()["math_score"]
Twelfth = Twelfth_scores.groupby([df_students.school]).mean()["math_score"]


Math_Score_Summary = pd.DataFrame(
    {"9th": Ninth,
    "10th": Tenth,
    "11th": Eleventh,
    "12th": Twelfth,
    })

Math_Score_Summary = Math_Score_Summary[["9th", "10th", "11th", "12th",]]
Math_Score_Summary.index.name = None

Math_Score_Summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



# Reading Scores by Grade


```python
#Reading scores

#by grade
Ninth_scores = df_students[(df_students["grade"] == "9th")]
Tenth_scores = df_students[(df_students["grade"] == "10th")] 
Eleventh_scores = df_students[(df_students["grade"] == "11th")] 
Twelfth_scores = df_students[(df_students["grade"] == "12th")]

#by school
Ninth = Ninth_scores.groupby([df_students.school]).mean()["reading_score"]
Tenth = Tenth_scores.groupby([df_students.school]).mean()["reading_score"]
Eleventh = Eleventh_scores.groupby([df_students.school]).mean()["reading_score"]
Twelfth = Twelfth_scores.groupby([df_students.school]).mean()["reading_score"]


Reading_Score_Summary = pd.DataFrame(
    {"9th": Ninth,
    "10th": Tenth,
    "11th": Eleventh,
    "12th": Twelfth,
    })

Reading_Score_Summary = Reading_Score_Summary[["9th", "10th", "11th", "12th",]]
Reading_Score_Summary.index.name = None

Reading_Score_Summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending


```python
#Scores by School Spending
 
School_Spending_Bins = [0, 585, 615, 645, 675]
Bin_Names = ["<$585", "$585-615", "$615-645", "$645-675"]

School_Summary["Spending Ranges (Per Student)"] = pd.cut(Per_Student_Budget, School_Spending_Bins, labels=Bin_Names)

School_Spending_Average_Math = School_Summary.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]
School_Spending_Average_Reading = School_Summary.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]
School_Spending_Percent_Passing_Math = School_Summary.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]
School_Spending_Percent_Passing_Reading = School_Summary.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]
School_Spending_Percent_Overall_Passing_Rate = (School_Spending_Percent_Passing_Math + School_Spending_Percent_Passing_Reading)/ 2

#set data frame
School_Spending_Summary = pd.DataFrame({"Average Math": School_Spending_Average_Math,
                                        "Average Reading": School_Spending_Average_Reading,
                                        "% Passing Math": School_Spending_Percent_Passing_Math,
                                        "% Passing Reading": School_Spending_Percent_Passing_Reading,
                                        "% Overall Passing Rate": School_Spending_Percent_Overall_Passing_Rate})


School_Spending_Summary = School_Spending_Summary[["Average Math", "Average Reading", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]

School_Spending_Summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math</th>
      <th>Average Reading</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>$585-615</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>&lt;$585</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Size


```python
       #Scores by School Spending
 
School_Size_Bins = [0, 1000, 2000, 5000]
Size_Bin_Names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

School_Size_Summary["School Size"] = pd.cut(df_schools["students"], School_Size_Bins, labels=Size_Bin_Names)

size_math_scores = School_Size_Summary.groupby(["School size"]).mean()["Average Math Score"]
size_reading_scores = School_Size_Summary.groupby(["School size"]).mean()["Average Reading Score"]
size_pathing_math = School_Size_Summary.groupby(["School size"]).mean()["% passing math"]                                             
size_pathing_reading = School_Size_Summary.groupby(["School size"]).mean()["% passing reading"]
overall_passing_rate = (size_passing_math + size_passing_reading)/2

size_summary = pd.DataFrame({"Average Math Score": size_math_scores,
                             "Average Reading Score": size_reading_scores,
                             "% Passing Math": size_passing_math,
                             "% Passing Reading": size_passing_reading,
                             "% Overall Passing": overall_passing_rate,
                            })
                                               
                                               
size_summary = size_summary({"Average Reading Score",
                             "% Passing Math",
                             "% Passing Reading", 
                             "% Overall Passing", 
                            })
size_summary                              
```

# Scores by School Type


```python
School_Summary = School_Summary.pivot_table(index="School Type",values=["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"])
School_Summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>91.708019</td>
      <td>90.363226</td>
      <td>93.052812</td>
      <td>43.583091</td>
      <td>43.842604</td>
    </tr>
    <tr>
      <th>District</th>
      <td>71.313543</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>101.709290</td>
      <td>106.961360</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```
