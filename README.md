# Movies_ETL
## Project Overview
I created one ETL function that took in 3 files, 2 csv files and a json file. I then consolodated the code to work as a single function instead of 100+ lines of code. Thought it would be an easy to automate the code but I noticed a few issues and don't think that automation would be efficient the way it's currently written.

### Assumption 1 
In this Data Analysis for Amazing Prime, I've noticed a few issues. The main one being the amount of time it took to process the Rating.csv file, I believe that it took around 30-45 minutes for my file to completely run. At over 26,000,000 rows, the code could break if there's not enough RAM on the computer running the code, which seems the be the case with me and that's even with splitting out the importing in chunks of 1,000,000. Might work if you make it smaller than that but I'm not sure how efficient that would be.

### Assumption 2
There also looks to be alot of information that isn't taken into much consideration, movies wise. Such as the 'belongs_to_collection' and 'based_on' columns which are mostly null, over 82% and 67% of values are null which won't be much helpful. We should focus on other information that is more crucial and commonly used throughout the industry.

### Assumption 3
In piggybacking off of Assumption 2, is it efficient to replace all NaN information to 0? Example in the follwing below:

***(running_time_extract = running_time_extract.apply(lambda col: pd.to_numeric(col, errors='coerce')).fillna(0))***

By replacing all NaN values with 0, wouldn't it bring down the average if it was ever pulled? Wouldn't it be better to pull the average of all non NaN values instead of pulling a possibly skewed average?

### Assumption 4
Also, instead of creating 10 different columns for ratings, it would be much cleaner to have a single column to have the average rating. I don't know of many people that look at ratings by a count, but creating a function would help here to automatically convert the rating count to an average rating. This can possibly be done by using an if statement to move onto the next column if the rating is NULL, otherwise it will grab the sum of the average rating from each column and then divide it by the count to attain the average. 

### Assumption 5
Final issue I noticed is adding the new data into the database, which is used via PostgreSQL. An error I encountered when trying to replace the data was with the MoviesDF, I copied the **(if_exists='append')** code from the Ratings portion and my code ended up adding it to the existing code instead of replacing it. This issue was easily resolved with some additional research, solution being **(if_exists='replace')**.
