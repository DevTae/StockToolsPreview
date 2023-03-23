# StockDatabasePreview
-----

*Made by DevTae*

This is the repository that summarizes about my own project named 'DevTae/StockDatabase'

- Overview
  - [Saving about `2367 stocks` and `9,952,847 daily datas` in File-System database](#saving-about-2367-stocks-and-9952847-daily-datas-in-file-system-database)
  - [Adopting `the asynchronous method` on downloading stock datas from Kiwoom OpenAPI and Naver Finance](#adopting-the-asynchronous-method-on-downloading-stock-datas-from-kiwoom-openapi-and-naver-finance)
  - [`Saving of 72% previous processing time` in calculating Leading Span of Ichimoku about `a data set of 10 million`](#saving-of-72-previous-processing-time-in-calculating-leading-span-of-ichimoku-about-a-data-set-of-10-million)
  - [Using these datas **to make own buying/selling strategy**](#using-these-datas-to-make-own-buyingselling-strategy)

- [Features in 'StockDatabase' Project](#features-in-stockdatabase-project)

<br/>

-----

### Saving about `2367 stocks` and `9,952,847 daily datas` in File-System database

- First, I have saved about **a lot of stock datas** in File-System database from Korea market (KOSPI, KOSDAQ) on 2023/02/17.

- There are so many informations as like `Stock Price`, `Volume`, `Adjusted Stock Price`, `MarketCap` in only one daily data.

<br/>

- This is the overview of class files and file-system database

![preview2](https://user-images.githubusercontent.com/55177359/211186525-b162f5e3-0e1a-40c0-af47-057d6e3afd78.png)

<br/>

- Example code is [here](https://github.com/DevTae/StockDatabasePreview/blob/main/DownloadDailyDatas.md)

<br/>

-----

<br/>

### Adopting `the asynchronous method` on downloading stock datas from Kiwoom OpenAPI and Naver Finance

- I make it to download datas `using asynchronous function` because of the download limit in Kiwoom OpenAPI.

  - **Calling Kiwoom API** could download the data of the `adjusting stock price event`, however, there is API limit rule that could download `only 1000 times in 1 hour` *(too slow)*
  
  - **Naver Finance Crawling** does *not have any restriction* on downloading *sise* datas [as like this link](https://finance.naver.com/robots.txt), however, there is `not any information about the adjusting stock price event` in *sise* datas
  
  - Finally, I decided to use **both Kiwoom API and Naver Finance** to `improve the download speed` and `get the information of the adjusting stock price event`

- You could see the running program briefly how to process [at bottom pictures](#features-in-stockdatabase-project)

<br/>

-----

<br/>

### `Saving of 72% previous processing time` in calculating Leading Span of Ichimoku about `a data set of 10 million`

There are two ways to get the Maximum and Minimum Value in Specific Range to calculate the Leading Span of Ichimoku *(n : a number of all daily datas contained every stocks)*

  - Calculate the Maximum and Minimum value using the `Linear way`
    - Whenever the index is changed, try to get a maximum and minimum value `calculating every 52 elements`
    - It would be needed the time `Θ(52 * n)` to calculate all daily datas on each stock
  
  - Calculate the Maximum and Minimum value using the `Segment Tree Algorithm`
    - Before calculating, **make a Segment Tree**
    - Whenever the index is changed, try to get a maximum and minimum value `using Segment Tree`
    - It would be needed the time `Θ(log(n) * n)` to calculate all daily datas on each stock
    
  - `Θ(52 * n)` vs `Θ(log(n) * n)`
    - I estimated that `n` is a average of having daily datas on each stock
    - `n` = `9,952,847 daily datas` / `2367 stocks` = `4204`
    - `Θ(52 * n) = 218,608` vs `Θ(log(n) * n) = 50,448`
    - The winner is `Θ(log(n) * n)`

<br/>

While developing the logic, I applied the `Segment Tree Algorithm`.

As a result, I made a `saving of 72% previous processing time` about `a data set of 10 million`. (You could see the detailed process [in here](https://github.com/DevTae/StockDatabasePreview/blob/main/SegmentTreeAlgorithm.md))

![result_capture](https://user-images.githubusercontent.com/55177359/222949478-7207a194-ed74-4f76-9d83-62f5a7e43ca6.png)

<br/>

-----

### Using these datas **to make own buying/selling strategy**

- It could be totally possible to **analyze the price patterns of stock** using this program with backtesting results.

- Until now, I have studied and analyzed so many stocks and price patterns using this program

![stock-analysis-archive](https://user-images.githubusercontent.com/55177359/222942273-c536fc6c-b441-4672-9667-41a61b0d4110.png)

<br/>

-----

## Features in 'StockDatabase' Project

- Update the daily datas automatically *from Kiwoom API and Naver Finance*

![updatedailydata](https://user-images.githubusercontent.com/55177359/222940109-4bb442aa-9ebb-429b-a3f5-9500225dcd30.gif)

<br/>

- Backtesting Simulation with the custom buying/selling strategy

![backtest](https://user-images.githubusercontent.com/55177359/222940351-1cef5cac-c554-4c6e-b07d-32591530f29f.gif)

<br/>

- Showing the chart of searched backtesting results *with many indicators*

![viewthechart](https://user-images.githubusercontent.com/55177359/222940379-a8a3c1b3-5ab4-4783-9026-75996ae861fa.gif)

<br/>

- Analyzing the rank of the theme and sector

![linearregression](https://user-images.githubusercontent.com/55177359/222940238-4b564d53-d80b-4bbd-a042-f160636f30b7.png)

<br/>

-----

### Information about Source Distribution

- The source code of this project is saved on private repository, because **I worried about the misusing this project to their buying / selling strategy**.

- Whenever if you have any questions for this project, contact me for anything using email.
  - e-mail is here. → dev.taehyeon.kim@gmail.com

<br/>

-----

### The End

Thank you for reading!

