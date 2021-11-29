Data Extraction and Cleaning with Pandas Project

For this proyect I will scrape a stocks information page to extract all Tickers from public companies available. I will then use an API to gather 6 months of stock price history and the industry sector for each company. Whit this information I'm looking to get a list of companies with historic stock performance that I can later use. Throughout the proyect I will be saving data onto CVSs to avoid re-running the API calls and re-scraping the website.

Approach

1. Scraping https://companiesmarketcap.com/ for information about public companies.

I will be using selenium as data needs to load prior to being able to extract it. Using requests on its own will not work.

Given the structure of the website, infomation about companies is represented in 58 differente pages, thus I will use an array to accumulate a dataframe for each page and then concatenate all into one.

After concatenating all dataframes I will do some cleaning to obtain relevant data. The original "Name" column actually contains both the name of the company and it's "Ticker" or ("Symbol"). I will need these to be on two different columns so I can actually use the symbol on my API requests. Additionally, I will need to do some cleaning on the "Market Cap" and "Price" columns so I can convert these columns into numbers (float).

On the "Market Cap" column I notice there is a letter representing the magnitude of the number. I will also need to extract this so I can later have a "Market Cap" Column with an absolute number (not abbreviated).

After cleaning, creating new columuns, and dropping unnecesary columns. I save this as a CSV (./output/stocks.csv)

2. Complementing information with API.

I will take all symbols or Tickers from the scraped data and feed these into an API so I can get historic stock price data.

The API I will be using is: yahoofinanceapi.com

For the API calls I will be using "time" to ensure I do not exceed the 300 calls/minute threshold and will be using try and except in case a symbol is not included in the API.

After my API calls are done and I have saved the information in a csv. I proceed to merge both files (1. the scraped data and 2. The data extracted from the API)

output file: ./output/merged.csv

Looking at the data, It's becoming a bit complicated to get anything meaningfull on the historic prices. Actually what I want to know is what stocks are the big winners, and big loosers. Eventually understand any correlations between these... i.e. is there a stock that when its a big winner other stock is a big looser?

As a starting point I want to understand both absolute and % increase/decrease of stock price in the last 6 months. Note: API data includes 6 months history.
To do this I will use melt to pivot date columns into a single column. This will allow me to more easily perform analyses on stock prices.

After melting I will sort to make sure information is displayed with date ascending.

3. Applying functions to data so we can get the 6-month-gain (absolute and percent) and a function that can get the gain between any two differente months.

After applying formulas and given they do take a while to run, I save the updated dataframe in a new file.

output file: ./output/merged_with_formulas.csv

5. Extracting sector data from API. While individual stock analyses is valuable, sometimes whole industries are affected or might be correlated, thus I want to add the industry sector for each stock.

output file: ./output/sectors.csv

I then merge the new sector data onto our merged dataframe and save it to a file.

output file: ./output/full_integration.csv

6. Up until this point it seems we have a pretty comprehensive data base of publick stock, nonetheless it still needs some transformations... Transformations (qcut + dummies)

I perform a qcut of 5 bins and rename these as big loss, loss, neutral, win, big win and create a column (6mo Veredict)

I take both 6mo Veredict and Sectors and convert these into dummy variables.

Final output: ./output/final_extract.csv
