# Use this cell to set up import statements for all of the packages that you
#   plan to use.
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Remember to include a 'magic word' so that your visualizations are plotted
#   inline with the notebook. See this page for more:
#   http://ipython.readthedocs.io/en/stable/interactive/magics.html
# Upgrade pandas to use dataframe.explode() function. 
!pip install --upgrade pandas==0.25.0



Data Wrangling¶
Tip: In this section of the report, you will load in the data, check for cleanliness, and then trim and clean your dataset for analysis. Make sure that you document your data cleaning steps in mark-down cells precisely and justify your cleaning decisions.
General Properties¶
Tip: You should not perform too many operations in each cell. Create cells freely to explore your data. One option that you can take with this project is to do a lot of explorations in an initial notebook. These don't have to be organized, but make sure you use enough comments to understand the purpose of each code cell. Then, after you're done with your analysis, create a duplicate notebook where you will trim the excess and organize your steps so that you have a flowing, cohesive report.


In [66]:



# Load your data and print out a few lines. Perform operations to inspect data
df = pd.read_csv("noshowappointments.csv")
df.head()
#   types and look for instances of missing or possibly errant data



PatientId
AppointmentID
Gender
ScheduledDay
AppointmentDay
Age
Neighbourhood
Scholarship
Hipertension
Diabetes
Alcoholism
Handcap
SMS_received
No-show
 
0
2.987250e+13
5642903
F
2016-04-29T18:38:08Z
2016-04-29T00:00:00Z
62
JARDIM DA PENHA
0
1
0
0
0
0
No
1
5.589978e+14
5642503
M
2016-04-29T16:08:27Z
2016-04-29T00:00:00Z
56
JARDIM DA PENHA
0
0
0
0
0
0
No
2
4.262962e+12
5642549
F
2016-04-29T16:19:04Z
2016-04-29T00:00:00Z
62
MATA DA PRAIA
0
0
0
0
0
0
No
3
8.679512e+11
5642828
F
2016-04-29T17:29:31Z
2016-04-29T00:00:00Z
8
PONTAL DE CAMBURI
0
0
0
0
0
0
No
4
8.841186e+12
5642494
F
2016-04-29T16:07:23Z
2016-04-29T00:00:00Z
56
JARDIM DA PENHA
0
1
1
0
0
0
No



In [67]:



#know more about  our data
df.info()






<class 'pandas.core.frame.DataFrame'>
RangeIndex: 110527 entries, 0 to 110526
Data columns (total 14 columns):
PatientId         110527 non-null float64
AppointmentID     110527 non-null int64
Gender            110527 non-null object
ScheduledDay      110527 non-null object
AppointmentDay    110527 non-null object
Age               110527 non-null int64
Neighbourhood     110527 non-null object
Scholarship       110527 non-null int64
Hipertension      110527 non-null int64
Diabetes          110527 non-null int64
Alcoholism        110527 non-null int64
Handcap           110527 non-null int64
SMS_received      110527 non-null int64
No-show           110527 non-null object
dtypes: float64(1), int64(8), object(5)
memory usage: 11.8+ MB




In [68]:



#finding any missing data

df.isnull().sum()





Out[68]:

PatientId         0
AppointmentID     0
Gender            0
ScheduledDay      0
AppointmentDay    0
Age               0
Neighbourhood     0
Scholarship       0
Hipertension      0
Diabetes          0
Alcoholism        0
Handcap           0
SMS_received      0
No-show           0
dtype: int64


In [69]:



#finding any duplicated rows 
df.duplicated().sum()





Out[69]:

0


In [70]:



df.nunique()





Out[70]:

PatientId          62299
AppointmentID     110527
Gender                 2
ScheduledDay      103549
AppointmentDay        27
Age                  104
Neighbourhood         81
Scholarship            2
Hipertension           2
Diabetes               2
Alcoholism             2
Handcap                5
SMS_received           2
No-show                2
dtype: int64


In [71]:



#some statistical analysis 
df.describe()





Out[71]:



PatientId
AppointmentID
Age
Scholarship
Hipertension
Diabetes
Alcoholism
Handcap
SMS_received
 
count
1.105270e+05
1.105270e+05
110527.000000
110527.000000
110527.000000
110527.000000
110527.000000
110527.000000
110527.000000
mean
1.474963e+14
5.675305e+06
37.088874
0.098266
0.197246
0.071865
0.030400
0.022248
0.321026
std
2.560949e+14
7.129575e+04
23.110205
0.297675
0.397921
0.258265
0.171686
0.161543
0.466873
min
3.921784e+04
5.030230e+06
-1.000000
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
25%
4.172614e+12
5.640286e+06
18.000000
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
50%
3.173184e+13
5.680573e+06
37.000000
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
75%
9.439172e+13
5.725524e+06
55.000000
0.000000
0.000000
0.000000
0.000000
0.000000
1.000000
max
9.999816e+14
5.790484e+06
115.000000
1.000000
1.000000
1.000000
1.000000
4.000000
1.000000




from the previous we conclude :
1. no duplicates row
2. no unique vallues 
3. no missing data


but there is some issues like:
1. there is a negative age value 
2. coulmns names



Data Cleaning¶
lets clean our data from issues that we find


In [72]:



#columns should be in lowercase & separeted with underscor (_)

df.rename(columns = lambda x: x.strip().lower().replace("-","_"),inplace = True)
df.head()





Out[72]:



patientid
appointmentid
gender
scheduledday
appointmentday
age
neighbourhood
scholarship
hipertension
diabetes
alcoholism
handcap
sms_received
no_show
 
