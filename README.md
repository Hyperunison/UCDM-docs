# Unison Common Data Model - Documentation

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
 
 

