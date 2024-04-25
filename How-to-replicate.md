# Replicating work

## Web scraping
This section will provide the steps taken to import the necessary libraries required to scrape the [Wikipedia](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_Kingdom) website and insert this into a pandas dataframe

### Step 1: Import Libraries
![Screenshot 2024-04-25 at 12.37.43](https://hackmd.io/_uploads/rJ7seTvbC.png)

### Step 2: Fetch and parse HTML content from the webpage 
![Screenshot 2024-04-25 at 12.55.54](https://hackmd.io/_uploads/Hy4JS6wbR.png)

Set the variable `url` to the address of the webpage, in this case the "List of largest companies in the United Kingdom". 

Then using the requests library sen the HTTP GET request to the URL that contains the HTML content of the webpage. This line creates an instance of the `BeautifulSoup` object. 

The `soup` contains the parsed HTML, which allows for easier extraction and manipulation of data from the webpage.

### Step 3: Locating and working with HTML table elements

![Screenshot 2024-04-25 at 13.02.24](https://hackmd.io/_uploads/BkVwITw-0.png)

`soup.find` specifically looks for the firts table element in 'wikitable sortable'.

the `.find_all` method to find a table tag in the HTML content. to use the second table on the webpage we use [1] to make sure we only get the data for the table called **2022 Forbes list**.

### Step 4: Retrieve table headers 

![Screenshot 2024-04-25 at 13.03.20](https://hackmd.io/_uploads/BklYo6PWA.png)

`find_all` retrieves every instance where the `<th>` tag is used in the HTML. `<th>` is used to define a header in a table and they usually contain the titles of headings for columns in the table.

Then run `world_titles` to make sure that you have got the correct headers.

### Step 5: Extracting text from each header

![Screenshot 2024-04-25 at 13.05.08](https://hackmd.io/_uploads/SJ_bvTP-0.png)
`for title in world titles`  itterates through each element in world_titles. `title.text` this accesses the text content of `<th>` element. `.strip()` removes the trailing whitespace.
 
## Creating a database and Formatting data

### Step 1: Import pandas

![Screenshot 2024-04-25 at 13.06.00](https://hackmd.io/_uploads/SyqND6w-0.png)

### Step 2: Creating dataframe and inserting `world_table_titles`

![Screenshot 2024-04-25 at 13.07.12](https://hackmd.io/_uploads/ry7YwaDZA.png)

### Step 3: Inserting extracted data into dataframe 

![Screenshot 2024-04-25 at 13.08.35](https://hackmd.io/_uploads/BJzJuTv-R.png)
`.find_all` finds all instances of `<tr>` withing thr table. **tr** stands for table rows and these elements are found in the table are sorted in the variable **column_data**.

![Screenshot 2024-04-25 at 13.08.43](https://hackmd.io/_uploads/ry1eOpPZC.png)

`column_data[1:]` excludes the first element of the list as this shows as the first row of `individual_row_data` is empty.. otherwise it results in there being a row with mismatched columns error. 

for each row in the loop, it finds all `<td>` elements within the row. And then `row_data` stores these **td** elements as a list. 

`individual_row_data` uses a list comprehension to extract the text from each `<td>` element stored in row_data. The `.text` retrieves the sting inside each td element and '.string' again removes any whitespacefrom the strings.

`len(df)` calculates the number of rows curretly inside the dataframe which helps append the new data correctly. 

.loc[length] will then appends a new row to the DataFrame where 'individual_row_data' contains the cleaned data from each cell of the current table row in the loop. 

## Exporting data as a csv

![Screenshot 2024-04-25 at 13.12.23](https://hackmd.io/_uploads/S1FhOTvW0.png)

this converts the DataFrame into a CSV format. 
we have used the paramater `index = false` to remove the first column which are the numbers put in by the pandas DataFrame as we do not require them as the 'Rank' column lists each company for us. 




