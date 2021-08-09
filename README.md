# Movies_ETL


## Overview
Amazing Prime is a streaming platform that streams movies and TV shows. The team at Amazing Prime wants to develop an algorithm to help them predict which low budget film is likely to become popular so that they can make a bid for the streaming rights at a bargain. To help them develop this algorithm they have decided to sponsor a hackathon.  Britta from Amazing Prime has been given the task to provide the participants with a clean dataset to work with. She has asked for our help.
Britta is working with two main data sources - a scrape from Wikipedia of all movies released since 1990 and ratings data from the movie land website. Her task is to extract the data from the two data sets, clean it and combine it to form one dataset and to load it into SQL for the participants of the hackathon to use.

## Resources

### Development Environment

* Anaconda – versions 4.10.3 
* Jupyter Notebook – version 6.10.0 
* Python – version 3.8.8

### Tools

* PostgreSQL and pgAdmin

### Dependencies

* Python Pandas library
* Python Numpy library
* JSON library 

### Resources

* Wikipedia-movies.JSON
* Movies_metadata.csv
* Ratings.csv

## ETL Process

<img width="960" alt="ETL 1" src="https://user-images.githubusercontent.com/85518330/128737542-80f72953-9d5b-4887-9989-df65ada15660.png">


### Extract  

In this phase we helped Britta extract scraped Wikipedia data stored as a JSON, and Kaggle data stored in CSVs.
We import the Wikipedia JSON file and load it into a list of dictionaries using the load() method. Using the with statement, we open the Wikipedia JSON file to be read into the variable file, and use Jason.load() to save the data to a new variable. Wiki_movies_raw is a list of dicts. Before we take a look at the data, we  check how many records were pulled in. We can use the len() function to do that
Next we go on to download the Kaggle dataset. MovieLens is a website run by the GroupLens research group. The Kaggle dataset pulls from the MovieLens dataset of over 20 million reviews. We extract it and decompress the CSV files. We're interested in the movies_metadata.csv and ratings.csv files.

### Transformation 

Once we have downloaded our raw datasets, its time to clean and transform them into clean and usable datasets. The cleaning is iterative three step process

* Inspect data to identify problems. 
* Make a plan and decide whether it is worth the time and effort to fix it. 
* Execute the repair. 

#### Wikipedia Data

We found our Wiki data to be specially messy as its from a web scraping. We begin cleaning it. We begin with inspecting the column headings as a list. 

* We identify headings that have nothing to do with movies and delete them
* We retain only movies that have directors or directed by and an IMDB_link next 
* We find that some of the data pertains to TV series (No. of episodes) we don’t need those rows, so we remove them
* We find and remove all alternative titles (titles in foreign languages etc)
* Next we consolidate  columns with similar information  e.g. “Director” or “Directed by” ….
* Another listing of columns with null values and keep columns that are 90% filled with data, the rest or columns are dropped
* Then we check data types of each column and fix the wrong ones 
* The box office and budgets columns aren’t written in a consistent manner across rows so we need to make them consistent
* We fix the release date columns next 
* We find and drop duplicate rows using the drop duplicates () method
* Our wiki data transformation is complete

#### Kaggle Data

We list out all the columns and check the 

* Data types and correct all the incorrect data types 
* We check the adult and video columns to make sure they are True and False Columns, we don’t want rows where adult is True, so we remove them, then we drop the adult column   completely, the video column has all True and False only  
* We change the data types of budget, ID and popularity to numeric 
* Release date is converted to datetime

#### Ratings Data

The ratings file has 26+ million rows so we check the same for null values. We find none so we are good to go 

####  Merging Wikipedia and Kaggle data sets 

<img width="475" alt="Wiki_Kaggle" src="https://user-images.githubusercontent.com/85518330/128738224-5afb12f5-c051-40cb-8362-5147e22d11a1.png">


Next we merge the cleaned Wikipedia and Kaggle data sets into one common set. We merge the two sets using IMDB ID and begin cleaning
We fill all null fields in Kaggle using wiki data

We reorder the columns to make the dataset easier to read for the hackathon participants. 
•	Identifying information (IDs, titles, URLs, etc.)
•	Quantitative facts (runtime, budget, revenue, etc.)
•	Qualitative facts (genres, languages, country, etc.)
•	Business data (production companies, distributors, etc.)
•	People (producers, director, cast, writers, etc.)

 
#### Merging Wiki, Kaggle and Ratings data
The ratings data file has 26+ million rows so in order to add to our file we could calculate some basic statistics like the mean and median rating for each movie. 
First, we need to use a groupby on the "movieId" and "rating" columns and take the count for each group. We can pivot this data so that movieId is the index, the columns will be all the rating values, and the rows will be the counts for each rating value. We merge the rating counts into our main movies dataframe and fill in the missing values if any 
This completes the Transformation phase

## Loading Data
Amazing Prime has decided the easiest way to make the data accessible for the hackathon is to provide a SQL database to the participants. The data needs to move from Pandas into a PostgreSQL database. We create a new database and use the built-in to_sql() method in Pandas to create a table for our merged movie data. We'll also import the raw ratings data into its own table.

# Our hackathon teams have a clean dataset to work on now

