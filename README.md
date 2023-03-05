# StockDatabasePreview
-----

*Made by DevTae*

This is the repository that summarizes about my own project named 'DevTae/StockDatabase'

- Overview
  - First, I have saved about `2367 stocks` and `9,952,847 daily datas` in file-system database from Korea market (KOSPI, KOSDAQ).
  - Second, I made a `saving of 72% previous processing time` in calculating Leading Span of Ichimoku about `a data set of 10 million`.
  - Third, I use these datas **to make own buying/selling strategy**.
  - Finally, Here are many features in 'StockDatabase' projects.

-----

First, I have saved about `2367 stocks` and `9,952,847 daily datas` in file-system database from Korea market (KOSPI, KOSDAQ) on 2023/02/17.

There are so many informations as like Stock Price, Volume, Adjusted Stock Price, MarketCap in only one daily data.

This below process is how to get daily datas.

1. Download the current list of korean stocks
2. Check the daily datas need the update or not (Price Update, Adjusting Stock Price, and so on)
3. If needed, Get the datas from Kiwoom Open API and Naver Finance
4. Save the datas in form of daily datas in file-system database

I make it to download datas `using asynchronous function` because of download time limit in Kiwoom OpenAPI.

<br/>

It is easy to use like below codes :
(this is preview)

```C#
// How to download the new stock daily datas from Kiwoom OpenAPI
int i = 0; // want to see in specific index number
Stock[] stocks = KwGetStockList().stocks; // user-defined function (except Ritz, ETF, ETN, Spac)
stocks[i].DailyDatas = KwGetDailyData(stock, null); // user-defined inner asynchronous function
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

- This is the overview of my class files and file-system database

![preview2](https://user-images.githubusercontent.com/55177359/211186525-b162f5e3-0e1a-40c0-af47-057d6e3afd78.png)

<br/>

-----

<br/>

Second, I made a `saving of 72% previous processing time` in calculating Leading Span of Ichimoku about `a data set of 10 million`.

<br/>

There are two ways to get the Maximum and Minimum Value in Specific Range to calculate the Leading Span of Ichimoku *(n : a number of all daily datas contained every stocks)*

  - Calculate the Maximum and Minimum value using the `Linear way`
    - Whenever the index is changed, try to get a maximum and minimum value `calculating every 52 elements`
    - It would be needed the time `Θ(52 * n)` to calculate all stocks
  
  - Calculate the Maximum and Minimum value using the `Segment Tree Algorithm`
    - Before calculating, **make a Segment Tree**
    - Whenever the index is changed, try to get a maximum and minimum value `using Segment Tree`
    - It would be needed the time `Θ(log(52) * n)` to calculate all stocks

<br/>

While developing the logic, I applied the `Segment Tree Algorithm`.

As a result, I made a `saving of 72% previous processing time` about `a data set of 10 million`. (You could see the detailed process in [here](https://github.com/DevTae/StockDatabasePreview/blob/main/SegmentTreeAlgorithm.md))

![result_capture](https://user-images.githubusercontent.com/55177359/222949478-7207a194-ed74-4f76-9d83-62f5a7e43ca6.png)

<br/>

-----

<br/>

Third, I use these datas **to make own buying/selling strategy**.

- Until now, I have studied and analyzed so many stocks and price patterns using this program

![stock-analysis-archive](https://user-images.githubusercontent.com/55177359/222942273-c536fc6c-b441-4672-9667-41a61b0d4110.png)

<br/>

-----

<br/>

Finally, Here are many features in 'StockDatabase' projects.


- Save and Update the daily datas automatically

![updatedailydata](https://user-images.githubusercontent.com/55177359/222940109-4bb442aa-9ebb-429b-a3f5-9500225dcd30.gif)

<br/>

- Backtest using the custom buying/selling strategy

![backtest](https://user-images.githubusercontent.com/55177359/222940351-1cef5cac-c554-4c6e-b07d-32591530f29f.gif)

<br/>

- View the chart of searched results with auto-analyzing-tool

![viewthechart](https://user-images.githubusercontent.com/55177359/222940379-a8a3c1b3-5ab4-4783-9026-75996ae861fa.gif)

<br/>

- Calculate the `Linear Regression Model` using the `Least Square Method`
- Analyze the rank of the theme and sector

![linearregression](https://user-images.githubusercontent.com/55177359/222940238-4b564d53-d80b-4bbd-a042-f160636f30b7.png)

<br/>

-----

<br/>

The End.

Thank you for reading!
