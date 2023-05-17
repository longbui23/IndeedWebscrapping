# Data 200: Database Systems and Data Management for Data Analytics


# Name: Long Bui

# Take Home Final Exam
<font color='red'>**Due Date:** May 16, 12pm (noon) </font>
---

Task: Scrape data from a job search website


Website: Indeed.com

Objective: Collect job postings that match certain keywords and location filters, and then perform data analysis to extract insights about the job market.

Instructions:

1. Choose a location and a set of keywords that are relevant to your field of study. For example, you can schoose a field such as data science, computer science, or any other field you are interested in. In terms of the location, you might choose to search for "data analytics" jobs in "San Francisco". You are free to choose the field and location on this assignment.
2. Scrape job postings from Indeed.com using selenium and python. Your code should extract the following information for each job posting:
- Job title
- Company name
- Job description
- Job location
- Date posted
3. Clean and preprocess the data as necessary. You may want to remove duplicates and perform other data cleaning tasks to prepare the data for analysis.
4. Use data analysis techniques to extract insights about the job market. For example, you might:
- Identify which companies are hiring the most for the given job titles and locations.
- Determine the distribution of job titles and their average salaries.
- Analyze the frequency of certain keywords in job descriptions, and determine which skills and qualifications are most in demand.
5. Write a report summarizing your findings. Your report should include tables and visualizations to support your conclusions, and should provide actionable insights that could be used by job seekers, employers, or policymakers.
Submit your code and report as a single .ipynb file (you can do it in this current notebook as a combination of code cells and markdown cells), along with any necessary instructions for running your code. Make sure your code is well-documented and organized, and that your report is well-written and easy to follow. <br> <br>
Note: You do not need to perform text analysis or create word clouds for this exam. However, if you are interested in learning more about these techniques, you may want to explore them on your own as a side project.
<br><br>
Here is the rubric that I will use to grade your final exam:

| Item                        | Weight |
|-----------------------------|--------|
| Code accuracy               | 25%    |
| Code clarity and annotation | 25%    |
| Exploratory data analysis   | 25%    |
| Discussion of findings      | 25%    |

<br>
Good luck!


# 1) Introduction

This project will scrape posts from the web: indeed.com to analyze and visualize the situation of the current employment market for data engineer in the US. This include:

        +Job title: specific title for the job in hiring (Ex: Senior/ Junior Data Engineer, Management Data Engineer,...)
        +Company: The company name that post the hiring news
        +Job Description:Describing the tasks that required for the job
        +Job location: where is the job in hiring (specific location or remote)
        +Date Posted: What date does the hiring post was uploaded

# 2) Data Scrapping


```python
#import packages + regex
#webdriver
from selenium import webdriver
import re
import requests

#additional packages
import time
import random
import numpy as np

#data manipulation packages
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
#define methods
def get_url(job_title, location, page):
    '''Generate a url from position and location'''
    
    #get url
    template = 'https://www.indeed.com/jobs?q={}&l={}%start={}'
    url = template.format(job_title, location, page)
    response = requests.get(url)
    
    return url
    
    
def get_job_info(jobs):
    '''get the job attributes'''
    
    #15 jobs per page
    for i in range(1,18):
        
        #find job's attributes
        #job title
        try:
            job_title = driver.find_element('xpath',\
                            '//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[1]/h2'\
                                                   .format(i)).text
        except:
            job_title = "N/A"
     
        #compnay
        try:
            company = driver.find_element('xpath',\
                            '//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[2]/span[1]'\
                                                   .format(i)).text
        except:
            company = "N/A"
            
        #short description
        try:
            description = driver.find_element('xpath',\
                            '//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[2]/tbody/tr[2]/td/div[1]/div'\
                                                   .format(i)).text
        except:
            description = "N/A"
            
        #location
        try:
            location = driver.find_element('xpath',\
                            '//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[2]/div[1]'\
                                                   .format(i)).text
        except:
            location = "N/A"
        
        #date_posted
        try:
            date_posted = driver.find_element('xpath',\
                            '//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[2]/tbody/tr[2]/td/div/span'\
                                                   .format(i)).text
        except:
            date_posted = "N/A"
        
        #append
        jobs.append([job_title, company, description, location, date_posted])
     
    #return info
    return jobs
```


