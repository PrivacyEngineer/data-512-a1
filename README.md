THE PROJECT

Title: English Wikipedia page views 2007-2018
Homework A1, version 1.0.
Goal: To construct, analyze and publish a dataset of monthly English Wikipedia mobile and desktop page traffic from the earliest to the most recent month were data is available.
Purpose: Demonstrate that we can follow best practies for open scientific research in designing and implementing a project, so that anyone can understand the process we followed and reproduce the results.

DATA ACQUISITION

The data employed for this analysis represents monthly non-spider (aka non-crawler) pageviews on Wikipedia from both, desktop and mobile clients (browser and app).
Data was gathered on 10/15/2018 form the Wikimedia REST API, Wikimedia Foundation, 2018. CC-BY-SA 3.0 https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions

We used two REST APIs:

1.- The Legacy pagecounts API (for monthly data December 2007 to July 2016): 
Documentation: https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts
Endpoint: https://wikimedia.org/api/rest_v1/#!/Pagecounts_data_(legacy)/get_metrics_legacy_pagecounts_aggregate_project_access_site_granularity_start_end

2.- The Pageviews API (for monthly data July 2015 through September 2018):
Documentation: https://wikitech.wikimedia.org/wiki/Analytics/AQS/Pageviews
Endpoint: https://wikimedia.org/api/rest_v1/#!/Pageviews_data/get_metrics_pageviews_aggregate_project_access_agent_granularity_start_end

Two data sets were downloaded using the legacy API, one for desktop and one for mobile access. 
Three data sets use the current format pageviews API, one for desktop and two for mobile (website and app).
All data sets are in JSON format and somewhat similar schemas. Some field names are different, and there's one field that exists in current format but not legacy:
- granularity: representing the period of collection, which is montly for this analysis.
- count (legacy)/views (current): representing the actual number of pageviews for each month.
- agent (only in current data): which is always "user" for this analysis (the other type would be spider)
- project: which in our case always refers to the English version of Wikipedia "en.wikipedia"
- timestamp: with format YYYYMMHHMM
- access: which can be "desktop-site" or "mobile-site" for legacy data, or "desktop", "mobile-web" or "mobile-app" for current format pageviews.
 
When comparing the data obtained through each of the two APIs it is important to note that legacy data does not distinguish between page counts of users and those of web crawlers (aka spiders), but current format does.
For current format we only downloaded pageviews for users, no spider data is included.
The five raw data data sets in JSON format are:
../raw_data/pagecounts_desktop-site_200712-201608.json

../raw_data/pagecounts_mobile-site_201412-201608.json

../raw_data/pageviews_desktop_201507-201809.json

../raw_data/pageviews_mobile-app_201507-201809.json

../raw_data/pageviews_mobile-web_201507-201809.json
 
DATA PROCESSING

All code employed for this analysis was written using Python 3.0 and can be found in the Jupyter notebook: ../src/hcds-a1-data-curation.ipynb
No other scripts or programs were employed. 
The only manual step we used the Windows Snipping Tool to snip an image of the final graph, which is stored in the results folder under the name ../results/WikiViews.jpg

Processing steps:
- Downloading of datasets using the Wikipedia REST APIs.
- Storage of original JSON data sets in the "raw_data" folder.
- Transformation of the raw data sets to data frames (table format).
- Aggregation of current format pageviews for mobile website and mobile app access types.
- Concatenation of all data sets into one frame.
- Transformation of concatenated data sets into a single matrix containing all data.
- Transformation of matrix into a data frame and storage of it in ../data_clean/en-wikipedia-traffic_200701-201809.csv
- Dropped a single data point corresponding to December 2007 as it appears to be only a partial count (value was too low).

DATA ANALYSIS

Goal is to analyze monthly English Wikipedia mobile and desktop page traffic from the earliest to the most recent month were data is available.
Only assumption is that data is complete and correct, except for a few outliers with low values at either the end or beginning of collection periods. 
These outliers were discarded. We then performed a visual inspection of all data with satisfactory results. 
Final results, the pageviews graph, was generated using the same Python 3.0 program that can be found in the Jupyter notebook in ../src/hcds-a1-data-curation.ipynb.
The .csv file can be found in ../data_clean/en-wikipedia-traffic_200701-201809.csv
