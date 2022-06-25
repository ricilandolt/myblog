---
title: Hello shadowr Package
author: 'Ricardo Landolt'
date: '2022-04-30'
slug: []
categories: []
tags: []
Categories:
  - Development
  - GoLang
Description: ''
Tags:
  - R
  - R Selenium
  - shadowr
  - scraping
---





# Introduction

Recently I wanted to scrape some websites and for two or three websites such as [https://configure.bmw.ch/de_CH/configure/F40](https://configure.bmw.ch/de_CH/configure/F40) or [https://www.mercedes-benz.ch/de/passengercars/models.html](https://www.mercedes-benz.ch/de/passengercars/models.html), I could not find certain dom elements because shadow dom elements were used on these websites. 

Shadow DOM is a web standard that offers component style and markup encapsulation. It is a critically important piece of the Web Components story as it ensures that a component will work in any environment even if other CSS or JavaScript is at play on the page. 

The problem is that Custom HTML tags can't be directly identified with selenium tools. If you just need to click a cookie accept button and if there is only a one level shadow dom structure, you could just [copy the js path](https://umaar.com/dev-tips/185-copy-js-path/) and let the selenium driver execute this js code.

But when you have a multi level shadow dom structure and have to do different things with the elements like get the text or click them, sooner or later things get complicated. You would have to handle a lot of long strings and depending on the action and amount of elements also modify/extend the strings.  

That is not reliable and after a little research I came across the [pyshadow](https://pypi.org/project/pyshadow/) module for python, which is able to handle such shadow dom elements. So I decided to release an r package shadowr to enable R users a more comfortable and reliable way to work with shadow dom elements. The Package is available on cran and you can find the github repo [here](https://github.com/ricilandolt/shadowr).  

# What the shadowr package does 

To use the shadowr package you have to use the class constructor shadow with your R Selenium Driver to initialize a shadow s4 object.  


```r
library(shadowr) 
library(RSelenium) 
remDr <- RSelenium::remoteDriver( 
  remoteServerAddr = "host.docker.internal", 
  port = 4445 , browser = "chrome") 
remDr$open(silent = TRUE) 
remDr$navigate(url) 
shadow_rd <- shadow(remDr) 
```

After that you can use the class methods of the shadow object, which is straight forward if you are already familiar with R Selenium. You can provide a css selector and search for single or multiple webelements like follows 


```r
element <- find_element(shadow_rd, 'paper-tab[title="Settings"]') 
elements <- find_elements(shadow_rd, 'paper-tab[title="Settings"]') 
element$getElementText() 
```

Optionally, you can also provide an additional webelement to the css selector, starting the search from there. 


```r
element <- find_element(shadow_rd, 'paper-tab[title="Settings"]') 
subelement <- find_element(shadow_rd, 'div',element) 
subelement $getElementText() 
```

Depending on the method, either a RSelenium::WebElement or a boolean is returned. You can access the RSelenium::WebElement like you used to with methods like getElementText, getElementAttribute or isElementDisplayed. Altough there are some similar functions in the shadowr package implemented. 




```r
element <- find_element(shadow_rd, 'paper-tab[title="Settings"]') 
is_checked(shadow_rd, element) 
is_disabled(shadow_rd, element) 
is_visible(shadow_rd, element) 
```

You can track development progress at https://github.com/ricilandolt/shadowr and report bugs at https://github.com/ricilandolt/shadowr/issues. 


