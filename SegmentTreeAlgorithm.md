# Applying Segment Tree Algorithm on Calculating Ichimoku Indicator

-----

*Made by DevTae*

<br/>

- What is Ichimoku indicator?
> It is a popular **Price Indicator** on stock market, that helps to analyze the price in many points of view. <br/>
> Among many Ichimoku Indicators, I chose to calculate only `LeadingSpan1` and `LeadingSpan2`.

<br/>

- How to calculate LeadingSpan1 and LeadingSpan2 in Ichimoku indicator?

```C#
// To get leadingSpan indicators, Let's start to calculate

shiftHighestInShort = (Highest Price between the (index - (9 - 1) - 26) and (index - 26))
shiftHighestInMid = (Highest Price between the (index - (26 - 1) - 26) and (index - 26))
shiftHighestInLong = (Highest Price between the (index - (52 - 1) - 26) and (index - 26))
shiftLowestInShort = (Lowest Price between the (index - (9 - 1) - 26) and (index - 26))
shiftLowestInMid = (Lowest Price between the (index - (26 - 1) - 26) and (index - 26))
shiftLowestInLong = (Lowest Price between the (index - (52 - 1) - 26) and (index - 26))

leadingSpan1[index] = (shiftedHighestInShort + shiftedLowestInShort + shiftedHighestInMid + shiftedLowestInMid) / 4;
leadingSpan2[index] = (shiftedHighestInLong + shiftedLowestInLong) / 2;
```

<br/>

-----

<br/>

- Two ways to Calculate the Maximum and Minimum Value in Specific Range *(n : a number of daily datas, m : a number of stocks)*

  - Calculate the Maximum and Minimum value using the `Linear way`
    - Whenever the index is changed, try to get a maximum and minimum value `calculating every 52 elements`
    - It would be needed the time `Θ(52 * n * m)` to calculate all stocks
  
  - Calculate the Maximum and Minimum value using the `Segment Tree Algorithm`
    - Before calculating, **make a Segment Tree**
    - Whenever the index is changed, try to get a maximum and minimum value `using Segment Tree`
    - It would be needed the time `Θ(log(52) * n * m)` to calculate all stocks

<br/>

-----

<br/>

- **Result of Difference** between the `Linear way` and `Segment Tree Algorithm`
  - Test the calculating LeadingSpan indicators from `2367 stocks` and `9,952,847 daily datas`
  - A time of calculating in **Linear way** needed `258761 ms`
  - A time of calculating using **Segment Tree Algorithm** needed `73335 ms`
  - Make a `Saving of 72% processing time` in calculating Leading Span of Ichimoku by using the `Segment Tree Algorithm`

