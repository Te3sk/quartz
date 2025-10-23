---
title: Scraper - Avoid Human Control
date: 2025-10-23
tags:
  - scraper
  - ui
  - python
  - tools
  - selenium
category: Support Tools - Scraper
status: in_corso
author: Te3sk
description: Breve descrizione del contenuto del documento.
---
# Introduction
TODO
Write an intro about the used technologies (python, bs4, selenium, ...) and the human control problem
# How Do Websites Detect Selenium?
Selenium is among the popular tools in the field of web scraping. As a result, websites with strict anti-bot policies try to identify its unique attributes before blocking access to their resources.

So, how is Selenium detected? Selenium bot detection mainly works by **testing for specified JavaScript variables that emerge while executing Selenium**. Bot detectors often check for the words "Selenium" or "WebDriver" in any of the variables (on the window object), as well as document variables named `$cdc_` and `$wdc_`.

They also check for the values of automation indicator flags in the WebDriver, like `useAutomationExtension` and `navigator.webdriver`. These attributes are enabled by default to allow a better testing experience and as a security feature.

Additionally, advanced Selenium detection methods may employ browser fingerprinting to identify characteristics unique to automated browsers. Selenium can also get blocked websites when they analyze user behavior patterns to spot inhuman speed or consistency in interactions, which can indicate bot activity.
# Top Methods to Avoid Bot Detection With Selenium
## 1. IP Rotation / Proxy
One of the major ways most bot detectors work is by inspecting IP behaviors. Web servers can draw a pattern from an IP address by maintaining a log for every request.

