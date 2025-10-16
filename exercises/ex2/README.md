# Exercise 2 - Analyzing consumer complaints using text embeddings and machine learning
SAP TechEd 2025, Hands-On Workshop: DA261 - Unlocking AI-driven insights from your business data in SAP HANA Cloud

## Understand exercise 2 scenario
In this exercise, you will analyze consumer complaints data using text embeddings and machine learning models
- 1) explore the financial consumer complaints data and text narratives using SAP HANA dataframes, preparing and selecting the data to be analyzed in detail
- 4) AutoML techniques to build a consumer complaints classification machine learning model predicting cases for Monetary-Relief, unlocking the semantic understanding of text data as text embedding feature with machine learning models

Optionally and if time permits, you may explore
- 2) how to apply text analysis models to the original consumer complaints text narratives in order to detect and classify the consumer's sentiment 
- 3) how to apply vector engine similarity search-queries within the consumer complaints text embeddings data  
  
<br>

![](/exercises/ex2/images/ex2_scenario.png)

<br><br>

## Getting started | How to Guide
During this exercise, you will work with Jupyter Notebooks and a Python environment available within SAP Business Application Studio.
You will mainly work with notebook scripts using the Python Machine Learning client for SAP HANA, which generates SQL for immediate execution within the connected SAP HANA Cloud instance.
<br><br>
### To start with the exercise
- open the notebook file __ex2_notebook.ipynb__ from the project explorer
  - from the context menu, select open
<br>![](/exercises/ex2/images/ex2_notebook_open.png)
<br>


- The notbook then opens
<br>![](/exercises/ex2/images/ex2_notebook_opened.png)
<br>

### Furthermore, throughout the exercise, 
- you will be asked to __review__ and __adjust/edit__ the python code within the notebook code-cells
    ![](/exercises/ex2/images/nbk_edit.png)  

and then execute the notebook cells
- you should start executing each cell individually using the Play-icon to the left of the cell, , 
    ![](/exercises/ex2/images/nbk_execute.png)
- at a later point, you may execute all cells following the current one, using the Play-icon with the Top-Down-arrow in the right upper corner of the cell.
<br>

### Keep the overview
Once the notebook file is opened,
- you can open the __Outline-View__ by clicking __Outline__ in the header of the notebook, 
- the notebook __Outline-View__ then opens to the left, and is helpful for oversight and navigation through the content of the notebook-file 
- You can arrange the size of outline-view to your needs.
<br>![](/exercises/ex2/images/nbk_outline.png)

### Reference to exercise solutions
An exercise solution notebook is provided in __../ex_solutions/ex2_notebook-SOLUTION.ipynb__ and can be opened from the project explorer

<br><br>

## Exercise 2.0 Connect to your SAP HANA Cloud instance
Note, this README-file and the following explanations here are meant only to provide an overview about the tasks, the actual detailed practical instructions are provided in the notebook file [__ex2_notebook.ipynb__](/exercises/ex2/ex2_notebook.ipynb)

### Step 0: Establish and check connection
- Execute the cells
<br>![](/exercises/ex2/images/ex2.0_S0_connect.png)



## Exercise 2.1 Exploring Consumer Complaints Data

### Step 1: Create the HANA dataframe for the consumer complaints data
In __Step 1__ of this exercise, 
- you will create a HANA dataframe for consumer complaints database table
  ![](/exercises/ex2/images/ex2.1_S1_hdf.png)
- and learn how to apply filtering rows when using the collect()-method to display result sets in Python
<br>![](/exercises/ex2/images/ex2.1_S1_hdf_collectTOP5.png)

### Step 2: Explore consumer complaints data 
In __step 2__ you will learn to apply hana-ml dataframe methods
- like __describe()-method__ providing detailed column value distribution and statistic insights
  ![](/exercises/ex2/images/ex2.1_S2_hdf_describe.png)
- to create hana dataframe based on __larger multi-line SQL statements__ in python
  ![](/exercises/ex2/images/ex2.1_S2_hdf_bigsql.png)
- to generate visualization like bar-charts from HANA dataframes.
  ![](/exercises/ex2/images/ex2.1_S2_barchart.png)



