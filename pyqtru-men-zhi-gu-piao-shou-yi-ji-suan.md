### PyQt入门之股票收益计算

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
import logging
import datetime
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *

import tushare as ts

INTEREST = 0.0704
LOGGING_LEVEL = logging.INFO


def logging_config(logging_level):
    # log_format = "[line: %(lineno)d] - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[File: %(filename)s line: %(lineno)d] - %(levelname)s - %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


class StockCalcClass(QWidget):
    def __init__(self, parent=None):
        super(StockCalcClass, self).__init__(parent)

        self.setWindowTitle('StockCalc')

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
        lbl_record_price.setFont(font)
        grid_layv.addWidget(lbl_record_price, 1, 0)

        # the price of record date
        self.edt_record_price = QLineEdit('48.04', self)
        grid_layv.addWidget(self.edt_record_price, 1, 1)

        # the price of open date
        lbl_open_price = QLabel('Open Price: ', self)
        lbl_open_price.setFont(font)
        grid_layv.addWidget(lbl_open_price, 2, 0)

        # the price of open date
        self.edt_open_price = QLineEdit('48.04', self)
        grid_layv.addWidget(self.edt_open_price, 2, 1)

        # the price of open date
        lbl_calc_result = QLabel('Calc Result: ', self)
        lbl_calc_result.setFont(font)
        grid_layv.addWidget(lbl_calc_result, 3, 0)

        # the price of open date
        self.tb_calc_result = QTextBrowser()
        grid_layv.addWidget(self.tb_calc_result, 3, 1, 1, 120)

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
        percent_list = [0.22, 0.24, 0.26, 0.28]

        total_income = 0
        stock_number = int(self.edt_stock_num.text())
        record_price = float(self.edt_record_price.text())
        open_price = float(self.edt_open_price.text())

        self.tb_calc_result.clear()

        print('stock number: {}, current price: {}, open price: {}'.format(stock_number, record_price, open_price))
        self.tb_calc_result.append('stock number: {}, current price: {}, open price: {}'.
                                   format(stock_number, record_price, open_price))

        for percent in percent_list:
            income_val = calc_sale_income(stock_number, percent, open_price)
            interest_val = calc_interest(stock_number, percent_list.index(percent))
            tax_val = calc_tax(stock_number, percent, record_price, open_price)
            total_income += income_val - interest_val - tax_val

            # logging.info('For the {} year, Sale Income: {:,.2f}, Principal & Interest: {:,.2f}, Income Tax: {:,.2f}, '
            #              'Net Income: {:,.2f}'.format(percent_list.index(percent) + 1, income_val, interest_val,
            #                                           tax_val, income_val - interest_val - tax_val))

            print('For the {} year, Sale Income: {:,.2f}, Principal & Interest: {:,.2f}, Income Tax: {:,.2f}, '
                  'Net Income: {:,.2f}'.format(percent_list.index(percent) + 1, income_val, interest_val, tax_val,
                                               income_val - interest_val - tax_val))

            self.tb_calc_result.append('For the {} year, Sale Income: {:,.2f}, Principal & Interest: {:,.2f}, '
                                       'Income Tax: {:,.2f}, Net Income: {:,.2f}'.
                                       format(percent_list.index(percent) + 1, income_val, interest_val, tax_val,
                                              income_val - interest_val - tax_val))

        print('Total Income: {:,.2f}'.format(total_income))
        self.tb_calc_result.append('Total Income: {:,.2f}'.format(total_income))

    def get_price(self):
        price_df = ts.get_realtime_quotes('603160')
        self.edt_record_price.setText(str(float(price_df.iloc[0, list(price_df.columns).index('price')])))
        self.edt_open_price.setText(str(float(price_df.iloc[0, list(price_df.columns).index('price')])))


def get_all_input_info():
    stock_number = int(input('Please input the stock number: '))
    # record_price = float(input('Please input the price of record day: '))
    open_price = float(input('Please input the price of open day: '))

    # return stock_number, record_price, open_price
    return stock_number, open_price


