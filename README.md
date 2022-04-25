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

## **Data Preparation & Feature Engineering:**
In order to manipulate the data as a whole, new features were created for each transaction type( REBILL, CHARGEBACK, REFUND) and defaulted to values of zero while the original transaction type column was deleted. Additionally the two tables were merged in 2 seperate ways to allow for further investigation into each proposed question.


## **Visualizations, Conclusions & Aggregations:**
### Question 1: What is the aproximate probability distribution between the test group and the control group?

Looking purely at the testsamples.csv table of the 59721 samples, 44886 of them belonged to the control group while 14835 went to the test group. This produces a 0.24840508363892097 test sample rate. By using a p-value of the test sample rate, an N value of the tital sample size of 59721 and running these values through a binomial distibution due to the ouputs being either 1 of 2 options. We get the following probability distribution:
![image](https://user-images.githubusercontent.com/54183001/165035402-6ae43359-76b1-4dee-a34c-5013d44991f8.png)
As you can see from the plot the peak of the distribution sits around the 15000 mark, this make sense due to it being the outcome of the random asignment of value in the A/B test. This is followed by steep drop offs on either side of the peak due to the large sample size.
### Question 2: Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL?

### Question 3:Is a user that must call-in to cancel more likely to generate more revenues?

### Question 4:Is a user that must call-in more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?

## Future Work
Including, datetime data would provide further context for this statistical analysis. Additionally, more investigation into the the users with multiple transactions could provide profitable insights.
