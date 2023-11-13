# Unison Common Data Model - Documentation

## Overview
### What is the purpose of the UCDM?
The UCDM is a light-weight way to map variables across multiple biobanks for federated data queries and analyses. 

### What is the philosophy behind the UCDM?
It uses the principles of the OMOP common data model, but does not require a biobank to be converted to the OMOP common data model. We are doing this for you on the fly. It is a very efficient way of going about this, as we only map variables a user is interested in, rather than having to map thousands of variables in a biobank.  

## UCDM Structure
### UCDM Universal Query Language (UQL)
The UCDM uses the Universal Query Language (UQL), a yaml based variant of the Standard Query Language (SQL). Database queries are defined in a yaml framework. For example the SQL query: 
```
SELECT column1, column5 AS alias5 FROM TABLE x WHERE column1 > 50
```

would be represented as 

```
SELECT:
  - column1
  - column5: alias5
FROM:
 - x
WHERE:
 - column1: >25

```

### UCDM Table structure

In the UCDM the tables below exist in the background. Tables other than patientID can be joined to the patient ID table as needed by the query to form a big table for a more comprehensive query.

#### PatientID table
In the UCDM the patientID table contains basic patient information. The columns present in this table are [list of columns] and can be queried like this: Mechanism on how to query here. 

At present the UCDM table features the following harmonized colums:
* race: race of patient (example values: 'White', 'Black', 'Chinese')
* gender: race of patient (example values: 'Male', 'Female')
* ethnicity: ethnicity of patient (example values: 'UK', 'DE', 'FR')
* year_of_birth: int (example values: 1970, 2000)
* date_of_birth: string (example values: '2020-01-01')
* (patientID) - for privacy reason this column is never displayed and only used internally

In the future these harmonized values will increase significantly. 

#### Measurement Table
The measurement table has three columns in the UCDM:
* measurement.name: name fo drug (example values: ???)
* measurement.value: (example values: ???)
* measurement.date: (example values: '2020-01-01')
* (patientID) - for privacy reason this column is never displayed and only used internally

#### Condition Table
The conditions table has three columns in the UCDM:
* condition.icd10: icd10 name of disease (example values: 'Oesophagitis', 'Chronic sinusitis')
* condition.start_date: (example values: '2020-01-01')
* condition.end_date: (example values: '2020-01-01')
* (patientID) - for privacy reason this column is never displayed and only used internally

#### Drug Table
* drug.name: name fo drug (example values: ???)
* drug.start_date: (example values: '2020-01-01')
* drug.end_date: (example values: '2020-01-01')
* (patientID) - for privacy reason this column is never displayed and only used internally

#### Procedure Table
* procedure.name: name fo drug (example values: ???)
* procedure.value: (example values: ???)
* procedure.date: (example values: '2020-01-01')
* procedure.quantity: quantity of procedure (example values: ???)
* (patientID) - for privacy reason this column is never displayed and only used internally

## UCDM Mappings
### How are biobanks mapped to the UCDM?

## Tutorials
### How to find biobanks of interest
### How to check biobank mappings
### Examples on how to create a cohort in one biobank
### Examples on how to create a cohort across multiple biobanks

## Overview
### What is the purpose of the UCDM?
### What is the philosophy behind the UCDM?

## UCDM Structure
### UCDM Universal Query Language (UQL)
### UCDM Table structure

## UCDM Mappings
### How are biobanks mapped to the UCDM?

## Tutorials
### How to find biobanks of interest
### How to check biobank mappings
### Examples on how to create a cohort in one biobank
### Examples on how to create a cohort across multiple biobanks

