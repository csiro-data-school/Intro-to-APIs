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
The E-utilities are made up of 9 programs that provide access to Entrez. You can find a list of these 9 programs in the table below. The information shown in this table was taken from [Eric Sayers A General Introduction to the E-utilities](https://www.ncbi.nlm.nih.gov/books/NBK25497/).

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

>## Find Pubmed articles 
> Use E-utility ESearch to perform a search about the condition Cold urticaria in PubMed where the results meet the following parameters: 
> - The results are sorted by publication date
> - The results contain cold urticaria in the title
>
>>## Solution
>>1. https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=cold+urticaria&sort=pub_date
>{: .solution}
{: .challenge}

>## Gather statistics on an Entrez database
> 
> 1. Identify the E-utility you would use to complete this task.
> 2. Identify the database you would like to view the statistics for.
> 3. Write the API call.
>
>>## Solution
>>- Here is an example solution:
>>1. https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi?db=pubmed
>{: .solution}
{: .challenge}


