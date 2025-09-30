# Getting started with your working environment 

## Overview

In this SAP TechEd 2025 Hands-on Workshop __Unlocking AI-driven insights from your business data in SAP HANA Cloud__ (DA261), the exercises are to be pursued either using __SAP HANA Cloud Database Explorer__ , or using a __Business Application Studio Python environment__.
<br><br>
You will work with the following technical environment
- a centrally provided __SAP HANA Cloud database system__ 
  - with your personal user-id and schema, with full access to the objects in their own schema
  - shared data provided within a separate, read-only, schema __DA261_SHARE__
- a __SQL console__ and database tools within a provided __SAP HANA Cloud Database Explorer__ instance (referred to as __DBX__),
  - _The SAP HANA Cloud database explorer is a web-based tool for browsing and working with SAP HANA database objects such as tables, views, functions, stored procedures. In addition, DBX can be used to import and export data, execute SQL statements, create remote sources, work with multi-model data such as graph, spatial and JSON collections, view trace files, and any other SQL activity._
- and a __Python Jupyter Notebook environment__ within a provided __SAP Business Application Studio__ instance (referred to as __BAS__)
  - _SAP Business Application Studio (BAS) is a cloud-based professional developer tool for building applications and extending SAP solutions._

The technical requirements to complete the exercises in this repository, including hardware and software specifications are taken care of by the provided systems used for the in-person Hands-on Workshop. Required _configuration tasks_ are outlined in the following [Getting Started instructions](exercises/ex0/) section.

Setup requirement instructions for pursuing this Hands-on Workshop in your local or trial / free-tier environment are documented in the [setup prerequisites](zappendix/setup_prerequisites.md)


## Step 1 - Getting started with SAP HANA Cloud Database Explorer and SQL console

### Logging into Database Explorer (DBX)
<li>Open the SAP HANA Database Explorer URL  

