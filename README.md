# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
In this problem, we are told to evaluate and compare the performance of 2 models where one is generated by HyperdriveRun and the other one is generated by AutoML.The dataset we used contains data about the bank customers. By analyzing the features of the data, our learning algorithm needs to predict whether the client will subscribe a term deposit or not.

As It is a binary classification problem, we choose to use **Logistic Regression** to classify.

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**\
In this pipeline architecture,in the **train.py** script, we used the [dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) of 32950 customers each containing 20 features and 1 label to classify whether the client will subscribe a term deposit or not. The Data was cleaned and the categorical columns was converted into numerical columns by the user defined **clean_data** function in **train.py** script. We used "--C" and "--max_iter" as the hyperparameter of our logistic regression learning algorithm. We used **RandomParameterSampling** to tune the hyperparameter, **BanditPolicy** as an early termination policy with evaluation_interval, slack_factor parameter and **scikit learn(SKLearn)** as estimator with **train.py** as entry_script. We didn't use the **GridParameterSampling** because it only supports discrete hyperparameters, but not the continuous hyperparameters. We also didn't use **TruncationSelectionPolicy** as early termination policy because for example, when evaluating a run at a interval N, its performance is only compared with the performance of other runs up to interval N even if they reported metrics for intervals greater than N. As our classification algorithm was **logistic regression**, we used **accuracy** as primary metric name with the goal to **maximize** the metric and got the best HyperDrive run with the accuracy of **91.03%** approximately.\
\
**What are the benefits of the parameter sampler you chose?**\
We choose Random Parameter Sampling because Random sampling supports discrete and continuous hyperparameters. It supports early termination of low-performance runs and hyperparameter values are randomly selected from the defined search space.\
\
**What are the benefits of the early stopping policy you chose?**\
We choose Bandit as early stopping policy because BanditPolicy terminates runs where the primary metric is not within the specified slack factor/slack amount compared to the best performing run.
## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**\
We provide **classification** as task, **accuracy** as primary metric, training data, label name and 3 fold cross validation. After trying out different model such as XGboostClassifier, RandomForrest, SGD and so on and completing 62 iterations generated by **AutoML**, it followed the **Voting Ensemble** technique to select the best model with **91.51%** of accuracy. To increase the accuracy, we can increase the number of fold in cross validation. But it may results in high variance and taking too much time to complete the run.\
The AutoML also automatically generated the following hyperparameter to aquire the best accuracy score.
- reg_lambda= 0.632
- silent= True
- subsample= 0.743
- subsample_for_bin = 200000
- subsample_freq= 0
- verbose = 10
## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**\
**Difference in accuracy**
 - HyperDrive Run : 91.03%
 - AutoML : 91.54%\
There is a slight improvement in accuracy while running the same problem in AutoML than that of running in HyperDrive.\
**Difference in architecture**\
As mentioned above, two models solve the same problem with two different algorithm and so the different parameters and overall architecture.\
\
There was differences because we execute the HyperDrive run with user defined algorithm but AutoML didn't require any user defined learning algorithm. AutoML automatically generated different learning algorithm and selected the model with best accuracy score. So, we can come to hte conclusion that, AutoML is more time and cost efficient.
## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
There are following areas of improvement for future experiments
 - we can try different learning algorithm
 - we can give wide range of value to choose as arguments of the parameters of RandomParameterSampling
 - we can try different optimization technique.\
These improvements might help the model to achieve better accuracy in less execution time.
## Proof of cluster clean up
**If you did not delete your compute cluster in the code, please complete this section. Otherwise, delete this section.**
**Image of cluster marked for deletion**
