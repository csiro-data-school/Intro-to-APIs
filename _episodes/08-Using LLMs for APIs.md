---
title: "Using LLMs for APIs"
teaching: 15
exercises: 5
questions:
- "How do LLMs help with API queries?"
- "Can LLMs clarify API documentation?"
- "How do LLMs aid in fixing API errors?"
- "Can LLMs translate code for API tasks?"
objectives:
- "Understand how LLMs like GPT-4 can assist in formulating and refining API queries."
- "Explore the potential of LLMs in interpreting API documentation and providing insights into query parameters and valid input values."
- "Learn how to leverage LLMs for troubleshooting API errors and translating code snippets for effective API utilization."
keypoints:
- "LLMs can guide the creation of API queries, though their effectiveness depends on their familiarity with the specific API and its parameters."
- "LLMs have the potential to clarify ambiguities in API documentation and suggest valid input values, enhancing understanding and usage of APIs."
- "LLMs can offer explanations for API error codes and assist in translating code snippets between languages, aiding in debugging and code adaptation."
- "Despite their capabilities, LLMs' knowledge is limited to their training data, necessitating caution and verification against up-to-date, official API documentation."
---
LLMs like GPT-4 can understand and generate text based on patterns learned from their training data. If an API's query parameters or the concept of valid input values have been discussed in the data the model was trained on, the model might be able to provide general guidance based on that information.

However, LLMs do not have real-time access to external databases or the internet, and they cannot interact with APIs directly to fetch or verify current data. Their knowledge is based on the information available up to their last training cut-off, which means they might not have data on newly developed APIs or recent changes to existing ones.

Hence, their responses should be taken with caution and it is always best to consult the official API documentation for the most accurate and up-to-date information.

## Query Construction
One of the ways LLMs like GPT-4 can help you is by constructing queries based on your requirement. However, receiving a perfect API query from GPT-4 depends on its familiarity with the API. However, always keep in mind that the response of an Artificially Intelligent agent should be interpreted in the context of its training data.

As a strategy, ask the LLM to construct a simple API query and check against the API documentation. If successful, try to gauge its familiarity with a more complex query. Let's try it out using National Center for Biotechnology Information (NCBI) E-utilities API. E-utilities documentation: [https://www.ncbi.nlm.nih.gov/books/NBK25499/](https://www.ncbi.nlm.nih.gov/books/NBK25499/)

>## Perform a search
> Search for articles related to "Asthma" that were added to the PubMed database between January 1, 2020, and December 31, 2020
> 1. Identify which E-utility to use for this task.
> 2. Identify what parameters you would need to specify
>>## Solution
>>https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=Asthma&datetype=edat&mindate=2020/01/01&maxdate=2020/12/31&retmode=xml
>{: .solution}
{: .challenge}