```python
# Set the URL of Indeed
url = "https://www.indeed.com/"

# Set the job title and location to search for
job_title = "data+engineer"
location = ""
```


```python
#create a list
jobs = []

#loops through pages
for page in range(0, 991, 10):
    
    #using web-drivers
    print(page)
    driver = webdriver.Chrome("C:/Users/Dell/Downloads/chromedriver.exe")
    url = get_url(job_title, location, 1)
    driver.get(url)
    
    #get job's information in that page
    get_job_info(jobs)

    #quit driver
    driver.quit() 
    
    #random time sleep
    time.sleep(random.uniform(2,3))
```

    0
    

    C:\Users\Dell\AppData\Local\Temp\ipykernel_26408\3705288331.py:9: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver = webdriver.Chrome("C:/Users/Dell/Downloads/chromedriver.exe")
    

    10
    20
    30
    40
    50
    60
    70
    80
    90
    100
    110
    120
    130
    140
    150
    160
    170
    180
    190
    200
    210
    220
    230
    240
    250
    260
    270
    280
    290
    300
    310
    320
    330
    340
    350
    360
    370
    380
    390
    400
    410
    420
    430
    440
    450
    460
    470
    480
    490
    500
    510
    520
    530
    540
    550
    560
    570
    580
    590
    600
    610
    620
    630
    640
    650
    660
    670
    680
    690
    700
    710
    720
    730
    740
    750
    760
    770
    780
    790
    800
    810
    820
    830
    840
    850
    860
    870
    880
    890
    900
    910
    920
    930
    940
    950
    960
    970
    980
    990
    


```python
#convert the data acquired to a pd-dataframe
jobs_df = pd.DataFrame(jobs, columns=['title','company','description','location','date_posted'])
jobs_df
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
      <th>title</th>
      <th>company</th>
      <th>description</th>
      <th>location</th>
      <th>date_posted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Data Engineer</td>
      <td>Codazen</td>
      <td>Collaborate with software engineers to create ...</td>
      <td>Hybrid remote in Irvine, CA</td>
      <td>Posted\nPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Research Engineer (Software/Data Science)</td>
      <td>Voltserver</td>
      <td>Skill in working with large data sets.\nStrong...</td>
      <td>East Greenwich, RI 02818</td>
      <td>Employer\nActive 9 days ago</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Big Data Full Stack Software Engineer (Top Sec...</td>
      <td>Reinventing Geospatial, Inc. (RGi)</td>
      <td>Rapidly prototype new methods for processing a...</td>
      <td>Hybrid remote in Herndon, VA</td>
      <td>Posted\nPosted 17 days ago</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cloud Data Engineer</td>
      <td>Deloitte</td>
      <td>2+years of data engineering and architecture e...</td>
      <td>Rosslyn, VA 22209 \n(Radnor-Ft Myer Heights area)</td>
      <td>Posted\nPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Senior Cloud Data Engineer</td>
      <td>Wis Phys Svc Ins Corp</td>
      <td>Experience developing new data platforms in th...</td>
      <td>Remote in Madison, WI 53713</td>
      <td>Posted\nPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1695</th>
      <td>Azure Data Engineer</td>
      <td>Integration Developer Network LLC</td>
      <td>Proven expertise with extracting data from a w...</td>
      <td>Remote</td>
      <td>Posted\nPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>1696</th>
      <td>Data Engineer with Cosmos DB or Cassandra DB</td>
      <td>Aptiva Healthcare</td>
      <td>Collaborate with data analysts and scientists ...</td>
      <td>Remote</td>
      <td>Posted\nPosted 4 days ago</td>
    </tr>
    <tr>
      <th>1697</th>
      <td>Data Engineer</td>
      <td>N9 IT SOLUTIONS</td>
      <td>Experience in the Microsoft Azure stack of dat...</td>
      <td>Remote</td>
      <td>Employer\nActive 8 days ago</td>
    </tr>
    <tr>
      <th>1698</th>
      <td>Data Engineer</td>
      <td>PSRTEK</td>
      <td>Please look for profile who worked as a Java d...</td>
      <td>Remote</td>
      <td>Employer\nActive 18 days ago</td>
    </tr>
    <tr>
      <th>1699</th>
      <td>Junior Health Data Management Engineer</td>
      <td>ThunderYard</td>
      <td>Evaluate and integrate data from multiple sour...</td>
      <td>Remote</td>
      <td>Posted\nPosted 4 days ago</td>
    </tr>
  </tbody>
</table>
<p>1700 rows × 5 columns</p>
</div>



# 2) Data Cleaning

Since for all the null data have been replaced with "N/A" in the scapping process, we does not need to inspect it anymore but let see a short description of our jobs_df.


```python
jobs_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1700 entries, 0 to 1699
    Data columns (total 5 columns):
     #   Column       Non-Null Count  Dtype 
    ---  ------       --------------  ----- 
     0   title        1700 non-null   object
     1   company      1700 non-null   object
     2   description  1700 non-null   object
     3   location     1700 non-null   object
     4   date_posted  1700 non-null   object
    dtypes: object(5)
    memory usage: 66.5+ KB
    

