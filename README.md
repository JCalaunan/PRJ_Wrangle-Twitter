# PROJECT - Wrangle Twitter data via API

## Table of contents 
* [Introduction](#introduction)
* [Setup](#setup)
* [References](#references)

## Introduction
Data in the real world and that provided for academia are completely different.
Data provided for academia is for teaching purposes and is generally ideal with minimal corrections required to provide examples.
Real world data is messy and untidy with no real structure, and would otherwise overwhelm students.

This projects intent is to simulate in between where real world data has been structured to suit the providers needs, gathering to put to practice that which has been learnt academically.

Generally:
- [ ] Gathering data, webscraping raw data or via an API
- [ ] Assessing format and quality of data, the conversions required to make analysing possible and straightforward
- [ ] Cleaning utilising programmatic methods to scale for future expansion and editing of unpredictable changes

## Import Libaries
Generally: <br>
- [ ] pip install pandas <br>, which by default will download numpy
- [ ] pip install requests <br>
- [ ] pip install tweepy <br>

`Optional - provides Table Of Contents`
- [ ] pip install jupyter_contrib_nbextensions

## Steps:
1. Twitter archive data:
- Read in provided CSV, from subfolder using pandas.
- Check file imported correctly.
- Create two copies, one backup of original and one to be cleaned.

2. Twitter Image Prediction data:
- Access URL and download image predictions using Requests.
- Binary read/write not required as image predictions would not be enclosed in a TSV file.
- Read in TSV file into pandas dataframe.
- Check file imported correctly.
- Create two copies, one backup of original and one to be cleaned.

3. Assess data:
- Visually assess using .sample(qty), files are already in folder ready to be assessed in external programs.
- Programmatically assess using .info, .describe, value_counts, isnull, 

Observations from .head():
`[V]=visual, [P]=programmatically, [O]=Optional`
- Col0 Tweet_id:
* [P] Integer datatype suits content
* [P] No nulls: .info()
* [P] No duplicates: df_twitter.tweet_id[df_twitter.tweet_id.duplicated()]
* [P] No outliers in values: .info()

- Col1-2 in_reply_to_status_id, in_reply_to_user_id:
* [P][V] Float datatype precision not required
* [P] Significant quantity of values are missing
* [P] Possible outlier values, e+ seen in describe min values

- Col3 timestamp:
* [P][V] Contains date and timestamp that can be split into additional columns = Date, Time
* [V][O] Contains +0000 at the end, research indicates its purpose is to display the timezone.
		 Extract into timezone column.
* Remove data past 01 Aug 2017 i.e. 2017/08/01 as requested

- Col4 source:
* [P][V] Contains HTML tags, extract URL

- Col5 text:
* [V] Contains description of the tweet, details of the dog, description of the picture

- Col6-7 retweet_status_id, ..._user_id
* [V] See Col1-2 comments above, similar findings

- Col8 retweet_status_timestamp
* [P][V] Significant values missing

- Col9

- Col10, 11 rating:
* Numerator exceeds 10
* Denominator is always 10, redundant information
`no change required as requested`

==================================================
Upgrades:
- add file tracker
- assess function
 - requirements: specify column (series) as functions do not apply to whole dataframe
 - argument = dataframe.seriesname


Results
- Prepare:
1.1 wrangle_act.ipynb
1.2 wrangle__report.pdf/html for documentation of steps
- 

## References
### Udacity
- [Udacity DAND Core 4 Lesson 2 (Gathering data), 12. Source: Downloading Files from the Internet](https://classroom.udacity.com/nanodegrees/nd002/parts/af503f34-9646-4795-a916-190ebc82cb4a/modules/86c36b91-055f-4970-8462-864f332c2ebb/lessons/96402d84-c99d-4982-9edf-2430ef30d222/concepts/ed908f34-ce67-44c0-acb1-d81abd5d9e37)
- [Udacity DAND Extracurricular 4. Lesson 6 (Scripting) 17. Reading and Writing Files](https://classroom.udacity.com/nanodegrees/nd002/parts/762c0200-e8a7-425b-be49-7080cc533c7d/modules/d2268785-db9d-4aaa-ab44-afec79099d7d/lessons/62fec647-9f0e-4551-8752-2139e2d4eb5f/concepts/43991399-3df7-48cf-a10c-792921e1b6bf)
### Docs
- [Requests documentation](https://requests.readthedocs.io/en/master/user/quickstart/)
- [Unofficial Jupyter Notebook Extensions](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html)
### Misc.
- [JUPYTER CONVERT: HOW TO GET A TABLE OF CONTENTS](https://littledatascientist.com/2019/01/20/jupter-convert-how-to-get-a-table-of-contents/)
- [Name](http link)