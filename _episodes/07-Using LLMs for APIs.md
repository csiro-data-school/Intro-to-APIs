---
title: "Using LLMs for APIs"
teaching: 15
exercises: 5
questions:
- "What is an API?"
- "Why do we need APIs?"
- "How APIs work?"
objectives:
- "Understand the need for an API."
- "Understand the main components of an API."
- "Familiarity using Library APIs."
keypoints:
- "An API is a way for two or more computer programs or components to communicate with each other."
- "HTTP is the protocol originally designed for requesting and receiving Web pages, but now also used as the basis for a variety of APIs. HTTPS is the encrypted version of HTTP."
- "Every page on the world wide web is identified with a URL or Uniform Resource Locator."
- "A request is how you tell a server what you want to see. A response will either give you what you asked for, or tell you why the server can't do that. Both requests and responses have a header, and optionally a body."
- "We can make requests and receive responses, as well as see their headers, using `curl`."
---
LLMs like ChatGPT can understand and generate text based on patterns learned from their training data. If an API's query parameters or the concept of valid input values have been discussed in the data the model was trained on, ChatGPT might be able to provide general guidance based on that information.

However, LLMs do not have real-time access to external databases or the internet, and they cannot interact with APIs directly to fetch or verify current data. Their knowledge is based on the information available up to their last training cut-off, which means they might not have data on newly developed APIs or recent changes to existing ones.

Hence, their responses should be taken with caution and it is always best to consult the official API documentation for the most accurate and up-to-date information.

## Query Construction
One of the ways LLMs like ChatGPT can help you is by constructing queries based on your requirement. However, receiving a perfect API query from ChatGPT depends on its familiarity with the API. However, always keep in mind that the response of an Artificially Intelligent agent should be interpreted in the context of its training data.

As a strategy, ask the LLM to construct a simple API query and check against the API documentation. If successful, try to gauge its familiarity with a more complex query. Let's try it out!

>## National Center for Biotechnology Information (NCBI) E-utilities API.
> Let's explore the E-utilities documentation: [https://www.ncbi.nlm.nih.gov/books/NBK25499/](https://www.ncbi.nlm.nih.gov/books/NBK25499/)
> Using this API, create an API query to 
>
>>## Solution
>>```python
>>import requests
>>import json
>>```
>{: .solution}
{: .challenge}

## Help Understanding API Documentation
Some API documentations lack clarity on the types and ranges of valid inputs expected for parameters. Often, query parameters are not intuitive and the documentation may not include example queries, making it challenging to construct complex queries due to ambiguities. However, Large Language Models (LLMs) like ChatGPT, trained on extensive web data, can accurately interpret specific query parameters and offer insights into valid input values and their ranges.


Let's have a look at the FBI Crime Data API documentation: https://cde.ucr.cjis.gov/LATEST/webapp/#/pages/docApi


## Understanding API Responses
You can also paste in a block of JSON response from an API request to ChatGPT and ask it to describe it for you.

## Resolving Errors
Sometimes you can receive error codes from an API request. LLMs like ChatGPT can help you understand and resolve these errors.

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
As a very simple yet helpful use-case of LLMs is to enquire about best practices when using API keys aor APIs in general.