>## Perform a search across all Entrez databases
> Perform a global Entrez search to determine racial/ethnic representation across database contents. 
> 1. Identify which E-utility to use for this task.
> 2. Identify how you will conceptualize and categorize racial/ethnic groupings for this task.
> - NIH racial and ethnic categories include American Indian or Alaska Native, Asian, Black or African American, Hispanic or Latino, Native Hawaiian or Other Pacific Islander, and White. [See NOT-OD-15-089](https://grants.nih.gov/grants/guide/notice-files/not-od-15-089.html) for more details.
> 3. Write and run the API strings that would enable you to uncover these representations. 
>
>>## Solution
>> Here is an example of API calls that could provide you with summary data based on NIH racial and ethnic categories. (There are many potential solutions to this exercise!) 
>>1. https://eutils.ncbi.nlm.nih.gov/gquery?term=african+AND+black&retmode=xml
>>2. https://eutils.ncbi.nlm.nih.gov/gquery?term=white&retmode=xml
>>3. https://eutils.ncbi.nlm.nih.gov/gquery?term=hispanic+OR+latino&retmode=xml
>>4. https://eutils.ncbi.nlm.nih.gov/gquery?term=native+hawaiian+OR+pacific+islander&retmode=xml
>{: .solution}
{: .challenge}


## Help Understanding API Documentation
Some API documentations lack clarity on the types and ranges of valid inputs expected for parameters. Often, query parameters are not intuitive and the documentation may not include example queries, making it challenging to construct complex queries due to ambiguities. However, Large Language Models (LLMs) like ChatGPT, trained on extensive web data, can accurately interpret specific query parameters and offer insights into valid input values and their ranges.


>## Find PMC articles 
> Use E-utility API to perform a search about the condition Peanut Allergy in PMC where the results meet the following parameters: 
> - The results are sorted by publication date
> - The results contain Peanut Allergy in the title
>
>>## Solution
>> https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pmc&term="Peanut+Allergy"[Title]&sort=pub+date&retmode=xml
>{: .solution}
{: .challenge}

Now compare the results with that of https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pmc&term=peanut+allergy&field=title&sort=pub+date

Why do you think there's a difference?
Let's query our cognitive co-pilot!

## Understanding API Responses
You can also paste in a block of JSON/XML response from an API request to GPT4 and ask it to describe it for you.

~~
...
<Field>
<Name>P1DAT</Name>
<FullName>P1DAT</FullName>
<Description>Date publication first accessible through Solr</Description>
<TermCount/>
<IsDate>Y</IsDate>
<IsNumerical>N</IsNumerical>
<SingleToken>Y</SingleToken>
<Hierarchy>N</Hierarchy>
<IsHidden>Y</IsHidden>
</Field>
</FieldList>
<LinkList>
<Link>
<Name>pubmed_assembly</Name>
<Menu>Assembly</Menu>
<Description>Assembly</Description>
<DbTo>assembly</DbTo>
</Link>
<Link>
<Name>pubmed_bioproject</Name>
<Menu>Project Links</Menu>
<Description>Related Projects</Description>
<DbTo>bioproject</DbTo>
</Link>
...
~~
{: .output}

## Resolving Errors
Sometimes you can receive error codes from an API request. LLMs like GPT-4 can help you understand and resolve these errors.

Try this query: https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pmc&term=peanut+allergy&field=title&sort=pub_date

Ask GPT-4 to explain the error.

## Modifying provided example code 
Let's try to attempt this task:
### Application: Retrieving large datasets
#### Goal: 
Download all chimpanzee mRNA sequences in FASTA format (>50,000 sequences).

#### Solution: 
First use ESearch to retrieve the GI numbers for these sequences and post them on the History server, then use multiple EFetch calls to retrieve the data in batches of 500.

- Input: $query – chimpanzee[orgn]+AND+biomol+mrna[prop]

- Output: A file named "chimp.fna" containing FASTA data.

Lucky for us, the API documentation contains examples applications, and this is one of them : https://www.ncbi.nlm.nih.gov/books/NBK25498/
However, Some of the scripts are in Perl, which you may not be familiar with.
LLMs like ChatGPT are very accurate in translating such small snippets of code from one language to another. Again, you need to remember that it depends on whether or not the LLM has been significantly exposed to a particular programming language in its training data.

Let's take the example Perl script provided for this goal:

```perl
use LWP::Simple;
$query = 'chimpanzee[orgn]+AND+biomol+mrna[prop]';

#assemble the esearch URL
$base = 'https://eutils.ncbi.nlm.nih.gov/entrez/eutils/';
$url = $base . "esearch.fcgi?db=nucleotide&term=$query&usehistory=y";

#post the esearch URL
$output = get($url);

#parse WebEnv, QueryKey and Count (# records retrieved)
$web = $1 if ($output =~ /<WebEnv>(\S+)<\/WebEnv>/);
$key = $1 if ($output =~ /<QueryKey>(\d+)<\/QueryKey>/);
$count = $1 if ($output =~ /<Count>(\d+)<\/Count>/);

#open output file for writing
open(OUT, ">chimp.fna") || die "Can't open file!\n";

#retrieve data in batches of 500
$retmax = 500;
for ($retstart = 0; $retstart < $count; $retstart += $retmax) {
        $efetch_url = $base ."efetch.fcgi?db=nucleotide&WebEnv=$web";
        $efetch_url .= "&query_key=$key&retstart=$retstart";
        $efetch_url .= "&retmax=$retmax&rettype=fasta&retmode=text";
        $efetch_out = get($efetch_url);
        print OUT "$efetch_out";
}
close OUT;
```
Let's say you are comfortable with Python. If you input this into ChatGPT, it produces a valid working python equivalent:

```python
import requests

def fetch_chimpanzee_mrna_sequences():
    # Define the query and the base URL
    query = 'chimpanzee[orgn]+AND+biomol+mrna[prop]'
    base_url = 'https://eutils.ncbi.nlm.nih.gov/entrez/eutils/'

    # ESearch: Retrieve the GI numbers and post them on the History server
    esearch_url = f"{base_url}esearch.fcgi?db=nucleotide&term={query}&usehistory=y"
    response = requests.get(esearch_url)
    web_env = response.text.split('<WebEnv>')[1].split('</WebEnv>')[0]
    query_key = response.text.split('<QueryKey>')[1].split('</QueryKey>')[0]
    count = int(response.text.split('<Count>')[1].split('</Count>')[0])

    # Prepare to retrieve data in batches
    retmax = 500
    with open("chimp.fna", "w") as outfile:
        for retstart in range(0, count, retmax):
            efetch_url = f"{base_url}efetch.fcgi?db=nucleotide&WebEnv={web_env}"
            efetch_url += f"&query_key={query_key}&retstart={retstart}"
            efetch_url += f"&retmax={retmax}&rettype=fasta&retmode=text"
            efetch_response = requests.get(efetch_url)
            outfile.write(efetch_response.text)

# Run the function
fetch_chimpanzee_mrna_sequences()
```

- Output validation: Don't forget to compare the output of the python script against that of the perl script

## General Queries Regarding Best Practices
As a very simple yet helpful use-case of LLMs is to enquire about best practices when using API keys or APIs in general.