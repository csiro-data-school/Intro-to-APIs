---
title: "Exercise: NCBI E-utilities"
teaching: 
- "5"
exercises: 
- "20"
questions:
- "How can we use the Entrez Programming Utilities (E-utilities) to search across the Entrez Molecular Sequence Database System?"
objectives:
- "Introduce the Entrez Molecular Sequence Database System (Entrez) and the databases it includes."
- "Provide sources for documentation and more information about using the E-utilities."
- "Develop API calls answer research questions using data pulled from Entrez through the E-utilities." 
keypoints:
- "By linking to the NCBI Entrez system through the E-utilities, you can make complicated data requests across a huge dataset."
---
## Background Information

#### Entrez Molecular Sequence Database System (Entrez)

Entrez is a molecular biology database system that provides integrated access to nucleotide and protein sequence data, genomic mapping informaiton, 3D structure data, PubMed MEDLINE, and more. This system is produced by the National Center for Biotechnology Information (NCBI). 

Entrez is NCBI's primary text search and retrieval system that integrates the PubMed database of biomedical literature with 38 other literature and molecular databases

The web based search interface for these NCBI databases is available to the public [here, through the U.S. National Library of Medicine](http://www.ncbi.nlm.nih.gov/Entrez/).

#### Databases included in Entrez
[You can find a full list of Entrez databases listed here](https://www.ncbi.nlm.nih.gov/books/NBK3837/).


#### Entrez Programming Utilities (E-utilities)
The E-utilities are made up of 9 programs that provide access to Entrez(https://www.ncbi.nlm.nih.gov/books/NBK25499/#_chapter4_ESummary_). You can find a list of these 9 programs in the table below. The information shown in this table was taken from [Eric Sayers A General Introduction to the E-utilities](https://www.ncbi.nlm.nih.gov/books/NBK25497/).

| E-utilities | Query string (base URL for the API) | Use |
| ------ | ------ | ------ |
| EInfo | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi | Provides the number of records indexed in each field of a given database, the date of the last update of the database, and the available links from the database to other Entrez databases. |
| ESearch | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi | Responds to a text query with the list of matching UIDs in a given database (for later use in ESummary, EFetch or ELink), along with the term translations of the query. |
| EPost | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/epost.fcgi | Accepts a list of UIDs from a given database, stores the set on the History Server, and responds with a query key and web environment for the uploaded dataset. |
| ESummary | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi | Responds to a list of UIDs from a given database with the corresponding document summaries. |
| EFetch | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi | Responds to a list of UIDs in a given database with the corresponding data records in a specified format. |
| ELink | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/elink.fcgi | Responds to a list of UIDs in a given database with either a list of related UIDs (and relevancy scores) in the same database or a list of linked UIDs in another Entrez database; checks for the existence of a specified link from a list of one or more UIDs; creates a hyperlink to the primary LinkOut provider for a specific UID and database, or lists LinkOut URLs and attributes for multiple UIDs. |
| EGQuery | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/egquery.fcgi | Responds to a text query with the number of records matching the query in each Entrez database. |
| ESpell | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/espell.fcgi | Retrieves spelling suggestions for a text query in a given database. | 
| ECitMatch | https://eutils.ncbi.nlm.nih.gov/entrez/eutils/ecitmatch.cgi | Retrieves PubMed IDs (PMIDs) corresponding to a set of input citation strings. |

#### E-utilities Documentation
- [Entrez Programming Utilities Help](https://www.ncbi.nlm.nih.gov/books/NBK25501/) 

>## Basic Searching
>API string: esearch.fcgi?db=<database>&term=<query>
>
>Input: Entrez database (&db); Any Entrez text query (&term)
 >
>Output: List of UIDs matching the Entrez query
>
> Example: Get the PubMed IDs (PMIDs) for articles about brain tumour published in Science in 2023
  >
>https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=science[journal]+AND+brain+tumour+AND+2023[pdat] 
> 
{: .checklist}
  
>## Basic Downloading
>API string: efetch.fcgi?db=<database>&id=<uid_list>&rettype=<retrieval_type>&retmode=<retrieval_mode>
 >
>Input: List of UIDs (&id); Entrez database (&db); Retrieval type (&rettype); Retrieval mode (&retmode)
 >
>Output: Formatted data records as specified
>
>Example: Download nuccore GIs 34577062 and 24475906 in FASTA format
 >
>https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=34577062,24475906&rettype=fasta&retmode=text 
> 
{: .checklist}

>## Getting Database Statistics and Search Fields
>API string: einfo.fcgi?db=<database>
 >
>Input: Entrez database (&db)
 >
>Output: XML containing database statistics
 >
>Note: If no database parameter is supplied, einfo will return a list of all valid Entrez databases.
 >
>Example: Find database statistics for Entrez Protein.
 >
>https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi?db=protein 
> 
{: .checklist}



## Exercises

>## Retrieve PubMed Articles About Cold Urticaria
> Using the Entrez API, write a python program to search for articles about "cold urticaria" in PubMed, sorted by publication date.
>>## Solution
>>```python
>>import requests
>>import json
>>
>># Base URL of the API endpoint
>>base_url = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi"
>>
>># Parameters to be sent with the request
>>params = {
>>    'db': 'pubmed',
>>    'term': 'cold urticaria',
>>    'sort': 'pub_date',
>>    'retmode': 'json'
>>}
>>
>># Making the GET request
>>response = requests.get(base_url, params=params)
>>
>># Checking the response status and handling the data
>>if response.status_code == 200:
>>    # Request was successful, process the data
>>    data = response.json()  # Parsing the response as JSON
>>    print("Data received from the API:")
>>    json_data = json.dumps(data, indent=4, sort_keys=True)
>>    print(json_data) 
>>else:
>>    # Handling request errors
>>    print(f"Request failed with status code: {response.status_code}")
>>```
>{: .solution}
{: .challenge}

>## Gather Information on the PubMed Database
> Using the Entrez API, write a python program to gather detailed information about the PubMed database.
>>## Solution
>>```python
>>import requests
>>import json
>>
>># Base URL of the API endpoint
>>base_url = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi"
>>
>># Parameters to be sent with the request
>>params = {
>>    'db': 'pubmed',
>>    'retmode': 'json'
>>}
>>
>># Making the GET request
>>response = requests.get(base_url, params=params)
>>
>># Checking the response status and handling the data
>>if response.status_code == 200:
>>    # Request was successful, process the data
>>    data = response.json()  # Parsing the response as JSON
>>    print("Data received from the API:")
>>    json_data = json.dumps(data, indent=4, sort_keys=True)
>>    print(json_data)
>>else:
>>    # Handling request errors
>>    print(f"Request failed with status code: {response.status_code}")
>>```
>{: .solution}
{: .challenge}


>## Search for Genetic Information About BRCA1
> Using the Entrez API, write a python program to search for genetic information about the gene BRCA1 in the Nucleotide database. Limit the results to 10 entries.
>>## Solution
>>```python
>>import requests
>>import json
>>
>># Base URL of the API endpoint
>>base_url = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi"
>>
>># Parameters to be sent with the request
>>params = {
>>    'db': 'nucleotide',
>>    'term': 'BRCA1',
>>    'retmax': '10',
>>    'retmode': 'json'
>>}
>>
>># Making the GET request
>>response = requests.get(base_url, params=params)
>>
>># Checking the response status and handling the data
>>if response.status_code == 200:
>>    # Request was successful, process the data
>>    data = response.json()  # Parsing the response as JSON
>>    print("Data received from the API:")
>>    json_data = json.dumps(data, indent=4, sort_keys=True)
>>    print(json_data)
>>else:
>>    # Handling request errors
>>    print(f"Request failed with status code: {response.status_code}")
>>```
>{: .solution}
{: .challenge}


>## Fetch Sequence Data for a Specific Nucleotide ID
> Use the Entrez API to fetch the sequence data for a specific nucleotide ID returned by a previous search. Let's Use one of the IDs returned by the ESearch query, for example, `359465566`.
>>## Solution
>>```python
>>import requests
>>import json
>>
>># Base URL of the EFetch API endpoint
>>efetch_url = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi"
>>
>># ID to fetch details for (taken from the ESearch result)
>>nucleotide_id = "359465566"
>>
>># Parameters for the EFetch request
>>params = {
>>    'db': 'nucleotide',
>>    'id': nucleotide_id,
>>    'rettype': 'fasta',
>>    'retmode': 'text'
>>}
>>
>># Making the GET request
>>response = requests.get(efetch_url, params=params)
>>
>># Checking the response status and handling the data
>>if response.status_code == 200:
>>    # Request was successful, process the data
>>    fasta_data = response.text  # Fetch the response as plain text
>>    print("FASTA data received from the API:")
>>    print(fasta_data)
>>else:
>>    # Handling request errors
>>    print(f"Request failed with status code: {response.status_code}")
>>```
>{: .solution}
{: .challenge}

>## Gather Information on the Taxonomy Database
> Using the Entrez API, write a python program to gather detailed information about the Taxonomy database.
>>## Solution
>>```python
>>import requests
>>import json
>>
>># Base URL of the API endpoint
>>base_url = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi"
>>
>># Parameters to be sent with the request
>>params = {
>>    'db': 'taxonomy',
>>    'retmode': 'json'
>>}
>>
>># Making the GET request
>>response = requests.get(base_url, params=params)
>>
>># Checking the response status and handling the data
>>if response.status_code == 200:
>>    # Request was successful, process the data
>>    data = response.json()  # Parsing the response as JSON
>>    print("Data received from the API:")
>>    json_data = json.dumps(data, indent=4, sort_keys=True)
>>    print(json_data)
>>else:
>>    # Handling request errors
>>    print(f"Request failed with status code: {response.status_code}")
>>```
>{: .solution}
{: .challenge}
