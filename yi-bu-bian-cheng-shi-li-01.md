### 异步编程示例01

```
import time
import random
import aiohttp
import asyncio
import threading


async def hello(index):       # 通过关键字async定义协程
    print('Hello world! index=%s, thread=%s' % (index, threading.currentThread()))
    sleep_time = random.randint(2, 5)
    await asyncio.sleep(sleep_time)     # 模拟IO任务
    print('Hello again! index=%s, sleep time=%d, thread=%s' % (index, sleep_time, threading.currentThread()))


def run_hello():
    loop = asyncio.get_event_loop()     # 得到一个事件循环模型
    tasks = [hello(1), hello(2)]        # 初始化任务列表
    loop.run_until_complete(asyncio.wait(tasks))    # 执行任务
    loop.close()                        # 关闭事件循环列表


async def get(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as resp:
            print(url, resp.status)
            print(url, await resp.text())


def run_get():
    loop = asyncio.get_event_loop()     # 得到一个事件循环模型
    tasks = [                           # 初始化任务列表
        get("http://zhushou.360.cn/detail/index/soft_id/3283370"),
        get("http://zhushou.360.cn/detail/index/soft_id/3264775"),
        get("http://zhushou.360.cn/detail/index/soft_id/705490")
    ]
    loop.run_until_complete(asyncio.wait(tasks))    # 执行任务
    loop.close()


def main():
    time_start = time.time()

    # run_hello()
    run_get()

    print('total use {} seconds'.format(time.time() - time_start))


if __name__ == "__main__":
    main()
```



