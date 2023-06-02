# INST767-IM01
Big Data Infrastructure Group Project
Members: Matthew Chan and Ujwal Gupta
Google Drive Link: https://drive.google.com/drive/folders/1VLTDFLBGpHKMQrLtpcL5TEElze0EMNvs?usp=drive_link

# Recipe Recommender System

## Background and Motivation
For our INST767 project, we built a recipe recommender system. The dataset that we got from Kaggle was recipes from Food.com. This dataset contained at least 220,000 rows and 12 columns, some of which include name of recipe, minutes, tags, steps, description, and ingredients. For this project, we built 4 versions of the recipe recommender system. The first two versions consisted of Python programs that provide recommendations based on a certain set of criteria. Some examples of that criteria include time range, cuisine nationality type, and meal type. Versions three and four consisted of Python Programs that operate like a search engine. The user would type in the meal they want to make along with any other criteria, such as cooking time. The program would then output a set of recipes based on that search query. 
Our motivation for this project was that we sometimes have trouble deciding what dish we want to make. Maybe we're feeling bored with our current recipes and want to try new ones, but we're not sure where to begin. Hopefully with these programs, we can generate a list of recipes based on our preferences. 

## Data Cleanup and Analysis
When it comes to data cleaning, our first step was to remove the columns that we felt were not needed for the recommender system. This includes:

- Number of ingredients
- Number of steps 
- Contributor_id
- Submitted
- Nutrition
- ID

The reason why we decided not to use these columns was because we believed columns such as Contributer_id, Submitted, and ID would have no value as information to the user. We also felt columns such as Number of Ingredients and Number of Steps would not be useful criteria to add in our recipe recommender system because a user would not be interested or even know how many ingredients or steps they want in a recipe. We also removed the nutrition column because we didn’t find a use for it when recommending recipes at the time. If we do decide to use these columns, it will be for future work. 

After we removed these columns, we then created a new column that would put the cooking time in minutes into a time range, called Time_column. For example, Versions 1 and 2 we created an overarching filter of time ranges to be: less than two hours, two to 24 hours, and longer than 24 hours. We further split less than two hours to include:
- 15 minutes or less
- 15 to 30 minutes
- 30 to 45 minutes
- 45 to 60 minutes
- 1 to 2 hours

For Versions 3 and 4, since these two programs involve a search query rather than selecting preferences, the time ranges only include:
- 15 minutes or less
- 15 to 30 minutes
- 30 to 45 minutes
- 45 to 60 minutes
- 1 to 2 hours
- 2 to 24 hours
- Longer than a day

The reason why we created a time range column is to help group the minutes for easier filtering. For example, if the recipe takes over 3 hours, we thought a time range would be easier to understand and search for rather than the user entering an exact cooking time in minutes. 

We also created a meal_type column that extracts the type of meal from the list of tags in the recipe tags column already in the dataset. Examples of meal types include:
- Vegetarian
- Vegan
- Chicken
- Turkey
- Pork
- Beef
- Seafood
- Lamb
- Duck
- Wild-game
- Meat
- Eggs
- Dessert
- Beverages
- Condiments

The reason why we created a meal_type column was because we wanted to consider the potential dietary restrictions the user might have. For example, if someone was a vegetarian, Versions 1 and 2 would allow the user to filter to recipe options that are only vegetarian. Whereas for Versions 3 and 4, the user can enter the meal they want and also mention the dietary restriction they have in the search query. 

The Python library used for the time range column and the meal_type column was pandas; it was used to modify the original dataset to add these to new columns. 

The next column that we created was Nationality_type and the reason for creating this column was to show the nationality of where the dish was from. Some examples of Nationalities include:
- French European
- Greek European
- Japanese American 
- European Italian 
- North-American Canadian 

The Python library we used was spaCy, which can extract information such as name entities (Tripathi, 2020). What name entities mean is that it can basically recognize if a certain word in a sentence belongs to a category such as an organization or company (Tripathi, 2020). For example, for the sentence “Apple is a U.S. company”, spaCy recognizes that Apple is a company and that the U.S. is a country (Tripathi, 2020). For our project, we created some code that can read each item in that tag list and it would extract the item in the list where spaCy recognized as a country or region.

The last column we created was to get the sentiment of the reviews in the description column for each recipe. This column was called Sentiment (polarity) and the reason why we created this column was to develop a ranking system for the recipes that would be displayed when the user enters their query/preferences. Therefore, it would be ranked by how positive the sentiment was for the recipe. A polarity score that ranges from 0 to 1 shows that the sentiment is positive whereas a polarity score that ranges from -1 to 0 shows that the sentiment is negative. To get this polarity score, we used a Python library called TextBlob. 

## Problem Setup or Formulation

