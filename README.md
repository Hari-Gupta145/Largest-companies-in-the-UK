# Largest companies in the UK according to the Forbes Global 2000 in 2022

## Overview:
This project aims to scrape and analyse data from wikipedia about the top firms in the UK according to the Forbes Global 2000 rankings from 2022. I've chosen this data as it is the first complete fiscal year since the Covid-19 pandemic and shows us how companies and industries in the UK will recover and grow in the future. The aim is to provide investors with a better idea of what the industry looks like now and how they could invest their money. 

## Set up 
For this project JupyterNotebook was used through downloading anaconda and was all written on python3. `BeautifulSoup` which is used to parse HTML and XML documents. and `requests` which is used to send HTTP requests using Python.

To clean the data I have used sqlite3 to better visualise and manipulate the table of data, which can be seen on largest_companies_UK_SQL.txt. 

Then to create the visualisations libraries that were used are: `pandas` used for data manipulation and analysis, `NumPy` used for large, multi-dimensional arrays and matrices, `Seaborn` which is a data visualisation library based on matplotlib, and finally `pyplot` which aids in plotting graphs and data visualisation.

## Replicating 
### Web scraping
This section will provide the steps taken to import the necessary libraries required to scrape the [Wikipedia](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_Kingdom) website **List of largest companies in the United Kingdom** and insert this into a pandas dataframe

#### Step 1: Import Libraries

```python
from bs4 import Beautiful soup 
import requests
```

#### Step 2: Fetch and parse HTML content from the webpage 
```python
url = "https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_Kingdom"

page = requests.get(url) 

soup = BeautifulSoup(page.text, 'html')
```

Set the variable `url` to the address of the webpage, in this case the "List of largest companies in the United Kingdom". 

Then using the requests library sen the HTTP GET request to the URL that contains the HTML content of the webpage. This line creates an instance of the `BeautifulSoup` object. 

The `soup` contains the parsed HTML, which allows for easier extraction and manipulation of data from the webpage.

#### Step 3: Locating and working with HTML table elements

```python
soup.find('table', class = 'wikitable sotable')
table = soup.find_all('table')[1]
print(table)
```
`soup.find` specifically looks for the firts table element in 'wikitable sortable'.

the `.find_all` method to find a table tag in the HTML content. since all tables in the HTML are called 'wikitable sortable' we have to specify which table we want to extract data from. To use the second table on the webpage we use **[1]** in the second line to make sure we only get the data for the table called **2022 Forbes list** on the wikipedia page.

#### Step 4: Retrieve table headers 
```python
world_titles = table.find_all('th')

world_titles
```
`find_all` retrieves every instance where the `<th>` tag is used in the HTML. `<th>` is used to define a header in a HTML table and they usually contain the titles of headings for columns in the table.

Then run `world_titles` to make sure that you have got the correct headers.

#### Step 5: Extracting text from each header

```python
world_table_titles = [title.text.strip() for title in world_titles]

print(world_table_titles)
```
`for title in world_titles`  itterates through each element in world_titles. `title.text` this accesses the text content of `<th>` element. `.strip()` removes the trailing whitespace.
 
### Creating a database and Formatting data
#### Step 1: Import pandas

```python
import pandas as pd
```

#### Step 2: Creating dataframe and inserting `world_table_titles`
```python
df = pd.DataFrame(columns = world_table_titles)
df
```

#### Step 3: Inserting extracted data into dataframe 

```python
column_data = table.find_all('tr')
```
`.find_all` finds all instances of `<tr>` within the table. `<tr>` stands for table rows and these elements are found in the table are sorted in the variable **column_data**.

```python
for row in column_data[1:]:
    row_data = row.find_all('td')
    individual_row_data = [data.text.strip() for data in row_data]
    
length = len(df)
df.loc[length] = individual_row_data
```
`column_data[1:]` excludes the first element of the list as this shows as the first row of *individual_row_data* is empty. otherwise it results in there being a row with mismatched columns error. 

for each row in the loop, it finds all `<td>` elements within the row. And then `row_data` stores these `<td>` elements as a list. 

`individual_row_data` uses a list comprehension to extract the text from each `<td>` element stored in row_data. The `.text` retrieves the sting inside each td element and '.string' again removes any whitespacefrom the strings.

`len(df)` calculates the number of rows curretly inside the dataframe which helps append the new data correctly. 

`.loc[length]` will then appends a new row to the DataFrame where 'individual_row_data' contains the cleaned data from each cell of the current table row in the loop. 

### Exporting data as a csv

```python
df.to_csv('/path/to/largest_UK_companies.csv', index = False)
```
This converts the DataFrame into a CSV format. 
we have used the paramater `index = false` to remove the first column which are the numbers put in by the pandas DataFrame as we do not require them as the 'Rank' column lists each company already. 


## Link to GitHub Repository
[Click Here 
](https://github.com/Hari-Gupta145/Largest-companies-in-the-UK)