## Exercise 2.2  Analyze consumer sentiment using text analysis (optional)
This exercise is meant to be an OPTIONAL add-on exercise, to only pursue based on interest and focus. If you continue with exercise 2.2, you likely not be able to complete other exercises in time of the session.
### Step 3: Explore sentiment of consumer complaints (optional)
In __step 3__ you will explore to detect the __sentiment__ from the text narratives of the complaints
- executing __text analysis__ for sentiment, on the __document-level__
  ![](/exercises/ex2/images/ex2.2_S3_complaints_sentiment.png)
- as well as detecting the user complaint sentiment on the __sentence-level__ of the complaint text.
  ![](/exercises/ex2/images/ex2.2_S3_complaints_sentiment_sentence.png)

<br><br>

## Exercise 2.3 Search in consumer complaints narratives using similarity search (optional)
This exercise is meant to be an OPTIONAL add-on exercise, to only pursue based on interest and focus. If you continue with exercise 2.3, you likely not be able to complete other exercises in time of the session.
### Step 4: Explore consumer complaints narratives using Text Embeddings and similarity search (optional)
In __step 4__ you will explore how to generate text embeddings within SAP HANA Cloud and apply similarity searches, and
- __create text embeddings__ from complaints text examples using the __vector_embedding SQL function__ 
  ![](/exercises/ex2/images/ex2.3_S4_hdf_embeddings.png)
- apply HANA dataframe __similarity search-methods__ against vector columns with text embeddings and how to embed different language query sentences
<br>![](/exercises/ex2/images/ex2.3_S4_simsearch.png)

<br><br>

## Exercise 2.4 - Predicting company monetary relief from consumer complaints using AutoML classification models 


### Step 5: Data selection and preparation 
In __step 5__ you will explore and prepare the HANA dataframe, which shall be used later to build the AutoML classification model, and
- use dataframe methods to identify and filter for companies with a high percentage of complaints which had been addressed by a monetary relief response,
  ![](/exercises/ex2/images/ex2.4_S5_hdf_monetary.png)
- apply filter, column renaming, value reformatting and more methods to prepare the data 


<br><br>

### Step 6: Consumer complaints Text Embedding vector dimension reduction
In __step 6__ you will learn how to apply __dimension reduction to text embedding vector columns__ using __Principal Component Analysis (PCA)__
- and apply __Vector-PCA__ to reduce the text embedding from 768 dimensions to a principal component vector of 64 dimensions
  ![](/exercises/ex2/images/ex2.4_S6_vecpca.png)

<br><br>

### Step 7: Build the AutoML classification model predicting company's monetary relief response to consumers
In __step 7__ you will learn how to use the __AutoML functions__ from the Predictive Analysis Library to automatically probe various algorithms and parameters to build the best possible classification model.
- __sample a hold-out set__ from the training data for later model performance evaluation
  ![](/exercises/ex2/images/ex2.4_S7_holdout.png)
- __define__ the __AutoML classification scenario__ and its __configuration__ for finding a best possible model
  ![](/exercises/ex2/images/ex2.4_S7_automl.png)
- __observe__ the __AutoML execution__ using the __AutoML monitoring__ viewer
  ![](/exercises/ex2/images/ex2.4_S7_automl_monitor.png)
- __evaluate__ the __best found AutoML classification pipeline model__ using the __score()-method__ applied to the __hold-out sample__
 ![](/exercises/ex2/images/ex2.4_S7_automl_scorestats.png)
- apply the __best found AutoML classification pipeline model__ for classification predictions on new complaints data using the __predict()-method__. 
 ![](/exercises/ex2/images/ex2.4_S7_automl_predictions.png)

<br><br>


## Summary

You've now completed exercise 2, fantastic !

Continue to - [Exercise 3 - Analyse Consumer Complaints Data using Knowledge Graphs](../ex3/README.md)





<br><br>

## Further reference information and examples 
Note the appendix section of the ex2_notebook.ipynb-file might include additional expert-level details for your offline study, incl. reference to the data used in the exercise.