They use [Web Application Firewalls (WAFs)](https://www.cloudflare.com/learning/ddos/glossary/web-application-firewall-waf/) to track and block IP address activities and blacklist suspicious IPs. The repetitive and programmatic request to the server might hurt the IP reputation and result in getting blocked permanently.

To avoid bot detection, you can use [IP rotation](https://www.zenrows.com/blog/ip-rotation-scraping) or [proxies](https://www.fortinet.com/resources/cyberglossary/proxy-server) with Selenium. This could be considered as one of the easiest Selenium anti-detect approaches where proxies act as an intermediary between the requester and the server. The responding server interprets the request as coming from the proxy server, not the client's computer. As a result, it won't be able to draw a pattern for behavioral analysis.
```python title="scraper.py"
# pip3 install selenium
from selenium import webdriver

# define the proxy server
PROXY = "<PROXY_IP_ADDRESS>:<PROXY_PORT>"

# set ChromeOptions()
options = webdriver.ChromeOptions()

# add the proxy as argument
options.add_argument(f"--proxy-server={PROXY}")
driver = webdriver.Chrome(options=options)

# send the request
driver.get("https://httpbin.io/ip")

# close the driver
driver.close()
```
[[Selenium - IP Rotation & Proxy|Detailed Guide]]
[GUIDE - How to Use a Proxy With Selenium in Python (2025)](https://www.zenrows.com/blog/selenium-proxy)
## 2. Disabling the Automation Indicator WebDriver Flags
While web scraping with Selenium, the WebDriver sends information to the server to indicate the request is automated.

The WebDriver is expected to have properties like `window.navigator.webdriver`, mandated by the W3C WebDriver Specification to allow better testability and as a security feature. This results in getting detected by the web servers, which leads to being flagged or denied access.

With the availability of `execute_cdp_cmd(cmd, cmd_args)` commands, **you can now easily execute Google-Chrome-DevTools commands using Selenium**. That makes it possible to change the default flagships.
```python title="scraper.py"
from selenium import webdriver 
 
# create Chromeoptions instance 
options = webdriver.ChromeOptions() 
 
# adding argument to disable the AutomationControlled flag 
options.add_argument("--disable-blink-features=AutomationControlled") 
 
# exclude the collection of enable-automation switches 
options.add_experimental_option("excludeSwitches", ["enable-automation"]) 
 
# turn-off userAutomationExtension 
options.add_experimental_option("useAutomationExtension", False) 
 
# setting the driver path and requesting a page 
driver = webdriver.Chrome(options=options) 
 
# changing the property of the navigator value for webdriver to undefined 
driver.execute_script("Object.defineProperty(navigator, 'webdriver', {get: () => undefined})") 
 
driver.get("https://www.google.com")

# close the driver
driver.close()
```
## 3. Rotating HTTP Header Information and User Agent
The HTTP header contains information about the browser, the operating system, the request type, the user language, the referrer, the device type, and so on.
```json title="Example"
{ 
	"Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9", 
	"Accept-Encoding": "gzip, deflate", 
	"Accept-Language": "en-US,en;q=0.9", 
	"Host": "httpbin.org", 
	"Upgrade-Insecure-Requests": "1", 
	"User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36", 
}
```
The values of some of these attributes are different for headless browsers by default, and **anti-bots identify these discrepancies to distinguish between legitimate visitors and bots**. To mitigate detection, rotating user agents in Selenium can be helpful.

Here's how we used rotating HTTP header information to avoid bot detection with Selenium:
```python title="scraper.py"
from selenium import webdriver

driver = webdriver.Chrome()

# initializing a list with two User Agents
useragentarray = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36",
]

for i in range(len(useragentarray)):
    # setting User Agent iteratively as Chrome 108 and 107
    driver.execute_cdp_cmd(
        "Network.setUserAgentOverride", {"userAgent": useragentarray[i]}
    )
    print(driver.execute_script("return navigator.userAgent;"))
    driver.get("https://httpbin.io/headers")

driver.close()
```
Manually maintaining and updating a list of User Agents can be tiresome and costly. Even with significant effort, a manually compiled list may still not be diverse or up-to-date enough to avoid detection.

Web scraping APIs, such as [ZenRows, offer auto-rotation of User Agents](https://www.zenrows.com/solutions/user-agent-rotator), which can be a valuable feature for your scraping projects. It continuously updates its User Agent lists, ensuring that your requests always use current and diverse User Agents. This approach significantly reduces the chances of detection and saves you time and resources in the long run.

[GUIDE - Change the Selenium User Agent: Steps & Best Practices](https://www.zenrows.com/blog/selenium-user-agent)
## 4. Avoid Patterns With a Selenium Bot
One of the major mistakes that automation testers make is to create a bot with a defined time frame. Humans don't have a solid consistency like a bot, so **it becomes fairly easy for the anti-bots to identify the consistent patterns of the bots**.

It's also a common mistake to rapidly navigate from page to page, which humans don't do.

Randomizing time frames, using waits, scrolling slower, and generally trying to mimic human behavior surely elevate the chance of avoiding bot detectors. Here's how to bypass Selenium detection by avoiding patterns:
```python title="scraper.py"
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

driver = webdriver.Chrome()
driver.get("https://scrapingcourse.com/ecommerce/")

# wait 3.5 seconds on the web page before trying anything
time.sleep(3.5)

# Wait for 3 seconds until finding the element
wait = WebDriverWait(driver, 3)
element = wait.until(
    EC.presence_of_element_located(
        (By.CSS_SELECTOR, ".woocommerce-loop-product__title")
    )
)
print("Product name: " + element.text)

# wait for 4.5 seconds before scrolling down 700px
time.sleep(4.5)
driver.execute_script("window.scrollTo(0, 700)")

# wait for 2 seconds before clicking a link
time.sleep(2)
wait = WebDriverWait(driver, 3)
element = wait.until(
    EC.presence_of_element_located(
        (By.CSS_SELECTOR, "a.woocommerce-loop-product__link")
    )
).click()

# wait for 5 seconds until finding the element
wait = WebDriverWait(driver, 5)
element = wait.until(
    EC.presence_of_element_located(
        (By.CLASS_NAME, "woocommerce-product-details__short-description")
    )
)
print("Description: " + element.text)

# close the driver after 3 seconds
time.sleep(3)
driver.close()
```
## 5. Remove JavaScript Signature
One of the ways bot detectors like [FingerprintJS](https://fingerprint.com/) and [Imperva](https://www.imperva.com/) work is by inspecting the JavaScript signature inside WebDrivers, like ChromeDriver and GeckoDriver.

This signature is stored in the `cdc_` variable. Websites look for the `cdc_` variable in the document before denying access.

We'll use a tool called [Agent Ransack](https://agent-ransack.en.softonic.com/) to search for this signature in the chromedriver.exe binary file. It works the same way for WebDrivers like GeckoDriver and EdgeDriver.
![Agent Ransack Screenshot](https://static.zenrows.com/content/large_normal_chromedriver_ee4221abc9.webp)
As you can see, the signature is `$cdc_asdjflasutopfhvcZLmcfl_`. In order to evade detection, we can change "cdc" to a string of the same length as "abc". First, we need to open the binary of ChromeDriver. We'll use the Vim editor to open and edit the binary file.

You can [download Vim here](https://www.vim.org/download.php). Click on "standard self-installing executable" for Windows. The program comes pre-installed by default for Mac and Linux.

After installation, open CMD and type this and navigate to the folder. Then, run the following command:
```bash title="Terminal"
vim.exe <pathTo>\chromedriver.exe
```
Then type `:%s/cdc_/abc_/g` to search and replace `cdc_` with `abc_`.
![Vim Terminal Screenshot](https://static.zenrows.com/content/large_search_and_replace_1005dd310d.webp)
Next, to exit Vim, type `:wq` and hit Enter to save the changes.

Some files might be generated with `~` at the end of the file names. Delete these files.
![File Manager Screenshot](https://static.zenrows.com/content/medium_new_files_3ed0fc08e0.jpg?format=webp)
Now, let's try the same search using Agent Ransack.
![Second Agent Ransack Screenshot](https://static.zenrows.com/content/large_final_1d72bbd09b.webp)
As you can see now, the `cdc_` signature variable isn't found in the file.
## 6. Using Cookies
When trying to scrape data from social media platforms or other sites that require some form of authentication, it's very common to log in repeatedly.

This iterative authentication request raises the alarm, and the account might be blocked or face a CAPTCHA or JavaScript challenge for verification.

In order to avoid this, we can use cookies. After logging in once, we can collect the login session cookies to reuse them in the future.
## 7. Follow the Page Flow
When interacting with a website, it's important to follow the same flow that a human user would.

That means clicking on links, filling out forms, and navigating the website naturally.

Following the page flow can make it less obvious that you're performing automation.
## 8. Using a Browser Extension
Another way to bypass Selenium detection is by using a browser extension, like uBlock Origin, to block JavaScript challenges and CAPTCHAs from being loaded on the page. That can help reduce the chances of your bot being detected by these challenges.

[uBlock Origin](https://chromewebstore.google.com/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en) is a free, open-source browser extension designed to block unwanted content (such as ads, tracking scripts and malware) from being loaded on web pages.

It can also be configured to block JavaScript challenges and CAPTCHAs, which can help reduce the chances of your bot being detected by these challenges.

To use uBlock Origin to avoid Selenium bot detection, you'll need to install the extension in your browser and configure it to block JavaScript challenges and CAPTCHAs.

You can then use Selenium to interact with the browser as you normally would, and uBlock Origin will automatically block any unwanted content from being loaded on the page.

It's important to note that uBlock Origin may not work with all websites, and it may not be able to block all types of JavaScript challenges and CAPTCHAs.

However, it can be a useful tool for reducing the chances of your bot being detected by these challenges, especially when combined with other methods.
## 9. Use Selenium Stealth Plugin
The [Selenium Stealth](https://pypi.org/project/selenium-stealth/) plugin is a powerful tool designed to help your Selenium-based scrapers avoid detection. It modifies Selenium's default configuration to mimic real browser fingerprints, making it harder for websites to identify your scraper as a bot.

Selenium Stealth works by implementing several key changes:

- It sets the WebDriver navigator property to false, hiding one of the most common indicators of automated browsing.
- In headless mode, it replaces the `HeadlessChrome` User Agent with an actual Chrome User Agent, making your requests appear more like those from a real browser.
- It adjusts various browser properties and behaviors to more closely resemble those of a typical user's browser.

These modifications can be particularly helpful when dealing with websites that employ basic to moderate anti-bot measures. By masking the telltale signs of automation, Selenium Stealth allows your scraper to fly under the radar in many scenarios where base Selenium would be detected and blocked.

However, it's important to note that Selenium Stealth isn't a perfect solution and has some limitations, including:

- It only partially patches Selenium, meaning some bot-like attributes would still be detectable by more sophisticated anti-bot systems.
- The plugin would struggle against advanced detection techniques used by complex security measures, such as those employed by Cloudflare.
- As anti-bot technologies evolve, the effectiveness of Selenium Stealth will decrease over time if it's not regularly updated.

Despite these limitations, Selenium Stealth remains a valuable tool in a web scraper's arsenal, particularly for sites with less stringent anti-bot measures. When combined with other techniques like proxy rotation and mimicking human behavior, it can significantly improve your scraper's ability to avoid detection.

To learn more about implementing Selenium Stealth and maximizing its effectiveness, check out our detailed tutorial on [using Selenium Stealth in Python](https://www.zenrows.com/blog/selenium-stealth).