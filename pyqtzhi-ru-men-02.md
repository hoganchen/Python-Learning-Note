### PyQt入门之Demo 02

```
# -*- coding:utf-8 -*-

"""
@Author:        hogan.chen@ymail.com
@Create Date:   2018-03-29
@Update Data:   2018-03-29
@Version:       V0.1.20180329
"""

import sys
import time
import datetime

import tushare as ts

from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

__author__ = 'Hogan Chen'


class StockCalcClass(QWidget):
    def __init__(self, parent=None):
        super(StockCalcClass, self).__init__(parent)

        self.setObjectName('StockCalc')

        font = QFont()
        font.setWeight(75)
        font.setBold(True)

        # vbox_layv = QVBoxLayout()
        grid_layv = QGridLayout()

        # grid_layv.setSpacing(10)

        # stock number
        lbl_stock_num = QLabel('Stock Number: ', self)
        lbl_stock_num.setFont(font)
        grid_layv.addWidget(lbl_stock_num, 0, 0)

        # stock number
        self.edt_stock_num = QLineEdit('10000', self)
        grid_layv.addWidget(self.edt_stock_num, 0, 1)

        # the price of record date
        lbl_record_price = QLabel('Record Price: ', self)
        # lbl_record_price.setFont(font)
        grid_layv.addWidget(lbl_record_price, 1, 0)

        # the price of record date
        self.edt_record_price = QLineEdit('48.04', self)
        grid_layv.addWidget(self.edt_record_price, 1, 1)

        # the price of open date
        lbl_open_price = QLabel('Open Price: ', self)
        # lbl_open_price.setFont(font)
        grid_layv.addWidget(lbl_open_price, 2, 0)

        # the price of open date
        self.edt_open_price = QLineEdit('48.04', self)
        grid_layv.addWidget(self.edt_open_price, 2, 1)

        # the price of open date
        lbl_open_price = QLabel('Open Price: ', self)
        # lbl_open_price.setFont(font)
        grid_layv.addWidget(lbl_open_price, 2, 0)

        # the price of open date
        lbl_calc_result = QLabel('Calc Result: ', self)
        # lbl_open_price.setFont(font)
        grid_layv.addWidget(lbl_calc_result, 3, 0)

        # the price of open date
        self.tb_calc_result = QTextBrowser()
        # lbl_open_price.setFont(font)
        grid_layv.addWidget(self.tb_calc_result, 3, 1)

        # get current price
        bt_price = QPushButton('Get Price', self)
        bt_price.clicked.connect(self.get_price)
        grid_layv.addWidget(bt_price, 4, 0)

        # calc result
        bt_calc = QPushButton('Calc', self)
        bt_calc.clicked.connect(self.calc_result)
        grid_layv.addWidget(bt_calc, 4, 1)

        # grid_layv.setColumnStretch(1, 10)
        # self.setGeometry(300, 300, 250, 150)

        # vbox_layv.addLayout(grid_layv)

        self.setLayout(grid_layv)

    def calc_result(self):
        pass

    def get_price(self):
        price_df = ts.get_realtime_quotes('603160')
        self.edt_record_price.setText(str(float(price_df.iloc[0, list(price_df.columns).index('price')])))
        self.edt_open_price.setText(str(float(price_df.iloc[0, list(price_df.columns).index('price')])))


class TianGong(QWidget):
    def __init__(self):
        super(TianGong,self).__init__()
        self.initUi()

    def initUi(self):
        self.createGridGroupBox()
        self.creatVboxGroupBox()
        self.creatFormGroupBox()
        mainLayout = QVBoxLayout()
        hboxLayout = QHBoxLayout()
        hboxLayout.addStretch()
        hboxLayout.addWidget(self.gridGroupBox)
        hboxLayout.addWidget(self.vboxGroupBox)
        mainLayout.addLayout(hboxLayout)
        mainLayout.addWidget(self.formGroupBox)
        self.setLayout(mainLayout)

    def createGridGroupBox(self):
        self.gridGroupBox = QGroupBox("Grid layout")
        layout = QGridLayout()

        nameLabel = QLabel("中文名称")
        nameLineEdit = QLineEdit("天宫二号")
        emitLabel = QLabel("发射地点")
        emitLineEdit = QLineEdit("酒泉中心")
        timeLabel = QLabel("发射时间")
        timeLineEdit = QLineEdit("9月15日")
        imgeLabel = QLabel()
        pixMap = QPixmap("tiangong.png")
        imgeLabel.setPixmap(pixMap)
        layout.setSpacing(10)
        layout.addWidget(nameLabel,1,0)
        layout.addWidget(nameLineEdit,1,1)
        layout.addWidget(emitLabel,2,0)
        layout.addWidget(emitLineEdit,2,1)
        layout.addWidget(timeLabel,3,0)
        layout.addWidget(timeLineEdit,3,1)
        layout.addWidget(imgeLabel,0,2,4,1)
        layout.setColumnStretch(1, 10)
        self.gridGroupBox.setLayout(layout)
        self.setWindowTitle('Basic Layout')

    def creatVboxGroupBox(self):
        self.vboxGroupBox = QGroupBox("Vbox layout")
        layout = QVBoxLayout()
        nameLabel = QLabel("科研任务：")
        bigEditor = QTextEdit()
        bigEditor.setPlainText("搭载了空间冷原子钟等14项应用载荷，以及失重心血管研究等航天医学实验设备 "
                "开展空间科学及技术试验.")
        layout.addWidget(nameLabel)
        layout.addWidget(bigEditor)
        self.vboxGroupBox.setLayout(layout)

    def creatFormGroupBox(self):
        self.formGroupBox = QGroupBox("Form layout")
        layout = QFormLayout()
        performanceLabel = QLabel("性能特点：")
        performanceEditor = QLineEdit("舱内设计更宜居方便天宫生活")
        planLabel = QLabel("发射规划：")
        planEditor = QTextEdit()
        planEditor.setPlainText("2020年之前，中国计划初步完成空间站建设")
        layout.addRow(performanceLabel,performanceEditor)
        layout.addRow(planLabel,planEditor)

        self.formGroupBox.setLayout(layout)


def main(argv):
    app = QApplication(argv)
    ex = StockCalcClass()
    # ex = TianGong()
    ex.show()
    sys.exit(app.exec_())


if __name__ == "__main__":
    print("Script start execution at %s\n\n" % str(datetime.datetime.now()))
    time_start = time.time()

    main(sys.argv)

    print("\n\nScript end execution at %s" % str(datetime.datetime.now()))
    print("Total Elapsed Time: %s seconds\n" % (time.time() - time_start))
```