Let's drop down some duplicate rows


```python
jobs_df.drop_duplicates()
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
      <th>title</th>
      <th>company</th>
      <th>description</th>
      <th>location</th>
      <th>date_posted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Data Engineer</td>
      <td>Codazen</td>
      <td>Collaborate with software engineers to create ...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Research Engineer (Software/Data Science)</td>
      <td>Voltserver</td>
      <td>Skill in working with large data sets.\nStrong...</td>
      <td>RI</td>
      <td>9 days ago</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Big Data Full Stack Software Engineer (Top Sec...</td>
      <td>Reinventing Geospatial, Inc. (RGi)</td>
      <td>Rapidly prototype new methods for processing a...</td>
      <td>remote</td>
      <td>17 days ago</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cloud Data Engineer</td>
      <td>Deloitte</td>
      <td>2+years of data engineering and architecture e...</td>
      <td>VA</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Senior Cloud Data Engineer</td>
      <td>Wis Phys Svc Ins Corp</td>
      <td>Experience developing new data platforms in th...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1628</th>
      <td>Senior Data Engineer - SQL, Snowflake</td>
      <td>CyberCoders</td>
      <td>If you are a Senior Data Engineer with excelle...</td>
      <td>remote</td>
      <td>today</td>
    </tr>
    <tr>
      <th>1652</th>
      <td>Data Engineer</td>
      <td>Jobot</td>
      <td>Work with stakeholders to support their data i...</td>
      <td>remote</td>
      <td>1 day ago</td>
    </tr>
    <tr>
      <th>1655</th>
      <td>Data Engineer II - Python</td>
      <td>Vaco</td>
      <td>Additionally, you will work in SQL for data ma...</td>
      <td>remote</td>
      <td>10 days ago</td>
    </tr>
    <tr>
      <th>1666</th>
      <td>ISD Engineer IV-Data Engineering (AI/ML/NLP)</td>
      <td>Navy Federal Credit Union</td>
      <td>Partner with data analysts, data modelers and ...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>1693</th>
      <td>Sr Informatica Data Engineer</td>
      <td>The Judge Group</td>
      <td>Informatica expertise with an emphasis on work...</td>
      <td>PA</td>
      <td>1 day ago</td>
    </tr>
  </tbody>
</table>
<p>310 rows × 5 columns</p>
</div>



The location column looks a little bit messy, let's change it into remote or just the 2 id of the state which the jobs in.


```python
#clean text
for index, place in jobs_df['location'].iteritems():
    list_place = place.lower().split()
    
    #convert remote
    for word in list_place:
        if word == "remote":
           jobs_df.iloc[index, 3] = 'remote'
           finish = True

    while finish == False:
        #convert state to its initial
        try:
            place = re.findall(r'[A-Z]{2}', place)[0]
            jobs_df.iloc[index, 3] = place
        #non_value
        except:
            jobs_df.iloc[index, 3] = "N/A"
            
        #break loop
        finish = True
            
    
    #reset finish value
    finish = False