Database Explorer [link](https://hana-cockpit-004.cfapps.eu10.hana.ondemand.com/hrtt/sap/hana/cst/catalog/cockpit-index.html?databaseid=C3683523)  
(https://hana-cockpit-004.cfapps.eu10.hana.ondemand.com/hrtt/sap/hana/cst/catalog/cockpit-index.html?databaseid=C3683523)

<li> Select Sign in to another account

<br>![](./images/DBX_BTP_sign-in.png)


- _Type_ __academy-platform__ as the IDP and then select __"Sign in with alternative identity provider"__.

<br>![](./images/DBX_BTP_sign-in_id-provider.png)


__Note__: If the following Application Authorization screen appears, select __AUTHORIZE__ to continue.

<br>![](./images/DBX_BTP_sign-in_authorize.png)


- Enter the UserId and password provided the workshop speaker and according to your desk-id
  - User ID: \<as provided, example AC######U##\>
  - Password: \<as provided\>

-  Then select __Continue__.
<br>![](./images/DBX_BTP_sign-in_finalstep.png)

- Once logged into DBX, the application will prompt you to enter credentials for logging in to SAP HANA Cloud  instance. Use the same credentials for the database connection as in the earlier step.
<br>![](./images/DBX_conn_credentials_prompts.png)

- Now, the SAP HANA Database Explorer is ready to be used:
<br>![](./images/DBX_ready.png)

- Execute the following SQL query
```sql
select * from dummy

```

- After successfully logging in to the SAP HANA Cloud Database Explorer, the next step is to copy the data for the exercises.
```sql 
?? Copy tables command
```
<br><br><br>

## Step 2 - Configuring your Python environment in SAP Business Application Studio

### Machine Learning model development to application integration workflow
1. Data for ML model development is accessed from Python Jupyter Notebook envrionment in SAP HANA Cloud 
2. Python machine learning client package (hana-ml) is used for ML model experiments and development
3. SQL-script HANA deployment infrastructure (HDI) and CAP design-time artifacts are generated from hana-ml and integrated into the BAS application project

***

### 2.1 Creating the SAP Business Application Studio DEV Space
This exercise uses the Jupyter Notebooks extension in SAP Business Application Studio (BAS). This enables the use of python coding to leverage large language models in addition to the SAP HANA Cloud vector engine.

1. Select to open Business Application Studio to get started.<br>
   BAS [link](https://sap-build-hana-cloud.eu10cf.applicationstudio.cloud.sap/index.html)
   link text https://sap-build-hana-cloud.eu10cf.applicationstudio.cloud.sap/index.html

2. Enter the same UserId and password provided by the workshop speaker before and according to your desk-id
     - User ID: \<as provided, example AC######U##\>
     - Password: \<as provided\>

3. In the landing page for BAS, click on Create Dev Space to create a virtual environment in which to start a project.

4. Choose the following for creating your dev space, 
      - __Dev space name__:  __DA261_AC207556U##__ 
        [replace ## with index of your assgined user id] 
      - select for __application type__ on the left __SAP HANA Native Application__ as the . 
      - select from __Additional SAP Extensions__ on the right hand side the __Python Tools__
      - and then select __Create Dev Space__ to start the process. This will create a dedicated development environment with all the tools required to run the Python Jupyter Notebooks.
<br>![](./images/BAS_create_dev_space.png)

1. It will take a few minutes for the Dev Space to start up. The status can be seen beside the name.
<br>![](./images/BAS_devspace_start.png)

1. Once the status changes to Running, select the name (__DA261_AC207556U##__) to open it.
<br>![](./images/BAS_devspace_running_open.png)

<br><br>

### 2.2 Cloning the DA261 repository project
The following steps are to setup the required notebook.

1 On the Get Started page in Business Application Studio, select the option Clone from Git to import a repository from Github.
<br>![](./images/BAS_clone_from_github.png)

2. A prompt will appear on top asking for a URL to the repository. Copy and paste in the following URL and then press Enter.
> https://github.com/SAP-samples/teched2025-DA261.git

<br>![](./images/BAS_clone_path.png)

3 The following message will appear. Select Open to see the imported files in the Explorer window.
<br>![](./images/BAS_open_cloned_repo.png)

4. The files should now be visible in the Explorer pane on the left hand side. 
<br>![](./images/BAS_cloned_repo_ready.png)

<br><br>

### 2.3 Configure the python runtime
- Select the Jupyter Notebook __ex0_1_python_setup.ipynb__ to open it.
<br>![](./images/BAS_python_setup_jnb.png)  

<br><br>

<br><br>

### 2.4 Configure Jupyter and Python Extensions

1. The notebook, with executable code cells, will be displayed on the screen.
   <br>![](./images/BAS_open_jnb.png)

> In the next section the python environment will be configured so that the code cells can be executed from within the Jupyter notebook.

2 Run the first code cell to install the required python modules.

> Note: To execute a code cell, click on the play icon beside the cell. It is also possible to execute it by clicking into the code cell and pressing Shift+Enter.
   <br>![](./images/BAS_python_pipinst1.png)

3 When the first cell is executed, a prompt asking to choose a kernel source will appear. Select Python Environments.
   <br>![](./images/BAS_python_select_env.png)

4 On the next screen, select the recommended Python kernel (3.13.1).
   <br>![](./images/BAS_python_select_env_3131.png)

5. The required libraries will now start installing, and some messages about port numbers will also be visible in the lower right corner. This could take a few minutes to complete also.
   <br>![](./images/BAS_python_install_env_log.png)

6 Once the libraries have been successfully installed, the kernel must be restarted. Do this by clicking on the Restart option in the menu bar at the top.
   <br>![](./images/BAS_python_env_restart.png)

7 On the following pop-up message box, click on Restart again to complete the process.
   <br>![](./images/BAS_python_env_restart_confirm.png)

> Note: It will take a few seconds for the kernel to restart and once done, the environment is ready for use!

### Prepare the HANA Cloud database connection properties
Create a copy of the [./ex0/temp_user.ini](ex0/temp_user.ini) file and rename it to [./ex0/user.ini](ex0/user.ini) 
<br>![](./images/BAS_python_copy_tmpuserini.png)
<br>![](./images/BAS_python_paste_tmpuserini.png)
<br>![](./images/BAS_python_rename_tmpuserini.png)
<br>![](./images/BAS_python_rename_userini.png)

<br><br>
and 
2. the user and the password given to you by the instructor are updated in `user.ini` file 
<br>![](./images/BAS_python_edit_userini.png)

then execute the next cells to test the connection
<br>![](./images/BAS_python_check-connection_userini.png)


In addition, with the following cells the connection with prompted connection infos can also be tested <br>
 Please follow the instructions in the Jupyter Notebook in BAS to continue with the rest of the lesson.

> Note: For the section on connecting to the SAP HANA Cloud instance with the vector engine, please use the following as the HANA hostname:
__13b7c15d-848f-40b5-9259-c9c36ab85f56.hna1.prod-eu10.hanacloud.ondemand.com__
  <br> <br>![](./images/BAS_python_connect_HC.png)


<br><br><br>

## 3 Test the Python connection

## Summary

Now that you have ... 
Continue to - [Exercise 1 - Exercise 1 Description](../ex1/README.md)
