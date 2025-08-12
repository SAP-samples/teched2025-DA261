
# Setup your working environment
## Your Hands-On Workshop environment
### Introduction

The data  
- blabla ... These tables contain some generic transactional sales data including details of customers, products, employees as well as review and location data. 

<br><br>





# Next unit
## SAP Business Technology Platform - Generative AI Hub with Vector grounding


<br><br><br>

# Appendix MISC stuff
## Introduction to SAP HANA Cloud/Generative AI Hub with Vector grounding

### SQL approach

> Note: SAP HANA Cloud vector engine native SQL can also be used in a RAG scenario using the L2DISTANCE and COSINE_SIMILARITY functions. For more on this please register for a free SAP HANA Cloud Basic Trial.

<li> ...
<br><br>

### Hands-on Retrieval Augmented Generation (RAG) workflow
1. Documents to be included in vector analysis are fed into the model.
2. The contents of the files are split into smaller chunks.
3. Embedding functions are used to create embeddings from the file/document chunks.
4. The embeddings are then stored as vectors in the SAP HANA Cloud Database.
5. When a query or prompt is submitted, the query itself is then embedded into vector form.
6. The query vector is compared to the values stored as vectors in SAP HANA Cloud via a similarity/semantic search.
7. The most appropriate results are forwarded, along with the original query, to a large language model such as Chat GPT.
8. The LLM uses the results of the HANA vector search to augment its own searching capabilities, and the final answer is returned to the user.

## Create Cloud Foundry SAP AI Core service key

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

## Create SAP GenAI Core service key

## misc
Change to the bin folder where the cf_create_bas_sso.py script is located.

To change folder use the following shell command as seen below:

> cd bin
<br>![](./images/BAS_terminal_bin.png)

### optional 
7. Create the SAP AI Core service key by executing the following command from the bin folder.

> #python3 cf_create_bas_sso.py --service "default_aicore" --service_key AC207556U10_key

The script will indicate if the creation was successful or not.

Note: This service key will be deleted once the workshop registration expires.

Once the key has been successfully created, the terminal window can be closed. Select the ‘X’ button to close it and return to the jupyter notebook.

### optional 
7. Create the SAP AI Core service key by executing the following command from the bin folder.

> #python3 cf_create_bas_sso.py --service "default_aicore" --service_key AC207556U10_key

The script will indicate if the creation was successful or not.

Note: This service key will be deleted once the workshop registration expires.

Once the key has been successfully created, the terminal window can be closed. Select the ‘X’ button to close it and return to the jupyter notebook.