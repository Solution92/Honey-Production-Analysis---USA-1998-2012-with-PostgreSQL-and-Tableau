# Honey-Production-Analysis-USA-1998-2012-with-PostgreSQL-and-Tableau

### Project Overview

In 2006, global concern was raised over the [rapid decline in the honeybee population](https://www.abc.net.au/news/2017-05-08/colony-collapse-ten-years-after-crisis-what-is-happening-to-bees/8507408), an integral component to American honey agriculture. Large numbers of hives were lost to Colony Collapse Disorder, a phenomenon of disappearing worker bees causing the remaining hive colony to collapse. Speculation about the cause of this disorder points to hive diseases and pesticides harming the pollinators, though no overall consensus has been reached. 

Twelve years later, [some industries are observing recovery](https://www.bloomberg.com/news/articles/2017-08-01/good-news-for-bees-as-numbers-recover-while-mystery-malady-wanes) but the [American honey industry is still largely struggling](https://www.washingtonpost.com/news/wonk/wp/2015/05/14/dying-bees-could-mean-the-end-of-american-honey/?utm_term=.31aeb8175b56). The U.S. used to locally produce over half the honey it consumes per year. Now, honey mostly comes from overseas, with 350 of the 400 million pounds of honey consumed every year originating from imports. This dataset provides insight into honey production supply and demand in America by state from 1998 to 2012.

The National Agricultural Statistics Service (NASS) is the primary data reporting body for the US Department of Agriculture (USDA). NASS's mission is to "provide timely, accurate, and useful statistics in service to U.S. agriculture". From datasets to census surveys, their data covers virtually all aspects of U.S. agriculture. Honey production is one of the datasets offered. 

### Project Objective

Here are the most insightful questions we want to concentrate on in this project:

- How has honey production yield changed from 1998 to 2012?
- Over time, which states produce the most honey? Which produces the least? Which have experienced the most change in honey yield?
- Does the data show any trends in terms of the number of honey-producing colonies and yield per colony before 2006, which was when concern over Colony Collapse Disorder spread nationwide?
- Are there any patterns that can be observed between total honey production and the value of production every year? How has the value of production, which in some sense could be tied to demand, changed every year?
- What are the minimum, maximum, and average prices?
- Determine the top Producing State.

### Tools Used

- PostgreSQL is Used For Data Cleaning and Transformation.
- Tableau is used for Analysis and Creating Reports.

### Data Source

You can access the dataset [here](https://www.kaggle.com/datasets/jessicali9530/honey-production)

### Data Cleaning and Preparation

The dataset comes in CSV file format and has 8 fields and 626 rows with some of the following as the column header: state, numcol,	yield_percol,	total_prod,	stocks,	price_perlb,	prod_value, and	year.

Below is part of the raw dataset.

![Honey_Raw](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/40199887-5e8c-437b-9179-d7c98001a61f)

### Data Analysis

Here is the comprehensive analysis with PostgreSQL

~~~SELECT * FROM HONEY_PRODUCTION;

SELECT STATE FROM HONEY_PRODUCTION;

-- KPI CALCULATION 

--Total Honey Production:

SELECT SUM(totalprod::numeric) AS total_honey_production
FROM honey_production;

2609848000

-- Average Yield per Colony:
--Calculate the average yield per colony for each state.

SELECT state, AVG(yieldpercol::numeric) AS avg_yield_per_colony
FROM honey_production
GROUP BY state;

--I want to get TOP 5 of this AVG rounded to the nearest whole number.

SELECT state, ROUND(AVG(yieldpercol::numeric)) AS avg_yield_per_colony
FROM honey_production
GROUP BY state
ORDER BY avg_yield_per_colony DESC
LIMIT 5;

"HI"	98
"LA"	96
"ND"	88
"MS"	87
"FL"	83

-- Total Production per Year:
--Determine the total honey production for each year.

SELECT year, SUM(totalprod::numeric) AS total_production
FROM honey_production
GROUP BY year;

"2011"	147201000
"2000"	219558000
"2007"	147621000
"2008"	162972000
"2002"	171265000
"2001"	185748000
"2010"	175294000
"1998"	219519000
"2012"	140907000
"2006"	154238000
"2004"	182729000
"2009"	145068000
"2003"	181372000
"1999"	202387000
"2005"	173969000

--Average Price per Pound:
--Calculate the average price per pound of honey for each year.

SELECT year, AVG(priceperlb) AS avg_price_per_pound
FROM honey_production
GROUP BY year;

"2011"	2.16
"2000"	0.79
"2007"	1.43
"2008"	1.62
"2002"	1.37
"2001"	0.91
"2010"	1.92
"1998"	0.83
"2012"	2.36
"2006"	1.30
"2004"	1.28
"2009"	1.81
"2003"	1.49
"1999"	0.80
"2005"	1.19

--Top States by Total Production:
--Identify the top states based on total honey production.

SELECT state, SUM(totalprod::numeric) AS total_production
FROM honey_production
GROUP BY state
ORDER BY total_production DESC
LIMIT 5;

"ND"	475085000
"CA"	347535000
"SD"	266141000
"FL"	247048000
"MT"	156562000


--ANALYSES in PostgreSQL? 

--Yearly Total Production:
--Determine the total honey production for each year.

SELECT year, SUM(totalprod::numeric) AS total_production
FROM honey_production
GROUP BY year;

year    total_production
"2011"	147201000
"2000"	219558000
"2007"	147621000
"2008"	162972000
"2002"	171265000
"2001"	185748000
"2010"	175294000
"1998"	219519000
"2012"	140907000
"2006"	154238000
"2004"	182729000
"2009"	145068000
"2003"	181372000
"1999"	202387000
"2005"	173969000

--State-wise Average Yield:
--Calculate the average yield per colony for each state.

SELECT state, AVG(yieldpercol::numeric) AS avg_yield_per_colony
FROM honey_production
GROUP BY state;

state   avg_yield_per_colony
"CA"	55.8000000000000000
"OR"	44.4000000000000000
"TX"	70.5333333333333333
"ND"	88.0666666666666667
"NV"	45.1818181818181818
"OH"	63.5333333333333333
"KY"	52.1333333333333333
"NY"	69.1333333333333333
"HI"	98.0000000000000000
"NM"	53.8666666666666667
"MS"	87.4666666666666667
"IN"	60.9333333333333333
"WV"	48.5333333333333333
"NE"	67.4666666666666667
"MO"	53.4000000000000000
"FL"	83.0666666666666667
"ME"	31.0666666666666667
"AR"	73.9333333333333333
"WI"	79.4000000000000000
"NC"	47.8000000000000000
"SD"	75.8000000000000000
"OK"	46.3333333333333333
"ID"	44.0000000000000000
"GA"	54.6666666666666667
"MN"	74.2666666666666667
"PA"	50.4000000000000000
"MD"	45.0000000000000000
"WY"	66.1333333333333333
"LA"	95.7333333333333333
"MT"	77.3333333333333333
"IL"	61.6666666666666667
"TN"	56.3333333333333333
"WA"	48.8000000000000000
"NJ"	36.9333333333333333
"MI"	68.1333333333333333
"AL"	67.5333333333333333
"UT"	46.5333333333333333
"IA"	65.7333333333333333
"VT"	66.9333333333333333
"CO"	62.8000000000000000
"SC"	68.0000000000000000
"VA"	40.2000000000000000
"AZ"	60.0666666666666667
"KS"	56.0666666666666667


--Price Range Analysis:
--Identify the minimum, maximum, and average price per pound of honey.

SELECT 
    MIN(priceperlb) AS min_price,
    MAX(priceperlb) AS max_price,
    AVG(priceperlb) AS avg_price
FROM honey_production;

min_price  max_price     avg_price
0.49	   4.15	         1.4095686900958448


--Top States by Total Production:
--Identify the top states based on total honey production.

SELECT state, SUM(totalprod::numeric) AS total_production
FROM honey_production
GROUP BY state
ORDER BY total_production DESC
LIMIT 5;

state   total_production
"ND"	475085000
"CA"	347535000
"SD"	266141000
"FL"	247048000
"MT"	156562000
~~~

### Result and Findings

We want to analytically answer the question with individual charts



- How has honey production yield changed from 1998 to 2012?

![Yearly prod  Yield](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/75827f3a-d744-493e-bbf8-2095946e2d9e)

From the line chat above it can be deduced that the honey production yield has increased from 131,568,000 in 1998 to 280,725,000 in 2012. The chart shows that there were significant fluctuations in the yield over these years with noticeable peaks around the years of 2000 and then again in between years about a decade later.



 - Over time, which states produce the most honey? Which produces the least? Which have experienced the most change in honey yield?

![Honey State Wise Avg](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/1cef34b2-bb91-42ce-b95f-6891513461be)

The bar chart simply shows that Louisiana (LA) produces the most honey with an average yield per hive of over 100 lbs. Maine (ME) produces the least honey with an average yield of just above 30 lbs per hive.



- Does the data show any trends in terms of the number of honey-producing colonies and yield per colony before 2006, which was when concern over Colony Collapse Disorder spread nationwide?

![Yearly prod  Yield](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/ee5d1441-139c-45ee-809d-b7f4ea6c1d4f)

Using the first line chart, yes, the data shows a trend of fluctuation in honey production yield before 2006. The production yield increased from 1997 to 1999, decreased sharply until 2001, and then increased again until it peaked in 2003. After that, there was a decline until 2006.



- Are there any patterns that can be observed between total honey production and the value of production every year? How has the value of production, which in some sense could be tied to demand, changed every year?

![Yearly prod  Yield](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/9b1ecf64-1b36-41a5-88a3-fc0c0f0aac08)

Using the first line chart also, there are patterns observable in the data. The total honey production and value of production seem to follow a similar trend over the years. Both metrics experienced a significant increase from 1997 to 2002, followed by a decline until around 2008, and then another increase thereafter. The value of production has fluctuated but generally follows the trend of total honey production.



- What are the minimum, maximum, and average prices?

![Honey Price Range](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/58bf9df3-67b7-49e0-b56b-bdf3160da1e3)

The minimum price per lb is 0.490000, the maximum price per lb is around 4.15000, and the average price per lb is 1.40957



- Determine the top Producing State.

![Top producing State](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/8fb92f9a-62c2-43d7-a98b-37e704c0c1b2)

North Dakota (ND) is the top-producing state with a total production of 475,085,000M.

### The Dashboard

![Honey Dashboard](https://github.com/Solution92/Honey-Production-Analysis---USA-1998-2012-with-PostgreSQL-and-Tableau/assets/144762124/07fd2bec-7585-4e7e-94a2-27711259b18a)

### Recommendation

- Based on the analysis of the honey production dataset, it is recommended to closely monitor and address the fluctuations in honey production yields, especially around years of significant peaks.

- Louisiana consistently stands out as the top honey-producing state, emphasizing the importance of supporting and sustaining beekeeping efforts in that region.
  
- Additionally, attention should be given to the trends in total honey production and production value, as they seem closely correlated, reflecting potential market demand dynamics.

- Furthermore, considering the fluctuations in honey prices, stakeholders should stay vigilant and adopt strategies to navigate price variations effectively.

- Finally, recognizing North Dakota as the top-producing state highlights the need for sharing best practices from successful honey-producing regions to enhance overall production nationwide.











