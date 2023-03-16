# How to Download DailyDatas
-----
*Made by DevTae*

<br/>

This below process is how to get daily datas.

1. Download the current list of korean stocks
2. Check the daily datas need the update or not (Price Update, Adjusting Stock Price, and so on)
3. If needed, Get the datas from Kiwoom Open API and Naver Finance
4. Save the datas in form of daily datas in file-system database

<br/>

It is easy to use like below codes :
*(this is preview)*

```C#
// How to download the new stock daily datas from Kiwoom OpenAPI
int i = 0; // want to see in specific index number
Stock[] stocks = KwGetStockList().stocks; // user-defined function (except Ritz, ETF, ETN, Spac)
stocks[i].DailyDatas = KwGetDailyData(stocks[i], null); // user-defined inner asynchronous function
```

```C#
// How to load all stocks in once
int i = 0; // want to see in specific index number
Stock[] stocks = Stock.ReadListFile("한국주식(키움)");
```

```C#
// How to load the stock daily datas named "삼성전자"
Market market = new Market("0", "한국주식(키움)", "0", "코스피");
Stock stock = new Stock(new StockInfo(ref market, "005930", "삼성전자"), null);
stock.DailyDatas = DailyData.ReadDailyData(ref stock).dailyDatas; // here is the daily datas
```
