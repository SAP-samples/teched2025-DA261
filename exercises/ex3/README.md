# Exercise 3 - Analyse Consumer Complaints Data using Knowledge Graphs

In this exercise, we will use SAP HANA Cloud Knowledge Graph to build a semantic layer on top of the Consumer Complaints dataset.

The exercise shows how to:
* Create a Knowledge Graph directly from relational complaint data stored in HANA Cloud.
* Model key entities such as Complaint, Consumer, Product, Issue, Company, and Outcome.
* Link the data using semantic relationships (e.g., a complaint is related to a product, associated with an issue, and handled by a company).
* Visualize the resulting Knowledge Graph using the SAP HANA Cloud Central tool.

This provides a more semantic and hierarchical view of the data, allowing you to perform queries and analysis that go beyond what is possible with flat SQL tables.

## Exercise 3.1 Sub Exercise 1 Description

After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex3/images/02_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello ABAP World! | ). 
```



## Exercise 3.2 Sub Exercise 2 Description

After completing these steps you will have...

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

2.	Click here.
<br>![](/exercises/ex3/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 4 - Excercise 4 ](../ex4/README.md)
