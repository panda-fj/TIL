# Seleniumの型タイプ

## Webdriver

```py
from selenium import webdriver
from selenium.webdriver.remote.webdriver import WebDriver


def test(driver: WebDriver):
    driver.get(url)

driver = webdriver.Chrome()
test(driver)
```

## 取得した element

```py
from selenium import webdriver
from selenium.webdriver.remote.webelement import WebElement


def test(element: WebElement):
    text = element.text


driver = webdriver.Chrome()
driver.get(url)
element = driver.find_element(By.CSS_SELECTOR, ".title")

test(element)
```

## 取得した element のリスト

```py
from typing import List

from selenium import webdriver
from selenium.webdriver.remote.webelement import WebElement


def test(elements: List[WebElement]):
    for element in elements:
        print(element.text)

driver = web
driver.Chrome()
driver.get(url)
elements = driver.find_elements(By.TAG_NAME, "tr")

test(elements)
```