![result_capture](https://user-images.githubusercontent.com/55177359/222949478-7207a194-ed74-4f76-9d83-62f5a7e43ca6.png)

<br/>

-----

<br/>

- Test codes are below

  ```C#
	// Segment Tree Class Code
	public class Node
	{
		public Node left;
		public Node right;
		public float max;
		public float min;
		public int start;
		public int end;

		public Node(ref DailyData[] dailyDatas, int start, int end)
		{
			this.start = start;
			this.end = end;

			if(start == end) {
				max = float.Parse(dailyDatas[start].AdjustedHigh);
				min = float.Parse(dailyDatas[start].AdjustedLow);
			} else {
				int mid = (start + end) / 2;
				left = new Node(ref dailyDatas, start, mid);
				right = new Node(ref dailyDatas, mid + 1, end);

				max = Math.Max(left.max, right.max);
				min = Math.Min(left.min, right.min);
			}
		}

		public float Max(int start, int end)
		{
			if(this.end < start || this.start > end) return Int32.MinValue;
			if(start <= this.start && this.end <= end) return max;

			return Math.Max(left.Max(start, end), right.Max(start, end));
		}

		public float Min(int start, int end)
		{
			if(this.end < start || this.start > end) return Int32.MaxValue;
			if(start <= this.start && this.end <= end) return min;

			return Math.Min(left.Min(start, end), right.Min(start, end));
		}
  ```


  ```C#  
	// To compare the running time using Segment Tree Algorithm or not
	Stock[] stocks = Stock.ReadListFile("한국주식(키움)");
	Stopwatch watch; // To get the time to calculate

	// Period Option of Indicator named 'Ichimoku'
	int nShortPeriod = 9;
	int nMidPeriod = 26;
	int nLongPeriod = 52;
	int nMaxPeriodOfRange = nLongPeriod;
	int nShift = nMidPeriod;

	// Initialize the Stopwatch
	watch = new Stopwatch();
	watch.Start();

	// Calculating Ichimoku Indicator in previous way
	for(int i = 0; i < stocks.Length; i++) {
		// Load the dailyDatas
		DailyData[] dailyDatas = DailyData.ReadDailyData(ref stocks[i]).dailyDatas;
		stocks[i].DailyDatas = dailyDatas;
		int size = stocks[i].DailyDatas.Length;

		float[] leadingSpan1 = new float[stocks[i].DailyDatas.Length];
		float[] leadingSpan2 = new float[stocks[i].DailyDatas.Length];

		// Calculate the leading span 1 and 2
		for(int j = 0; j < size; j++) {
			if(j < nMaxPeriodOfRange - 1 + nShift) {
				continue;
			} else {
				float shiftedHighestInShort = float.MinValue;
				float shiftedHighestInMid = float.MinValue;
				float shiftedHighestInLong = float.MinValue;
				float shiftedLowestInShort = float.MaxValue;
				float shiftedLowestInMid = float.MaxValue;
				float shiftedLowestInLong = float.MaxValue;

				// During nShortPeriod, Get the Maximum and Minimum value
				for(int k = j - (nShortPeriod - 1) - nShift; k <= j - nShift; k++) {
					shiftedHighestInShort = Math.Max(shiftedHighestInShort, float.Parse(dailyDatas[k].AdjustedHigh));
					shiftedLowestInShort = Math.Min(shiftedLowestInShort, float.Parse(dailyDatas[k].AdjustedLow));
				}

				// During nMidPeriod, Get the Maximum and Minimum value
				for(int k = j - (nMidPeriod - 1) - nShift; k <= j - nShift; k++) {
					shiftedHighestInMid = Math.Max(shiftedHighestInMid, float.Parse(dailyDatas[k].AdjustedHigh));
					shiftedLowestInMid = Math.Min(shiftedLowestInMid, float.Parse(dailyDatas[k].AdjustedLow));
				}

				// During nLongPeriod, Get the Maximum and Minimum value
				for(int k = j - (nLongPeriod - 1) - nShift; k <= j - nShift; k++) {
					shiftedHighestInLong = Math.Max(shiftedHighestInLong, float.Parse(dailyDatas[k].AdjustedHigh));
					shiftedLowestInLong = Math.Min(shiftedLowestInLong, float.Parse(dailyDatas[k].AdjustedLow));
				}

				leadingSpan1[j] = (shiftedHighestInShort + shiftedLowestInShort + shiftedHighestInMid + shiftedLowestInMid) / 4;
				leadingSpan2[j] = (shiftedHighestInLong + shiftedLowestInLong) / 2;
			}
		}

		// do something ...

		stocks[i].DailyDatas = null;

	}

	watch.Stop();

	Console.WriteLine("Calculating Ichimoku Indicator in previous way : " + watch.ElapsedMilliseconds.ToString() + " ms");

	// Initialize the Stopwatch
	watch = new Stopwatch();
	watch.Start();

	// Calculating Ichimoku Indicator using Segment Tree Algorithm
	for(int i = 0; i < stocks.Length; i++) {
		// Load the daily datas from each stock
		DailyData[] dailyDatas = DailyData.ReadDailyData(ref stocks[i]).dailyDatas;
		stocks[i].DailyDatas = dailyDatas;
		int size = stocks[i].DailyDatas.Length;

		float[] leadingSpan1 = new float[stocks[i].DailyDatas.Length];
		float[] leadingSpan2 = new float[stocks[i].DailyDatas.Length];

		// Use the Segment Tree named 'Node' user-defined class to get a maximum, minimum value in specific range
		Node head = new Node(ref dailyDatas, 0, dailyDatas.Length - 1);

		// Calculate the leading span 1 and 2
		for(int j = 0; j < size; j++) {
			if(j < nMaxPeriodOfRange - 1 + nShift) {
				continue;
			} else {
				float shiftedHighestInShort = head.Max(j - (nShortPeriod - 1) - nShift, j - nShift);
				float shiftedHighestInMid = head.Max(j - (nMidPeriod - 1) - nShift, j - nShift);
				float shiftedHighestInLong = head.Max(j - (nLongPeriod - 1) - nShift, j - nShift);
				float shiftedLowestInShort = head.Min(j - (nShortPeriod - 1) - nShift, j - nShift);
				float shiftedLowestInMid = head.Min(j - (nMidPeriod - 1) - nShift, j - nShift);
				float shiftedLowestInLong = head.Min(j - (nLongPeriod - 1) - nShift, j - nShift);

				leadingSpan1[j] = (shiftedHighestInShort + shiftedLowestInShort + shiftedHighestInMid + shiftedLowestInMid) / 4;
				leadingSpan2[j] = (shiftedHighestInLong + shiftedLowestInLong) / 2;
			}
		}

		// do something ...

		stocks[i].DailyDatas = null;
	}

	watch.Stop();

	Console.WriteLine("Calculating Ichimoku Indicator using Segment Tree Algorithm : " + watch.ElapsedMilliseconds.ToString() + " ms");
  ```
  
<br/>

-----

<br/>

The End.

Thank you for reading!
