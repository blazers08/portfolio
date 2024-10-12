---
title: Deploy a selenium based crawler to Docker
author: Denny
pubDatetime: 2020-04-16T18:33:05.569Z
slug: A simple tutorial of using selenium gird in python crawling and deploy on docker
featured: true
draft: false

canonicalURL: https://medium.com/analytics-vidhya/deploy-a-selenium-based-crawler-to-docker-fd26f53f52e7
tags:
  - Selenium Webdriver
  - Docker Compose
  - Selenium Grid
  - Selenium
  - Python
description: Build a python crawler image running on selenium grid hub with docker
---

_Tutorial on setting up docker compose to run crawler with selenium grid_

Recently, I got a crawler project. The crawler is placed on a PC Computer with Windows 10 Professional and the crawler is based on selenium.

Though I had experience on crawler with selenium but I have no experience to pack them into a docker image. However I do have packing my Scrapy crawler into a image before. I will share it in my later post.

Building a selenium based crawler docker image is totally different from packing a Scrapy image.

## Current Environment and Operation

The crawler is put on Windows 10 Professional and using **_Task Scheduler_** to crawl data from **Steam** every week. The current solution runs very smoothly and doesn’t occur any problems. However we want the crawler can be deployed very fast and everywhere in the future. Thus, it’s my mission to change the current crawler into a docker image.

## Try and Error

My first idea is to build a python3 image and put the crawler in it. However, I faced a problem.

> WebDriverException: Message: ‘chromedriver’ executable needs to be in PATH.

I think everyone must meets this problem before and can easily fix it.

I try to fix this through download chromedriver and unzip it inside the image, but failed.

So, I came up another way. I build a python-alpine image and put the crawler inside. However, I still face some issues about chromedriver and selenium.

## Final Solution

At last, I found that Selenium provide a Docker images for Selenium Grid Server called [Docker-selenium](https://github.com/SeleniumHQ/docker-selenium).

In my case, I use this image: **_selenium/standalone-chrome_**

Below is my crawler Dockerfile

```dockerfile
FROM python:3WORKDIR ./COPY requirements.txt ./
RUN pip install -r requirements.txtCOPY /steam_parser/ ./
```

Yap. It’s really really short.

Because I need to run two images synchronously and the crawler image id depend on selenium/standalone-chrome. Thus, I need a docker-compose.

Below is my docker-compose.yml

```yaml
version: "3"
services:chromedriver:
    image: selenium/standalone-chrome
    ports:
      - "4444:4444"steam_parser:
    build: .
    command: python ***.py
    volumes:
      - /c/Users/***/Desktop/test:/data/.
    links:
      - chromedriver
```

I think it’s easy to understand if you have basic docker knowledge.

Now, the setting of docker is completed. We need to do change some codes in .py

We need to import selenium and connect to the remote driver.

```python
from selenium.webdriver.common.desired_capabilities import DesiredCapabilitiessleep(5)
driver = webdriver.Remote(  command_executor='http://your_local_ip/wd/hub',
   desired_capabilities=DesiredCapabilities.CHROME)
```

Before crawling, I let the crawler to sleep 5 secs and then the crawler will use the **_standalone-chrome_** to crawl data.

You need to find out your local IP using `ifconfig` or `ipconfig` due to your OS.

Finally, we just run below command

```bash
docker-compose up --build --abort-on-container-exit
```

You can tell that the **_chromedriver_** start a new session.

You can check the session through the below url.

```
http://your_local_ip/wd/hub/static/resource/hub.html
```

You can print some certain output to check if the crawler is running correctly.

For me, I print 1, 2 and date to check.

Below is the result when crawler finished crawling data. The container will stop automatically because I use`--abort-on-container-exit` or the chromedriver container will keep running.

Finished crawling

You can check your local file to see the result if you have output your data into .json or other else.

## Conclusion

It is quite a good learning opportunity for me through this project. Docker is such a good tool to build everything fast and smooth. I consider docker as LEGO pieces, you can put any pieces you need or you want to build a masterpiece or small and delicate.

Welcome to response if you have any questions or opinions for me. Thank you.

## Ref.

https://github.com/SeleniumHQ/docker-selenium?source=post_page-----fd26f53f52e7--------------------------------

https://selenium-python.readthedocs.io/getting-started.html?source=post_page-----fd26f53f52e7--------------------------------#using-selenium-with-remote-webdriver
