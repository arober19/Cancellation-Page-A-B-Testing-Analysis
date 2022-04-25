# **Project Brief**
### **Aidan Robertson      -     Monday April 25th 2022**
## **Objectives:**  
  **Analysis**

* What is the aproximate probability distribution between the test group and the control group
* Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL?
* Is a user that must call-in to cancel more likely to generate more revenues?
* Is a user that must call-in more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?
 
 **Visualization**
* Analysis must be coded in R or Python
* Analysis must be submitted to a github repository
* Analysis must be written in markdown format
* Please include at least 1 vizualization with your analysis
* Please use statistical significance tools to answer the questions we've asked
* Include the code you used to perform the analysis in the github repository
* Send us the link to your repository once you've completed the analysis

## **Current State:**

* The table testsamples.csv consisted of 59721 rows and 2 columns indicating the the sample id and test group. The table transdata.csv consisted of 7430 rows and 4 columns indicating the transaction id, sample id, transaction type and transaction amount. While looking for errors in the data no duplicate values or rows were found as well as zero Nan or null values were identified. It seems little to no data cleaning was required.

## **Data Preparation, Feature Engineering & Exploratory Data Analysis Observations:**
In order to manipulate the data as a whole, new features were created for each transaction type( REBILL, CHARGEBACK, REFUND) and defaulted to values of zero while the original transaction type column was deleted. Additionally the two tables were merged in 2 seperate ways to allow for further investigation into each proposed question.

Some interesting insights while exploring the data were

## **Visualizations, Conclusions & Aggregations:**
### Question 1: What is the aproximate probability distribution between the test group and the control group?

Looking purely at the testsamples.csv table of the 59721 samples, 44886 of them belonged to the control group while 14835 went to the test group. This produces a 0.24840508363892097 test sample rate. By using a p-value of the test sample rate, an N value of the tital sample size of 59721 and running these values through a binomial distibution due to the ouputs being either 1 of 2 options. We get the following probability distribution:

![image](https://user-images.githubusercontent.com/54183001/165035402-6ae43359-76b1-4dee-a34c-5013d44991f8.png)

As you can see from the plot the peak of the distribution sits around the 15000 mark, this make sense due to it being the outcome of the random asignment of value in the A/B test. This is followed by steep drop offs on either side of the peak due to the large sample size.
### Question 2: Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL?

In order to assess the rebill rate, the transaction data was grouped together by the sample id and the mean of the rebill values were taken. This allowed the transaction data to be match to the sample data using the sample_id column. After that the rebill rate along with the standard deviation and standard error was calculated to be:

test_group	| rebill_rate	| std_deviation	| std_error
  --- 	 |	 ---   |    ---   |   ---   |
   0	       |     0.021	 |     0.143	    |  0.001
   1	       |     0.105	 |     0.306	    |  0.003

Plotting this out gave the following:

![image](https://user-images.githubusercontent.com/54183001/165036709-052d52b1-dccb-41a4-9e15-582a58198d60.png)

As you can see there the rebill rate for users that needed to call in to cancel are significanty higher however in order to confirm this a statistical significance test was performed.

Our null hypothesis being that there is no significant difference between the control group and test group and our alternate hypothesis being that the test group results in at least 1 additional rebill.

By using a z-test to test our results we get the following outputs:

z statistic: -44.20
p-value: 0.000
ci 95% for control group: [0.020, 0.022]
ci 95% for test group: [0.100, 0.110]

Since our p-value of 0.000 is well under our a = 0.05 we may reject the null hypothesis and this means that the test group performed better than the control group. Therefore yes, users that must call in to cancel are more likely to generate at least 1 additional rebill.

### Question 3:Is a user that must call-in to cancel more likely to generate more revenues?

Looking at the revenue in an equivalent manner, this time the transaction data was grouped together by sample id however this time a summation of the transaction amounts were taken. After this it was merge into the sample data table. The revenue per user rate was calculated as well as the std deviation and std error:

test_group	|  revenue_rate  | 	std_deviation	  |  std_error
---  |    ---   |   ---    |    ---    | 
    0	     |     2.001	     |     20.447	      |  0.097
    1	     |     6.433	     |     25.704	      |  0.211
    
Again showing this as a barplot produces:

![image](https://user-images.githubusercontent.com/54183001/165039998-dee24756-808f-4606-b0e0-b341b1be94fa.png)

Here the test group is shown to have a significantly large revenue rate compared to the control group but once again must go throuhg a statistical significane method.

In this case since, revenue is a non-binomial metric the Mann-Whitney-Wilcoxon rank-sum test was used rather than a z-test.

Here are the results:

MannwhitneyuResult(statistic=1428270492.0, pvalue=0.0)

As this produced a p-value of essentially zero which is well below our a=0.05 threshold we can reject the null hypothesis of the the test and control having similar revenue rates. Thus accepting the alternate hypothesis that the test group performs significantly better. As a result a user that must call-in to cancel is more likely to generate more revenue.

### Question 4:Is a user that must call-in more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?

Taking the previusly merged samples data frame in Question 2 the chargeback rate per user which is the number of chargebacks divide by the number of rebills was calculated to be:

|index|test\_group|chargeback\_rate|
|---|---|---|
|0|0|0\.05504505835426208|
|1|1|0\.016683230270906946|

Which when plotted out looks like this:

![image](https://user-images.githubusercontent.com/54183001/165050042-8895214c-56cd-4e41-b9c2-40c35313360d.png)

You can clearly see that the chargeback rate of the test group is much smaller than that of the control group. Suggesting that users calling in to cancel may produce a lower chargeback rate.

Thus make sure this is statistically significant a ________ test was performed.

## Future Work
Including, datetime data would provide further context for this statistical analysis. Additionally, more investigation into the the users with multiple transactions could provide profitable insights. In hindsight, I also would have like to do an analysis on the proper sample sizes for this kind of test and to determine whether a balanced dataset would be more beneficial.
