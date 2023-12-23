# StockToolsPreview
-----

*Made by `DevTae`*

This is the repository that summarizes about my own project named `DevTae/StockTools`

- Program Features
  - `StockDatabase`, `StockDatabaseHelper`
    - File-System Database
    - **Downloading** Adjusted Price Datas, Theme, Industry datas
    - **Calculating** Many Indicators as like Ichimoku
    - Auto Update Helper Tool
  - `StockBacktester`
    - **Calculating** Trendlines on Each Stock Chart
    - **Calculating** Statistical Clusters about Theme and Industry
    - **Backtesting** Simple Profitability Ratio Result Using Custom Signal
  - `AutoTradingBot`
  - [Screenshots in 'StockDatabase' and 'StockBacktester' Project](#screenshots-in-stockdatabase-and-stockbacktester-project)

- Technical Contents
  - [`StockDatabase` Project](#stockdatabase-project)
    - [Saving about `2367 stocks` and `9,952,847 daily datas` in File-System database](#saving-about-2367-stocks-and-9952847-daily-datas-in-file-system-database)
    - [Adopting `the asynchronous method` on loading stock datas from Kiwoom OpenAPI and Naver Finance](#adopting-the-asynchronous-method-on-loading-stock-datas-from-kiwoom-openapi-and-naver-finance)
    - [`Saving of 72% previous processing time` in calculating indicator named `Leading Span of Ichimoku` about `a data set of 10 million`](#saving-of-72-previous-processing-time-in-calculating-indicator-named-leading-span-of-ichimoku-about-a-data-set-of-10-million)
  - [`StockBacktester` Project](#stockbacktester-project)
    - [Using these `backtesting datas` **to make own buying/selling strategy**](#using-these-backtesting-datas-to-make-own-buyingselling-strategy)
  - [Information about Source Distribution](#information-about-source-distribution)

<br/>

-----

## Program Features

### Screenshots in 'StockDatabase' and 'StockBacktester' Project

- Update the daily datas automatically `from Kiwoom API and Naver Finance`

![updatedailydata](https://user-images.githubusercontent.com/55177359/222940109-4bb442aa-9ebb-429b-a3f5-9500225dcd30.gif)

<br/>

- Backtesting Simulation with the custom buying/selling strategy as like `Uptrend Signal` (*on the bottom right*) 

![backtest](https://user-images.githubusercontent.com/55177359/222940351-1cef5cac-c554-4c6e-b07d-32591530f29f.gif)

<br/>

- Showing the chart of searched backtesting results with many indicators like `trendlines`, `theme clusters`

![viewthechart](https://user-images.githubusercontent.com/55177359/222940379-a8a3c1b3-5ab4-4783-9026-75996ae861fa.gif)

![21 12 28 모아텍 자동패턴분석](https://github.com/DevTae/StockToolsPreview/assets/55177359/0a93f669-c543-4c64-995d-a2375571937b)

<br/>

-----

## Technical Contents

### `StockDatabase` Project

#### Saving about `2367 stocks` and `9,952,847 daily datas` in File-System database

- I have saved about **a lot of stock datas** in File-System database from Korea market (KOSPI, KOSDAQ) until 2023/02/17.
  - There are so many informations as like `Stock Price`, `Volume`, `Adjusted Stock Price`, `MarketCap` in each daily data.
  - Below this, that is the structure of `class diagram`.
  - Example code is [here](https://github.com/DevTae/StockDatabasePreview/blob/main/DownloadDailyDatas.md)

```
📦Stock
 ┣ 📂StockInfo             // 종목에 대한 정보가 담겨 있음.
 ┃ ┣ 📂Market
 ┃ ┃ ┣ 📜Country           // ex. "한국주식(키움)", ...
 ┃ ┃ ┣ 📜Code              // ex. 0, 1, 2, ...
 ┃ ┃ ┣ 📜Name              // ex. "코스피", "코스닥", ...
 ┃ ┃ ┗ 📜Index             // Static Variable 에 있는 Country 배열의 Index
 ┃ ┣ 📜Code                // ex. "005930"
 ┃ ┣ 📜Name                // ex. "삼성전자"
 ┃ ┣ 📜DebutedDate         // ex. "20190301" (상장일)
 ┃ ┗ 📜ActiveSharesRatio   // ex. 60.5 (유통비율)
 ┣ 📂PriceData
 ┃ ┣ 📂DailyData
 ┃ ┃ ┣ 📜Date
 ┃ ┃ ┣ 📜Open              // 시가
 ┃ ┃ ┣ 📜AdjustedOpen      // 시가 수정주가
 ┃ ┃ ┣ 📜High              // 고가
 ┃ ┃ ┣ 📜AdjustedHigh      // 고가 수정주가
 ┃ ┃ ┣ 📜Low               // 저가
 ┃ ┃ ┣ 📜AdjustedLow       // 저가 수정주가
 ┃ ┃ ┣ 📜Close             // 종가
 ┃ ┃ ┣ 📜AdjustedClose     // 종가 수정주가
 ┃ ┃ ┣ 📜Volume            // 거래량
 ┃ ┃ ┣ 📜AdjustedVolume    // 수정 거래량
 ┃ ┃ ┣ 📜Shares            // 상장 주식 수
 ┃ ┃ ┣ 📜MarketCap         // 시가총액
 ┃ ┃ ┣ 📜AdjustedEvent     // 수정주가 이벤트 발생 코드 (in Kiwoom API docs)
 ┃ ┃ ┣ 📜AdjustedRatio     // 수정주가 변동 비율
 ┃ ┃ ┗ 📜CheckAdjusted     // 수정주가 변동 여부 확인 column
 ┃ ┗ 📂WeeklyData
 ┣ 📜LastUpdatedDate       // 마지막 업데이트 날짜
 ┣ 📜LastAdjustedDate      // 마지막 수정주가 변동 날짜
 ┗ 📜LastAdjustedIndex     // 마지막 수정주가 변동 index
```

![preview2](https://user-images.githubusercontent.com/55177359/211186525-b162f5e3-0e1a-40c0-af47-057d6e3afd78.png)

<br/>

#### Adopting `the asynchronous method` on loading stock datas from Kiwoom OpenAPI and Naver Finance

- The program downloads many datas `using asynchronous function` because of the **download limit in Kiwoom OpenAPI**.

  - **Calling Kiwoom API** could download the data of the `adjusting stock price event`, however, there is API limit rule that could download `only 1000 times in 1 hour` *(too slow)*
  
  - **Naver Finance Scraping** does *not have any restriction* on loading *sise* datas [as like this link](https://finance.naver.com/robots.txt), however, there is `not any information about the adjusting stock price event` in *sise* datas
  
  - Finally, I decided to use **both Kiwoom API and Naver Finance** to `improve the loading speed` and `get the information of the adjusting stock price event`

- You could see the running program briefly how to process [at bottom pictures](#features-in-stockdatabase-and-stockbacktester-project)\

<br/>

#### `Saving of 72% previous processing time` in calculating indicator named `Leading Span of Ichimoku` about `a data set of 10 million`

There are two ways to get the Maximum and Minimum Value in Specific Range to calculate the Leading Span of Ichimoku *(n : a number of all daily datas contained every stocks)*

  - Calculate the Maximum and Minimum value using the `Linear way`
    - Whenever the index is changed, try to get a maximum and minimum value `calculating every 52 elements`
    - It would be needed the time `Θ(52 * n)` to calculate all daily datas on each stock
  
  - Calculate the Maximum and Minimum value using the `Segment Tree Algorithm`
    - Before calculating, **make a Segment Tree** *(it needs `Θ(n * log(n))`)*
    - Whenever the index is changed, try to get a maximum and minimum value `using Segment Tree`
    - It would be needed the time `Θ(log(n) * n)` to calculate all daily datas on each stock
    
  - `Θ(52 * n)` vs `Θ(log(n) * n)`
    - I estimated that `n` is the average of having daily datas on each stock
    - `n` = `9,952,847 daily datas` / `2367 stocks` = `4204`
    - `Θ(52 * n) = 218,608` vs `Θ(log(n) * n) = 50,448`
    - The winner is the `Θ(log(n) * n)`

<br/>

While developing the logic, I applied the `Segment Tree Algorithm`.

As a result, I made a `saving of 72% previous processing time` about `a data set of 10 million`. (You could see the detailed process [in here](https://github.com/DevTae/StockDatabasePreview/blob/main/SegmentTreeAlgorithm.md))

![result_capture](https://user-images.githubusercontent.com/55177359/222949478-7207a194-ed74-4f76-9d83-62f5a7e43ca6.png)

<br/>

-----

### `StockBacktester` Project

#### Using these `backtesting datas` **to make own buying/selling strategy**

- It could be totally possible to **analyze the price patterns of stock** using this program with backtesting results.
  - Until now, I have studied and analyzed so many stocks and price patterns using this program
  - Moreover, I tried to analyze the stock price using `Deep Learning Model`.
    - You could see on [this link](https://github.com/DevTae/StockPricePredictionPreview)

![stock-analysis-archive](https://user-images.githubusercontent.com/55177359/222942273-c536fc6c-b441-4672-9667-41a61b0d4110.png)

<br/>

-----

### Information about Source Distribution

- The source code of this project is saved on private repository, because **I worried about the misusing some strategies in this project to their buying / selling decision**.

- Whenever if you have any questions for this project, contact me for anything using email.
  - e-mail is here. → dev.taehyeon.kim@gmail.com

<br/>

-----

### The End

Thank you for reading!

