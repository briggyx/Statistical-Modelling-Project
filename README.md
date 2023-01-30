# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
The objective of this project is to analyze bike-sharing data in the Bay Area, including San Francisco, Oakland, and San Jose. The data is obtained by querying the CityBikes API for all the bike-sharing stations in the area. The project also involves querying the Yelp and Foursquare APIs for points of interest (POI) within 1000m of each bike station. The focus of this aspect of the project is on the distance from each bike station and other attributes of each POI.

The next step is to clean and transform the data received from the APIs using Python. The data is consolidated between the three APIs and stored in various dataframes that will be used for exploratory data analysis. The data is then loaded into a SQL database using Python.

The exploratory data analysis (EDA) involves both statistical analysis and visualizations. This helps to identify trends and patterns in the data. Statistical models are also used to identify trends and patterns. The results of the statistical models are then interpreted and insights are extracted that can be valuable to stakeholders. 


## Process
### Part 1: Connecting to CityBikes API (all_APIs_and_models.ipynb)
- Query City Bikes API for JSON data, examine US city names format, check network info for San Francisco Bay Area.
- Open URL for station info and status JSON, request data, convert list of dicts to dicts with station IDs as keys. 
- Set up empty lists for data frame, loop over station data to populate lists, create data frame from lists. 
- Calculate new info and add to data frame, inspect it. 
- Check linear correlations between variables using pair plots and Pearson's test. 
- Create histogram of total docks at bike stations, find percentiles of station size, investigate which stations belong to each percentile.

### Part 2: Connecting to Foursquare and Yelp APIs, joining API data and EDA (all_APIs_and_models.ipynb)
Retrieve data from FSQ and Yelp; combine data from CityBikes to create joined dataframes:
- Create a helper function to search for POIs in one category from FSQ and Yelp, both of which return a list of POIs represented as JSON dicts, for use in the get_nearby_from_api function. 
- Define a function to retrieve results for a list of categories in one row of the bike data frame, using either API's helper function, and return a data frame. 
- Clean non-alphanumeric characters using regular expressions, load API keys, and fetch data for all bike stations using both APIs. 
- Tidy, sort, and save the results as a data frame. 
- Split the POI information columns into pairs for each API, identify duplicates using "identifier" tuples and filtered POI information, and fetch additional data for each POI. Set the indexes of both data frames to be the bike station IDs and arrange columns in the desired order. 
- Export the final data frames as .csv files.

Exploratory data analysis:
- Compare the distributions of the distances of points of interest from bike stations between Yelp and Foursquare through histograms.
- Plot a scatter graph of the POI counts per station for both APIs.
- Conduct a t-test to determine if the ratings from the two APIs are significantly different.
- Examine the distribution of both raw and Bayesian ratings for both APIs.
- Analyze the comparison between the Bayesian adjusted ratings and the raw ratings for each API.
- Investigate the connection between the Bayesian adjusted ratings and the number of ratings for both APIs.
- Determine the correlation between the ratings and the correlation between the Bayesian adjusted ratings for both APIs.

### Part 3: Creating a SQL database (SQL_database.ipynb)
Create tables in a new SQL database using Python:
- In Python, connect to an SQLite database.
- Define a function named "execute_query" to execute SQL queries. 
- Fill the new table "data_by_business" with values from "DataByBusiness.csv" and verify if the SQLite table matches the original .csv. 
- Fill the new table "data_by_station" with values from "DataByStation.csv".

Next, validate the accuracy of tables in the new SQL database:
- Extract records from an SQLite database using a helper function execute_read_query.
- Verify if the Bike_Station_Coordinates, POI_API columns in data_by_station table have any missing values.
- Verify if any columns in data_by_business table, which should not have empty cells, have any missing values.
- Compare the number of rows in data_by_business table to DataByBusiness.csv.
- Compare the number of rows in data_by_station table to DataByStation.csv.
- Search for instances of repeated businesses in data_by_business table.
- Look for repeated rows in data_by_station table.

