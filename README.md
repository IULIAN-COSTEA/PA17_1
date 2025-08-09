# Call Center Marketing Campaign for a Bank

**Overview**: In this practical application, your goal is to compare the performance of the classifiers we encountered in this section, namely K Nearest Neighbor, Logistic Regression, Decision Trees, and Support Vector Machines.  We will utilize a dataset related to marketing bank products over the telephone.  

### Dataset source location and context description

The dataset comes from the UCI Machine Learning repository [link](https://archive.ics.uci.edu/ml/datasets/bank+marketing).  The data is from a Portugese banking institution and is a collection of the results of multiple marketing campaigns during a two years period.

### Dataset details 
This marketing dataset is related to 17 marketing campaigns that occurred between May 2008 and November 2010, run by a Portuguese bank that used its own contact-center to do directed marketing campaigns for a long-term deposit application with good interest rates.

Input variables:
   # bank client data:
   1 - age (numeric)
   2 - job : type of job (categorical: "admin.","unknown","unemployed","management","housemaid","entrepreneur","student","blue-collar","self-employed","retired","technician","services") 
   3 - marital : marital status (categorical: "married","divorced","single"; note: "divorced" means divorced or widowed)
   4 - education (categorical: "unknown","secondary","primary","tertiary")
   5 - default: has credit in default? (binary: "yes","no")
   6 - balance: average yearly balance, in euros (numeric) 
   7 - housing: has housing loan? (binary: "yes","no")
   8 - loan: has personal loan? (binary: "yes","no")
   # related with the last contact of the current campaign:
   9 - contact: contact communication type (categorical: "unknown","telephone","cellular") 
  10 - day: last contact day of the month (numeric)
  11 - month: last contact month of year (categorical: "jan", "feb", "mar", ..., "nov", "dec")
  12 - duration: last contact duration, in seconds (numeric)
   # other attributes:
  13 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)
  14 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric, -1 means client was not previously contacted)
  15 - previous: number of contacts performed before this campaign and for this client (numeric)
  16 - poutcome: outcome of the previous marketing campaign (categorical: "unknown","other","failure","success")

  Output variable (desired target):
  17 - y - has the client subscribed a term deposit? (binary: "yes","no")



**Overall technical findings**

- For these experiments using accuracy as key evaluation metric is very misleading and not recommended becuae of the very unbalanced data set 90% for the majority class. Considering the business context, identification of bank's clients that would accept a marketing campaing promotion (in this case a good interest rate deposit) the most important evaluation metrics are Precision, Recall and F1-Score overall but even more important in the context of the minority class (which in this case is the accepted promotion). Considering the above the experiments were designed to optimise(increase) F1-Score, which is a better metric for unbalanced dataset because is representing the optimal point between precision and recall metrics.
- Models selection and optimization techniques considered two stages:
   1. Selecting the best model using GridSearch and adjusting the hyperparameters for the best F1-score
   2. Finding the optimal probability threeshold for each model comparing the results for **F1-Score maximization**
- Techniques used to deal with unbalanced nature of the dataset and its size in Experiment A:
   - **Oversampling** using SMOTE
   - **UnderSampling** using: RandomUnderSampler, NearMiss-version1, EditedNearestNeighbours
   - **class_weight='balanced'** for all models where applicable
   - **Adjustable dataset sampling for SVM** which is the most cumputational expenside model (started with smaller test samples to run more kernels and after selecting the best kernel by increased the test sample
- During the experimentation I've concluded that oversampling the minority class created a better dataset than undersampling the majority class. The results are showing that all models performs better on the oversampled dataset. This is clearly observed by comparing the F1-Score, Precision and Recall metrics for both experiments. I have used Confusion Matrix, and Classification Report for a deeper dive into the performance of each model
  - The **best models from Experiment-A and also overall** is: **LogisticRegression Model** with below with below results:


<div align="center">
  <img src="data/Experiment-A_res.png" alt="Experiment-A Results">
</div>

    
  - The best models for Experiment-B is: **Support Vector Machine (SVM)** with below results:


<div align="center">
  <img src="data/Experiment-B_res.png" alt="Experiment-B Results">
</div>

     
  - For each models (where reasonable possible) I've also extracted the most relevant features:
- In both experiments I've managed to get a pretty good ROC score 0.93 for Experiment-B and 0.91 for Experiment-A. Nevertheless, ROC for Experiment-B is slighly better but only because the optimization was performed for F1-Score and not ROC. However ROC, confirms that bots experiments show good good results.
  
<div align="center">
  <img src="data/ROCs_Plot.jpg" alt="ROCs Results">
</div>

- Nest step is to try new more powerfull models to further improve F1-Score


**Business related findings and recommendations:**

With the **selected Logistic Regression model** if the bank intend to launcg a very targets campaing that optimizes the campaing cost then they should be able to **identify a list of clients that should be targeted by the new promotion campaign**. 

The model predict that **45.5% of the clients in this list will accept the promotion!**
This will represent **60.59%** of the total number of all clients in the database that would accept the promotion.
This is explained by below Classification Reports that shown the **Precision** and **Recall** as **%** for the **Class 1 (Clients that would accept the promotion)**:

<div align="center">
  <img src="data/Experiment-A_Confusion&Classification.jpg" alt="Experiment-A Results">
</div>

If the bank is not very concerned with the overall campaign cost and want to maximize the total number of clients that will accept the promotion, then they can use the model to select from bank database all clients that should not be part of the campaing. This will allow the bank to exclude from the campaing the clients that would not take the promotion anyway. This approach will give the bank all clinets that would not accpt the promotion with a probability of **95.95%**. 
In this scenario the bank will have a bigger list of clients than in the first scenario. In this list the percentace of clinets that will accept the promotion is significantly bigger than **45.5%**. In this case the bank would miss only **5%** of the total clients that are in the database but not selected for the marketing campaing.  

#### Next step to improve the results

Nest step is to try new more powerfull models to further improve F1-Score
