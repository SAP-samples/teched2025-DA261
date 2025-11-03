# Exercise 3 - Analyse Consumer Complaints Data using Knowledge Graphs

In this exercise, we will use **SAP HANA Cloud Knowledge Graph** to build a semantic layer on top of the Consumer Complaints dataset.

The exercise shows how to:
* Create a Knowledge Graph directly from relational complaint data stored in HANA Cloud.
* Model key entities such as **Complaint**, **Consumer**, **Product**, **Issue**, **Company**, and **Outcome**.
* Link the data using semantic relationships (e.g., a complaint is related to a product, associated with an issue, and handled by a company).
* Run **SPARQL queries** on the Knowledge Graph to perform advanced analysis that goes beyond flat SQL queries.
* Visualize the resulting Knowledge Graph using the **SAP HANA Cloud Central tool** (optional).


This provides a more semantic and hierarchical view of the data, allowing you to analyse consumer complaints with **hierarchies, relationships, and reasoning**.

## Exercise 3.1 Create the Knowledge Graph

After completing these steps you will have created a Knowledge Graph from the Consumer Complaints table.

1. Open the SAP HANA Database Explorer and navigate to SQL Console section.  
   Database Explorer [link](https://hana-cockpit-004.cfapps.eu10.hana.ondemand.com/hrtt/sap/hana/cst/catalog/cockpit-index.html?databaseid=C3683523)  
(https://hana-cockpit-004.cfapps.eu10.hana.ondemand.com/hrtt/sap/hana/cst/catalog/cockpit-index.html?databaseid=C3683523)


2. Run the following SQL query to create triples from your consumer complaints table:
   - **Adjust the Graph instance name __<consumerComplaintsBase_U##>__ to match the index of your assgined user id**
   **[replace ## with index of your assgined user id]**
        
```sql
CALL SPARQL_EXECUTE('
PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
PREFIX cc:<http://consumer.demo.com/complaints#>

INSERT {
  GRAPH <consumerComplaintsBase_U##> {
    # Complaint
    ?ComplaintURI rdf:type cc:Complaint .
    ?ComplaintURI rdfs:label ?ComplaintID .
    ?ComplaintURI cc:complaintNarrative ?Narrative .
    
    # Company
    ?ComplaintURI cc:handledBy ?CompanyURI .
    ?CompanyURI rdf:type cc:Company .
    ?CompanyURI rdfs:label ?Company .
    
    # Product
    ?ComplaintURI cc:isRelatedTo ?ProductURI .
    ?ProductURI rdf:type cc:Product .
    ?ProductURI rdfs:label ?ProductName .
    ?ProductURI cc:subproductName ?SubProductName .
    
    # Issue
    ?ComplaintURI cc:associatedIssue ?IssueURI .
    ?IssueURI rdf:type cc:Issue .
    ?IssueURI rdfs:label ?IssueName .
    ?IssueURI cc:subIssueName ?SubIssueName .
    
    # Outcome
    ?ComplaintURI cc:hasOutcome ?OutcomeURI .
    ?OutcomeURI rdf:type cc:Outcome .
    ?OutcomeURI rdfs:label ?CompanyResponse .
    ?OutcomeURI cc:companyResponse ?CompanyResponse .
    
    # Consumer
    ?ConsumerURI rdf:type cc:Consumer .
    ?ConsumerURI cc:locationState ?State .
    ?ConsumerURI cc:locationZipcode ?Zip .
    ?ConsumerURI cc:tags ?Tags .
    ?ConsumerURI cc:consentProvided ?Consent .
    
    # Complaint, Consumer link
    ?ConsumerURI cc:hasComplaint ?ComplaintURI .
    ?ComplaintURI cc:hasConsumer ?ConsumerURI .
  }
}
WHERE {
  { sql_table(''SELECT TO_NVARCHAR("ComplaintID") AS ID_STR, "Company" AS "Company", CAST("ComplaintNarrative" AS NVARCHAR(5000)) AS "Narrative", CAST("Product" AS NVARCHAR(500)) AS "ProductName", CAST("SubProduct" AS NVARCHAR(500)) AS "SubProductName", CAST("Issue" AS NVARCHAR(500)) AS "IssueName", CAST("SubIssue" AS NVARCHAR(500)) AS "SubIssueName", CAST("CompanyResponse" AS NVARCHAR(1000)) AS "CompanyResponse", "State" AS "State", "ZipCode" AS "Zip", "Tags" AS "Tags", "ConsumerConsent" AS "Consent" FROM "DA261_SHARE"."CONSUMER_COMPLAINTS"'') }
  BIND(URI(CONCAT("http://consumer.demo.com/complaint/",ENCODE_FOR_URI(?ID_STR))) AS ?ComplaintURI) .
  BIND(URI(CONCAT("http://consumer.demo.com/company/",ENCODE_FOR_URI(?Company))) AS ?CompanyURI) .
  BIND(URI(CONCAT("http://consumer.demo.com/product/",ENCODE_FOR_URI(?ProductName))) AS ?ProductURI) .
  BIND(URI(CONCAT("http://consumer.demo.com/issue/",ENCODE_FOR_URI(?IssueName))) AS ?IssueURI) .
  BIND(URI(CONCAT("http://consumer.demo.com/consumer/",ENCODE_FOR_URI(?ID_STR))) AS ?ConsumerURI) .
  BIND(URI(CONCAT("http://consumer.demo.com/outcome/",ENCODE_FOR_URI(?ID_STR))) AS ?OutcomeURI) .
  BIND(?ID_STR AS ?ComplaintID) .
}
','',?,?);
```

The above command transformed rows in the **CONSUMER_COMPLAINTS** table into a **Knowledge Graph** - **'consumerComplaintsBase'**.

* **Complaint** – each complaint row becomes a node (`cc:Complaint`) with its ID and narrative.
* **Company** – linked to the complaint via `cc:handledBy`.
* **Product and Sub-Product** – linked to the complaint via `cc:isRelatedTo`.
* **Issue and Sub-Issue** – linked to the complaint via `cc:associatedIssue`.
* **Outcome** – the company’s response is modeled as an `cc:Outcome` linked via `cc:hasOutcome`.
* **Consumer** – information about the consumer (state, ZIP, tags, consent) is stored as a `cc:Consumer` node.
* **Relationships** – links like `cc:hasConsumer`, `cc:isRelatedTo`, `cc:associatedIssue` turn the flat table into a connected graph.

In short:
- A single row in the table now becomes a web of connected entities.
- Each entity has its own URI (unique identifier).
- Complaints are no longer just records, now, they are central nodes connected to companies, products, issues, outcomes, and consumers.

This structure enables you to run **SPARQL queries** to explore relationships and hierarchies that would be very hard to capture with plain SQL.

The response from Knowledge Graph creation will look like  
![](/exercises\ex_solutions\images/ex3.1-response-create-kg.png)



## Exercise 3.2 Query the Knowledge Graph

Now that we have created the Knowledge Graph, let’s run some SPARQL queries against it.  
We will use `SPARQL_TABLE` to query the graph directly from SQL.

1. **Check contents of the Knowledge Graph**

   Run the following query to fetch a few complaints with their associated company, product, issue, and outcome:
   - **Note, adjust the Graph instance reference in the query __<consumerComplaintsBase_U##>__ to match the index of your assgined user id before execution**

   ```sql
   SELECT *
   FROM SPARQL_TABLE('
   PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>
   PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
   PREFIX cc:<http://consumer.demo.com/complaints#>

   SELECT ?ComplaintID ?Company ?Product ?Issue ?Outcome
   FROM <consumerComplaintsBase_U##>
   WHERE {
     ?ComplaintURI rdf:type cc:Complaint .
     ?ComplaintURI rdfs:label ?ComplaintID .
     ?ComplaintURI cc:handledBy ?CompanyURI .
     ?CompanyURI rdfs:label ?Company .
     ?ComplaintURI cc:isRelatedTo ?ProductURI .
     ?ProductURI rdfs:label ?Product .
     ?ComplaintURI cc:associatedIssue ?IssueURI .
     ?IssueURI rdfs:label ?Issue .
     ?ComplaintURI cc:hasOutcome ?OutcomeURI .
     ?OutcomeURI rdfs:label ?Outcome .
   }
   LIMIT 20
   ');
   ```
  This will return a tabular view of sample complaints and their linked entities.

 The result response from Knowledge Graph content query will look like  
![](/exercises\ex_solutions\images/ex3.2-check-kg-content.png)

1. **Filter complaints with a specific outcome**

   For example, to see all complaints that were Closed with monetary relief, run:
      - note, adjust the Graph instance reference in the query __<consumerComplaintsBase_U##>__ to match the index of your assgined user id before execution

   ```sql
   SELECT *
   FROM SPARQL_TABLE('
   PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>
   PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
   PREFIX cc:<http://consumer.demo.com/complaints#>

   SELECT ?ComplaintID ?Company ?Product ?Issue ?Outcome
   FROM <consumerComplaintsBase_U##>
   WHERE {
     ?ComplaintURI rdf:type cc:Complaint .
     ?ComplaintURI rdfs:label ?ComplaintID .
     ?ComplaintURI cc:handledBy ?CompanyURI .
     ?CompanyURI rdfs:label ?Company .
     ?ComplaintURI cc:isRelatedTo ?ProductURI .
     ?ProductURI rdfs:label ?Product .
     ?ComplaintURI cc:associatedIssue ?IssueURI .
     ?IssueURI rdfs:label ?Issue .
     ?ComplaintURI cc:hasOutcome ?OutcomeURI .
     ?OutcomeURI rdfs:label ?Outcome .
     FILTER(?Outcome = "Closed with monetary relief")
   }
   LIMIT 20
   ');
   ```

### What did we just do?

* Queried the Knowledge Graph to fetch complaints and their linked entities.  
* Verified that relationships between Complaint, Company, Product, Issue, and Outcome are working as expected.  
* Applied a filter to restrict results to complaints where the outcome was 'Closed with monetary relief'

 The result response from Knowledge Graph content query with filter will look like  
![](/exercises\ex_solutions\images/ex3.2-check-kg-content_w-filter.png)

## Exercise 3.3 Add Product and Issue Hierarchies

So far, our Knowledge Graph has been modeled with complaints, companies, products, issues, outcomes, and consumers as they appear in the source table.  
However, the **raw product names** and **issue descriptions** in the table are very granular (e.g., *“Credit card”*, *“Account status incorrect”*).  

By introducing **hierarchies**, we can group these granular values into broader, meaningful categories, like:
- **Products** : e.g., *“Credit card”*, *“Store card”*, and *“General-purpose card”* all roll up to **Credit Cards**.
- **Issues** : e.g., *“Late fee”*, *“Payment not applied”* roll up to **Billing & Payment Issues**, while *“Identity theft”* rolls up to **Identity Fraud**.

### Why do we need hierarchies?
This makes queries:
- **Simpler** : you can ask *“How many complaints relate to Credit Cards?”* without listing every sub-product.
- **More powerful** : SPARQL can use `rdfs:subClassOf*` to traverse the hierarchy, automatically rolling up details to categories.
- **Beyond SQL** : SQL would require **lookup tables, recursive joins, or complex CASE statements** to achieve the same grouping. With Knowledge Graphs, the relationships are **modeled once** and queries automatically respect the hierarchy.

### Why not just use SQL?

Yes, you could simulate this in SQL with CASE statements or by maintaining separate mapping tables. But that could quickly become:
- **Hard to maintain** : every time a new product or issue type is introduced, you have to update CASE logic or mapping tables in multiple queries.  
- **Rigid** : SQL cannot easily “walk” a hierarchy of arbitrary depth. In contrast, SPARQL with `rdfs:subClassOf*` can roll up through as many levels of the hierarchy as needed.  
- **Limited in reuse** : each SQL query has to repeat the same join logic. In a Knowledge Graph, the hierarchy is **part of the model** and can be reused across all queries.  
- **Less intuitive** : SQL works row by row. Knowledge Graphs work naturally with relationships, so questions like *“Show me all complaints linked to any type of loan”* are expressed much more naturally.

---
### On the other hand, why not just use Property Graphs or SQL Hierarchies in HANA?

HANA already supports property graphs and SQL hierarchies, but Knowledge Graphs add extra benefits:

- **Standards based** : RDF + SPARQL are W3C standards, portable across tools and easier to integrate with external ontologies.  
- **Semantic reasoning** : hierarchies are built once and reused with `rdfs:subClassOf*`, no need for recursive joins or CASE logic.  
- **Multiple hierarchies** : you can model product, issue, geography, and others together and traverse them in one query.  
- **Reusable & future proof** : once modeled, all queries benefit; also connects easily to AI/ML tools expecting RDF.  

In short, property graphs and SQL hierarchies work, but Knowledge Graphs provide reusability, flexibility, and semantic power, meaning you model knowledge once and then query it at different levels of detail without rewriting logic, making analysis simpler, reusable, and more powerful.

### Now let's start with creating Product Hierarchy knowledge graph

The following code inserts hierarchy triples mapping each product to a broader product category.
- note, adjust the Hierachy name in the query __<consumerComplaintsProductHierarchy_U##>__ to match the index of your assgined user id before execution

```sql
/* Insert product hierarchy triples, with
Bank Accounts: Checking or savings account, CD (Certificate of Deposit), Other banking product or service
Credit Cards: Credit card, General-purpose credit card or charge card, Store card
Loans: Mortgage, Auto loan or lease, Student loan, Payday loan, title loan, or personal loan
Credit Reporting: Credit reporting, credit repair services, or other personal consumer reports
Debt Collection: Debt collection (all subtypes)
Money Services / Payments: Money transfer, virtual currency, or money service, Mobile / digital wallets
Other Financial Products: Prepaid cards, Credit repair / identity protection services, Miscellaneous products
*/

CALL SPARQL_EXECUTE('
PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
PREFIX cc:<http://consumer.demo.com/complaints#>

INSERT {
  GRAPH <consumerComplaintsProductHierarchy_U##> {
    ?ProductURI rdfs:subClassOf ?CategoryURI .
  }
}
WHERE {
  { sql_table(''SELECT DISTINCT CAST("Product" AS NVARCHAR(500)) AS "ProductName" FROM "DA261_SHARE"."CONSUMER_COMPLAINTS"'') }
  BIND(URI(CONCAT("http://consumer.demo.com/product/",ENCODE_FOR_URI(?ProductName))) AS ?ProductURI) .

  BIND(
    IF(CONTAINS(LCASE(?ProductName),"checking"), cc:BankAccounts,
    IF(CONTAINS(LCASE(?ProductName),"savings"), cc:BankAccounts,
    IF(CONTAINS(LCASE(?ProductName),"credit card"), cc:CreditCards,
    IF(CONTAINS(LCASE(?ProductName),"mortgage"), cc:Loans,
    IF(CONTAINS(LCASE(?ProductName),"loan"), cc:Loans,
    IF(CONTAINS(LCASE(?ProductName),"credit reporting"), cc:CreditReporting,
    IF(CONTAINS(LCASE(?ProductName),"debt collection"), cc:DebtCollection,
    IF(CONTAINS(LCASE(?ProductName),"money"), cc:MoneyServices,
       cc:OtherProductCategory)))))))) AS ?CategoryURI
  ) .
}
','',?,?);
```
 The response from Knowledge Graph ProductHierarchy creation will look like  
![](/exercises\ex_solutions\images/ex3.3-create-product-hier-response.png)

### Now next, let's create Issue Hierarchy knowledge graph

The following code inserts hierarchy triples mapping each issue to a broader issue category.
- note, adjust the Hierachy name in the query __<consumerComplaintsIssueHierarchy_U##>__ to match the index of your assgined user id before execution

```sql
CALL SPARQL_EXECUTE('
PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
PREFIX cc:<http://consumer.demo.com/complaints#>

INSERT {
  GRAPH <consumerComplaintsIssueHierarchy_U##> {
    ?IssueURI rdfs:subClassOf ?CategoryURI .
  }
}
WHERE {
  { sql_table(''SELECT DISTINCT CAST("Issue" AS NVARCHAR(500)) AS "IssueName" FROM "DA261_SHARE"."CONSUMER_COMPLAINTS"'') }
  BIND(URI(CONCAT("http://consumer.demo.com/issue/",ENCODE_FOR_URI(?IssueName))) AS ?IssueURI) .

  BIND(
    IF(CONTAINS(LCASE(?IssueName),"advertis"), cc:AdvertisingMarketing,
    IF(CONTAINS(LCASE(?IssueName),"application"), cc:ApplicationOrigination,
    IF(CONTAINS(LCASE(?IssueName),"credit report"), cc:CreditReporting,
    IF(CONTAINS(LCASE(?IssueName),"identity"), cc:IdentityFraud,
    IF(CONTAINS(LCASE(?IssueName),"fraud"), cc:IdentityFraud,
    IF(CONTAINS(LCASE(?IssueName),"collect"), cc:DebtCollection,
    IF(CONTAINS(LCASE(?IssueName),"account"), cc:AccountServicing,
       cc:OtherIssueCategory))))))) AS ?cat1
  ) .

  # Second stage: refine payments / loans / transactions
  BIND(
    IF(CONTAINS(LCASE(?IssueName),"fee"), cc:BillingPayment,
    IF(CONTAINS(LCASE(?IssueName),"charge"), cc:BillingPayment,
    IF(CONTAINS(LCASE(?IssueName),"payment"), cc:BillingPayment,
    IF(CONTAINS(LCASE(?IssueName),"loan"), cc:LoanIssues,
    IF(CONTAINS(LCASE(?IssueName),"mortgage"), cc:LoanIssues,
    IF(CONTAINS(LCASE(?IssueName),"card"), cc:PaymentTransaction,
    IF(CONTAINS(LCASE(?IssueName),"purchase"), cc:PaymentTransaction,
    IF(CONTAINS(LCASE(?IssueName),"transaction"), cc:PaymentTransaction,
    IF(CONTAINS(LCASE(?IssueName),"bankruptcy"), cc:BankruptcyRelief,
       ?cat1))))))))) AS ?CategoryURI
  ) .
}
','',?,?);
```
 The response from Knowledge Graph IssueHierarchy creation will look like  
![](/exercises\ex_solutions\images/ex3.3-create-isse-hier-response.png)

### What did we just do?

- Created two new hierarchy graphs: **`consumerComplaintsProductHierarchy`** and **`consumerComplaintsIssueHierarchy`**.  
- Mapped granular product names (e.g., *“Credit card”*, *“Checking account”*) to broader categories like **Credit Cards** or **Bank Accounts**.  
- Mapped granular issue names (e.g., *“Account status incorrect”*, *“Late fee”*) to broader categories like **Account Servicing** or **Billing & Payment**.  
- Built a classification model where SPARQL can automatically roll up details to higher categories using `rdfs:subClassOf*`.  

## Exercise 3.4 Querying with Hierarchies

With the Product and Issue hierarchies in place, we can now run more powerful SPARQL queries that automatically roll up granular values into higher-level categories.  
This is where the Knowledge Graph approach clearly outshines flat SQL.

1. **Complaints mentioning “credit card” - grouped by Product Category**
- note, adjust the Grpah instance name __<consumerComplaintsBase_U##>__ and Hierachy name __<consumerComplaintsProductHierarchy_U##>__ in the query  to match the index of your assgined user id before execution
- 
   ```sql
   SELECT * FROM SPARQL_TABLE('
   PREFIX cc:<http://consumer.demo.com/complaints#>
   PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
   PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>

   SELECT ?ProductCategory (COUNT(?Complaint) AS ?Count)
   WHERE {
     GRAPH <consumerComplaintsBase_U##> {
       ?Complaint rdf:type cc:Complaint .
       ?Complaint cc:complaintNarrative ?Narrative .
       FILTER(CONTAINS(LCASE(?Narrative),"credit card")) .
       ?Complaint cc:isRelatedTo ?ProductURI .
     }
     GRAPH <consumerComplaintsProductHierarchy_U##> {
       ?ProductURI rdfs:subClassOf* ?ProductCategory .
     }
   }
   GROUP BY ?ProductCategory
   ORDER BY DESC(?Count)
   ');
   ```
   Shows how narratives can be filtered with text search, then linked to product hierarchies for aggregation.

The response from Knowledge Graph query with product hierarchy will look like  
![](/exercises\ex_solutions\images/ex3.4-query-w-product-hier.png)


2. **Credit card complaints - by ProductCategory × IssueCategory × Company × Outcome × State**
  
   This query shows how multiple hierarchies and attributes can be combined in one SPARQL query.
- note, adjust the Grpah instance name __<consumerComplaintsBase_U##>__ and Hierachy name __<consumerComplaintsProductHierarchy_U##>__ , __<consumerComplaintsIssueHierarchy_U##>__ in the query  to match the index of your assgined user id before execution
   ```sql
   SELECT * FROM SPARQL_TABLE('
   PREFIX cc:<http://consumer.demo.com/complaints#>
   PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
   PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>

   SELECT ?ProductCategory ?IssueCategory ?Company ?Outcome ?State (COUNT(?Complaint) AS ?Count)
   WHERE {
   GRAPH <consumerComplaintsBase_U##> {
    ?Complaint rdf:type cc:Complaint .
    ?Complaint cc:complaintNarrative ?Narrative .
    FILTER(CONTAINS(LCASE(?Narrative),"credit card")) .

    # links
    ?Complaint cc:isRelatedTo ?ProductURI .
    ?Complaint cc:associatedIssue ?IssueURI .
    ?Complaint cc:handledBy ?CompanyURI .
    ?CompanyURI rdfs:label ?Company .
    ?Complaint cc:hasOutcome ?OutcomeURI .
    ?OutcomeURI rdfs:label ?Outcome .
    ?Complaint cc:hasConsumer ?ConsumerURI .
    ?ConsumerURI cc:locationState ?State .
   }

   GRAPH <consumerComplaintsProductHierarchy_U##> {
    ?ProductURI rdfs:subClassOf* ?ProductCategory .
   }
   GRAPH <consumerComplaintsIssueHierarchy_U##> {
    ?IssueURI rdfs:subClassOf* ?IssueCategory .
   }
   }
   GROUP BY ?ProductCategory ?IssueCategory ?Company ?Outcome ?State
   ORDER BY DESC(?Count)
   ');
   ```
The response from Knowledge Graph query with product and issue hierarchy filters will look like  
![](/exercises\ex_solutions\images/ex3.4-query2-w-product-issue-hier-filter.png)


3. **Top Issue Categories by State**
- note, adjust the Grpah instance name __<consumerComplaintsBase_U##>__ and Hierachy name  __<consumerComplaintsIssueHierarchy_U##>__ in the query  to match the index of your assgined user id before execution
   ```sql
   SELECT * FROM SPARQL_TABLE('
   PREFIX cc:<http://consumer.demo.com/complaints#>
   PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
   PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>

   SELECT ?State ?IssueCategory (COUNT(?Complaint) AS ?Count)
   WHERE {
   GRAPH <consumerComplaintsBase_U##> {
    ?Complaint rdf:type cc:Complaint .
    ?Complaint cc:hasConsumer ?Consumer .
    ?Consumer cc:locationState ?State .
    ?Complaint cc:associatedIssue ?IssueURI .
   }
   GRAPH <consumerComplaintsIssueHierarchy_U##> {
    ?IssueURI rdfs:subClassOf* ?IssueCategory .
   }
   }
   GROUP BY ?State ?IssueCategory
   ORDER BY ?State DESC(?Count)
   ');
   ```
   This query shows how easily we can combine geography (consumer state) with semantic issue hierarchies.

The response from Knowledge Graph query with top issues by state will look like  
![](/exercises\ex_solutions\images/ex3.4-query3-top-issue-by-stater.png)

## Exercise 3.5 Validate the Knowledge Graph with SHACL

The SAP HANA Cloud Knowledge Graph engine supports **SHACL (Shapes Constraint Language)** validation.  
This allows us to define **rules** for our graph data, for example, every complaint must have a narrative, or credit card complaints must always be closed.  
In business scenarios, SHACL helps ensure **data quality and policy compliance** directly inside the graph, before data is used for analytics or regulatory reporting.  

---

### Step 1: Create SHACL shapes for complaints

In this step we create rules (called shapes) for our complaints data.  
These rules say:  
- Every complaint must have a narrative, a company, and an outcome.  
- Credit Card complaints must always be closed with an outcome.

```sql
CALL SPARQL_EXECUTE('
PREFIX sh:<http://www.w3.org/ns/shacl#>
PREFIX cc:<http://consumer.demo.com/complaints#>

INSERT DATA {
  GRAPH <shapes-complaints_U##> {
    # General complaint rules
    cc:ComplaintShape a sh:NodeShape ;
      sh:targetClass cc:Complaint ;

      sh:property [
        sh:path cc:complaintNarrative ;
        sh:minCount 1 ;
        sh:message "Every complaint must have a narrative." ;
      ] ;

      sh:property [
        sh:path cc:handledBy ;
        sh:minCount 1 ;
        sh:message "Every complaint must be linked to a company." ;
      ] ;

      sh:property [
        sh:path cc:hasOutcome ;
        sh:minCount 1 ;
        sh:message "Every complaint must have an outcome." ;
      ] .

    # Credit card special rule
    cc:CreditCardComplaintShape a sh:NodeShape ;
      sh:targetClass cc:Complaint ;

      sh:property [
        sh:path cc:isRelatedTo ;
        sh:hasValue cc:CreditCards ;
      ] ;

      sh:property [
        sh:path cc:hasOutcome ;
        sh:minCount 1 ;
        sh:message "Every Credit Card complaint must be closed with an outcome." ;
      ] .
  }
}', '', ?, ?);
```

The response from Knowledge Graph SHACL (Shapes Constraint Language) creation will look like
![](/exercises\ex_solutions\images/ex3.5-create-SHACL-shapes-response.png)


### Step 2: Let us insert bad complaints to force violations

```sql
CALL SPARQL_EXECUTE('
PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX cc:<http://consumer.demo.com/complaints#>

INSERT DATA {
  GRAPH <consumerComplaintsBase_U##> {
    # Missing narrative
    cc:BadComplaint1 rdf:type cc:Complaint .
    cc:BadComplaint1 cc:handledBy cc:Equifax .
    cc:BadComplaint1 cc:hasOutcome "Closed with explanation" .

    cc:BadComplaint2 rdf:type cc:Complaint .
    cc:BadComplaint2 cc:handledBy cc:Equifax .

    # Credit card complaint with no outcome
    cc:BadCreditCardComplaint1 rdf:type cc:Complaint .
    cc:BadCreditCardComplaint1 cc:complaintNarrative "Credit card charge dispute unresolved." .
    cc:BadCreditCardComplaint1 cc:isRelatedTo cc:CreditCards .
    cc:BadCreditCardComplaint1 cc:handledBy cc:Equifax .
    
    # Credit card complaint with no outcome
    cc:BadCreditCardComplaint2 rdf:type cc:Complaint .
    cc:BadCreditCardComplaint2 cc:complaintNarrative "Credit card charge issue that I reported is still pending resolution. No customer support." .
    cc:BadCreditCardComplaint2 cc:isRelatedTo cc:CreditCards .
    cc:BadCreditCardComplaint2 cc:handledBy cc:Equifax .
  }
}
', '', ?, ?);
```
Here we added a couple of complaints on purpose that break the rules. We did this so that when we run validation, SHACL will have something to catch and report back to us.

The response from Knowledge Graph insert query of bad complaints to force SHACL validation will look like
![](/exercises\ex_solutions\images/ex3.5-insert-bad-complaints.png)



### Step 3: Now, let us run the validation

```sql
CALL SPARQL_EXECUTE('
USING <shapes-complaints_U##>
VALIDATE <consumerComplaintsBase_U##>
CREATE REPORT GRAPH <report-complaints_U##>
', '', ?, ?);
```
Now, when we ran the validation:

- HANA checked all the complaints in the graph against the rules we created.  
- A new report graph `<report-complaints>` was created with the results.  
- Because we added bad complaints, the report now shows which ones broke the rules.  
- For example:  
  - `BadComplaint1` is missing a narrative.  
  - `BadCreditCardComplaint1` has no outcome.  

In short, SHACL caught the problems for us and listed them clearly.

The response from SHACL Knowledge Graph validation query will look like
![](/exercises\ex_solutions\images/ex3.5-validate-inserted-complaints-create-report.png)



### Step 4: In the last step, let us list the violations that we have caught

```sql
SELECT * FROM SPARQL_TABLE('
PREFIX sh:<http://www.w3.org/ns/shacl#>

SELECT ?focusNode ?message ?path
FROM <report-complaints_U##>
WHERE {
  ?v a sh:ValidationResult ;
     sh:focusNode ?focusNode ;
     sh:resultMessage ?message ;
     sh:resultPath ?path .
}
');
```
Here wee asked the system to show us the validation results.  Now we can clearly see which complaints broke the rules and why. This makes it easy to spot and fix bad data right away.

The SHACL validation report content query will look like
![](/exercises\ex_solutions\images/ex3.5-list-violoations-report.png)

## Exercise 3.6 Visualize the Knowledge Graph (Optional)

Once the Knowledge Graph is created, you can visualize it using the SAP HANA Cloud Central tool. Please note that this functionality is not currently supported on the TechEd HANA Cloud landscape.
You are welcome to explore it at your own pace using your own licensed SAP HANA Cloud instance via the SAP HANA Cloud Central tool.

1. In HANA Cloud Central, go to the **Database Objects** section.
   ![](/exercises/ex3/images/HCC_DatabaseObjects.png) 
2. On the top right corner, click on your **profile icon** and open **Settings**.
   ![](/exercises/ex3/images/HCC_Settings.png) 
3. In the **Displayed Database Object Types** list, check the option **RDF Named Graphs**.
4. Click **Save**.
   ![](/exercises/ex3/images/HCC_Select_RDFNamedGraphs.png) 
5. Now return to the **Database Objects** page and select the section **RDF Named Graph**.
   ![](/exercises/ex3/images/HCC_View_RDFNamedGraphs.png) 
6. You will see the Knowledge Graph you just created – **`consumerComplaintsBase`** listed.
7. Click on it and then open the **Graph Ontology** tab.
8. You can now **visualize the Knowledge Graph** with the respective classes and relationships created in the previous step.
   ![](/exercises/ex3/images/HCC_View_GraphOntology.png) 


By enabling **RDF Named Graphs** in the HANA Cloud Central, we are able to see and explore the Knowledge Graph visually:
- **Nodes** represent entities such as Complaint, Company, Product, Issue, Outcome, and Consumer.
- **Edges** represent the relationships such as `cc:handledBy`, `cc:isRelatedTo`, and `cc:associatedIssue`.
- This visualization helps confirm that our triples were created correctly and provides an intuitive way to understand how consumer complaint data is connected.

## Summary and Next Steps

In this exercise set, we:  
- Created a Knowledge Graph from flat complaint records in HANA Cloud.    
- Queried the graph with SPARQL to explore linked data and apply filters.  
- Introduced product and issue hierarchies for roll-up queries.  
- Validated our data with SHACL to catch missing or inconsistent information.  
- Visualized the graph to see how complaints, companies, products, issues, and consumers are connected. (optional)

Together, these steps showed how Knowledge Graphs bring relationships, hierarchies, and quality checks into analysis, things that are hard to achieve with plain SQL.

### What else is possible?

While we only worked on one dataset stored in HANA, Knowledge Graphs open many more possibilities:  
- **Multi-hop queries**: find consumers linked to products that share issues across companies.  
- **External data integration**: connect open datasets (e.g., financial ratings, government data) with your own complaints.  
- **Cross-domain insights**: link complaint data with HR, sales, or risk data to reveal broader business patterns.  
- **AI and reasoning**: use semantic rules and ontologies to enrich queries and power intelligent assistants.  

This is just the beginning. Knowledge Graphs move you from isolated tables to a connected view of knowledge that drives new business insights.