### Part 4: Building a Model (all_APIs_and_models.ipynb)
- Examine the correlation between the count of POIs and the characteristics of bike stations using the same method for obtaining POI counts.
- Investigate the relationship between the total number of docks and the number of nearby POIs.
- Determine the correlation between the number of POIs from Yelp and the total number of docks, and between the number of POIs from Foursquare and the total number of docks.
- Check if the occupancy rate (percentage of available bikes) of the bike station is related to the number of POIs.
- Examine the correlation between the number of POIs from Yelp and the occupancy rate, and between the number of POIs from Foursquare and the occupancy rate.
- Get the coordinates for stations with POI counts in the peaks and write both data to a CSV file. Plot the coordinates using external tools (www.gpsvisualizer.com).
- Merge the POI counts and coordinate data, and plot it all with POIs as markers, using color on an external website (www.gpsvisualizer.com).

## Results
### Part 1: Connecting to CityBikes API (all_APIs_and_models.ipynb)
- In the Bay Area, 499 bike stations were located. The information in the dataframe includes geographic location, number of bikes available, empty docks, total docks (station size), and bike occupancy percentage. This data was saved as BikeStationData.csv. 
- The correlation between Station_Occupancy and Total_Docks, and between BikesAvailable and Total_Docks, Total_Docks and DocksAvailable, and BikesAvailable and DocksAvailable, is weak as indicated by the small correlation coefficient (|r|). The weak correlations are still considered statistically significant based on the low p-values. 
- The median station size was 18, 21, 25 for the 25th, 50th, and 75th percentiles respectively, and a dataframe showing the station rankings was created (BikeStationDataPercentiles.csv). 
- In the analysis, BikesAvailable was the dependent variable, and the R-squared value of 0.456 meant that 45.6% of the variation in bikes available could be explained by the independent variables. The significant results of the model were supported by the high F-statistic (417.1) and low p-value (9.19e-68).

### Part 2: Connecting to Foursquare and Yelp APIs, joining API data and EDA (all_APIs_and_models.ipynb)
- Two dataframes are present: "FinalPOIByBusiness.csv" contains information on unique businesses located near bike stations in the Bay Area as found by both APIs, while "FinalPOIByStation.csv" groups POI by bike stations and has unique POI for each group but may repeat between different stations. 
- Yelp tends to find POI closer to bike stations while FSQ finds POI further away. 
- The POI count per bike station has a bimodal distribution. 
- A scatter plot of POI count per station by API shows high correlation (R-square 0.990). 
- A t-test shows significant difference in ratings between the APIs (p=1.3404045164848164e-44). 
- Both APIs have more positive ratings than negative, and the Bayesian average spikes at the mean for low individual rating counts. 
- The relationship between Bayesian adjusted rating and rating count shows convergence on mean for low ratings, but divergence to arithmetic mean for high ratings, indicating no correlation between raw ratings or Bayesian adjusted ratings from the two APIs.

### Part 3: Creating a SQL database (SQL_database.ipynb)
- A SQLite database was created: bikestations_POI_sql_database.sqlite
- It consists of two tables, data_by_business and data_by_station
- data_by_station is related to data_by_business by two foreign keys, POI_Name and POI_Address

### Part 4: Building a Model (all_APIs_and_models.ipynb)
- The size of a bike station (total docks) does not seem to be related to the number of nearby POIs. 
- No correlation is found between the number of POIs from Yelp and the total docks, or the number of POIs from Foursquare and the total docks. 
- Similarly, there is no correlation between the bike station occupancy (percentage of available bikes) and POI count. 
- There are a large number of stations with around 160 POIs and 90 POIs. These may represent stations clustered in different areas with overlapping POIs. 
- The two clusters with the highest number of POIs are in downtown San Francisco and near downtown SF/Oakland. Stations with the lowest number of POIs are in Oakland and San Jose. 
- Knowing the locations with the highest number of POIs could be useful for bike sharing companies to optimize their station placement.

## Challenges 
- Removing duplicates and consolidating the POIs found by both Yelp and FSQ for each bike station was a significant challenge. 
- A given API may have returned the same POI for a station more than once. 
- Some linear regressions showed no significant correlation, but in some cases, a small P-value was obtained, which was conflicting.

## Future Goals
- Explore data using the SQLite database and compare it to using Pandas. 
- Develop a classification model that separates a city into geographic areas with high, medium, or low numbers of POIs within 1000m of a specific point using a multinomial regression model. 
- Experiment with various forms of exploratory data analysis.


