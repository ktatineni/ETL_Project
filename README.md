# ETL Project

# Goal

The goal of our ETL project is to extract, transform, and load a count on the New York City businesses and schools according to zip code.


# EXTRACT

# About the Data

For our data sets, we targeted the open source City of New York’s database called “NYC OpenData”.  Specifically, for the first data set, we sourced data from the database called “Legally Operating Businesses” which features data from legally operating NYC businesses/individuals that hold a DCA license (source website: https://data.cityofnewyork.us/Business/Legally-Operating-Businesses/w7w3-xahh).  This weekly updated data was provided by the Department of Consumer Affairs (DCA), was made pubic on February 22, 2016, and was last updated on May 10, 2019.  We loaded the .xlsx file titled “DCA_Legally_Operating_Businesses_03062015.xlsx” as a csv file using Pandas.  This data set contains 195,091 rows and 27 columns.  Columns include, for instance, content related to license number, type, status; to industry; to business name, address, and phone number; and to location (latitude and longitude).

For the second data set, we sourced data from the database called “2013 - 2018 Demographic Snapshot School” which features student demographic and enrollment statistics broken down by school from academic years 2013-2014 through 2017-2018 (source website: https://data.cityofnewyork.us/Education/2013-2018-Demographic-Snapshot-School/s52a-8aq6).  This data was provided by the NYC Department of Education, was made pubic on May 2, 2018, and was last updated on September 10, 2018.  We loaded the .xlsx file titled “2013_-_2018_Demographic_Snapshot_District_DD.xlsx” as a csv file using Pandas.  This data set contains 8,972 rows and 39 columns.  Columns include, for instance, content related to school name, year, enrollment; to grades K through 12; to counts and percentages on female and male students; to race (Asian, Black, Hispanic, white, and multiple race categories not represented); to students with disabilities; to English language learners; and to poverty and economic need index.


# Data Extraction

After declaring the dependencies, we started with a simple data extraction by loading, first, the “2013 - 2018 Demographic Snapshot School” data set into Jupyter Notebook using Panda ‘read_csv()’ method.  Since the data included multiple academic years and thus the same school name was listed more than once (multiple rows), we performed a ‘groupby’ function to generate a list of all the unique school names.  The output was 1,834 unique school names.  In order to compare the school count and business count in relation to NYC zip codes, we had to source for the school zip codes (which was not included as part of the original database).  We sourced for the school zip codes using a Google API call with the following (https://maps.googleapis.com/maps/api/place/findplacefromtext/json?) as the base_url, iterated through the ‘School Name’ column using a for-loop, and generated an additional csv file (named “school_zip code.csv”).  We noted that of the 1,834 unique school names, running the Google API only resulted in 1,617 corresponding zip codes; 217 school names were not recognized by the Google API.  Possible reasons are due to an incomplete school address and/or the affected schools are too new and/or too small; and ,therefore, not recognized by the API.  


# TRANSFORM

After extracting the school data, we performed a count on unique school names for each corresponding zip code.  The output was displayed in a Pandas dataframe.  Next, we performed a simple data extraction by uploading the “Legally_Operating_Businesses.csv” data set into our notebook using, again, the Panda ‘read_csv()’ method.  In order to output the businesses count, we performed a count on the number of times a specific zip code appeared.  The output was displayed in a Pandas dataframe.  To complete the final data transformation, we performed an inner join on zip code using the ‘merge’ method and generated an updated Panda dataframe.  This method not only removed non-New York City zip codes but also the zip codes that did not contain any businesses and/or schools (as NaN fields).  Following the data cleanup, the output generated 186 (from 3,569) rows.


# LOAD

We then used PyMySQL client and the SQLAlchemy toolkit to load the cleaned data as MySQL.  We first created an engine using the single call ‘create_engine()’, then procured a connection resource to issue our SQL into the database using ‘engine.execute()’.  Finally, we viewed the result and printed it.
