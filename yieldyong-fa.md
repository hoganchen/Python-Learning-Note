### Yield用法示例01

```
import time,queue

con1 = None
con2 = None
flag = 1
start_flag = 0

def consumer(name):
    print("-->starting eating xoxo")
    while True:
        new_xo = yield
        print("%s is eating xoxo %s" % (name, new_xo))

def producer():
    print('con1: {}'.format(con1))
    print('con2: {}'.format(con2))

    # use next function or __next__ method to start the generator
    # https://www.zhihu.com/question/28105502

    if 0 == start_flag:
        next(con1)
        next(con2)
    elif 1 == start_flag:
        con1.send(None)
        con2.send(None)
    elif 2 == start_flag:
        # The grammar of python 2
        # con1.next()
        # con2.next()

        r = con1.__next__()
        print('r: {}'.format(r))
        r = con2.__next__()
        print('r: {}'.format(r))

    n = 0
    while n < 5:
        n += 1

        if 0 == flag:
            # use the send function to pass the n value to yield of consumer function
            con1.send(n)
            con2.send(n)
        elif 1 == flag:
            con1.send(n + 100)
            con2.send(n + 100)
        elif 2 == flag:
            # the next function can not pass the n value to yield of consumer function,
            # so the new_xo always be None
            next(con1)
            next(con2)
        elif 3 == flag:
            # this two sentence is same as the above 2 lines
            con1.send(None)
            con2.send(None)

        # assert(n < 5)
        print("\033[32;1mproducer\033[0m is making xoxo %s" % n)


def main():
    global con1, con2

    con1 = consumer("c1")
    con2 = consumer("c2")

    p = producer()


if __name__ == "__main__":
    main()
```