```


```python
#printing out the data frame
jobs_df.head(5)
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
      <th>title</th>
      <th>company</th>
      <th>description</th>
      <th>location</th>
      <th>date_posted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Data Engineer</td>
      <td>Codazen</td>
      <td>Collaborate with software engineers to create ...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Research Engineer (Software/Data Science)</td>
      <td>Voltserver</td>
      <td>Skill in working with large data sets.\nStrong...</td>
      <td>RI</td>
      <td>9 days ago</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Big Data Full Stack Software Engineer (Top Sec...</td>
      <td>Reinventing Geospatial, Inc. (RGi)</td>
      <td>Rapidly prototype new methods for processing a...</td>
      <td>remote</td>
      <td>17 days ago</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cloud Data Engineer</td>
      <td>Deloitte</td>
      <td>2+years of data engineering and architecture e...</td>
      <td>VA</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Senior Cloud Data Engineer</td>
      <td>Wis Phys Svc Ins Corp</td>
      <td>Experience developing new data platforms in th...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
  </tbody>
</table>
</div>



Sucessfully converting location column! Now changing to the next colu,n we can observe that "\n" still being included because of the scrapping process. Let's strip it out!


```python
for index, date in jobs_df['date_posted'].iteritems():
    
    #Non-value date
    if date == "N/A":
        new_date = ["N/A"]
        
    else:
        #convert for 30+ days
        new_date = re.findall(r'\d+\+? days ago', date)
    
        #convert for 1-30 days
        if new_date == []:
            new_date = re.findall(r'\d+ days ago', date)
            
            #convert 1 day (without s in day)
            if new_date == []:
                new_date  = re.findall(r'\d+ day ago', date)
            
                #convert today
                if new_date == []:
                    new_date = ["today"]
        
    #return date value
    jobs_df.iloc[index, 4] = new_date[0]
    
    #reset new_date value
    new_date = []
```


```python
#printing the data frame
jobs_df.head(5)
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
      <th>title</th>
      <th>company</th>
      <th>description</th>
      <th>location</th>
      <th>date_posted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Data Engineer</td>
      <td>Codazen</td>
      <td>Collaborate with software engineers to create ...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Research Engineer (Software/Data Science)</td>
      <td>Voltserver</td>
      <td>Skill in working with large data sets.\nStrong...</td>
      <td>RI</td>
      <td>9 days ago</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Big Data Full Stack Software Engineer (Top Sec...</td>
      <td>Reinventing Geospatial, Inc. (RGi)</td>
      <td>Rapidly prototype new methods for processing a...</td>
      <td>remote</td>
      <td>17 days ago</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cloud Data Engineer</td>
      <td>Deloitte</td>
      <td>2+years of data engineering and architecture e...</td>
      <td>VA</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Senior Cloud Data Engineer</td>
      <td>Wis Phys Svc Ins Corp</td>
      <td>Experience developing new data platforms in th...</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
  </tbody>
</table>
</div>



Missions accomplished! Now we have a good data for analyzing.

# 3) Data Analysis & Visualization

Let's inspect summary of this dataframe.


```python
jobs_df.describe()
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
      <th>title</th>
      <th>company</th>
      <th>description</th>
      <th>location</th>
      <th>date_posted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1700</td>
      <td>1700</td>
      <td>1700</td>
      <td>1700</td>
      <td>1700</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>195</td>
      <td>211</td>
      <td>280</td>
      <td>35</td>
      <td>29</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Data Engineer</td>
      <td>N/A</td>
      <td>N/A</td>
      <td>remote</td>
      <td>30+ days ago</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>250</td>
      <td>200</td>
      <td>200</td>
      <td>1006</td>
      <td>321</td>
    </tr>
  </tbody>
</table>
</div>



Such a fascinating information! We are having 195 diffrent types of data engineer are being employed: some will work with the general data structure of the company, some will specialize in using sql to create relational database, some will be database administrator, some will be extremely skillful on tools such as AWS, Haadoop,... 

Let's observe company groupment.


```python
jobs_df.groupby("company").agg({"title":"count"}).sort_values(by="title", ascending=False)
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
      <th>title</th>
    </tr>
    <tr>
      <th>company</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>N/A</th>
      <td>200</td>
    </tr>
    <tr>
      <th>Integration Developer Network LLC</th>
      <td>94</td>
    </tr>
    <tr>
      <th>Aptiva Healthcare</th>
      <td>89</td>
    </tr>
    <tr>
      <th>ThunderYard</th>
      <td>75</td>
    </tr>
    <tr>
      <th>N9 IT SOLUTIONS</th>
      <td>67</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>Epsilon</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Envision</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Edutek, Ltd.</th>
      <td>1</td>
    </tr>
    <tr>
      <th>EPM Scientific</th>
      <td>1</td>
    </tr>
    <tr>
      <th>isolved</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>211 rows × 1 columns</p>
</div>



N/A usually represent hiring jobs through third contacts of HR. Hence, it usually come from small companies. It is interesting to find out that data engineer market are growing in small-industry sector. Below that, most of the companies are hiring for data engineers are tech companies. It is a little bit weird that we does not see Amazon with AWS. Perharps they prefer hiring through Linkedln than Indeed ?

Now, let's observe the location


```python
jobs_df.groupby("location").agg({"title":"count"}).sort_values(by="title", ascending=False).head(10)
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
      <th>title</th>
    </tr>
    <tr>
      <th>location</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>remote</th>
      <td>1006</td>
    </tr>
    <tr>
      <th>N/A</th>
      <td>201</td>
    </tr>
    <tr>
      <th>VA</th>
      <td>56</td>
    </tr>
    <tr>
      <th>PA</th>
      <td>47</td>
    </tr>
    <tr>
      <th>FL</th>
      <td>34</td>
    </tr>
    <tr>
      <th>NY</th>
      <td>33</td>
    </tr>
    <tr>
      <th>NC</th>
      <td>33</td>
    </tr>
    <tr>
      <th>IL</th>
      <td>28</td>
    </tr>
    <tr>
      <th>CA</th>
      <td>25</td>
    </tr>
    <tr>
      <th>TX</th>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



A dominant number of data engineering jobs right now are preferred remote! This is a good new for people who sits in Florida, enjoying the summer, but can still work for big tech companies in Seattle or the Bay! Behind, most of the hirer does not provide their location also. I hope what they also means is remote is elligibe. Furthermore, it it such an interesting that the East Coast are hiring more than the East Coast such as Virgnia, Pennsylvania, FLorida, and New York,... Let's visualize this a little bit.


```python
#countplot
sns.countplot(data=jobs_df, order=jobs_df['location'].value_counts().index, x="location")

#additional figures
plt.xticks(rotation = 90)
plt.title('Jobs hiring per state',  fontsize=40)
plt.ylabel('Number of hiring', fontsize=25)
plt.xlabel('Location', fontsize=25)
plt.show()
```


    
![png](output_34_0.png)
    


Let's inspect job titles


```python
jobs_df["title"].value_counts()
```




    Data Engineer                                       250
    N/A                                                 200
    Senior Data Engineer                                174
    Azure Data Engineer                                 129
    Data Engineer with Cosmos DB or Cassandra DB         89
                                                       ... 
    Data Engineer - Azure Spark (Databricks)              1
    Data Visualization Engineer- TS/SCI w poly            1
    Tableau Data Visualization Engineer                   1
    Data Science & Technology Integration - Engineer      1
    Sr Informatica Data Engineer                          1
    Name: title, Length: 195, dtype: int64



General Data Engineer still be one of the most high-demand jobs in the Data Engineer Market and then, it follows by data engineer who specializes in tools that manage datalakes,... Let's visualize this!


```python
#create df
job_title = pd.DataFrame(jobs_df["title"].value_counts().head(10))

#barplot
sns.barplot(data=job_title, x=job_title.index, y= "title")

#additional figures
plt.xticks(rotation = 90)
plt.title('Jobs hiring by name',  fontsize=40)
plt.ylabel('Number of hiring', fontsize=25)
plt.xlabel('Name', fontsize=25)
plt.show()
```


    
![png](output_38_0.png)
    


Now let's analyze date posting


```python
jobs_df.groupby("date_posted").agg({"title":"count"}).sort_values(by="title", ascending=False).head(10)
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
      <th>title</th>
    </tr>
    <tr>
      <th>date_posted</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30+ days ago</th>
      <td>321</td>
    </tr>
    <tr>
      <th>N/A</th>
      <td>200</td>
    </tr>
    <tr>
      <th>4 days ago</th>
      <td>199</td>
    </tr>
    <tr>
      <th>today</th>
      <td>159</td>
    </tr>
    <tr>
      <th>8 days ago</th>
      <td>107</td>
    </tr>
    <tr>
      <th>10 days ago</th>
      <td>100</td>
    </tr>
    <tr>
      <th>11 days ago</th>
      <td>95</td>
    </tr>
    <tr>
      <th>5 days ago</th>
      <td>88</td>
    </tr>
    <tr>
      <th>9 days ago</th>
      <td>63</td>
    </tr>
    <tr>
      <th>18 days ago</th>
      <td>61</td>
    </tr>
  </tbody>
</table>
</div>



There have been more than 300 jobs that have existed in indeed post for more than 1 months. This means that a the market is running under shortage of labor where many companies cannot find a perfect candidate to operate requiring tasks. Suprisingly, 4 days ago, there are roughly 200 jobs being submitted to indeed. Besides, there are 200 jobs that the poster does not show what is the date it was posting. Today, there are also 159 jobs are posting. Such a postive number for participant in this market.

Now let's visualize this.


```python
#countplot
sns.countplot(data=jobs_df, order=jobs_df['date_posted'].value_counts().index, x="date_posted")

#additional figures
plt.xticks(rotation = 90)
plt.title('Jobs hiring by date',  fontsize=40)
plt.ylabel('Number of hiring', fontsize=25)
plt.xlabel('Location', fontsize=25)
plt.show()
```


    
![png](output_43_0.png)
    


Now let's come to the hardes part: Text analysis. Let's analyze those short description. First, we will try to see what the most popular words in these descriptions


```python
#import word cloud
from wordcloud import WordCloud

#text
text= ""
for index, description in jobs_df['description'].iteritems():
    text += description.lower()

text.split()  
# Create word cloud
wordcloud = WordCloud(width=800, height=400).generate(text)

# Create and display the plot
plt.figure(figsize=(10, 5))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.show()
```


    
![png](output_45_0.png)
    


From the world cloud above, we can see that experience is the most important factor the any recruiters to look for. Besides that, we can see some skils and knowledge such as data model, extracting data, streaming data, design data pipeline, data analst, azure cloud, data technolofies, data warehousing, etc are also important factors that recruiter need from the applicants.

# 4) Summary

In general, we can conclude that the market for data engineer is extremely appealing when it is now in shortage of supply and the demand is ever increasing. Especially, many companies wants to hire general data engineer jobs (also in different levels such as senior, junior) and less demand for specialize data engineer jobs. Although data engineer are in shortage of supply, recruiters still expect that their candidates have some exprience in this field and knowledgeable in develop data and data model, how to stream and extract data, with other types of data skills, including data analysis. A big plus for this job is many companies allow their engineers to work remote.
