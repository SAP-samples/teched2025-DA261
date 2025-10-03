# Exercise 1 - Outlier Analysis on financial transaction data using Isolation Forests
SAP TechEd 2025, Hands-On Workshop: DA261 - Unlocking AI-driven insights from your business data in SAP HANA Cloud

## Understand exercise 1 scenario
In this exercise, you will explore how to apply machine learning techniques like Isolation Forest to financial transaction data for Outlier Analysis.
<br><br>

## Getting started | How to Guide
During this exercise, you will work with Jupyter Notebooks and a Python environment available within SAP Business Application Studio.
You will mainly work with notebook scripts using the Python Machine Learning client for SAP HANA, which generates SQL for immediate execution within the connected SAP HANA Cloud instance.
<br><br>
To start with the exercise
- open the notebook file __ex1_notebook.ipynb__ from the project explorer
  - from the context menu, select open
  - ![](/exercises/ex1/images/ex1_notebook_open.png)
- The notbook then opens
  ![](/exercises/ex1/images/ex1_notebook_opened.png)
<br><br>


Throughout the exercise, 
- you will be asked to __review__ and __adjust/edit__ the python code within the notebook code-cells
    ![](/exercises/ex1/images/nbk_edit.png)  

and then execute the notebook cells
- you should start executing each cell individually using the Play-icon to the left of the cell, , 
    ![](/exercises/ex1/images/nbk_execute.png)
- at a later point, you may execute all cells following the current one, using the Play-icon with the Top-Down-arrow in the right upper corner of the cell.

<br>

Once the notebook file is opened, you can open the  by clicking __Outline__ in the header of the notebook, the notebook __outline-view__ the opens to the left, and is helpful for oversight and navigation through the content of the notebook. You can arrange size of outline-view to your needs.
<br>![](/exercises/ex1/images/nbk_outline.png)


<br><br>

## Exercise 1.0 - Connect to your SAP HANA Cloud instance

In this Exercise 1 step 0, you will execute the python cells in the notebook-file to establish the SAP HANA Cloud datbase connection.
<br>![](/exercises/ex1/images/0-start.png)

Follow the instructions in the notebook-file.

After completing these steps you will see the connection could be established in the cell output.
<br>![](/exercises/ex1/images/0-connected.png)



<br><br>

## Exercise 1.1 Exploring financial transaction data with SAP HANA dataframes

In Step 1 of this exercise, 
- you will learn how to create a HANA dataframe
- how to use methods to understand the data structure of the result set underlying the dataframe select query
- understand when data is moved from HANA into the python environment

![](/exercises/ex1/images/ex1.1-S1-dataframe.png)

<br>
In Step 2 of this exercise, 
- you will learn how to use dataframe methods to filter the underlying query result set query, change its projections or apply aggregations
  
![](/exercises/ex1/images/ex1.1-S2-hdf-filter.png)

<br><br>

## Exercise 1.2 Outlier analysis using Isolation Forests
![](/exercises/ex1/images/ex1.2-S3-if.png)
![](/exercises/ex1/images/ex1.2-S4-if_shap.png)

<br><br>

## Exercise 1.3 Applying massive data parallel Isolation Forests for outlier analysis
![](/exercises/ex1/images/ex1.3-S5-if_massive.png)
![](/exercises/ex1/images/ex1.3-S6-if_massive_shap.png)

<br><br>

## Template
<br>![](/exercises/ex1/images/ex1_notebook_opened.png)


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

## Appendix
1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.

```