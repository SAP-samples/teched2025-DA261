# Exercise 2 - Analyzing consumer complaints using text embeddings and machine learning
SAP TechEd 2025, Hands-On Workshop: DA261 - Unlocking AI-driven insights from your business data in SAP HANA Cloud

## Understand exercise 2 scenario
In this exercise, you will explore how to analyze financial transaction data (ACDOCA, Universal Journal) for Outlier Analysis using machine learning techniques like Isolation Forest.
<br><br>

## Getting started | How to Guide
During this exercise, you will work with Jupyter Notebooks and a Python environment available within SAP Business Application Studio.
You will mainly work with notebook scripts using the Python Machine Learning client for SAP HANA, which generates SQL for immediate execution within the connected SAP HANA Cloud instance.
<br><br>
To start with the exercise
- open the notebook file __ex2_notebook.ipynb__ from the project explorer
  - from the context menu, select open
<br>![](/exercises/ex2/images/ex2_notebook_open.png)
<br>


- The notbook then opens
<br>![](/exercises/ex2/images/ex2_notebook_opened.png)
<br><br>

Throughout the exercise, 
- you will be asked to __review__ and __adjust/edit__ the python code within the notebook code-cells
    ![](/exercises/ex2/images/nbk_edit.png)  

and then execute the notebook cells
- you should start executing each cell individually using the Play-icon to the left of the cell, , 
    ![](/exercises/ex2/images/nbk_execute.png)
- at a later point, you may execute all cells following the current one, using the Play-icon with the Top-Down-arrow in the right upper corner of the cell.



<br><br>

## Exercise 2.0 Connect to your SAP HANA Cloud instance
### Step 0: Establish and check connection
- Execute the cells
<br>![](/exercises/ex2/images/ex2.0_S0_connect.png)



## Exercise 2.1 Exploring Consumer Complaints Data

### Step 1: Create the HANA dataframe for the consumer complaints data

- Execute the cells
<br>![](/exercises/ex2/images/ex2.1_S1_hdf.png)

### Step 2: Explore consumer complaints data 
- Execute the cells
<br>![](/exercises/ex2/images/ex2.1_S2_hdf_describe.png)

## Exercise 2.2  Analyze consumer sentiment using text analysis (optional)
### Step 3: Explore sentiment of consumer complaints (optional)
- Execute the cells
<br>![](/exercises/ex2/images/ex2.1_S3_complaints_disputed.png)

## Exercise 2.3 Search in consumer complaints narratives using similarity search (optional)
### Step 4: Explore consumer complaints narratives using Text Embeddings and similarity search (optional)

- Execute the cells
<br>![](/exercises/ex2/images/ex2.1_S4_hdf_embeddings.png)


<br><br>

## Exercise 2.4 - Predicting company monetary relief from consumer complaints using AutoML classification models 


### Step 5: Data selection and preparation 
- Execute the cells

<br>![](/exercises/ex2/images/ex2.2_S5_hdf_monetary.png)



<br><br>

### Step 6: Consumer complaints Text Embedding vector dimension reduction
- Execute the cells
<br>![](/exercises/ex2/images/ex2.2_S6_hdf_vecpca.png)

### Step 7: Build the AutoML classification model predicting company's monetary relief response to consumers
- Execute the cells
<br>![](/exercises/ex2/images/ex2.2_S7_hdf_automl.png)

<br><br>

## Template



## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)


## Appendix
<br>![](/exercises/ex2/images/02_02_0010.png)
1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc = 0.
    response->set_status( i_code = 200
                     i_reason = 'Everything is fine').
    RETURN.
  ENDIF.

```

2.	Insert this line of code.
```abap
response->set_text( |Hello ABAP World! | ). 
```