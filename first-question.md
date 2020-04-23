### Python位操作

以下代码用于检查元组\(1, 3, 4, 5, 6, 7\)中每个元素的值那些bit位置1

```
iphy_sel = 1

def func(param1, param2, *param3):
    print("param1 = {}, param2 = {}, param3 = {}".format(param1, param2, param3))

con_param = (1, 3, 5, 7, 9, 11, 13, 15)

for iphy_sel in (1, 3, 4, 5, 6, 7):
    phy_num = 0
    for index in range(8):
        if 0x1 == (iphy_sel >> index) & 0x1:
            phy_num += 1
    phys_con_param = con_param * phy_num
    func(iphy_sel, phy_num, *phys_con_param)
    # print("{}".format(phys_con_param))
```



