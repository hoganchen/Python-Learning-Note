### Python查看package的版本

* ##### Python查看package的版本

```
Python 3.6.5 |Anaconda, Inc.| (default, Apr 29 2018, 16:14:56) 
[GCC 7.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> import tushare
>>> dir(tushare)
['MailMerge', 'TraderAPI', '__author__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', '__version__', '__warningregistry__', 'bar', 'bdi', 'broker_tops', 'cap_tops', 'close_apis', 'codecs', 'coins', 'coins_bar', 'coins_snapshot', 'coins_tick', 'coins_trade', 'day_boxoffice', 'day_cinema', 'forecast_data', 'fund', 'fund_holdings', 'futures', 'get_apis', 'get_area_classified', 'get_balance_sheet', 'get_cash_flow', 'get_cashflow_data', 'get_cffex_daily', 'get_concept_classified', 'get_cpi', 'get_czce_daily', 'get_day_all', 'get_dce_daily', 'get_debtpaying_data', 'get_deposit_rate', 'get_fund_info', 'get_future_daily', 'get_gdp_contrib', 'get_gdp_for', 'get_gdp_pull', 'get_gdp_quarter', 'get_gdp_year', 'get_gem_classified', 'get_gold_and_foreign_reserves', 'get_growth_data', 'get_h_data', 'get_hist_data', 'get_hists', 'get_hs300s', 'get_index', 'get_industry_classified', 'get_instrument', 'get_intlfuture', 'get_k_data', 'get_latest_news', 'get_loan_rate', 'get_markets', 'get_money_supply', 'get_money_supply_bal', 'get_nav_close', 'get_nav_grading', 'get_nav_history', 'get_nav_open', 'get_notices', 'get_operation_data', 'get_ppi', 'get_profit_data', 'get_profit_statement', 'get_realtime_quotes', 'get_report_data', 'get_rrr', 'get_shfe_daily', 'get_shfe_vwap', 'get_sina_dd', 'get_sme_classified', 'get_st_classified', 'get_stock_basics', 'get_suspended', 'get_sz50s', 'get_terminated', 'get_tick_data', 'get_today_all', 'get_today_ticks', 'get_token', 'get_zz500s', 'global_realtime', 'guba_sina', 'inst_detail', 'inst_tops', 'internet', 'is_holiday', 'latest_content', 'lpr_data', 'lpr_ma_data', 'margin_detail', 'margin_offset', 'margin_target', 'margin_zsl', 'moneyflow_hsgt', 'month_boxoffice', 'new_cbonds', 'new_stocks', 'notice_content', 'os', 'pledged_detail', 'pro', 'pro_api', 'pro_bar', 'profit_data', 'profit_divis', 'quotes', 'realtime_boxoffice', 'reset_instrument', 'set_token', 'sh_margin_details', 'sh_margins', 'shibor_data', 'shibor_ma_data', 'shibor_quote_data', 'stock', 'stock_issuance', 'stock_pledged', 'sz_margin_details', 'sz_margins', 'tick', 'top10_holders', 'top_list', 'trade_cal', 'trader', 'util', 'xsg_data']
>>> tushare.__version__
'1.2.17'
>>> help(tushare)
```

* ##### pip安装特定版本

[http://toy.linuxtoy.org/2013/12/11/installing-specific-package-versions-with-pip.html](http://toy.linuxtoy.org/2013/12/11/installing-specific-package-versions-with-pip.html)

https://pip.pypa.io/en/stable/reference/pip\_install/

```
pip 安装特定版本的 Python 包

December 11, 2013 @ 01:38 PM

要用 pip 安装特定版本的 Python 包，只需通过 == 操作符 指定，例如：

pip install -v pycrypto==2.3

将安装 pycrypto 2.3 版本。
```



