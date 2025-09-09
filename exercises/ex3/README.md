# Exercise 3 - Analyse Consumer Complaints Data using Knowledge Graphs

In this exercise, we will use **SAP HANA Cloud Knowledge Graph** to build a semantic layer on top of the Consumer Complaints dataset.

The exercise shows how to:
* Create a Knowledge Graph directly from relational complaint data stored in HANA Cloud.
* Model key entities such as **Complaint**, **Consumer**, **Product**, **Issue**, **Company**, and **Outcome**.
* Link the data using semantic relationships (e.g., a complaint is related to a product, associated with an issue, and handled by a company).
* Visualize the resulting Knowledge Graph using the **SAP HANA Cloud Central tool**.
* Run **SPARQL queries** on the Knowledge Graph to perform advanced analysis that goes beyond flat SQL queries.

This provides a more semantic and hierarchical view of the data, allowing you to analyse consumer complaints with **hierarchies, relationships, and reasoning**.

## Exercise 3.1 Create the Knowledge Graph

After completing these steps you will have created a Knowledge Graph from the Consumer Complaints table.

1. Open the SAP HANA Cloud Central tool and navigate to the Knowledge Graph section.  
<br>![](/exercises/ex3/images/02_01_0010.png)

2. Run the following SQL query to create triples from your consumer complaints table:
```sql
CALL SPARQL_EXECUTE('
PREFIX rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs:<http://www.w3.org/2000/01/rdf-schema#>
PREFIX cc:<http://consumer.demo.com/complaints#>

INSERT {
  GRAPH <consumerComplaintsBase> {
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
  { sql_table(''SELECT TO_NVARCHAR("ComplaintID") AS ID_STR, "Company" AS "Company", CAST("ComplaintNarrative" AS NVARCHAR(5000)) AS "Narrative", CAST("Product" AS NVARCHAR(500)) AS "ProductName", CAST("SubProduct" AS NVARCHAR(500)) AS "SubProductName", CAST("Issue" AS NVARCHAR(500)) AS "IssueName", CAST("SubIssue" AS NVARCHAR(500)) AS "SubIssueName", CAST("CompanyResponse" AS NVARCHAR(1000)) AS "CompanyResponse", "State" AS "State", "ZipCode" AS "Zip", "Tags" AS "Tags", "ConsumerConsent" AS "Consent" FROM "DBADMIN"."CONSUMER_COMPLAINTS"'') }
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

### What did we just do?

The above command transformed rows in the **CONSUMER_COMPLAINTS** table into a **Knowledge Graph** - **'consumerComplaintsBase'**.

* **Complaint** – each complaint row becomes a node (`cc:Complaint`) with its ID and narrative.
* **Company** – linked to the complaint via `cc:handledBy`.
* **Product and Sub-Product** – linked to the complaint via `cc:isRelatedTo`.
* **Issue and Sub-Issue** – linked to the complaint via `cc:associatedIssue`.
* **Outcome** – the company’s response is modeled as an `cc:Outcome` linked via `cc:hasOutcome`.
* **Consumer** – information about the consumer (state, ZIP, tags, consent) is stored as a `cc:Consumer` node.
* **Relationships** – links like `cc:hasConsumer`, `cc:isRelatedTo`, `cc:associatedIssue` turn the flat table into a connected graph.

In short:
- A single row in the table now becomes a **web of connected entities**.
- Each entity has its own URI (unique identifier).
- Complaints are no longer just records — they are central nodes connected to companies, products, issues, outcomes, and consumers.

This structure enables you to run **SPARQL queries** to explore relationships and hierarchies that would be very hard to capture with plain SQL.

