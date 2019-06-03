---
layout: post
title:      "Exploratory Data Analysis - King County Housing Prices"
date:       2019-06-03 18:04:03 +0000
permalink:  exploratory_data_analysis_-_king_county_housing_prices
---


About two years ago, my husband and I bought our first house.  He had bought a townhome previously, but I was a first-time buyer.  If you have ever watched the HGTV network, you know that first-time homebuyers almost always overestimate their buying power and I definitely fell into that category.  I had thought our budget would afford us all of the features that I wanted in our new home – a big fenced-in yard, plenty of bedrooms and baths, spacious living areas --- the works.

I quickly came to realize that either I was going to pay a lot more for all the things I wanted in a house or I was going to have to choose which features were most important to me and stick within our budget.   It turns out hardwood floors and marble countertops are not all that important to me!

I didn’t know it at the time, but as I sorted through our future home’s various features, I was doing some (very primitive) exploratory data analysis.  I was intuitively eliminating features or certain neighborhoods that I knew would push the price of the house over our budget.  

For the King County Housing project, I did the same exploratory data analysis (EDA), but on a much larger scale.  King County is in Washington state and has a population of about 2.2 million people.  The goal of the project was to create a regression model that would predict a home price, based on a set of given features.  The original dataset had details for 21,597 home sales in the area.  Each record included the following: 

•	id - unique identified for a house
•	dateDate - house was sold
•	pricePrice - is prediction target
•	bedroomsNumber - of Bedrooms/House
•	bathroomsNumber - of bathrooms/bedrooms
•	sqft_livingsquare - footage of the home
•	sqft_lotsquare - footage of the lot
•	floorsTotal - floors (levels) in house
•	waterfront - House which has a view to a waterfront
•	view - Has been viewed
•	condition - How good the condition is ( Overall )
•	grade - overall grade given to the housing unit, based on King County grading system
•	sqft_above - square footage of house apart from basement
•	sqft_basement - square footage of the basement
•	yr_built - Built Year
•	yr_renovated - Year when house was renovated
•	zipcode - zip
•	lat - Latitude coordinate
•	long - Longitude coordinate
•	sqft_living15 - The square footage of interior housing living space for the nearest 15 neighbors
•	sqft_lot15 - The square footage of the land lots of the nearest 15 neighbors


After an initial cleaning, I set out to answer three questions: 
•	How does the price of renovated homes compare with the price of new homes built the same year?
•	Is there an optimal time to sell?
•	Do more views increase the price of a house?
By answering these questions, I would look at how individual variables affected our target variable and ultimately determine if they would be useful in the final model. 

#### New vs. Renovated Homes

During data cleaning, the yr_renovated column had some null values that had to be replaced.  Using the value_counts method, I found that 20,853 home out of 21,597 had not been renovated.  In order to get an accurate picture of how the price of new vs. renovated homes compared, I created a new dataframe dropping the records where a home had not been renovated.  I did this by isolating records where `yr_renovated` value was equal to 0.0.  Once I had the records isolated, I used ```.drop()``` to remove them from our dataframe and defined this as a new variable . After the new dataframe was created, I grouped the records using `.groupby('yr_built')['price'].mean()` and plotted the average price per year, using Matplotlib.Plotly.  I did the same thing for year renovated.
 
As expected, new homes typically demand a higher price than renovated homes.  The only times this was not true was after a severe housing crash, such as the 1980s and early 2000s.  

#### Best Time to Sell

To determine the best time to sell, I looked at how many home sales occurred on a given date or in a given month.  To do this, I created an empty dictionary, then used a for-loop to iterate over the list of home sale dates from our dataset.  For each iteration, the `.get()` function would check to see if the date was already in the dictionary.  If there was no entry for the date, a new key would be added to the dictionary with a default value of 0. Once the key was added to the dictionary, a counter would add 1 to the value each time the date appeared. 

```
day_of_sales = {}
for i in list(df.date.dt.day):
    day_of_sales[i] = day_of_sales.get(i, 0) + 1 `
```
 
Once the dictionary was created, it was graphed using a barplot from the Seaborn library to visually see which dates occurred most frequently.  I repeated the same process for months sold.  
 
#### Views per House

For my final question, I wanted to know if a house that had more showings resulted in a higher sold price.  I used a barplot from the Seaborn library to visualize the average price for homes that had been viewed 0, 1, 2, 3, and 4 times.  I set the independent variable (views) along the x-axis and our dependent variable (price) along the y-axis.  I also used a boxplot from the Seaborn library to determine if any outliers might be skewing the results. 
 
Our graphs a clear positive relationship between multiple views and higher home prices, even with outliers across all values.


EDA is an important part of building the final model.  It allowed us to investigate our data and become familiar with what it contains.  For this project, our preliminary analysis looked at how individual variables affected price, whereas our model considered multiple variables together.  Our analysis also answered questions that were not directly related to price by looking at the frequency of home sales per date and month. EDA allows you to explore relationships that you may not have anticipated between variables and narrow your focus, so you can provide meaningful insights into your data and answer questions that are interesting to you or your company. 

