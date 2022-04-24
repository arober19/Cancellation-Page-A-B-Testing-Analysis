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
* In testSamples.csv, you will find a list of unique users that were randomized in the A/B test.

* sample_id : is the unique identifier for the sample
* test_group : is the group in which the sample was placed, 0= control group, 1=test group
* In transData.csv, you will find a list of transactions generated by randomized users after their randomization:

* transaction_id : is the unique identifier for the transaction
* sample_id : is a foreign key that links transactions to test samples
* transaction_type : is the transaction type for a transaction, can be REBILL, CHARGEBACK or REFUND
* transaction_amount : is the amount generated for a transaction, this can be a negative value

## **Data Preparation:**


## **Feature Engineering:**

## **Table Outputs & Aggregations:**