[here](#place-2)
## Overview
Welcome to the Unison UCDM documentation. In the following, we will describe the structure of the Unison common data model (UCDM) and provide tutorials on how to use it. 

### What is the purpose of the UCDM? 

## UCDM Structure



## Tutorials



Query Tutorial:
 
Query Example 1: Review distribution of a meta-data variable across a population
 
Tutorial: In this example we will query the distribution of the variable ‘year_of_birth’ across all available biobanks. Note that you can query only variables that have previously been harmonized in the Unison common data model (link on how to do this). Already harmonized meta data variables are suggested with the autocomplete feature. If now specific data resource is specified in the ‘FROM’ section, as in this case, only variables that are harmonized across all biobanks are available.

```yaml
SELECT:
  - year_of_birth
```
 
Background: This query will select the UCDM harmonized ‘year_of_birth’ variable across all available biobanks/datasets and distributions of this variable across the entire data resource can be reviewed in the next step.
 
 
 
Query Example 2: Review distribution by year of birth and race across all datasets
 
Tutorial: As in query example 1, we will query the distribution of the variables `year_of_birth` and `race` across all available biobanks. Note that you can query only variables that have previously been harmonized in the Unison common data model (link on how to do this). Already harmonized meta data variables are suggested with the autocomplete feature. If now specific data resource is specified in the ‘FROM’ section, as in this case, only variables that are harmonized across all biobanks are available.
 

```yaml
SELECT:
  - year_of_birth
  - race
```
 
Background: This query will select the UCDM harmonized ‘year_of_birth’ and ‘race’ variables across all available biobanks/datasets and the distributions of this variable across the entire data resource can be reviewed in the next step.
 
Query Example 3: Review distributions by year of birth and race of all males
 
Tutorial: Now we use what we’ve learned in query example 1 and 2 and add a more finegrained cohort selection using the `where` argument. Note that variables in the ‘where’ clause need to be harmonized across all queried biobanks in the Unison common data model (link on how to do this). In this example query, we select `year_of_birth` and `race` from all Unison harmonized variables and we then select only those individuals that have a ‘White’ or ‘Black’ string in the ‘race’ harmonized meta-data field.

```yaml
select:
  - year_of_birth
  - race
where:
  - race == 'White' or race == 'Black'
```

Background: This query will select the ‘year_of_birth’ and ‘race’ variables across all available biobanks/datasets and then select from these what’s specified in the where clause. The distribution of this variable across the entire data resource can be reviewed in the next step.
 
Query example 4: Review diagnosis distribution for all patients
 
Tutorial: Now we’d like to see the distribution of all diseases across all available biobanks / datasets. Now, this is going to be a slightly more complex query, as we need to join the general patient database table with the diagnosis database table.
 
We start by defining an alias. We state that ‘diag’ should refer to the content of the diag1.icd10 column.
 

```yaml
select:
  diag: diag1.icd10
```
 
The above statement is equivalent to `SELECT diag1.icd10 as diag` in SQL annotation.
 
In the second part of the query we will initiate a join of the diag1 table containing disease annotations and the main patient characteristics datatable based on the patient id column.
 
The shorthand form of this query is:

```yaml
select:
  - diag1.icd10
JOIN:
  - condition: diag1
```
 
If we were to write out this join in SQL syntax, it would look like:

```sql
SELECT diag1.icd10
FROM patient
JOIN condition ON condition.patient_id = patient.id
```
 
So the full query to see all icd10 codes of diseases represented across all databases is:

```yaml
select:
  diag: diag1.icd10
JOIN:
  - condition: diag1
```
 
Background: the diag1 table is a pre-existing table in the Unison common data model. It contains, among other things, the icd10 disease codes associated with each patient.
 
The diag1 table has the following columns: A, B, C, ..X (also review section ‘Unison harmonized diagnosis code table’). How it is generated is described here [link to documentation section]. In general, the diag1 table is used by joining it onto the main patient data table using the patient_id variable.
 
Tutorial: Unison Common Data Model – Table Structures
Unison common data model table structures follow as much as possible the guidelines of the OMOP common data model.
 
Unison harmonized main patient data table: Explanatory text
Unison harmonized diagnosis code table: Explanatory text
Unison harmonized measurement code table: Explanatory text
Unison harmonized procedure code table: Explanatory text

## Unison Harmonized data model and table structure

## Biobank table structure and integration into Unison

## FAQ Section
* How do I connect to a biobank to which I have access to?
* Do I need to get my biobanks and data sources to install the Unison software?
* I'm interested in a variable that is not in the harmonized Unison common data model - what do I do?
* I have thousands of column entries to map - how do I do this?
  Use the Unison chatGPT mapping tool. 
* How do I link my private dataset with the biobanks? How can I be sure that my data is safe and isn't exposed?

## What's confusing
* Drug / Measurment / Condition tables need to be joined to be available. The join is happening after variables from these tables are selected. The logical flow would be to first join and then select from the merged table.
* Aliases are assigned by drug: d1, rather than join drug as d1, for example - hard to follow for the uninitiated user.
* Alias assignment seems to be necessary - or can I do a join without creating an alias?
 
 
Tutorial: How to harmonize a variable and add it to the Unison common data model
To be added.
 
 

