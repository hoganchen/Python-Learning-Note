### time sleep支持不同参数

```
import re
import time

sleep_info = '0.001s'

match_obj = re.match(r'^([\d.]+)([smhSMH]*)$', sleep_info)

if match_obj:
    sleep_value = float(match_obj.group(1))
    sleep_type = match_obj.group(2)

    if '' == sleep_type or sleep_type.lower() == 's':
        print('sleep %f seconds' % sleep_value)
        time.sleep(sleep_value)
    elif sleep_type.lower() == 'm':
        print('sleep %f seconds' % (sleep_value * 60))
        time.sleep(sleep_value * 60)
    elif sleep_type.lower() == 'h':
        print('sleep %f seconds' % (sleep_value * 3600))
        time.sleep(sleep_value * 3600)
    else:
        pass

```



