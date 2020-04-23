### Python Time模块使用

```
import time
print('Before sleep, time: {}'.format(time.time()))
time.sleep(2)
print('After sleep, time: {}'.format(time.time()))
print('gctime: {}'.format(time.gmtime()))
print('localtime: {}'.format(time.localtime()))
print('time.gmtime: {}'.format(time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.gmtime())))
print('time.localtime: {}'.format(time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.localtime())))

Before sleep, time: 1528093793.600307
After sleep, time: 1528093795.6045246
gctime: time.struct_time(tm_year=2018, tm_mon=6, tm_mday=4, tm_hour=6, tm_min=29, tm_sec=55, tm_wday=0, tm_yday=155, tm_isdst=0)
localtime: time.struct_time(tm_year=2018, tm_mon=6, tm_mday=4, tm_hour=14, tm_min=29, tm_sec=55, tm_wday=0, tm_yday=155, tm_isdst=0)
time.gmtime: Mon, 04 Jun 2018 06:29:55 +0000
time.localtime: Mon, 04 Jun 2018 14:29:55 +0000
```



