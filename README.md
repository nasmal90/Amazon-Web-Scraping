# 📊 Amazon Web Scraping Using Python

## 📝 Introduction
This project demonstrates how to scrape product data (such as title and price) from Amazon using Python. It automates the process of collecting price data over time and stores it in a CSV file. This approach is perfect for tracking price changes, analyzing trends, and making informed purchasing decisions.

---

## 🎯 Objectives
- **Scrape product details** like title, price, and more from Amazon product pages.
- **Store scraped data** in a CSV file with corresponding timestamps.
- **Automate the scraper** to track price fluctuations over time.

---

## 🛠️ Tools & Libraries Used
- **Python**: The main programming language used for web scraping and automation.
- **BeautifulSoup**: To parse and extract data from HTML.
- **Requests**: For sending HTTP requests to Amazon product pages.
- **Pandas**: For reading, processing, and saving CSV data.
- **CSV Module**: For handling CSV operations efficiently.

---

## 🚀 Key Steps

### 1. Scraping Product Data
We first retrieve product details from an Amazon page using `BeautifulSoup`. Here's the core snippet of the scraping logic:

```python
import requests
from bs4 import BeautifulSoup
import csv
from datetime import datetime

# URL of the product to be scraped
url = 'https://www.amazon.com/product-link'
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"}

# Function to scrape the product title, price, and save them to CSV
def check_price():
    page = requests.get(url, headers=headers)
    soup = BeautifulSoup(page.content, "html.parser")

    # Extract product title
    title = soup.find(id="productTitle").get_text().strip()

    # Extract price data (whole part and fractional part)
    whole_price = soup.find("span", {"class": "a-price-whole"}).get_text().strip()
    fractional_price = soup.find("span", {"class": "a-price-fraction"}).get_text().strip() or "00"

    price = f"{whole_price}.{fractional_price}"

    # Get current date
    today = datetime.today().strftime('%Y-%m-%d')

    # Save data to CSV
    data = [title, price, today]
    with open('AmazonWebScraperDataset.csv', 'a+', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerow(data)

    print(f"Data scraped and saved: {data}")
```
### 2. Sample Output
When you run the check_price() function, it captures and saves the product details as shown below:
```
Data scraped and saved: ['Apple iPhone 15 (128 GB) - Blue', '687.00', '2024-09-05']
```
### 3. Automating the Scraper
To track the price over time, we can automate the scraper to run daily or at any defined interval. The following code demonstrates how to run it every 24 hours:
```
import time

while True:
    check_price()
    time.sleep(86400)  # Wait for 24 hours (86400 seconds) before running again
```
### 4. Storing Data in CSV
Each time the scraper runs, it appends the product title, price, and date to a CSV file (AmazonWebScraperDataset.csv). The file structure is as follows:

| Product                         | Price  | Date       |
|----------------------------------|--------|------------|
| Apple iPhone 15 (128 GB) - Blue  | 687.00 | 2024-09-05 |
| Apple iPhone 15 (128 GB) - Blue  | 687.00 | 2024-09-06 |
| Apple iPhone 15 (128 GB) - Blue  | 687.00 | 2024-09-07 |

### 5. Reading CSV Data
To review or further analyze the stored data, use Pandas to read and print the CSV file contents:
import pandas as pd
```
# Load and display the CSV file
df = pd.read_csv('AmazonWebScraperDataset.csv')
print(df)
```
## 📈 Results & Insights
- The scraper efficiently collects and stores product information over time.
- This system can be extended to monitor price fluctuations or detect price drops.
## 🔮 Future Enhancements
- Add reviews and ratings: Expand the scraper to capture additional details like product reviews.
- Email Alerts: Set up notifications when prices fall below a specified threshold.
- Data Visualization: Use tools like Matplotlib or Plotly to visualize price trends and gain better insights.
