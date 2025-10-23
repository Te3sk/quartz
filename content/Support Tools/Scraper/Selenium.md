---
title: Selenium Guide
date: 2025-10-23
tags:
  - scraper
  - bot
  - ui
  - tools
  - selenium
  - python
category: Support Tools - Scraper
status: in_corso
author: Te3sk
description: Breve descrizione del contenuto del documento.
---
Selenium supports automation of all the major browsers in the market through the use of _WebDriver_. WebDriver is an API and protocol that defines a language-neutral interface for controlling the behaviour of web browsers. Each browser is backed by a specific WebDriver implementation, called a _driver_. The driver is the component responsible for delegating down to the browser, and handles communication to and from Selenium and the browser.
# Install a Selenium library
[Official Documentation](https://www.selenium.dev/documentation/webdriver/getting_started/install_library/)
## Requirements by language
The minimum supported Python version for each Selenium version can be found in “Supported Python Versions” on [PyPi](https://pypi.org/project/selenium/).

```shell title="Terminal"
pip install selenium
```
# Eight Basic Components
1. **Start the [session](https://www.selenium.dev/documentation/webdriver/drivers/)**
```python
driver = webdriver.Chrome()
```
2. **Take action on browser:** In this example we are [navigating](https://www.selenium.dev/documentation/webdriver/interactions/navigation/) to a web page
```python
driver.get("https://www.selenium.dev/selenium/web/web-form.html")
```
3. **Request browser information:** There are a bunch of types of [information about the browser](https://www.selenium.dev/documentation/webdriver/interactions/) you can request, including window handles, browser size / position, cookies, alerts, etc.
```python
title = driver.title
```
4. **Establish Waiting Strategy:** Synchronizing the code with the current state of the browser is one of the biggest challenges with Selenium, and doing it well is an advanced topic. Essentially you want to make sure that the element is on the page before you attempt to locate it and the element is in an interactable state before you attempt to interact with it. An implicit wait is rarely the best solution, but it’s the easiest to demonstrate here, so we’ll use it as a placeholder. Read more about [Waiting strategies](https://www.selenium.dev/documentation/webdriver/waits/).
```python
driver.implicitly_wait(0.5)
```
5. **Find an element:** The majority of commands in most Selenium sessions are element related, and you can’t interact with one without first [finding an element](https://www.selenium.dev/documentation/webdriver/elements/)
```python
text_box = driver.find_element(by=By.NAME, value="my-text")
submit_button = driver.find_element(by=By.CSS_SELECTOR, value="button")
```
6. **Take action on element:** There are only a handful of [actions to take on an element](https://www.selenium.dev/documentation/webdriver/elements/interactions/), but you will use them frequently.
```python
text_box.send_keys("Selenium")
submit_button.click()
```
7. **Request element information:** Elements store a lot of [information that can be requested](https://www.selenium.dev/documentation/webdriver/elements/information/).
```python
message = driver.find_element(by=By.ID, value="message")
text = message.text
```
8. **End the session:** This ends the driver process, which by default closes the browser as well. No more commands can be sent to this driver instance. See [Quitting Sessions](https://www.selenium.dev/documentation/webdriver/drivers/#quitting-sessions).
```python
driver.quit()
```

# Waiting Strategies
Perhaps the most common challenge for browser automation is ensuring that the web application is in a state to execute a particular Selenium command as desired. The processes often end up in a _race condition_ where sometimes the browser gets into the right state first (things work as intended) and sometimes the Selenium code executes first (things do not work as intended). This is one of the primary causes of _flaky tests_.

All navigation commands wait for a specific `readyState` value based on the [page load strategy](https://www.selenium.dev/documentation/webdriver/drivers/options/#pageloadstrategy) (the default value to wait for is `"complete"`) before the driver returns control to the code. The `readyState` only concerns itself with loading assets defined in the HTML, but loaded JavaScript assets often result in changes to the site, and elements that need to be interacted with may not yet be on the page when the code is ready to execute the next Selenium command.
## Implicit waits
```python title="Implicit Waits"
driver.implicitly_wait(2)
```
Selenium has a built-in way to automatically wait for elements called an _implicit wait_. An implicit wait value can be set either with the [timeouts](https://www.selenium.dev/documentation/webdriver/drivers/options/#timeouts) capability in the browser options, or with a driver method (as shown below).
This is a global setting that applies to every element location call for the entire session. The default value is `0`, which means that if the element is not found, it will immediately return an error. If an implicit wait is set, the driver will wait for the duration of the provided value before returning the error. Note that as soon as the element is located, the driver will return the element reference and the code will continue executing, so a larger implicit wait value won’t necessarily increase the duration of the session.
## Explicit waits
```python title="Explicit Waits"
wait = WebDriverWait(driver, timeout=2)
wait.until(lambda _ : revealed.is_displayed())
```
_Explicit waits_ are loops added to the code that poll the application for a specific condition to evaluate as true before it exits the loop and continues to the next command in the code. If the condition is not met before a designated timeout value, the code will give a timeout error. Since there are many ways for the application not to be in the desired state, explicit waits are a great choice to specify the exact condition to wait for in each place it is needed. Another nice feature is that, by default, the Selenium Wait class automatically waits for the designated element to exist.
### Customization
```python title="Customization
errors = [NoSuchElementException, ElementNotInteractableException]
wait = WebDriverWait(driver, timeout=2, poll_frequency=.2, ignored_exceptions=errors)
wait.until(lambda _ : revealed.send_keys("Displayed") or True)
```
The Wait class can be instantiated with various parameters that will change how the conditions are evaluated.
This can include:
- Changing how often the code is evaluated (polling interval)
- Specifying which exceptions should be handled automatically
- Changing the total timeout length
- Customizing the timeout message

For instance, if the _element not interactable_ error is retried by default, then we can add an action on a method inside the code getting executed (we just need to make sure that the code returns `true` when it is successful):
# Browser Interactions
## Get Browser Information
- **Get Title:** You can read the current page title from the browser
```python title="Get Title"
title = driver.title
```
- **Get Current URL:** You can read the current URL from the browser’s address bar using:
```python title="Get Current URL"
url = driver.current_url
```
- **Navigate to:** The first thing you will want to do after launching a browser is to open your website. This can be achieved in a single line:
```python title="Navigate To"
driver.get("https://www.selenium.dev/selenium/web/index.html")
```
- **Back, Forward and Refresh:** Pressing the browser’s back, forward and refresh button:
```python title="Back"
driver.back()
driver.forward()
```
## Alerts




# TODO List
- [ ] [[#Eight Basic Components|Eight Basic Components - 1]]: check selenium session page and decide if there are something to write here
- [ ] Continue from [[#Alerts]] ([this doc page](https://www.selenium.dev/documentation/webdriver/interactions/alerts/))