### 异步编程示例02

https://docs.python.org/3/library/asyncio.html

https://python-parallel-programmning-cookbook.readthedocs.io/zh\_CN/latest/chapter4/

https://segmentfault.com/a/1190000007851357

https://segmentfault.com/a/1190000008814676

http://python.jobbole.com/87310/

http://python.jobbole.com/88291/

https://cuiqingcai.com/6160.html

https://linux.cn/article-8265-1.html

https://linux.cn/article-8266-1.html

https://linux.cn/article-8267-1.html

https://lotabout.me/2017/understand-python-asyncio/

http://blog.dexbol.com/2018/06/02/asynchronous-python

http://bigsec.com/bigsec-news/wechat-20171130-Python

http://www.dongwm.com/archives/%E4%BD%BF%E7%94%A8Python%E8%BF%9B%E8%A1%8C%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B-asyncio%E7%AF%87/

https://www.ibm.com/developerworks/cn/analytics/library/ba-on-demand-data-python-3/index.html

https://www.zchen.info/archives/python-asynchronous-programming-from-pep255-to-pep525.html

```
import time
import random
import asyncio
import datetime


async def compute(x, y):
    print("Compute %s + %s ..." % (x, y))
    await asyncio.sleep(random.randint(1, 10))  # 协程compute不会继续往下面执行，直到协程sleep返回结果
    return x + y


async def print_sum(x, y):
    result = await compute(x, y)  # 协程print_sum不会继续往下执行，直到协程compute返回结果
    print("%s + %s = %s" % (x, y, result))


def run_async_tasks():
    loop = asyncio.get_event_loop()
    task_list = []
    for i in range(10):
        task_list.append(print_sum(random.randint(1, 100), random.randint(1, 100)))
    # loop.run_until_complete(print_sum(1, 2))
    loop.run_until_complete(asyncio.wait(task_list))
    # 多次事件循环，最后一次调用前的loop不能关闭
    loop.close()


async def produce(queue, n):
    for x in range(n):
        # produce an item
        print('producing {}/{}'.format(x, n))
        # simulate i/o operation using sleep
        await asyncio.sleep(random.random())
        item = str(x)
        # put the item in the queue
        await queue.put(item)


async def consume(queue):
    while True:
        # wait for an item from the producer
        item = await queue.get()

        # process the item
        print('consuming {}...'.format(item))
        # simulate i/o operation using sleep
        await asyncio.sleep(random.random())

        # Notify the queue that the item has been processed
        queue.task_done()


async def produce_consume(n):
    queue = asyncio.Queue()
    # schedule the consumer
    consumer = asyncio.ensure_future(consume(queue))
    # run the producer and wait for completion
    await produce(queue, n)
    # wait until the consumer has processed all items
    await queue.join()
    # the consumer is still awaiting for an item, cancel it
    consumer.cancel()


def run_produce_consume():
    loop = asyncio.get_event_loop()
    loop.run_until_complete(produce_consume(10))
    loop.close()


def main():
    run_async_tasks()
    run_produce_consume()


if __name__ == "__main__":
    print('Script start execution at %s\n' % str(datetime.datetime.now()))
    time_start = time.time()

    main()

    print('\nScript end execution at %s' % str(datetime.datetime.now()))
    print('Total Elapsed Time: %s seconds' % (time.time() - time_start))
```



