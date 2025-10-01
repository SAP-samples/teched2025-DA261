# DA261 - Unlocking AI-driven insights from your business data in SAP HANA Cloud

- [Exercise 9 - template exercise chapter](exercises/ex9/)
    - [Exercise 9.1 - Exercise 9 Sub Exercise 1 Description](exercises/ex9#exercise-91-sub-exercise-1-description)
    - [Exercise 9.2 - Exercise 9 Sub Exercise 2 Description](exercises/ex9#exercise-92-sub-exercise-2-description)

## Description

This repository contains the material for the SAP TechEd 2025 Hands-on Workshop __Unlocking AI-driven insights from your business data in SAP HANA Cloud__ (Session-ID DA261).  


## Overview

In this workshop you will learn how to utilize SAP HANA Cloud's AI capabilities for custom AI applications on SAP Business Technology Platform. Find out about tabular AI use cases served from the database, similarity search with the SAP HANA Cloud vector engine and the text embedding model, and retrieval augmented generation (RAG) using the new SAP HANA Cloud knowledge graph engine.

## Requirements and prerequisites

The requirements to follow the exercises in this repository are
<li>Use of a SAP HANA Cloud instance (SAP HANA Cloud free tier or trial would as well work)
<li>SAP HANA Database Explorer (or DBeaver) SQL Console
<li>SAP Business Application Studio with Python tools and Jupyter Notebook environment, with hana-ml packages.
  

  
1. You have an SAP BTP Trial account [Get a Free Account on SAP BTP Trial](https://developers.sap.com/tutorials/hcp-create-trial-account.html)
2. You have completed the [Setup SAP Build Code](https://developers.sap.com/tutorials/build-code-setup.html) tutorial.
3. You have to set up [SAP HANA Cloud Instance](https://developers.sap.com/tutorials/hana-cloud-deploying.html)
4. You have an account in [SAP Business Accelerator Hub](https://api.sap.com/)

Only proceed to the below exercise once you have gone through the [Pre-requistes check](/exercises/prerequistes/)

## Exercises

- [Getting Started](exercises/ex0/)
- [Exercise 1 - Manage JSON Data](exercises/ex1/)
    - [Exercise 1.1 - Importing OpenStreetMap street network data](exercises/ex1#11)

## Reuse
[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2025-DA261)](https://api.reuse.software/info/github.com/SAP-samples/teched2025-DA261)All content can be reused.

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support
Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2025 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.


## END
Introduction to SAP HANA Cloud/Generative AI Hub with Vector grounding


Hands-on Retrieval Augmented Generation (RAG) workflow
Documents to be included in vector analysis are fed into the model.

The contents of the files are split into smaller chunks.

Embedding functions are used to create embeddings from the file/document chunks.

The embeddings are then stored as vectors in the SAP HANA Cloud Database.

When a query or prompt is submitted, the query itself is then embedded into vector form.

The query vector is compared to the values stored as vectors in SAP HANA Cloud via a similarity/semantic search.

The most appropriate results are forwarded, along with the original query, to a large language model such as Chat GPT.

The LLM uses the results of the HANA vector search to augment its own searching capabilities, and the final answer is returned to the user.

Try it out!

Jupyter Notebook Extension in Business Application Studio

This exercise uses the Jupyter Notebooks extension in SAP Business Application Studio (BAS). This enables the use of python coding to leverage large language models in addition to the SAP HANA Cloud vector engine.

Select to open Business Application Studio to get started.

Log in with your user (AC207556U10) and provided credentials.

In the landing page for BAS, click on Create Dev Space to create a virtual environment in which to start a project.

For the Dev Space name, use AC207556U10_VectorDB and select SAP HANA Native Application as the application type. On the right hand side, under the list of Additional SAP Extensions, choose Python Tools and then select Create Dev Space to start the process. This will create a dedicated development environment with all the tools required to run the notebook.

It will take a few minutes for the Dev Space to start up. The status can be seen beside the name.

Once the status changes to Running, select the name (AC207556U10_VectorDB) to open it.

Create Cloud Foundry SAP AI Core service key

The Welcome to SAP HANA page will appear. Follow the below sequence to log in to Cloud Foundry:
Select the Cloud Foundry icon
Expand Services folder and select the login icon.

The Cloud Foundry Sign In panel will appear. Overwrite the Cloud Foundry Endpoint with the following value, and then choose SSO as the authentication method:

Login Option Selection
Cloud Foundry Endpoint <https://api.cf.eu10-004.hana.ondemand.com>
Authentication method SSO Passcode

Select the Open a new browser page to generate your SSO passcode.

Enter academy-platform as the identity provider and then select the Sign in with alternative provider option.

Copy the generated passcode to the clipboard.

Paste the copied passcode in the Enter your SSO passcode field.

Note: If prompted for clipboard permissions by your browser, select Allow.

Sign into the Cloud Foundry Target by clicking the Sign In button.

Proceed by selecting the desired Cloud Foundry Target and Space as seen below.

Cloud Foundry Target Selection
Cloud Foundry Organization TRIAL - Shared_sap-core-ai-eu10
Space dev
Select Apply

Observe all the services associated with the target:

Create SAP GenAI Core service key
The following steps are to setup the required notebook.

On the Get Started page in Business Application Studio, select the option Clone from Git to import a repository from Github.

A prompt will appear on top asking for a URL to the repository. Copy and paste in the following URL and then press Enter.

<https://github.com/SAP-samples/sap-genai-hub-with-sap-hana-cloud-vector-engine.git>

The following message will appear. Select Open to see the imported files in the Explorer window.

The files should now be visible in the Explorer pane on the left hand side. Select the Jupyter Notebook genAI_vectordb.ipynb to open it.

Note: If the following message appears, select Save as plain text on the dev space for future use.

Open a new terminal in BAS by following the numbered sequence of steps.

Change to the bin folder where the cf_create_bas_sso.py script is located.

To change folder use the following shell command as seen below:

cd bin

Create the SAP AI Core service key by executing the following command from the bin folder.

python3 cf_create_bas_sso.py --service "default_aicore" --service_key AC207556U10_key

The script will indicate if the creation was successful or not.

Note: This service key will be deleted once the workshop registration expires.

Once the key has been successfully created, the terminal window can be closed. Select the ‘X’ button to close it and return to the jupyter notebook.

Configure Jupyter and Python Extensions

The notebook, with executable code cells, will be displayed on the screen.

In the next section the python environment will be configured so that the code cells can be executed from within the Jupyter notebook.

Run the first code cell to install the required python modules.

Note: To execute a code cell, click on the play icon beside the cell. It is also possible to execute it by clicking into the code cell and pressing Shift+Enter.

When the first cell is executed, a prompt asking to choose a kernel source will appear. Select Python Environments.

On the next screen, select the recommended Python kernel (3.11.2).

The required libraries will now start installing, and some messages about port numbers will also be visible in the lower right corner. This could take a few minutes to complete also.

Once the libraries have been successfully installed, the kernel must be restarted. Do this by clicking on the Restart option in the menu bar at the top.

On the following pop-up message box, click on Restart again to complete the process.

Note: It will take a few seconds for the kernel to restart and once done, the environment is ready for use!

Please follow the instructions in the Jupyter Notebook in BAS to continue with the rest of the lesson.

Note: For the section on connecting to the SAP HANA Cloud instance with the vector engine, please use the following as the HANA hostname:
13b7c15d-848f-40b5-9259-c9c36ab85f56.hna1.prod-eu10.hanacloud.ondemand.com

Next unit
SAP Business Technology Platform

Generative AI Hub with Vector grounding
