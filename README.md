# StockDatabasePreview
-----

*Made by DevTae*

This is the repository that summarizes about my own project named 'DevTae/StockDatabase'

- Overview
  - First, I have saved about `2367 stocks` and `9,952,847 daily datas` in file-system database from Korea market (KOSPI, KOSDAQ).
  - Second, I applied `the asynchronous method` on downloading stock datas from Kiwoom OpenAPI and Naver Finance
  - Thrid, I made a `saving of 72% previous processing time` in calculating Leading Span of Ichimoku about `a data set of 10 million`.
  - Fourth, I used these datas **to make own buying/selling strategy**.
  - Finally, Here are many features in 'StockDatabase' projects.

-----

First, I have saved about `2367 stocks` and `9,952,847 daily datas` in file-system database from Korea market (KOSPI, KOSDAQ) on 2023/02/17.

There are so many informations as like `Stock Price`, `Volume`, `Adjusted Stock Price`, `MarketCap` in only one daily data.

<br/>

- This is the overview of class files and file-system database

![preview2](https://user-images.githubusercontent.com/55177359/211186525-b162f5e3-0e1a-40c0-af47-057d6e3afd78.png)

<br/>

- Example code is [here](https://github.com/DevTae/StockDatabasePreview/blob/main/DownloadDailyDatas.md)

<br/>

-----

<br/>

Second, I applied `the asynchronous method` on downloading stock datas from Kiwoom OpenAPI and Naver Finance

<br/>

- I make it to download datas `using asynchronous function` because of the download limit in Kiwoom OpenAPI.

  - **Calling Kiwoom API** is very comfort to save all stock datas `including the information adjusting stock price event`
  - But, **Calling Kiwoom API** could be possible `only 1000 times in 1 hour` because of their API limit rule *(too slow)*
  - In contrast, **Naver Finance Crawling** does *not have any restriction* on downloading *sise* datas [as like this link](https://finance.naver.com/robots.txt)
  - But, **Naver Finance** does `not have any information about the adjusting stock price event` in *sise* datas
  - Finally, I decided to use **both Kiwoom API and Naver Finance** to `improve the download speed` and `get the information of the adjusting stock price event`

- You could see briefly how to process at **bottom pictures** that show features of the running program

<br/>

-----

<br/>

Third, I made a `saving of 72% previous processing time` in calculating Leading Span of Ichimoku about `a data set of 10 million`.

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

As a result, I made a `saving of 72% previous processing time` about `a data set of 10 million`. (You could see the detailed process [in here](https://github.com/DevTae/StockDatabasePreview/blob/main/SegmentTreeAlgorithm.md))

![result_capture](https://user-images.githubusercontent.com/55177359/222949478-7207a194-ed74-4f76-9d83-62f5a7e43ca6.png)

<br/>

-----

<br/>

Fourth, I used these datas **to make own buying/selling strategy**.

- It could be totally possible to **analyze the price patterns of stock** using this program with backtesting results.

- Until now, I have studied and analyzed so many stocks and price patterns using this program

![stock-analysis-archive](https://user-images.githubusercontent.com/55177359/222942273-c536fc6c-b441-4672-9667-41a61b0d4110.png)

<br/>

-----

<br/>

Finally, Here are many features in 'StockDatabase' projects.


- Update the daily datas automatically *from Kiwoom API and Naver Finance*

![updatedailydata](https://user-images.githubusercontent.com/55177359/222940109-4bb442aa-9ebb-429b-a3f5-9500225dcd30.gif)

<br/>

- Backtesting Simulation with the custom buying/selling strategy

![backtest](https://user-images.githubusercontent.com/55177359/222940351-1cef5cac-c554-4c6e-b07d-32591530f29f.gif)

<br/>

- View the chart of searched backtesting results *with auto-analyzing-tool*

![viewthechart](https://user-images.githubusercontent.com/55177359/222940379-a8a3c1b3-5ab4-4783-9026-75996ae861fa.gif)

<br/>

- Calculate the `Linear Regression Model` using the `Least Square Method`
- Analyze the rank of the theme and sector

![linearregression](https://user-images.githubusercontent.com/55177359/222940238-4b564d53-d80b-4bbd-a042-f160636f30b7.png)

<br/>

-----

<br/>

**\<Information about Source Distribution\>**

- The source code of this project is saved on private repository, because **I worried about the misusing this project to their buying / selling strategy**.

- Whenever if you have any questions for this project, contact me for anything using email.
  - e-mail is here. → dev.taehyeon.kim@gmail.com

<br/>

-----

<br/>

The End.

Thank you for reading!

