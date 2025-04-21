# 1. 开始
```bash
pip install requests
```
 GET 请求 `requests.get`
 使用`response.ok` 获取是否成功
 `response.text` 获取响应内容
```python
import requests  
  
response =  requests.get("http://books.toscrape.com/")  
if response.ok:  
    print(response.text)  
else:  
    print(response.status_code)
```
# 2.定义请求标头

```python
import requests  
headers={  
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36"  
}  
  
response = requests.get("https://movie.douban.com/top250",headers = headers)  
print(response.status_code)
```

# 3.使用BeautifulSoup解析html文件
```bash
pip install bs4
```
```python
from bs4 import BeautifulSoup  
import requests  
response =  requests.get("http://books.toscrape.com/")  
context = response.text  
soup = BeautifulSoup(context,"html.parser")  
all_prices = soup.find_all("p",attrs={"class":"price_color"})  
books = {}  
all_books = soup.find_all("h3")  
for book in all_books:  
    print(book.a["title"])  
for price in all_prices:  
    print(price.string[2:])
```
# 4.爬取豆瓣
```python
import requests  
from bs4 import BeautifulSoup  
  
headers={  
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36",  
                 "Cookie":"bid=RobuhiuJkqI; ap_v=0,6.0; push_noty_num=0; push_doumail_num=0; dbcl2=\"241913730:rmrhDG8bGls\"; ck=t20n",  
}  
movies = []  
for i in range(0,251,25):  
    url = "https://movie.douban.com/top250?start="+str(i)  
    response = requests.get(url,headers = headers)  
    html = response.text  
    soup = BeautifulSoup(html,"html.parser")  
    all_movies = soup.find_all("span",attrs={"class":"title"})  
    for movie in all_movies:  
        if "/" not in movie.string:  
            movies.append(movie.string)  
for movie in movies:  
    print(movie)
```
