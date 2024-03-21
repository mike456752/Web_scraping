import time
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By
import pandas as pd

def scrape_all_reviews(url):
    options = webdriver.ChromeOptions()
    options.add_argument("--headless")  # Run Chrome in headless mode (no GUI)
    driver = webdriver.Chrome(options=options)

    all_reviews = []
    page_number = 1

    try:
        while True:
            page_url = f"{url}?pageNumber={page_number}"
            driver.get(page_url)
            time.sleep(5)  # Add a delay to allow dynamic loading (adjust as needed)

            soup = BeautifulSoup(driver.page_source, "html.parser")
            review_elements = soup.find_all("div", class_="a-section review aok-relative")

            if not review_elements:
                break

            for element in review_elements:
                review_text = element.find("span", class_="a-size-base review-text review-text-content").text.strip()
                rating_element = element.find("span", class_="a-icon-alt") or element.find("i", class_="review-rating")
                rating = rating_element.text.strip() if rating_element else "N/A"
                date = element.find("span", class_="a-size-base a-color-secondary review-date").text.strip()
                review_data = {
                    "text": review_text,
                    "rating": rating,
                    "date": date
                }
                all_reviews.append(review_data)

            page_number += 1
    finally:
        driver.quit()

    return all_reviews

url = "https://www.amazon.com/product-reviews/B08KGLMB1L"
all_reviews = scrape_all_reviews(url)

df = pd.DataFrame(all_reviews)

df