def calc_tax(stock_number, percent, record_price, open_price):
    total_increase_per_month = ((record_price + open_price) / 2 * stock_number * percent -
                                stock_number * 48.04 * percent) / 12
    tax_percent = 0
    sub_number = 0

    if 0 < total_increase_per_month <= 1500:
        tax_percent = 0.03
        sub_number = 0
    elif 1500 < total_increase_per_month <= 4500:
        tax_percent = 0.10
        sub_number = 105
    elif 4500 < total_increase_per_month <= 9000:
        tax_percent = 0.2
        sub_number = 555
    elif 9000 < total_increase_per_month <= 35000:
        tax_percent = 0.25
        sub_number = 1005
    elif 35000 < total_increase_per_month <= 55000:
        tax_percent = 0.30
        sub_number = 2755
    elif 55000 < total_increase_per_month <= 80000:
        tax_percent = 0.35
        sub_number = 5505
    elif 80000 < total_increase_per_month:
        tax_percent = 0.45
        sub_number = 13505

    logging.debug('total_increase_per_month: {}, tax_percent: {}, sub_number: {}'.
                  format(total_increase_per_month, tax_percent, sub_number))
    tax = (total_increase_per_month * tax_percent - sub_number) * 12

    return tax


def calc_interest(stock_number, index):
    capital = 0
    interest = 0

    if 0 == index:
        capital = stock_number * 48.04
        interest = stock_number * 48.04 * 0.22
    elif 1 == index:
        capital = stock_number * 48.04 * (1 - 0.22)
        interest = stock_number * 48.04 * 0.24
    elif 2 == index:
        capital = stock_number * 48.04 * (1 - 0.22 - 0.24)
        interest = stock_number * 48.04 * 0.26
    elif 3 == index:
        capital = stock_number * 48.04 * (1 - 0.22 - 0.24 - 0.26)
        interest = stock_number * 48.04 * 0.28

    interest_val = capital * INTEREST + interest

    return interest_val


def calc_sale_income(stock_number, percent, open_price):
    logging.debug('stock_number: {}, percent: {}, open_price: {}'.format(stock_number, percent, open_price))
    income_val = stock_number * percent * open_price * (1 - 0.001)

    return income_val


def main(argv):
    logging_config(LOGGING_LEVEL)
    '''
    price_df = ts.get_realtime_quotes('603160')
    current_price = float(price_df.iloc[0, list(price_df.columns).index('price')])

    stock_base_info = get_all_input_info()

    # base_info = (10000, current_price, current_price)
    base_info = (stock_base_info[0], current_price, stock_base_info[1])

    percent_list = [0.22, 0.24, 0.26, 0.28]
    total_income = 0

    print('stock number: {}, current price: {}, open price: {}'.format(base_info[0], base_info[1], base_info[2]))

    for percent in percent_list:
        income_val = calc_sale_income(base_info[0], percent, base_info[2])
        interest_val = calc_interest(base_info[0], percent_list.index(percent))
        tax_val = calc_tax(base_info[0], percent, base_info[1], base_info[2])
        total_income += income_val - interest_val - tax_val

        # logging.info('For the {} year, Sale Income: {:,.2f}, Principal & Interest: {:,.2f}, Income Tax: {:,.2f}, '
        #              'Net Income: {:,.2f}'.format(percent_list.index(percent) + 1, income_val, interest_val, tax_val,
        #                                           income_val - interest_val - tax_val))
        print('For the {} year, Sale Income: {:,.2f}, Principal & Interest: {:,.2f}, Income Tax: {:,.2f}, '
              'Net Income: {:,.2f}'.format(percent_list.index(percent) + 1, income_val, interest_val, tax_val,
                                           income_val - interest_val - tax_val))

    print('Total Income: {:,.2f}'.format(total_income))
    '''

    app = QApplication(argv)
    ex = StockCalcClass()
    ex.show()
    sys.exit(app.exec_())

if __name__ == "__main__":
    print("Script start execution at %s\n\n" % str(datetime.datetime.now()))
    time_start = time.time()
    main(sys.argv)
    print("\n\nScript end execution at %s" % str(datetime.datetime.now()))
    print("Total Elapsed Time: %s seconds\n" % (time.time() - time_start))

```



