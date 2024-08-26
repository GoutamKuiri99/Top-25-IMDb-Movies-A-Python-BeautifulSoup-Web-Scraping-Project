# Displaying Top Rated 25 Movies from IMDB
IMDb (Internet Movie Database) is one of the most popular  movie rating platforms. 
In this project, we will utilize BeautifulSoup to extract IMDb’s top 25 highest-rated
movies, showcasing their titles, ratings, and release years.

![image](https://github.com/user-attachments/assets/e969bc21-3bf3-43b7-8aa3-460e20647a13)


## Importing Required Libraries

To begin, we need to import the necessary libraries for web scraping and data manipulation.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```


Next, we will fetch the IMDb Top 25 Movies page and parse it using BeautifulSoup.

```python
url = 'https://www.imdb.com/chart/top/'
r = requests.get(url)
print(r)

soup = BeautifulSoup(r.text, 'lxml')
print(soup)
```
## Handling 403 Forbidden Errors

When you encounter a 403 Forbidden response, it typically indicates that the server is rejecting your request. This can occur if the server detects automated activity or if your request headers do not closely resemble those of a real browser.

### Methods to Bypass a 403 Error

To overcome a 403 Forbidden error and successfully scrape data with BeautifulSoup, you can try the following methods:

### 1. Use a Realistic User-Agent and Headers

Servers often check the `User-Agent` and other headers to determine if the request is coming from a legitimate browser. You can mimic a real browser by setting these headers.

Here is an example of how to include headers in your request:

```python
import requests
from bs4 import BeautifulSoup

# URL for IMDb's top-rated movies
url = 'https://www.imdb.com/chart/top/'

# Define headers to mimic a real browser
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36',
    'accept-language': 'en-US,en;q=0.9',
    'referer': 'https://www.imdb.com/chart/top/',
    'Connection': 'keep-alive'
}

# Send a GET request to the website with headers
r = requests.get(url, headers=headers)

# Print the status code to check if request is successful
print(r.status_code)

# Parse the page content with BeautifulSoup
soup = BeautifulSoup(r.text, 'lxml')
print(soup)
```

## Web Scraping IMDb's Top 25 Movies

The following Python script uses BeautifulSoup to scrape IMDb's Top 25 Movies and extracts information such as movie names, years, durations, ratings, and vote counts.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Lists to store the scraped data
Movie_Name = []
Year = []
duration = []
Rated = []
Ratings = []
Rating_Votecount = []

# URL of IMDb's top-rated movies page
url = 'https://www.imdb.com/chart/top/'

# Headers to mimic a real browser request
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36',
    'accept-language': 'en-US,en;q=0.9',
    'referer': 'https://www.imdb.com/chart/top/',
    'Connection': 'keep-alive'
}

# Send a GET request to the IMDb URL
r = requests.get(url, headers=headers)
print(f"Status Code: {r.status_code}")

if r.status_code == 200:
    # Parse the page content with BeautifulSoup
    soup = BeautifulSoup(r.text, 'lxml')

    # Extract movie names
    Name = soup.find_all('h3', class_='ipc-title__text')[1:26]
    for i in Name:
        n = i.text
        Movie_Name.append(n)

    # Extract years
    Year_elements = soup.find_all('div', class_='sc-b189961a-7 btCcOY cli-title-metadata')[:25]
    for item in Year_elements:
        sub_elements = item.find_all('span', class_='sc-b189961a-8 hCbzGp cli-title-metadata-item')[:25]
        if sub_elements:
            First_element = sub_elements[0].text.strip()
            Year.append(First_element)

    # Extract durations
    durations = soup.find_all('div', class_='sc-b189961a-7 btCcOY cli-title-metadata')[:25]
    for item in durations:
        sub_elements = item.find_all('span', class_='sc-b189961a-8 hCbzGp cli-title-metadata-item')[:25]
        if sub_elements:
            Middle_element = sub_elements[1].text.strip()
            duration.append(Middle_element)

    # Extract ratings
    items = soup.find_all('div', class_='sc-b189961a-7 btCcOY cli-title-metadata')[:25]
    for item in items:
        sub_elements = item.find_all('span', class_='sc-b189961a-8 hCbzGp cli-title-metadata-item')[:25]
        if sub_elements:
            last_element = sub_elements[-1].text.strip()
            Rated.append(last_element)

    # Extract rating scores
    Rating = soup.find_all('span', class_='ipc-rating-star--rating')[:25]
    for i in Rating:
        p = i.text
        Ratings.append(p)

    # Extract vote counts
    vote_count_elements = soup.find_all('span', class_='ipc-rating-star--voteCount')[:25]
    for i in vote_count_elements:
        text = i.text.replace('(', "").replace(')', '').strip()
        Rating_Votecount.append(text)

# Create a DataFrame with the extracted data
data = {
    'Movie_Name': Movie_Name,
    'Year': Year,
    'Duration': duration,
    'Rated': Rated,
    'Ratings': Ratings,
    'Rating_Votecount': Rating_Votecount
}

df = pd.DataFrame(data)

# Display the DataFrame
print(df)
```
## Output

The following table displays the top 25 movies from IMDb, including details such as movie names, release years, durations, ratings, and vote counts. The data is extracted and presented in a tabular format using Pandas.

![Top 50 IMDb Movies Output](https://github.com/user-attachments/assets/312a72bd-37a6-49c0-a032-ecb40d99b709)

*Note: The displayed output showcases the results obtained from scraping IMDb's top 25 movies page.*


**Thank you for your interest and time. Feel free to give your valuable suggestions and connect with me**   
https://www.linkedin.com/in/goutam-kuiri-949b632a6
