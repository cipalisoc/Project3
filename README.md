# The Most Successful Films Produced
## Analyzing Factors in Film that Make Successful Movies

Christian Palisoc
 
For this project, I was tasked to produce a MySQL database on Movies from a subset of IMDB's publicly available dataset. The project is divided into four parts, all outlined below. Ultimately, this database was used to analyze what makes a movie successful, and to provide recommendations to the stakeholder on how to make a successful movie.

# Data Source
Overview/Data Dictionary: https://www.imdb.com/interfaces/  
Downloads page: https://datasets.imdbws.com/  
The Movie Data Base (TMDB): https://www.themoviedb.org/

# Part 1
The first part of the the project consisted of downloading several files from IMDB’s movie data set and filter/clean the subset of moves requested by the stakeholder. Per stakeholder requirements, I only included information for movies based on the following specifcations:  
- Exclude any movie with missing values for genre or runtime
- Include only full-length movies (titleType = "movie").
- Include only fictional movies (not from documentary genre)
- Include only movies that were released 2000 - 2021 (include 2000 and 2021)
- Include only movies that were released in the United States

# Part 2
This portion of the project required the addition of box office revenue and profit, MPAA Rating, and budget to my IMDB data. The data was extracted from The Movie Data Base (TMDB) by constructing an API function and tested on movies that started in 2000 and 2001. In order to ensure your function for extracting movie data from TMDB is worked, the function was tested on  2 movie IDs: tt0848228 ("The Avengers") and tt0332280 ("The Notebook"), which ran without error and returned the correct movie's data for both test IDs.  

Some exploratory data analysis was performed and produced the following insights:

![ratings](https://github.com/cipalisoc/Project3/blob/main/number%20of%20movies%20by%20rating.jpg?raw=true)  
The visual depicts the number of movies broken down by MPAA rating (certification) produced in 2000 and 2001.  

Of the movies produced in 2000 and 2001, `1114` movies had some valid financial information (values > 0 for budget OR revenue). Furthermore, the average revenue per MPAA rating are as follows:  
- G:       `$66,632,610`
- NC-17:   `$1,668`
- NR:      `$2,116,980`
- PG:      `$60,006,795`
- PG-13:   `$70,670,934`
- R:       `$16,002,325`
- Unrated: `$0`

The average film budget per MPAA rating category are as follows:  
- G:       `$22,052,064`
- NC-17:   `$0`
- NR:      `$1,391,430`
- PG:      `$24,115,342`
- PG-13:   `$30,623,141`
- R:       `$9,622,302`
- Unrated: `$0`

# Part 3  
For the third part of the project, a construction of a new MySQL database was done after preparing the data for a relational database in parts 1 and 2. The database was then exported to a .sql file in my repository using MySQL Workbench.  

The following data were included in the database:
- Title Basics:
  - Movie ID (tconst)
  - Primary Title
  - Start Year
  - Runtime (in Minutes)
  - Genres
- Title Ratings:
  - Movie ID (tconst)
  - Average Movie Rating
  - Number of Votes
- The TMDB API Results (multiple files):
  - Movie ID
  - Revenue
  - Budget
  - Certification (MPAA Rating)

# Part 4
This last portion of the project demonstrated the use of hypothesis testing to answer four key questions in what faactors make a successful film.

## Does the MPAA rating of a movie affect how much revenue the movie generates?
### Null and Alternative Hypothesis
- Null Hypothesis: The MPAA rating of a movie does not make a difference in the revenue that the movie makes.
- Alternative Hypothesis: There is a significant difference in the revenue a movie makes based on MPAA rating.
- significance value (alpha) = .05

Since the question is asking about several MPAA ratings, Tukey's Pairwise Multiple Comparisons Test was used for the hypothesis test and got the following reults:
`KruskalResult(statistic=1098.2315002317162, pvalue=3.229276673064315e-235)`  
The overall results show that there is a significan difference in the revenues based on MPAA ratings and with the Kruskal-Wallis test, the p-value is less than our alpha of .05. Comparing these results to the barplot previously confirms Kruskal-Wallis test and we can reject the null hypothesis. This is futher illustred in the barplot depicting the difference between the ratings based on revenue:  
![rev mpaa](https://github.com/cipalisoc/Project3/blob/main/Rev%20by%20MPAA.jpg?raw=true)

## Are movies that belong to a collection more popular than movies that don't belong to a collection?
### Null and Alternative Hypothesis
- Null Hypothesis: There is no difference in the polularity between movies that belong in a collection and those that don't belong in a collection
- Alternative Hypothesis: There a significant difference in the popularity between movies that belong in a collection than those that don't.
- significance value (alpha) = .05

To answer this question, a 2-sample t-test was performed with the following results: `Ttest_indResult(statistic=23.863302063378775, pvalue=3.2432966180925253e-112)`  
Since our p-value is < .05 (alpha), we reject the null hypothesis and accept that there is a significant difference in the popularity between movies that belong to a collection and those that don't belong to a collection. This conclusion further affirms the results in the barplot:
![movie pop](https://github.com/cipalisoc/Project3/blob/main/movie%20pop.jpg?raw=true)

## Do bigger budget films (>$10Million) have higher average votes than films with a lesser budget?
### Null and Alternative Hypothesis
- Null Hypothesis: There is no difference in the average votes between high-budget movies and low-budget movies
- Alternative Hypothesis: There a significant difference in the average votes between high-budget movies and low-budget movies
- significance value (alpha) = .05

![vodte_budget](https://github.com/cipalisoc/Project3/blob/main/vote_budget.jpg?raw=true)

Based on the graphic above, it isn't explicitly clear whether higher budget films have higher average votes (viewer film rating) than films with a lesser budget. We determined this with a 2-sample t-test and produced the following result:  
`Ttest_indResult(statistic=79.21185999428174, pvalue=0.0)`  
Since the p-value is less than the alpha level of 0.05, we can reject the null hypothesis and conclude that there is a significant difference between average votes of big-budget films and low-budget films.

## Do movies that are over 2.5 hours long earn more revenue than movies that are 1.5 hours long (or less)?
### Null and Alternative Hypothesis
- Null Hypothesis: There is no difference in the revenue between movies that are 2.5 hours or longer than movies that are 1.5 hours long or less
- Alternative Hypothesis: There a significant difference in the revenue between movies that are 2.5 hours or longer than movies that are 1.5 hours long or less
- significance value (alpha) = .05

![len_rev](https://github.com/cipalisoc/Project3/blob/main/len_rev.jpg?raw=true) 
As shown on the plot, it seems that movies whose runtime are 150 minutes or more earn higher revenues than movies with less than 150 minutes of runtime. We confirmed this using a 2-Sample t-test, which yielded the result,  
`Ttest_indResult(statistic=4.291900422607449, pvalue=2.0452739840729665e-05)`  
Thereby confirming that since the resulting p-value of the 2-sample t-test is less than our alpha of .05, can reject the null hypothesis and accept our alternative hypothesis that there is a significant difference in the revenues of long films (> 150 mins) versus short films (< 150 mins).

# Recommendations
Based on the data and results gathered, typically movies that are PG-13 or R rated, have budgets with $10 Million or more, belong to a collection, and have runtimes of more than 150 minutes have the higher earnings. A successful movie will cater to a more mature audience that we see as the typical summer-movie blockbuster. The most successful movie franchises follow this criteria as well including: Marvel movies, Mission: Impossible series, and The Fast and the Furious franchise just to name a few.

# For Further Information
For any additional questions, please contact:

- Chris Palisoc
- cipalisoc@gmail.com

