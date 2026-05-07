---
title: 第一个爬虫项目：爬取豆瓣电影 Top250
date: 2026-05-02 19:20:00 +0800
categories: [编程, Python]
tags: [python, 爬虫, 项目]
---

## 为什么想做爬虫

学了这么久 Python，一直想做个有意思的项目。看到网上很多人说爬虫入门简单，而且能做很多实用的东西，就决定试试。

我的目标是爬取豆瓣电影 Top250 的电影名称和评分，然后保存到文件里。

## 需要的库

网上查了一下，爬虫主要用这两个库：
- `requests`：用来发送网络请求
- `BeautifulSoup`：用来解析 HTML

安装很简单：

```bash
pip install requests
pip install beautifulsoup4
```

装的时候还挺快的，没遇到什么问题。

## 第一次尝试

先写个最简单的版本，看能不能获取网页内容：

```python
import requests

url = "https://movie.douban.com/top250"
response = requests.get(url)
print(response.text)
```

运行之后...报错了：`HTTPError: 418`

查了一下才知道，豆瓣有反爬虫机制，需要加上请求头伪装成浏览器。

## 添加请求头

```python
import requests

url = "https://movie.douban.com/top250"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
}

response = requests.get(url, headers=headers)
print(response.status_code)  # 200 表示成功
```

这次成功了！`status_code` 是 200，说明请求成功。

## 解析 HTML

拿到网页内容后，需要从里面提取电影信息。这里用 BeautifulSoup：

```python
import requests
from bs4 import BeautifulSoup

url = "https://movie.douban.com/top250"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
}

response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.text, "html.parser")

# 找到所有电影条目
movies = soup.find_all("div", class_="item")

for movie in movies:
    # 提取电影名称
    title = movie.find("span", class_="title").text
    # 提取评分
    rating = movie.find("span", class_="rating_num").text
    print(f"{title} - {rating}")
```

**踩坑记录**：刚开始我不知道怎么找到正确的 class 名称，后来学会了用浏览器的"检查元素"功能，右键点击网页上的电影名称，选择"检查"，就能看到对应的 HTML 代码了。

这个方法真的很有用，现在我每次写爬虫都会先用浏览器检查一下页面结构。

## 爬取多页

豆瓣 Top250 有 10 页，每页 25 部电影。观察 URL 发现规律：
- 第 1 页：`https://movie.douban.com/top250?start=0`
- 第 2 页：`https://movie.douban.com/top250?start=25`
- 第 3 页：`https://movie.douban.com/top250?start=50`

所以可以用循环爬取所有页面：

```python
import requests
from bs4 import BeautifulSoup
import time

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
}

movies_list = []

for page in range(10):
    url = f"https://movie.douban.com/top250?start={page * 25}"
    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.text, "html.parser")
    
    movies = soup.find_all("div", class_="item")
    
    for movie in movies:
        title = movie.find("span", class_="title").text
        rating = movie.find("span", class_="rating_num").text
        movies_list.append({"title": title, "rating": rating})
    
    print(f"已爬取第 {page + 1} 页")
    time.sleep(1)  # 暂停 1 秒，避免请求太频繁

print(f"共爬取 {len(movies_list)} 部电影")
```

`time.sleep(1)` 很重要，如果请求太快可能会被网站封 IP。我第一次没加这个，爬到第 5 页就被拒绝访问了，加上之后就没问题了。

## 保存到文件

把数据保存到 JSON 文件：

```python
import json

with open("douban_top250.json", "w", encoding="utf-8") as file:
    json.dump(movies_list, file, ensure_ascii=False, indent=4)

print("数据已保存到 douban_top250.json")
```

也可以保存成 CSV 格式，方便用 Excel 打开：

```python
with open("douban_top250.csv", "w", encoding="utf-8") as file:
    file.write("电影名称,评分\n")
    for movie in movies_list:
        file.write(f"{movie['title']},{movie['rating']}\n")

print("数据已保存到 douban_top250.csv")
```

## 完整代码

```python
import requests
from bs4 import BeautifulSoup
import time
import json

def crawl_douban_top250():
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
    }
    
    movies_list = []
    
    for page in range(10):
        url = f"https://movie.douban.com/top250?start={page * 25}"
        
        try:
            response = requests.get(url, headers=headers, timeout=10)
            response.raise_for_status()
            
            soup = BeautifulSoup(response.text, "html.parser")
            movies = soup.find_all("div", class_="item")
            
            for movie in movies:
                title = movie.find("span", class_="title").text
                rating = movie.find("span", class_="rating_num").text
                movies_list.append({"title": title, "rating": rating})
            
            print(f"已爬取第 {page + 1} 页")
            time.sleep(1)
            
        except Exception as e:
            print(f"爬取第 {page + 1} 页时出错：{e}")
    
    # 保存到 JSON
    with open("douban_top250.json", "w", encoding="utf-8") as file:
        json.dump(movies_list, file, ensure_ascii=False, indent=4)
    
    # 保存到 CSV
    with open("douban_top250.csv", "w", encoding="utf-8") as file:
        file.write("电影名称,评分\n")
        for movie in movies_list:
            file.write(f"{movie['title']},{movie['rating']}\n")
    
    print(f"完成！共爬取 {len(movies_list)} 部电影")

if __name__ == "__main__":
    crawl_douban_top250()
```

## 遇到的问题

1. **反爬虫机制**：需要添加 User-Agent 请求头
2. **请求频率**：要加延时，不然会被封 IP
3. **异常处理**：网络不稳定时可能会失败，要加 try-except
4. **编码问题**：保存文件时要指定 `encoding="utf-8"`

## 运行结果

程序运行大概 10 秒左右（因为每页暂停 1 秒），成功爬取了 250 部电影的信息。打开 CSV 文件，数据都在，很有成就感！

## 总结

第一次做爬虫项目，虽然遇到了一些问题，但最后还是成功了。爬虫确实挺有意思的，可以获取很多有用的数据。

不过也要注意：
- 遵守网站的 robots.txt 规则
- 不要频繁请求，给服务器造成压力
- 爬取的数据仅供学习使用，不要用于商业目的

下次想试试爬取更复杂的网站，或者加上数据分析的功能。