0
2.987250e+13
5642903
F
2016-04-29T18:38:08Z
2016-04-29T00:00:00Z
62
JARDIM DA PENHA
0
1
0
0
0
0
No
1
5.589978e+14
5642503
M
2016-04-29T16:08:27Z
2016-04-29T00:00:00Z
56
JARDIM DA PENHA
0
0
0
0
0
0
No
2
4.262962e+12
5642549
F
2016-04-29T16:19:04Z
2016-04-29T00:00:00Z
62
MATA DA PRAIA
0
0
0
0
0
0
No
3
8.679512e+11
5642828
F
2016-04-29T17:29:31Z
2016-04-29T00:00:00Z
8
PONTAL DE CAMBURI
0
0
0
0
0
0
No
4
8.841186e+12
5642494
F
2016-04-29T16:07:23Z
2016-04-29T00:00:00Z
56
JARDIM DA PENHA
0
1
1
0
0
0
No



In [73]:



#remove negative value for age columns 

df.drop(df[df["age"]< 0].index, inplace= True)
df.describe()





Out[73]:



patientid
appointmentid
age
scholarship
hipertension
diabetes
alcoholism
handcap
sms_received
 
count
1.105260e+05
1.105260e+05
110526.000000
110526.000000
110526.000000
110526.000000
110526.000000
110526.000000
110526.000000
mean
1.474934e+14
5.675304e+06
37.089219
0.098266
0.197248
0.071865
0.030400
0.022248
0.321029
std
2.560943e+14
7.129544e+04
23.110026
0.297676
0.397923
0.258266
0.171686
0.161543
0.466874
min
3.921784e+04
5.030230e+06
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
25%
4.172536e+12
5.640285e+06
18.000000
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
50%
3.173184e+13
5.680572e+06
37.000000
0.000000
0.000000
0.000000
0.000000
0.000000
0.000000
75%
9.438963e+13
5.725523e+06
55.000000
0.000000
0.000000
0.000000
0.000000
0.000000
1.000000
max
9.999816e+14
5.790484e+06
115.000000
1.000000
1.000000
1.000000
1.000000
4.000000
1.000000



In [74]:



#rename columns 

df.rename(columns = {"hipertension":"hypertension"},inplace= True)






Exploratory Data Analysis¶
Research Question 1 (what percnetage of patient skipped their appointments ?!)¶


In [75]:



#general view of data 

df.hist(figsize=(10,10));




In [76]:



# lets get percentage of patient showed up their appointments
counts = df.no_show.value_counts()

print(counts,"\n") 

counts.plot(kind="pie", autopct = "%1.1f%%", title ="percentage of patient showed up their appointments")






No     88207
Yes    22319
Name: no_show, dtype: int64 




Out[76]:

<matplotlib.axes._subplots.AxesSubplot at 0x7fe9648631d0>



we find that 20% of patients skipped their appointment , lets try to know the reasson about that



Research Question 2 (who cares more about their health male or female ?!)¶


In [77]:



# percentage of gender (male & female)

gen_show = df.groupby(["no_show","gender"], as_index =False).size()
print(gen_show ,"\n")
gen_show.plot(kind= "pie", autopct = "%1.1f%%" , title ="percentage of male & female showed their appointments")






no_show  gender
No       F         57245
         M         30962
Yes      F         14594
         M          7725
dtype: int64 




Out[77]:

<matplotlib.axes._subplots.AxesSubplot at 0x7fe96426c978>



we find that percntage of female that skipped their appointments is higher than male, but percntage of female that attend is more than male . in the end female are more committed to attending



Research Question 3 (there is relation between disease & attending the appointments?!¶


In [61]:



# diabetes no diabetes = 0 , have diabetes = 1

diabet = df.groupby(["diabetes","no_show"], as_index =False).size()
print(diabet, "\n")

diabet.plot(kind ="pie" , autopct ="%1.1f%%" , title ="diabetes & attending the appointment")






diabetes  no_show
0         No         81694
          Yes        20889
1         No          6513
          Yes         1430
dtype: int64 




Out[61]:

<matplotlib.axes._subplots.AxesSubplot at 0x7fe96463a940>



from this analysis , we see the patient that do not have diabetes are higher than patient have dibetes and skipped their appointments.so, there is no relation between having diabetes & attending appoinments



# hypertension 
bp_disease = df.groupby(["hypertension","no_show"], as_index =False).size()

print(bp_disease)

bp_disease.plot(kind = "pie", autopct = "%1.1f%%", title =" hypertension & attending the appiontment")

hypertension  no_show
0             No         70178
              Yes        18547
1             No         18029
              Yes         3772
dtype: int64


<matplotlib.axes._subplots.AxesSubplot at 0x7fe96228afd0>



the percentage of patients who attended the appointment and do not suffer from hypertension is high . on the other hand we find a 16.8% of patients who attended the appointment & have hypertension . so, it is possible that is a realation between hypertension & attending the appointments

Research Question 4 ( there is relation between Age & show up the appointment?¶

#view relation between age & no_show
plt.figure(figsize= (20,5))
sns.countplot(x= df["age"], hue = df["no_show"])

<matplotlib.axes._subplots.AxesSubplot at 0x7fe9642d5438>

we find age less than 1 year has a high rate than other ages . on the other hand the older (age higher than 70 )have low rate to attend their appointments

Conclusions¶
Finally, from the previous, we can see the factors affecting attendance and non-attendanc the appointment like age , gender specialy femal more than male , some disease such as hypertension have an impact factor
Limitations¶
we found some errors in the beginning like :
1. negative value in column age 
2. some columns named not correct 
from subprocess import call
call(['python', '-m', 'nbconvert', 'Investigate_a_Dataset.ipynb'])



