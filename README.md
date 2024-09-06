{\rtf1\ansi\ansicpg1252\cocoartf2761
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0

\f0\fs24 \cf0 # Amazon Web Scraping Using Python\
\
## Introduction\
\
This project demonstrates how to scrape product data (such as title and price) from Amazon using Python. It automates the process of collecting price data over time and stores it in a CSV file, making it suitable for tracking price changes and trends.\
\
## Objective\
\
The main objectives of this project include:\
- Scraping product information (e.g., title, price) from an Amazon product page.\
- Appending the scraped data, along with the date, into a CSV file.\
- Automating the scraper to run at regular intervals to track price changes over time.\
\
---\
\
## Tools Used\
\
- **Python:** Primary language used for scraping and data processing.\
- **BeautifulSoup:** Library used for web scraping and parsing HTML data.\
- **Requests:** HTTP library to send requests to Amazon.\
- **Pandas:** For reading and writing CSV files.\
- **CSV Module:** For handling CSV operations.\
\
---\
\
## Key Steps in the Project\
\
### 1. Web Scraping Code\
\
The following code extracts the product title and price from an Amazon product page. It then saves the information to a CSV file.\
\
```python\
# Import necessary libraries\
import requests\
from bs4 import BeautifulSoup\
import csv\
from datetime import datetime\
\
# Define the URL of the product and headers to mimic a browser request\
url = 'https://www.amazon.com/product-link'\
headers = \{"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"\}\
\
# Function to scrape product title, price, and save to CSV\
def check_price():\
    page = requests.get(url, headers=headers)\
    soup1 = BeautifulSoup(page.content, "html.parser")\
\
    # Extract product title\
    title = soup1.find(id="productTitle").get_text().strip()\
\
    # Extract the whole part and fractional part of the price\
    whole_price = soup1.find("span", \{"class": "a-price-whole"\}).get_text().strip()\
    fractional_price_element = soup1.find("span", \{"class": "a-price-fraction"\})\
    \
    # If fractional price is missing, use "00"\
    if fractional_price_element:\
        fractional_price = fractional_price_element.get_text().strip()\
    else:\
        fractional_price = "00"\
    \
    # Combine whole and fractional price\
    price = f"\{whole_price\}.\{fractional_price\}".strip()\
\
    # Add the current date\
    today = datetime.today().strftime('%Y-%m-%d')\
\
    # Append data to the CSV\
    data = [title, price, today]\
    with open('AmazonWebScraperDataset.csv', 'a+', newline='', encoding='utf-8') as f:\
        writer = csv.writer(f)\
        writer.writerow(data)\
    \
    # Print the scraped data for verification\
    print(f"Data scraped and saved: \{data\}")\
```\
\
### 2. Output Example\
\
When you run the `check_price()` function, it extracts the product title, price, and date, and saves them in the CSV. Below is an example of the output printed to the console:\
```plaintext\
Data scraped and saved: ['Apple iPhone 15 (128 GB) - Blue', '687.00', '2024-09-05']\
```\
### 3. Automating the Scraper\
\
The following code runs the scraper at set intervals (e.g., once a day). For testing purposes, you can adjust the interval to a shorter duration (e.g., 2 seconds).\
```python\
# Python code here\
\
import time\
\
# Run the scraper every 24 hours (86400 seconds)\
while True:\
    check_price()\
    time.sleep(86400)  # Waits for 24 hours before running the scraper again\
```\
\
### 4. Saving Data to CSV\
\
All scraped data (product title, price, and date) is saved in the `AmazonWebScraperDataset.csv` file. Each row contains the following data:\
- **Product Title:** The name of the product.\
- **Price:** The current price of the product.\
- **Date:** The date when the data was collected.\
\
### 5. ### Example CSV Output\
\
Below is a sample output of the CSV file:\
\
| Product                         | Price  | Date       |\
|----------------------------------|--------|------------|\
| Apple iPhone 15 (128 GB) - Blue  | 687.00 | 2024-09-05 |\
| Apple iPhone 15 (128 GB) - Blue  | 687.00 | 2024-09-06 |\
| Apple iPhone 15 (128 GB) - Blue  | 687.00 | 2024-09-07 |\
\
\
### 6. Reading the CSV Data\
\
You can use the following code to read and print the contents of the CSV file:\
```python\
# Python code here\
\
import pandas as pd\
\
# Read the CSV file and print the data\
df = pd.read_csv('AmazonWebScraperDataset.csv')\
print(df)\
```\
---\
\
## Results & Insights\
\
- The scraper successfully collects product data over time and stores it in a CSV file.\
- This project can be expanded to track price trends or detect price drops for specific products.\
\
## Future Improvements\
\
- Track additional product details such as reviews and ratings.\
- Add email alerts when prices drop below a specific value.\
- Build a dashboard using Matplotlib or Plotly to visualize price trends.\
}