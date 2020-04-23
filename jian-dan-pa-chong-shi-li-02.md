### 简单爬虫示例02

```
import re
import time
import gzip
import random
import urllib.request
from bs4 import BeautifulSoup

url_link = 'https://item.jd.com/2316993.html'

match_obj = re.match(r'^http[s]*://([^/]*)', url_link)
if match_obj:
    host = match_obj.group(1)

header = {'Host': host,
          'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0',
          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
          'Accept-Language': 'en-US,en;q=0.5',
          # 'Accept-Encoding': 'gzip, deflate, sdch',
          'Accept-Encoding': 'gzip, deflate',
          'DNT': '1',
          'Connection': 'keep-alive',
          'Upgrade-Insecure-Requests': '1',
          }
print(header)

enable_gzip = False
if header.get('Accept-Encoding') is not None and re.search(r'gzip', str(header.get('Accept-Encoding'))):
    enable_gzip = True

url_request = urllib.request.Request(url_link, headers=header)
url_open = urllib.request.urlopen(url_request, timeout=5)
if enable_gzip:
    html_doc = url_open.read()
    # print(html_doc)
    # html_doc = gzip.decompress(html_doc).decode('utf-8')
    html_doc = gzip.decompress(html_doc)

else:
    html_doc = url_open.read().decode('utf-8')

print(html_doc)

```



