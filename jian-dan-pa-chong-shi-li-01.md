### 简单爬虫示例01

```
import re
import time
import gzip
import random
import urllib.request
from bs4 import BeautifulSoup

url_link = 'http://mebook.cc/download.php?id=6064'
url_link = 'http://mebook.cc/6064.html'
url_link = 'http://mebook.cc/download.php?id=7456'
url_link = 'http://www.kxdaili.com/dailiip/1/1.html'
url_link = 'https://www.kuaidaili.com/free/inha/2/'
url_link = 'http://www.xicidaili.com/wn/2'

UserAgentList = ['Mozilla/5.0 (Windows NT 6.1; rv:28.0) Gecko/20100101 Firefox/28.0',
                 'Mozilla/5.0 (X11; Linux i686; rv:30.0) Gecko/20100101 Firefox/30.0',
                 'Mozilla/5.0 (Windows NT 5.1; rv:31.0) Gecko/20100101 Firefox/31.0',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:33.0) Gecko/20100101 Firefox/33.0',
                 'Mozilla/5.0 (Windows NT 10.0; WOW64; rv:40.0) Gecko/20100101 Firefox/40.0',
                 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0',
                 'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)',
                 'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; Media Center PC 6.0; '
                 'InfoPath.3; MS-RTC LM 8; Zune 4.7)',
                 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/6.0)',
                 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; WOW64; Trident/6.0)',
                 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0)',
                 'Mozilla/5.0 (IE 11.0; Windows NT 6.3; Trident/7.0; .NET4.0E; .NET4.0C; rv:11.0) like Gecko',
                 'Mozilla/5.0 (IE 11.0; Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko',
                 'Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/30.0.1599.101 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/31.0.1623.0 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/37.0.2062.103 Safari/537.36',
                 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/40.0.2214.38 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/46.0.2490.71 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36']

match_obj = re.match(r'^http[s]*://([^/]*)', url_link)
if match_obj:
    host = match_obj.group(1)

header = {'Host': host,
          'User-Agent': UserAgentList[5],
          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
          'Accept-Language': 'en-US,en;q=0.5',
          # 'Accept-Encoding': 'gzip, deflate, sdch',
          'Accept-Encoding': 'gzip, deflate',
          'DNT': '1',
          'Connection': 'keep-alive',
          'Upgrade-Insecure-Requests': '1',
          }
print(header)

url_request = urllib.request.Request(url_link, headers=header)
# url_request.add_header('User-agent', 'Mozilla/5.0')
# url_request.add_header('User-agent', 'User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36 MicroMessenger/6.5.2.501 NetType/WIFI WindowsWechat QBCore/3.43.656.400 QQBrowser/9.0.2524.400')
# url_request.add_header('User-Agent', random.choice(UserAgentList))
# url_request.add_header('User-Agent', UserAgentList[5])
url_open = urllib.request.urlopen(url_request, timeout=5)
html_doc = url_open.read()
html_doc = gzip.decompress(html_doc).decode('utf-8')
print(html_doc)

bt_soup = BeautifulSoup(html_doc, 'html.parser')
print(bt_soup.find_all('tr'))

# for http://www.kxdaili.com/dailiip/1/1.html
match_obj = re.search(r'<td>(\d+\.\d+.\d+.\d+)</td>\s+<td>(\d+)</td>', str(bt_soup.find_all('tr')[1]))
print(match_obj.group(0))
print(match_obj.group(1))
print(match_obj.group(2))

# for https://www.kuaidaili.com/free/inha/2/
match_obj = re.search(r'<td data-title="IP">(\d+\.\d+.\d+.\d+)</td>\s+<td data-title="PORT">(\d+)</td>\s+<td data-title="匿名度">\w+</td>\s+<td data-title="类型">([\w]+)</td>', str(bt_soup.find_all('tr')[1]))
print(match_obj.group(0))
print(match_obj.group(1))
print(match_obj.group(2))

# http://www.xicidaili.com/wn
match_obj = re.search(r'<td>(\d+\.\d+.\d+.\d+)</td>\s+<td>(\d+)</td>\s+.*?<td>(\w+)</td>', str(bt_soup.find_all('tr')[2]), re.S)
re.search(r'<td>(\d+\.\d+.\d+.\d+)</td>\s+<td>(\d+)</td>\s+.*?<td>(\w+)</td>', str(bt_soup.find_all('tr')[2]), re.S).group(0)
print(match_obj.group(0))
print(match_obj.group(1))
print(match_obj.group(2));

re.search(r'<a\s+href="/wn/(\d+)">\d+</a>\s+<a\s+class="next_page"', str(bt_soup.find_all('div', 'pagination'))).group(1)


# 2018-01-19 23:32:01,748 [line: 39] - DEBUG - IP: 122.231.38.43  Port:808
# 2018-01-19 23:32:01,751 [line: 39] - DEBUG - IP: 120.236.142.103        Port:8888
# 2018-01-19 23:32:01,753 [line: 39] - DEBUG - IP: 120.79.162.100 Port:1080
# 2018-01-19 23:32:01,756 [line: 39] - DEBUG - IP: 223.241.117.65 Port:8010
# 2018-01-19 23:32:01,758 [line: 39] - DEBUG - IP: 114.101.46.160 Port:65309
# 2018-01-19 23:32:01,760 [line: 39] - DEBUG - IP: 111.195.228.131        Port:8123
# 2018-01-19 23:32:01,762 [line: 39] - DEBUG - IP: 39.108.171.142 Port:80
# 2018-01-19 23:32:01,768 [line: 39] - DEBUG - IP: 218.60.149.144 Port:80
# 2018-01-19 23:32:01,770 [line: 39] - DEBUG - IP: 120.76.79.21   Port:80
# 2018-01-19 23:32:01,779 [line: 39] - DEBUG - IP: 121.40.65.178  Port:8080


op_result = pandas.DataFrame(columns=['ip', 'port'])
print(hex(id(op_result)))

def op_pandas(op_result):
    print(hex(id(op_result)))
    op_result = op_result.append({'ip': '122.231.38.43', 'port': '808'}, ignore_index=True)
    print(hex(id(op_result)))
    op_result = op_result.append({'ip': '120.236.142.103', 'port': '8888'}, ignore_index=True)
    print(hex(id(op_result)))
    op_result = op_result.append({'ip': '120.79.162.100', 'port': '1080'}, ignore_index=True)
    print(hex(id(op_result)))
    op_result = op_result.append({'ip': '223.241.117.65', 'port': '8010'}, ignore_index=True)
    print(hex(id(op_result)))
    # print(op_result)

op_pandas(op_result)
print(hex(id(op_result)))
print(op_result)


base_url = 'https://www.kuaidaili.com/free/inha/{}/'

UserAgentList = ['Mozilla/5.0 (Windows NT 6.1; rv:28.0) Gecko/20100101 Firefox/28.0',
                 'Mozilla/5.0 (X11; Linux i686; rv:30.0) Gecko/20100101 Firefox/30.0',
                 'Mozilla/5.0 (Windows NT 5.1; rv:31.0) Gecko/20100101 Firefox/31.0',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:33.0) Gecko/20100101 Firefox/33.0',
                 'Mozilla/5.0 (Windows NT 10.0; WOW64; rv:40.0) Gecko/20100101 Firefox/40.0',
                 'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)',
                 'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; Media Center PC 6.0; '
                 'InfoPath.3; MS-RTC LM 8; Zune 4.7)',
                 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/6.0)',
                 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; WOW64; Trident/6.0)',
                 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0)',
                 'Mozilla/5.0 (IE 11.0; Windows NT 6.3; Trident/7.0; .NET4.0E; .NET4.0C; rv:11.0) like Gecko',
                 'Mozilla/5.0 (IE 11.0; Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko',
                 'Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/30.0.1599.101 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/31.0.1623.0 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/37.0.2062.103 Safari/537.36',
                 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/40.0.2214.38 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/46.0.2490.71 Safari/537.36',
                 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 '
                 '(KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36']

for index in range(1, 11):
    """
    header = {'Connection': 'keep-alive',
          'Cache-Control': 'max-age=0',
          'Upgrade-Insecure-Requests': '1',
          'User-Agent': random.choice(UserAgentList),
          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
          # 'Accept-Encoding': 'gzip, deflate, sdch',
          'Accept-Language': 'zh-CN,zh;q=0.8',
          }
    """
    url_link = base_url.format(index)
    # url_request = urllib.request.Request(url_link, headers=header)
    url_request = urllib.request.Request(url_link)
    # url_request.add_header('User-agent', 'Mozilla/5.0')
    # url_request.add_header('User-agent', 'User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36 MicroMessenger/6.5.2.501 NetType/WIFI WindowsWechat QBCore/3.43.656.400 QQBrowser/9.0.2524.400')
    url_request.add_header('User-Agent', random.choice(UserAgentList))
    # url_request.add_header(header)
    url_open = urllib.request.urlopen(url_request)
    html_doc = url_open.read().decode('utf-8')
    print(html_doc)
    time.sleep(2)


base_url = 'http://www.xicidaili.com/wn/{}'
proxies_list = [{'http': '47.96.250.208:3128'}, {'http': '47.93.185.19:80'}, {'http': '122.4.238.66:8080'}, {'http': '183.163.46.201:42419'}, {'http': '182.92.242.11:80'}, {'http': '117.146.19.161:3128'}, {'http': '183.163.46.146:42419'}, {'http': '114.222.151.115:808'}, {'http': '121.31.198.170:8123'}, {'http': '180.121.162.86:47998'}]

for index in range(1, 11):
    proxies = proxies_list[random.randrange(0, len(proxies_list))]
    # proxies = {'http': '114.222.151.115:808'}
    print(proxies)
    url_link = base_url.format(index)
    proxy_handler = urllib.request.ProxyHandler(proxies)
    opener = urllib.request.build_opener(proxy_handler)
    urllib.request.install_opener(opener)
    # url_request.add_header('User-Agent', random.choice(UserAgentList))
    url_request = urllib.request.Request(url_link)
    url_request.add_header('User-agent', 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0')
    while True:
        try:
            url_open = urllib.request.urlopen(url_request)
            html_doc = url_open.read().decode('utf-8')
        except Exception as err:
            time.sleep(2)
        else:
            break
    print(html_doc)
    break


import re
import time
import gzip
import random
import urllib.request
from bs4 import BeautifulSoup

url_link = 'https://proxy.mimvp.com/free.php?proxy=in_tp&sort=&page=1'

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
    html_doc = gzip.decompress(html_doc).decode('utf-8')
else:
    html_doc = url_open.read().decode('utf-8')

print(html_doc)


bt_soup = BeautifulSoup(html_doc, 'html.parser')
print(bt_soup.find_all('td', 'tbl-proxy-port'))

re.findall(r'class=\'tbl-proxy-ip\'[^>]+(\d+\.\d+\.\d+\.\d+).*?port=(\w+).*?title=\'[\w/]+\'>([\w/]+)', html_doc).group(0)
```



