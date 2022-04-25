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

The table testsamples.csv consisted of 59721 rows and 2 columns indicating the the sample id and test group. The table transdata.csv consisted of 7430 rows and 4 columns indicating the transaction id, sample id, transaction type and transaction amount. While looking for errors in the data no duplicate values or rows were found as well as zero Nan or null values were identified. It seems little to no data cleaning was required.

## **Data Preparation, Feature Engineering & Exploratory Data Analysis Observations:**
In order to manipulate the data as a whole, new features were created for each transaction type( REBILL, CHARGEBACK, REFUND) and defaulted to values of zero while the original transaction type column was deleted. Additionally the two tables were merged in 2 seperate ways to allow for further investigation into each proposed question.

An interesting insight while exploring the data were when it came to users that had multiple transactions of any kind to their sample id, the control group users who had to cancel via a web page form on average had much higher transactions per user with an average of 3.75 transactions per sample id compared to the test group which had to call in with an average of 2.06 transaction per sample id. 

![image](https://user-images.githubusercontent.com/54183001/165142131-5f859930-2219-4a07-aed6-ab4b2209876f.png)

![image](https://user-images.githubusercontent.com/54183001/165142233-920b05bf-89df-4d94-a94d-6263a2f4df07.png)

Based off of the distribution of the frequency of recurring transactions from the control group to the test group, it appears that the test group falls off quickly after first transactions compared to the control group distribution which does not skew as heavily towards the lower recurring transactions.

This potentially suggests that after initially being billed those who must call in to cancel are more likely to go through with cancellation afterwards where as those who must use the web form to cancel are more likely to continue to be billed further. More analysis is required on this statement. 

## **Visualizations, Conclusions & Aggregations:**
### Question 1: What is the aproximate probability distribution between the test group and the control group?

Looking purely at the testsamples.csv table of the 59721 samples, 44886 of them belonged to the control group while 14835 went to the test group. This produces a 0.24840508363892097 test sample rate. The test group samples are treated as successes in this scenario as they are indicated by values of 1. A binomial distribution was used due to the outputs only being either 1 of 2 options. Using the test sample rate as the p-value and the total sample size of 59721 for our value n, we get the following probability distribution:

![image](https://user-images.githubusercontent.com/54183001/165135200-17753bfd-6603-4c2e-b896-a96f733f78da.png)

As you can see from the plot the peak of the distribution sits around the 15000 mark, this make sense due to it being the outcome of the random assignment of values in the A/B test. This is followed by steep drop offs on either side of the peak due to the large sample size.
### Question 2: Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL?

In order to assess the rebill rate, the transaction data was grouped together by the sample id and the mean of the rebill values were taken. This allowed the transaction data to be matched to the sample data using the sample_id column. After that the rebill rate along with the standard deviation and standard error was calculated to be:

test_group	| rebill_rate	| std_deviation	| std_error
  --- 	 |	 ---   |    ---   |   ---   |
   0	       |     0.021	 |     0.143	    |  0.001
   1	       |     0.105	 |     0.306	    |  0.003

Plotting this out gave the following:

![image](https://user-images.githubusercontent.com/54183001/165142498-40f01cee-40eb-47d4-a0e5-d6c3e10f6813.png)

As you can see there the rebill rate for users that needed to call in to cancel are significanty higher however in order to confirm this a statistical significance test was performed.

Our null hypothesis being that there is no significant difference between the control group and test group and our alternate hypothesis being that the test group results in at least 1 additional rebill.

By using a z-test to test our results we get the following outputs:

z statistic: -44.20
p-value: 0.000
ci 95% for control group: [0.020, 0.022]
ci 95% for test group: [0.100, 0.110]

Since our p-value of 0.000 is well under our a = 0.05 we may reject the null hypothesis and this means that the test group performed better than the control group. Therefore yes, users that must call in to cancel are more likely to generate at least 1 additional rebill.

### Question 3: Is a user that must call-in to cancel more likely to generate more revenues?

Looking at the revenue in an equivalent manner, this time the transaction data was grouped together by sample id however this time a summation of the transaction amounts were taken. After this it was merged into the sample data table. The revenue per user rate was calculated as well as the std deviation and std error:

test_group	|  revenue_rate  | 	std_deviation	  |  std_error
---  |    ---   |   ---    |    ---    | 
    0	     |     2.001	     |     20.447	      |  0.097
    1	     |     6.433	     |     25.704	      |  0.211
    
Again showing this as a barplot produces:

![image](https://user-images.githubusercontent.com/54183001/165142745-d3eb9b10-9e35-4e6f-bc1c-2f651bea2dc8.png)

Here the test group is shown to have a significantly large revenue rate compared to the control group but once again must go throuhg a statistical significance method.

In this case since, revenue is a non-binomial metric and the Mann-Whitney-Wilcoxon rank-sum test was used rather than a z-test.

Here are the results:

MannwhitneyuResult(statistic=1428270492.0, pvalue=0.0)

As this produced a p-value of essentially zero which is well below our a=0.05 threshold we can reject the null hypothesis of the the test and control having similar revenue rates. Thus accepting the alternate hypothesis that the test group performs significantly better. As a result a user that must call-in to cancel is more likely to generate more revenue.

### Question 4: Is a user that must call-in more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?

Taking the previusly merged samples data frame in Question 2 the chargeback rate per user which is the number of chargebacks divide by the number of rebills again divided by their respective sample sizes was calculated to be:

|index|test\_group|chargeback\_rate|
|---|---|---|
|0|0|1\.2263302222132085e-06|
|1|1|1\.1245857951403403e-06|

Which when plotted out looks like this:

![image](https://user-images.githubusercontent.com/54183001/165142874-edda25b0-d04c-4743-8dcd-63b488cc34a9.png)

You can see that the chargeback rate of the test group is slightly smaller than that of the control group however it is almost negligible. Suggesting that users calling in to cancel produce the same chargeback rate as users using a web page form to cancel.

Thus, making sure this is statistically significant a z-test was performed which gave the follwing results:

z statistic: 0.00
p-value: 1.000
ci 95% for control group: [0.000, 0.002]
ci 95% for test group: [0.000, 0.002]

With a p-value well over a= 0.05 we failed to reject the null hypothesis.The test group performed approximately the same as the control group. In conclusion the users that must call in to cancel are not more likely to produce a higher chargeback rate.

## Future Work
Including datetime data would provide further context for this statistical analysis. Additionally, more investigation into the the users with multiple transactions could provide profitable insights. In hindsight, I also would have like to do an analysis on the proper sample sizes for this kind of test and to determine whether a balanced dataset would be more beneficial.
