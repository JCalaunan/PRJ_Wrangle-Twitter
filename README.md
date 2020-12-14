# PROJECT - Wrangle Twitter data via API

## Table of contents 
* [Introduction](#introduction)
* [Initial Setup](#setup)
* [Global Functions](#functions)
* [Methodology](#method)
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


## Initial Setup
Generally, run from conda/terminal: <br>
`replace pip with conda for anaconda environment`
- [ ] pip install pandas, which by default will download numpy<br>
- [ ] pip install requests <br>
- [ ] pip install tweepy <br>

`Optional - provides Table Of Contents`
- [ ] pip install jupyter_contrib_nbextensions
- [ ] pip install -U pandas-profiling


## Global Functions
### Definition:
`add_files(*filename)`
- returns file added, as a string
- global variable: filelist
* tracks files added so far in a list, saves time scrolling
* can pass multiple parameters, for loop will iterate to catch all

`get_values(df, col)`
- returns value_counts, as a series
- parameter: <dataframe>, <dataframe.column>
* automates .value_counts() for all columns/variables in a dataframe
* based on .values, will print out if duplicates are evident and the max number reoccuring number.
* does not provide feedback on indexes

`go_assess(df)`
- calls get_values for each column in a dataframe
- parameter: <dataframe>


## Methodology:
1. Gather data
1.1 Twitter archive data:
- Read in provided CSV, from subfolder using pandas.
- Check file imported correctly.
- Create two copies, one backup of original and one to be cleaned.

1.2 Twitter Image Prediction data:
- Access URL and download image predictions using Requests.
- Binary read/write not required as image predictions would not be enclosed in a TSV file.
- Read in TSV file into pandas dataframe.
- Check file imported correctly.
- Create two copies, one backup of original and one to be cleaned.

1.3 Import tweet_id JSON:
- Sign up to twitter and refer to developer documentation
- Access twitter API, obtain token required for API
- Wait for script run, read file and print finish time
- Visually analyse text file for holistic overview
- Contains {} to denote in correct format to be read in as JSON
- Read in data from stored .txt file

3. Assess data:
3.1 Iteration 1
- Visually assess using .sample(qty), files are already in folder ready to be assessed in external programs.
- Programmatically assess using .info, .describe, value_counts, isnull
Quality checklist (Completeness, Validity, Accuracy, Consistency):
- [ ] Format
- [ ] Duplicates
- [ ] Nulls
- [ ] Data Types
- [ ] Matching 
Structure checklist
- [ ] Single values per variable/column
- [ ] Variables match purpose of table i.e. correct schema


3.1.1 Observations from .head() - Twitter data archive:
`[V]=visual, [P]=programmatically, [O]=Optional`
* 17 Variables in total

- Col0 tweet_id:
[P]
* Integer datatype suits content
* No nulls: .info()
* No duplicates: df_twitter.tweet_id[df_twitter.tweet_id.duplicated()]
* No outliers in values: .info()

- Col1-2 in_reply_to_status_id, in_reply_to_user_id:
[P][V] 
* Float datatype precision not required
[P]
* Significant quantity of values are missing
* Possible outlier values, e+ seen in describe min values different in relation to the other values

- Col3 timestamp:
* [P][V] Contains date and timestamp that can be split into additional columns = Date, Time
* [V][O] Contains +0000 at the end, research indicates its purpose is to display the timezone. Extract into timezone column.
* Remove data past 01 Aug 2017 i.e. 2017/08/01 as requested

- Col4 source:
* [P][V] Contains HTML tags, extract URL

- Col5 text:
[V] 
* Contains description of the tweet, details of the dog, description of the picture, URL

- Col6-7 retweet_status_id, ..._user_id:
[V] 
* See Col1-2 comments above, similar findings

- Col8 retweet_status_timestamp:
[P][V]
*  Significant values missing

- Col9 expanded_urls:
* [P][V] Contains duplicates within row value

- Col10, 11 rating:
[P][V]
* Numerator exceeds 10, values greater than 100 are present, most likely decimal points were not factored in.
* Denominator is always 10, redundant information
`no change required as requested`

- Col12 name:
* [P][V] .value_counts shows significant number of `names not provided` and `duplicates`

3.1.2 Observations from .head() - Twitter image recognition:
`[V]=visual, [P]=programmatically, [O]=Optional`
[P][V]
* Globally, there are `no null` entries for all columns
* 12 Variables in total

- Col0 tweet_id:<br>
[P] 
* Integer datatype matches that found in twitter dataframe, will merge based on this primary/foreign key
[P][V] 
* Data is valid
* Correct integer lengths
* unique and conforms to a schema
* No structure issues found.

- Col1 jpg_url
[P][V]
* Values appear consistent
* Variable is descriptive, however inaccurate as there are extensions not of .jpg
* Datatype suits content
[P]
* Duplicates are present and seem correct as these could be retweets, possibly?

- Col2 img_num
[P][V]
* Appears completely
* Unsure of purpose, information lacking
* Max value is 4, min is 1
* Duplicates values are expected, the data here appears to be categorical, unsure of how it is quantified/measured from initial observation

- Col3 p1, Col6 p2, Col9 p3
[P][V]
* Mix of lower and proper case
* Data validity/machine learning prediction accuracy issues, i.e. canoe, suit, candle are prevalent. Purpose of the file is to provide predictive images of dogs
* Contains no white space
* Consistency - String has a mix of lower and proper case

- Col4 p1_conf, Col7 p2_conf, Col10 p3_conf
[P][V]
* Confidence of the p1 observation made by the program
* Datatype suits
* Value is not greater than 1, i.e. 100%

- Col5 p1_dog, Col8 p2_dog, Col11 p3_dog
[P][V]
* Data validity issue with false numbers not matching those found in col3 mask, cross reference required to see what col5 false value equate to those found matched in col3

3.1.3 Observations after write to .txt file - Twitter API scrape:
[V]
* text file shows structure of data is correct and formatted to suit JSON
[P]
* imports as dict file type after using json_loads
* for loop required to sift through objects key within file
* append and merge into data frame
* no nulls

- Col0 tweet_id
[P][V]
* Data quality fixed during import of .txt file, tweet_id and tweet_idstr were available keys.

- Col1 retweet_count
[P][V]
* Correct data type to suit values within column


- Col2 favourite_count
[P][V]
* Correct data type to suit values within column


3.1.4 Define data dictionary (basic description)
3.1.4.1 Twitter Archive
tweet_id:					numeric, user identifier
in_reply_to_status_id:		numeric, user identifier, with NaN
in_reply_to_user_id:		numeric, user identifier, with NaN
timestamp:					date & time, YYYY-MM-DD HH:MM:SS+GMT
source:						string, html tag with URL
text:						string, twitter user text
retweet_status_id:			numeric, user identifier, with NaN
retweet_status_user_id:		numeric, user identifier, with NaN
retweet_status_timestamp:	numeric, user identifier, with NaN
expanded_urls:				string, user twitter URL
rating_numerator:			numeric, exceeding 10, extracted from text column
rating_denominator:			numeric, value = 10 across column
name:						string, name extract from text column
floofer:					category, dog type extracted from text column
doggo:						category, dog type extracted from text column
pupper:						category, dog type extracted from text column
puppo:						category, dog type extracted from text column

3.1.4.2 Twitter Image Predictions
tweet_id:					numeric, user identifier
jpg_url:					string, image URL
img_num:					number, corresponds to algorithm with highest probability
p1:							string, predicted image 1 out of top 3
p1_conf:					numeric value, algorithm confidence in recognition
p1_dog:						boolean, image is a dog
p2:							see p1
p3:							see p1

3.1.4.3 Twitter API extract
tweet_id:					numeric, user identifier
retweet_count:				numeric, retweet count of twitter id
favourite_count:			numeric, favourite count of twitter id

3.2 Iteration 2
* Size of the three archives differ and are inconsistent. Join dataframes on lowest number of tweet_id's.

3.3 Iteration 3
* Names were incorrect and needed to be extracted from text column

4.1 Cleaning Summary:
4.1.1 Quality Issues:
1. col0: tweet_id data type change to string, all dataframes
df_twitter
2. col3: change timestamp datatype to datetime
3.1 col4: split string to remove html tag and extract text within
3.2 col4: rename column heading from source to add source_app
4. col1,2,6,7: change datatype from float to string
5.1 remove whitespaces in string/object columns
9. review col12 to ensure correct name transferred over
10. check numerator rating against text and valid/correct
11. remove retweets, indicated by RT @ in text column, retweet status id and in reply to id


twitter_image_predictor
5.2 remove whitespaces in string/object columns
6. col3,6,9: change to lower case
7. col1: rename from jpg_url to img_url
8. col2: rename from img_num to conf_tweet_img

twitter_api
5.3 remove whitespaces in string/object columns


4.1.2 Structure Issues:
1. timestamp split into three columns, date, time, timezone
2. categorize dog type into one column
3. merge, denormalize dataframe to contain the relevant columns required for analysis
3.1 twitter_data to contain all relevant twitter data



==================================================
Upgrades:
- add file tracker
- assess function
 - requirements: specify column (series) as functions do not apply to whole dataframe
 - argument = dataframe.seriesname
- create memory release, for dataframes that have been copied
- add function to create compiled dataframes i.e raw and clean
- container to list all functions present and the arguments required

Results
- Prepare:
1.1 wrangle_act.ipynb
1.2 wrangle__report.pdf/html for documentation of steps
- 



## References
### Udacity
- [Udacity DAND Core 4 Lesson 2 (Gathering data), 12. Source: Downloading Files from the Internet](https://classroom.udacity.com/nanodegrees/nd002/parts/af503f34-9646-4795-a916-190ebc82cb4a/modules/86c36b91-055f-4970-8462-864f332c2ebb/lessons/96402d84-c99d-4982-9edf-2430ef30d222/concepts/ed908f34-ce67-44c0-acb1-d81abd5d9e37)
- [Udacity DAND Extracurricular 4. Lesson 6 (Scripting) 17. Reading and Writing Files](https://classroom.udacity.com/nanodegrees/nd002/parts/762c0200-e8a7-425b-be49-7080cc533c7d/modules/d2268785-db9d-4aaa-ab44-afec79099d7d/lessons/62fec647-9f0e-4551-8752-2139e2d4eb5f/concepts/43991399-3df7-48cf-a10c-792921e1b6bf)
- [Dog types into one column](https://knowledge.udacity.com/questions/389519)
### Docs
- [Requests doc](https://requests.readthedocs.io/en/master/user/quickstart/)
- [Unofficial Jupyter Notebook Extensions](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html)
- [Twitter Developer Portal](https://developer.twitter.com/en/docs/developer-portal/overview)
- [Reading and Writing JSON to a File in Python](https://stackabuse.com/reading-and-writing-json-to-a-file-in-python/)
- [JSON documentation](https://docs.python-guide.org/scenarios/json/)
- [Pandas strip string](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.strip.html)
- [Pandas split string](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.split.html#pandas.Series.str.split)
- [BeautifulSoup install and docs](https://pypi.org/project/beautifulsoup4/)
- [](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html)
- [Pandas drop columns](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.drop.html?highlight=drop#pandas.Series.drop)
- [Pandas extract str based on regex pattern](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.extract.html)
- [Read to CSV](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_csv.html)
- [Python path create directory doc](https://docs.python.org/3/library/pathlib.html#pathlib.Path.mkdir)
- [Pandas profiling documentation](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/pages/installation.html)
- [SweetViz](https://pypi.org/project/sweetviz/)

### Misc.
- [JUPYTER CONVERT: HOW TO GET A TABLE OF CONTENTS](https://littledatascientist.com/2019/01/20/jupter-convert-how-to-get-a-table-of-contents/)
- [Index Match](https://stackoverflow.com/questions/21800169/python-pandas-get-index-of-rows-which-column-matches-certain-value)
- [Does not contain string](https://www.xspdf.com/resolution/50135853.html#:~:text=%22b%22)%5D-,Search%20for%20%22does%2Dnot%2Dcontain%22%20on%20a%20DataFrame,an%20object%20dtype.%20%3E%3E%3E)
- [HTML <a> type Attribute](https://www.w3schools.com/tags/att_a_type.asp)
- [Apply BeautifulSoup function to Pandas DataFrame
](https://stackoverflow.com/questions/53189494/apply-beautifulsoup-function-to-pandas-dataframe)
- [Pandas replace strings](https://stackoverflow.com/questions/27060098/replacing-few-values-in-a-pandas-dataframe-column-with-another-value)
- [Ignoring NaNs with str.contains](https://stackoverflow.com/questions/28311655/ignoring-nans-with-str-contains)
- [Python regex - visual guide](https://www.debuggex.com/#cheatsheet)
- [Show Tableau dashboard in Jupyter Notebook](https://medium.com/@ikekramer/tableau-visual-in-jupyter-notebook-7b9faf60e8fd)

## Incomplete functions


## Problems
SweetViz compare
TypeError: 

Cannot convert series 'tweet_id' in COMPARED from its TYPE_TEXT
to the desired type TYPE_NUMERIC.
Check documentation for the possible coercion possibilities.
POSSIBLE RESOLUTIONS:
 -> Use the feat_cfg parameter (see docs on git) to force the column to be a specific type (may or may not help depending on the type)
 -> Modify the source data to be more explicitly of a single specific type
 -> This could also be caused by a feature type mismatch between source and compare dataframes:
    In that case, make sure the source and compared dataframes are compatible.