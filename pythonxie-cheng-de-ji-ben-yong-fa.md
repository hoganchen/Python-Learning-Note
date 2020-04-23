### Python协程的基本用法

```
#!/usr/bin/env python
# encoding:utf-8

import asyncio
import requests
import time


async def download(url):  # 通过async def定义的函数是原生的协程对象
    response = requests.get(url)
    print(response.text)


async def wait_download(url):
    await download(url)  # 这里download(url)就是一个原生的协程对象
    print("get {} data complete.".format(url))


async def main():
    start = time.time()
    await asyncio.wait([
        wait_download("http://www.163.com"),
        wait_download("http://www.mi.com")])
    end = time.time()
    print("Complete in {} seconds".format(end - start))


loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```



