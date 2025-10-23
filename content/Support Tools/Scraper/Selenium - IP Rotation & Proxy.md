---
title: Selenium - IP Rotation & Proxy
date: 2025-10-23
tags:
  - scraper
  - ui
  - tools
  - selenium
  - python
category: Support Tools - Scraper
status: in_corso
author: Te3sk
description: Breve descrizione del contenuto del documento.
---
# What Is a Selenium Proxy?
A proxy acts as an intermediary between a client and a server. Through it, the client makes requests to other servers anonymously and securely and avoids geographical restrictions.

Headless browsers can be configured to use proxy servers like HTTP clients. **A proxy helps protect your IP address and avoid blocks when [scraping protected websites, like Amazon, with Selenium](https://www.zenrows.com/blog/scraping-amazon-selenium).**

Using Selenium with proxy is particularly useful for browser automation activities such as testing and web scraping. Keep reading to learn how to set up a proxy in Selenium for web scraping!
# How to Set Up a Proxy in Selenium
Let's start by setting up a basic Python script to control Chrome with Selenium.

The snippet below initializes a headless Chrome driver and visits [httpbin](https://httpbin.io/ip), a webpage that returns the IP address of the client making the request. Finally, the script prints the response HTML.
```python title="scraper.py"
# pip install selenium webdriver-manager
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.options import Options

# set Chrome options to run in headless mode
options = Options()
options.add_argument("--headless=new")

# initialize Chrome driver
driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install()), 
    options=options
)

# navigate to the target webpage
driver.get("https://httpbin.io/ip")

# print the HTML of the target webpage
print(driver.page_source)

# release the resources and close the browser
driver.quit()
```
