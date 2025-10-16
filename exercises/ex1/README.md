# Exercise 1 - Outlier Analysis on financial transaction data using Isolation Forests
SAP TechEd 2025, Hands-On Workshop: DA261 - Unlocking AI-driven insights from your business data in SAP HANA Cloud

## Understand exercise 1 scenario
In this exercise, you will explore how to apply machine learning techniques like Isolation Forest to financial transaction data for Outlier Analysis.
1. You will explore on how to work with SAP HANA dataframes in Python for data exploration, preparation and filtering to a sclide of financial booking transactions to be analysed
2. You will apply the Isolation Forest algorithm to predict outliers from the data slice analyzed, visualize outlier points and use AI explainability methods to gain transparancy on why the model predicted a transaction row to be an outlier
3. Apply the outlier analysis in parallel on each "G/L Account" data slice using massive data-parallel processing
<br>

![](/exercises/ex1/images/ex1_scenario.png)

<br><br>

## Getting started | How to Guide
During this exercise, you will work with Jupyter Notebooks and a Python environment available within SAP Business Application Studio.
You will mainly work with notebook scripts using the Python Machine Learning client for SAP HANA, which generates SQL for immediate execution within the connected SAP HANA Cloud instance.
<br><br>
### To start with the exercise
- open the notebook file __ex1_notebook.ipynb__ from the project explorer
  - from the context menu, select open
  - ![](/exercises/ex1/images/ex1_notebook_open.png)
- The notbook then opens
  ![](/exercises/ex1/images/ex1_notebook_opened.png)
<br><br>


### Furthermore, throughout the exercise, 
- you will be asked to __review__ and __adjust/edit__ the python code within the notebook code-cells
    ![](/exercises/ex1/images/nbk_edit.png)  

and then execute the notebook cells
- you should start executing each cell individually using the Play-icon to the left of the cell, , 
    ![](/exercises/ex1/images/nbk_execute.png)
- at a later point, you may execute all cells following the current one, using the Play-icon with the Top-Down-arrow in the right upper corner of the cell.

<br>

### Keep the overview
Once the notebook file is opened,
- you can open the __Outline-View__ by clicking __Outline__ in the header of the notebook, 
- the notebook __Outline-View__ then opens to the left, and is helpful for oversight and navigation through the content of the notebook-file 
- You can arrange the size of outline-view to your needs.
<br>![](/exercises/ex1/images/nbk_outline.png)


### Reference to exercise solutions
An exercise solution notebook is provided in __../ex_solutions/ex1_notebook-SOLUTION.ipynb__ and can be opened from the project explorer

<br><br>

## Exercise 1.0 - Connect to your SAP HANA Cloud instance
Note, this README-file and the following explanations here are meant only to provide an overview about the tasks, the actual detailed practical instructions are provided in the notebook file [__ex1_notebook.ipynb__](/exercises/ex1/ex1_notebook.ipynb)

### Step 0: Establish and check connection
- As the initial step you will execute the python cells in the notebook-file to establish the SAP HANA Cloud database connection.
<br>![](/exercises/ex1/images/0-start.png)

- Further follow the instructions in the notebook-file.
- After completing these steps you will see the connection could be established in the cell output.
<br>![](/exercises/ex1/images/0-connected.png)



<br><br>

## Exercise 1.1 Exploring financial transaction data with SAP HANA dataframes
### Step 1: Create HANA dataframe for the financial business transaction sample table
In __Step 1__ of this exercise, 
- you will learn how to create a HANA dataframe
- how to use methods to understand the data structure of the result set and its underlying SQL query statement
- understand when data is moved from HANA into the python environment

![](/exercises/ex1/images/ex1.1-S1-dataframe.png)  
  
- you will have further learned how to display filtered results sets from HANA dataframes in python  

![](/exercises/ex1/images/ex1.1-S1-collectTOP5.png)


<br>

### Step 2: Explore the ACDOCA sample data using HANA dataframe methods
In __Step 2__ of this exercise,  

- you will learn how to use dataframe methods to filter the underlying query result set, change the column projections and more
  ![](/exercises/ex1/images/ex1.1-S2-hdf-filter2.png)

<br><br>

## Exercise 1.2 Outlier analysis using Isolation Forests
### Step 3: Execute basic outlier analysis using Isolation Forest
In __Step 3__ of this exercise,  
- you will learn how to execute an outlier analysis using the Isolation Forest-method from the Predictive Analysis Library (PAL)
  ![](/exercises/ex1/images/ex1.2-S3-if.png)
- review the outlier analysis results in tables and graphics  
  ![](/exercises/ex1/images/ex1.2-S3-if-res-tab.png)
  ![](/exercises/ex1/images/ex1.2-S3-if-res-plot.png)
- and have learned how to inspect the generated and executed SQL statements
  ![](/exercises/ex1/images/ex1.2-S3-if-SQL.png)

### Step 4: Add shapley explanations to Isolation Forest outlier predictions
In __Step 4__ of this exercise,  
- you will learn how to apply __AI explainability__ using __SHAPley Additive Explanations__ with the outlier predictions, thus being able to provide transparency with the outlier predictions, on why a datapoint has been classified as an outlier by the isolation forest model 
  ![](/exercises/ex1/images/ex1.2-S4-if_shap.png)

- will have generated AI explainability __summary plots__ SHAPley explainer , illustrating each features relative impact within the overall isolation forest outlier detection model.
  ![](/exercises/ex1/images/ex1.2-S4-if_shap_summary.png)


<br><br>

## Exercise 1.3 Applying massive data parallel Isolation Forests for outlier analysis
### Step 5: Data-parallel outlier analysis per "G/L Account"
In __Step 5__ of this exercise,  
- you will learn how to apply the Isolation Forest outlier analysis in parallel on each subset of a "G/L Account", i.e. execute a massive data-paralle analysis
![](/exercises/ex1/images/ex1.3-S5-if_massive.png)



<br><br>

## Summary

You've now completed exercise 1, great !

Continue to - [Exercise 2 - Analyzing consumer complaints using text embeddings and machine learning](../ex2/README.md)


<br><br>

## Further reference information and examples 
Note the appendix section of the ex1_notebook.ipynb-file might include additional expert-level details for your offline study, incl. reference to the data used in the exercise.