### Version 1
For our first challenge, we were wondering how we can match recipes based on a person’s preferences. For example, if someone preferred a recipe that was vegetarian and the cooking time was less than 45 minutes, how can we print out 10 or more recipes that have that criteria? The solution that we created was a program that asks the user a certain set of questions about their recipe preferences. Here, one of the questions involved asking the user their preferred time range. Once the user has entered all of their answers to the criteria, the program would display a result of recipes based on those answers. This result set of recipes were ranked by sentiment. 

### Version 2

Version 2 followed a similar process to Version 1, where the program would ask the user a set of questions. However, the difference between Version 1 and Version 2 is that in Version 1, every time a question was answered, the data frame would get filtered based on that answer. For example, if the user selected vegetarian for the question that asks for meal_type, the dataset would get filtered to only include vegetarian based dishes. Whereas for Version 2, the dataset would get filtered all at once at the end when all of the questions were answered. The end result of this is that we found that the result of the two versions were identical when using the same chosen answers; however, the run time for Version 2 was much faster since the dataset was only filtered once. This means that the better option for a recipe recommender system that is based on answering a set of questions was Version 2. 

Both Version 1 and Version 2 have a similar feature where it can catch when the user has entered a wrong input that does not match the criteria. For example if the user typed ‘J’ when the only options were ‘A’, ‘B’, ‘C’, and ‘D’, the program would tell the user that their input is incorrect. 

### Version 3
In Version 3, rather than having a program that limits the user to a certain set of criteria, we also wanted to create a recipe search engine that filters based on keywords or phrases from the user’s input. This version is not only ranked by sentiment polarity and by word matches, but also searches and matches each word in the user-input to each column in a row, for every row. For example, the user input of ‘Italian pizza less than 45 minutes’ displayed several rows with a cooking time greater than 45 minutes, and recipes like chocolate silk pie and seared wasabi tuna are not at all Italian pizza. The result was not accurate because this matched the word case anywhere in the row without making any sense. 

### Version 4
Since Version 3's results were not accurate, we now wanted to create a better search engine that would produce better results. This led us to use Whoosh in our 4th and final version. According to the Whoosh ReadMe document, Whoosh is a search engine based Python library and the creator of Whoosh says that it takes inspiration from Lucene (“Introduction to Whoosh”, n.d.). The way it works is that it creates an index of the columns that you want to be searched. In our case, it was every column in our cleaned dataset except the review column. After the columns were indexed and the user entered their search query, Whoosh would then try to match the words in the search query with the index. The result of using Whoosh in Version 4 in comparison to Version 3 was a more accurate output of the recipes that made more sense. For example, the query was “Italian pizza less than 45 minutes”. The output of this query was that all of the recipes seem to be pizza related based on the recipe titles, all of the minutes were under 45 minutes, and most of the nationalities were Italian. It also had the same ranking system where the recipes were ranked by sentiment. The only issue that we faced with Version 4 was that our program’s runtime was quite long, taking roughly 5 minutes. This could be due to our dataset being very large since it has more than 230,000 rows and Whoosh had to index all of those rows every time the program ran.  

In all of the versions, two features remained the same. The first feature was the ranking by Sentiment and the second feature was a while loop that would continue to ask the user their preferences or search query for another recipe. The reason why we added this while loop was because if they wanted to search for more recipes, this would give the user the option to do so without having to re-run the program. If the user wants to end the program, they can just type ‘stop’.

## Lessons Learned
When it comes to lessons, the first lesson that we learned was learning how to build a recommender system. This is something that we have not done before and with the feedback we received from the Professor and with the four versions that we developed, we now have a better understanding of how a search engine should work. We also learned about Whoosh and that was a great experience on learning a library that's designed for searching items. And although we didn’t have time to implement a console to display the recipe list, we did learn about how to use TKinter, which is a Python Library for graphical user interface (GUI).

## Next Steps/Future Plan
For our next steps or future plan, we plan to add more features like recommending by main ingredient or recommending similar recipes based on the same/new criteria. We also want to build a console to display the results using a webpage or TKinter. We were able to begin creating a starting interface, but we found it challenging to integrate the interface with our queries and with the remaining time we had left. We would also like to learn and use Cosine Similarity for the recommender system because we think this could help in recommending similar recipes based on ingredients. And finally, although we didn’t have enough time to review and implement some suggestions from our peer reviews of the proposal for this project, we hope to incorporate them in the future. One great idea was using a Python library called Fuzzywuzzy, which would help eliminate the issue with spelling mistakes in the user input. 

### Works Cited 
Using the Raw Recipe csv: RAW_recipes.csv
- https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?resource=download 
- https://towardsdatascience.com/named-entity-recognition-ner-using-spacy-nlp-part-4-28da2ece57c6
- https://whoosh.readthedocs.io/en/latest/intro.html




