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
# Requirements
Note: If you haven't upgraded to Selenium 4 yet, do it, since WebDriver comes built-in with the latest versions. You can verify your current version using `pip show selenium` and upgrade to the newest version with `pip install --upgrade selenium`.
# How to Set Up a Proxy in Selenium
Let's start by setting up a basic Python script to control Chrome with Selenium.

The snippet below initializes a headless Chrome driver and visits [httpbin](https://httpbin.io/ip), a webpage that returns the IP address of the client making the request. Finally, the script prints the response HTML.
```python title="scraper.py" {7-9}
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

The code will print the following HTML:
```html title="Output"
<html>
	<head>
		<meta name="color-scheme" content="light dark"><meta charset="utf-8">
	</head>
	<body>
		<pre>
			{
			  "origin": "50.217.226.40:80"
			}
		</pre>
		<div class="json-formatter-container"></div>
	</body>
</html>
```
You're now ready to set up your Selenium proxy in Python using the Chrome driver.

To use Selenium proxy, you need to:
1. Retrieve a valid proxy server.
2. Specify it in the [`--proxy-server`](https://www.chromium.org/developers/design-documents/network-settings/) Chrome option.
3. Visit your target page.

Let's go over the whole process step-by-step.

First, get a free proxy address from the [Free Proxy List](https://free-proxy-list.net/) website. Configure Selenium with [`Options`](https://www.selenium.dev/documentation/webdriver/drivers/options/) to launch Chrome using a proxy. Then, print the body content of the target webpage.

```python title="scraper.py" {8, 10-13}
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By

# define the proxy address and port
proxy = "20.235.159.154:80" 

# set Chrome options to run in headless mode using a proxy
options = Options()
options.add_argument("--headless=new")
options.add_argument(f"--proxy-server={proxy}")

# initialize Chrome driver
driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install()),
    options=options
)

# navigate to the target webpage
driver.get("https://httpbin.io/ip")

# print the body content of the target webpage
print(driver.find_element(By.TAG_NAME, "body").text)

# release the resources and close the browser
driver.quit()
```
The controlled instance of Chrome will now perform all requests through the specified proxy.
Here's what it'll return:
```json title="Output"
{
  "origin": "20.235.159.154:80"
}
```
## Selenium Proxy Authentication
Some proxy servers rely on authentication to restrict access to users without valid credentials. That's usually the case with commercial solutions or premium proxies.

The Selenium syntax to specify a username and password in an authenticated proxy URL looks like this:
```python title="scraper.py"
<PROXY_PROTOCOL>://<YOUR_USERNAME>:<YOUR_PASSWORD>@<PROXY_IP_ADDRESS>:<PROXY_PORT>
```
However, using a URL in `--proxy-server` won't work because the Chrome driver ignores the username and password by default. That's where a third-party plugin, such as [`Selenium Wire`](https://github.com/wkeeling/selenium-wire), comes to the rescue.

Selenium Wire extends Selenium to give you access to the requests made by the browser and change them as desired. Run the command below to install it:
```bash title="Terminal"
pip install blinker==1.7.0 selenium-wire
```

Use Selenium Wire for proxy authentication, as shown below:
```python title="scraper.py" {7-11, 13-14, 16-21, 25}
from seleniumwire import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By

# configure the proxy
proxy_username = "<YOUR_USERNAME>"
proxy_password = "<YOUR_PASSWORD>"
proxy_address = "20.235.159.154"
proxy_port = "80"

# formulate the proxy url with authentication
proxy_url = f"http://{proxy_username}:{proxy_password}@{proxy_address}:{proxy_port}"

# set selenium-wire options to use the proxy
seleniumwire_options = {
    "proxy": {
        "http": proxy_url,
        "https": proxy_url
    },
}

# set Chrome options to run in headless mode
options = Options()
options.add_argument("--headless=new")

# initialize the Chrome driver with service, selenium-wire options, and chrome options
driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install()),
    seleniumwire_options=seleniumwire_options,
    options=options
)

# navigate to the target webpage
driver.get("https://httpbin.io/ip")

# print the body content of the target webpage
print(driver.find_element(By.TAG_NAME, "body").text)

# release the resources and close the browser
driver.quit()
```
# Best Protocols for a Proxy in Selenium
When it comes to choosing a protocol for a Selenium proxy, the most common options are HTTP, HTTPS, and SOCKS5.

HTTP proxies send data over the internet, while HTTPS proxies encrypt it to provide an extra security layer. That's why the latter is more popular and secure.

Another useful protocol for Selenium proxies is SOCKS5, also known as SOCKS. It supports a wider range of web traffic, including email and FTP, which makes it a more versatile protocol.

**Overall, HTTP and HTTPS proxies are good for web scraping and crawling, and SOCKS finds applications in tasks that involve non-HTTP traffic.**
# Use a Rotating Proxy in Selenium With Python
If your script makes several requests in a short interval, the server may consider it suspicious and block your IP. Websites can detect and block requests from specific IP addresses, making it difficult for you to scrape data effectively.

However, using a rotating proxy approach can solve this problem. **By [switching proxies in Selenium](https://www.zenrows.com/blog/rotating-proxy-selenium-python) after a particular period or number of requests, your end IP will keep changing. This makes you appear as a different user each time, preventing the server from banning you.**

Let's learn how to build a proxy rotator in Selenium with `selenium-wire`.

First, you need to create a pool of proxies. In this example, we'll use some free proxies.

Store them in an array as follows: