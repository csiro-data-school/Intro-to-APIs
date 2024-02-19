---
title: "API Queries in Shell"
teaching: 50
exercises: 20
questions:
- "How do I fetch data from API's using the Unix shell?"
- "How do I process API responses to get plain text, usable by standard Unix shell tools?"
objectives:
- "Understand how to fetch data from APIs using the Unix shell"
- "Choose and operate tools to convert API response to plain text"
keypoints:
- "You can request and process data from APIs using Unix shell tools"
- "JSON or XML results can be parsed to plain text, to be consumed by standard Unix shell tools"
---


## Fetching data from the web

With a [URL query](https://joshuadull.github.io/APIs-for-Libraries/03-Creating-url-queries/index.html) in hand, the [Unix shell](https://librarycarpentry.org/lc-shell/) provides a powerful command line driven interface for processing and interacting with large amounts of text data.  As the response from many APIs will be in a text-based format, such as JSON or XML, various shell tools will provide us with the functionality  to acquire data and process the response, allowing for easy use of the large existing set of Unix tools.


## Shell Tools  
Building on the skill from [Unix shell](https://librarycarpentry.org/lc-shell/) lesson, we add the following command line programs for working with APIs

- `curl`   - A command line tool for transferring data from URLs
- `jq`  - A command line JSON processor
- `xmlstarlet`  - A command line XML parser

In this lesson we will use `curl` command for interacting with our API endpoints and either `jq` or `xmlstarlet` to process the return based on the type response that is given by the endpoint.  In both cases we use the Unix shell pipe `|` to redirect the output of the `curl` command into the next for processing.

## API `Get` Requests  
A tool for transferring data via URL, the `curl  ` command allows use to interact with API endpoints from the command line.  A full-featured tool, the `curl` command  has built-in functionality to work with *most* types of API endpoint and authentication schemes. The full manual of usage options are available on the command line by entering `man curl`
 

Using the Voyager API, we enter the command `curl` followed by the URL for the Bibliographic item with the ISBN 9780415704953 

~~~
$ curl "https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953"
~~~
{: .bash}
~~~
{"record":[{
"title":"Animism and the question of life /",
"author":"Praet, Istvan, 1974-",
"pdescription":"xiv, 198 pages ; 24 cm.",
"publisher":"Routledge,",
"pubplace":"New York :",
"pubdate":"2014.",
"isxn":"9780415704953",
"oclcmrn":"ocn853435847",
"bibid":"11736943",
"items":[
{"loccode":"sml","itemenum":"NA","itypename":"Circulating","callno":"GN471 .P73X 2014 (LC)","itemstatus":"Not Charged","barcodestatus":"Active","itemchron":"NA","itemid":"10677986","itemformat":"SPM","itypecode":"circ","itemspinelabel":"NA","itemstat":"NA","locname":"SML, Stacks, LC Classification","barcode":"39002123260565","mfhdid":"11858762","availdate":"NA"}
]
}]}

~~~
{: .output}

The API response can be written to a file using the angled bracket `>` followed by a file name:

~~~
$ curl "https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953" > file.txt
~~~
{: .bash}

Alternatively, the output can be piped to another command for further action:

~~~
$ curl "https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953" | wc -l
~~~
{: .bash}

This example uses the `wc`` command with the `-l` option to count the number of lines in the response.   



## JSON filtering with `jq`

The `jq` command transforms JSON content through selection and filtering.  Using the Unix shell "pipe" `|` allows us to take the API response that we receive from our `curl` command, and process it in various ways with `jq`  


 

#### Pretty-print with `jq .`
Be default, the JSON returned in most API calls has the white-space removed, while this is more efficient for transfer and makes no operational difference, it makes the data difficult for humans to read.  The `jq` tool can be used to pretty-print, or format the JSON in a human-readable way, using the `.` command.

~~~
$ curl "https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953"  | jq '.'
~~~
{: .bash}
~~~
{
  "record": [
    {
      "title": "Animism and the question of life /",
      "author": "Praet, Istvan, 1974-",
      "pdescription": "xiv, 198 pages ; 24 cm.",
      "publisher": "Routledge,",
      "pubplace": "New York :",
      "pubdate": "2014.",
      "isxn": "9780415704953",
      "oclcmrn": "ocn853435847",
      "bibid": "11736943",
      "items": [
        {
          "loccode": "sml",
          "itemenum": "NA",
          "itypename": "Circulating",
          "callno": "GN471 .P73X 2014 (LC)",
          "itemstatus": "Not Charged",
          "barcodestatus": "Active",
          "itemchron": "NA",
          "itemid": "10677986",
          "itemformat": "SPM",
          "itypecode": "circ",
          "itemspinelabel": "NA",
          "itemstat": "NA",
          "locname": "SML, Stacks, LC Classification",
          "barcode": "39002123260565",
          "mfhdid": "11858762",
          "availdate": "NA"
        }
      ]
    }
  ]
}

~~~
{: .output}


### Filter by Key
`jq` can be used to select the values of a specified key, or keys.  This allows for specific pieces of data, from the overall JSON object to be filtered and returned.

In this example, we return just the title.
~~~
$ curl https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953  | jq '.record[].title'
~~~
{: .bash}
~~~
"Animism and the question of life /"
~~~
{: .output}

Since the Voyager API JSON response begins with the root "record" key, with use the element key `.record` followed by the open and close square brackets`[]` to put all of the child elements in an array, the final selector is the `.title` to select all of the title elements for items in the this JSON response.


We can specific multiple keys in a single filter, In this example, we return the title and author by specified both keys, separated by a comma.
~~~
$ curl "https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953"  | jq '.record[].title, .record[].author'

~~~
{: .bash}
~~~
"Animism and the question of life /"
"Praet, Istvan, 1974-"

~~~
{: .output}

## XML Processing



~~~
$ curl "https://libapp.library.yale.edu/VoySearch/GetBibMarc?isxn=9780415704953" 
~~~
{: .bash}
~~~
<?xml version="1.0" encoding="UTF-8"?><collection xmlns="http://www.loc.gov/MARC21/slim"><record><leader>01082pam a2200361 i 4500</leader><controlfield tag="001">11736943</controlfield><controlfield tag="005">20131211185012.0</controlfield><controlfield tag="008">130405s2014    nyua     b    001 0 eng  </controlfield><datafield tag="010" ind1=" " ind2=" "><subfield code="a">  2013013852</subfield></datafield><datafield tag="020" ind1=" " ind2=" "><subfield code="a">9780415704953</subfield></datafield><datafield tag="020" ind1=" " ind2=" "><subfield code="a">0415704952</subfield></datafield><datafield tag="024" ind1="8" ind2=" "><subfield code="a">40022828439</subfield></datafield><datafield tag="035" ind1=" " ind2=" "><subfield code="a">(OCoLC)ocn864945837</subfield></datafield><datafield tag="035" ind1=" " ind2=" "><subfield code="a">(NhCcYBP)  2013013852</subfield></datafield><datafield tag="035" ind1=" " ind2=" "><subfield code="a">11736943</subfield></datafield><datafield tag="040" ind1=" " ind2=" "><subfield code="a">DLC</subfield><subfield code="b">eng</subfield><subfield code="e">rda</subfield><subfield code="c">DLC</subfield><subfield code="d">OCLCO</subfield><subfield code="d">YDXCP</subfield><subfield code="d">NhCcYBP</subfield></datafield><datafield tag="042" ind1=" " ind2=" "><subfield code="a">pcc</subfield></datafield><datafield tag="050" ind1="0" ind2="0"><subfield code="a">GN471</subfield><subfield code="b">.P73 2014</subfield></datafield><datafield tag="079" ind1=" " ind2=" "><subfield code="a">ocn853435847</subfield></datafield><datafield tag="090" ind1=" " ind2=" "><subfield code="a">GN471</subfield><subfield code="b">.P73X 2014 (LC)</subfield></datafield><datafield tag="100" ind1="1" ind2=" "><subfield code="a">Praet, Istvan,</subfield><subfield code="d">1974-</subfield></datafield><datafield tag="245" ind1="1" ind2="0"><subfield code="a">Animism and the question of life /</subfield><subfield code="c">Istvan Praet.</subfield></datafield><datafield tag="264" ind1=" " ind2="1"><subfield code="a">New York :</subfield><subfield code="b">Routledge,</subfield><subfield code="c">2014.</subfield></datafield><datafield tag="300" ind1=" " ind2=" "><subfield code="a">xiv, 198 pages ;</subfield><subfield code="c">24 cm.</subfield></datafield><datafield tag="336" ind1=" " ind2=" "><subfield code="a">text</subfield><subfield code="2">rdacontent</subfield></datafield><datafield tag="337" ind1=" " ind2=" "><subfield code="a">unmediated</subfield><subfield code="2">rdamedia</subfield></datafield><datafield tag="338" ind1=" " ind2=" "><subfield code="a">volume</subfield><subfield code="2">rdacarrier</subfield></datafield><datafield tag="490" ind1="1" ind2=" "><subfield code="a">Routledge studies in anthropology ;</subfield><subfield code="v">15</subfield></datafield><datafield tag="504" ind1=" " ind2=" "><subfield code="a">Includes bibliographical references and index.</subfield></datafield><datafield tag="650" ind1=" " ind2="0"><subfield code="a">Animism.</subfield></datafield><datafield tag="650" ind1=" " ind2="0"><subfield code="a">Anthropology of religion.</subfield></datafield><datafield tag="650" ind1=" " ind2="0"><subfield code="a">Life.</subfield></datafield><datafield tag="830" ind1=" " ind2="0"><subfield code="a">Routledge studies in anthropology ;</subfield><subfield code="v">15.</subfield></datafield></record></collection>

~~~
{: .output}

Converting response from Voyager API to text, select element

#### Pretty-print with `xmlstarlet`

~~~
$ curl -s "https://libapp.library.yale.edu/VoySearch/GetBibMarc?isxn=9780415704953" | xmlstarlet format --indent-tab

~~~
{: .bash}
~~~
<?xml version="1.0" encoding="UTF-8"?>
<collection xmlns="http://www.loc.gov/MARC21/slim">
  <record>
    <leader>01082pam a2200361 i 4500</leader>
    <controlfield tag="001">11736943</controlfield>
    <controlfield tag="005">20131211185012.0</controlfield>
    <controlfield tag="008">130405s2014    nyua     b    001 0 eng  </controlfield>
    <datafield tag="010" ind1=" " ind2=" ">
      <subfield code="a">  2013013852</subfield>
    </datafield>
    <datafield tag="020" ind1=" " ind2=" ">
      <subfield code="a">9780415704953</subfield>
    </datafield>
    <datafield tag="020" ind1=" " ind2=" ">
      <subfield code="a">0415704952</subfield>
    </datafield>
    <datafield tag="024" ind1="8" ind2=" ">
      <subfield code="a">40022828439</subfield>
    </datafield>
    <datafield tag="035" ind1=" " ind2=" ">
      <subfield code="a">(OCoLC)ocn864945837</subfield>
    </datafield>
    <datafield tag="035" ind1=" " ind2=" ">
      <subfield code="a">(NhCcYBP)  2013013852</subfield>
    </datafield>
    <datafield tag="035" ind1=" " ind2=" ">
      <subfield code="a">11736943</subfield>
    </datafield>
    <datafield tag="040" ind1=" " ind2=" ">
      <subfield code="a">DLC</subfield>
      <subfield code="b">eng</subfield>
      <subfield code="e">rda</subfield>
      <subfield code="c">DLC</subfield>
      <subfield code="d">OCLCO</subfield>
      <subfield code="d">YDXCP</subfield>
      <subfield code="d">NhCcYBP</subfield>
    </datafield>
    <datafield tag="042" ind1=" " ind2=" ">
      <subfield code="a">pcc</subfield>
    </datafield>
    <datafield tag="050" ind1="0" ind2="0">
      <subfield code="a">GN471</subfield>
      <subfield code="b">.P73 2014</subfield>
    </datafield>
    <datafield tag="079" ind1=" " ind2=" ">
      <subfield code="a">ocn853435847</subfield>
    </datafield>
    <datafield tag="090" ind1=" " ind2=" ">
      <subfield code="a">GN471</subfield>
      <subfield code="b">.P73X 2014 (LC)</subfield>
    </datafield>
    <datafield tag="100" ind1="1" ind2=" ">
      <subfield code="a">Praet, Istvan,</subfield>
      <subfield code="d">1974-</subfield>
    </datafield>
    <datafield tag="245" ind1="1" ind2="0">
      <subfield code="a">Animism and the question of life /</subfield>
      <subfield code="c">Istvan Praet.</subfield>
    </datafield>
    <datafield tag="264" ind1=" " ind2="1">
      <subfield code="a">New York :</subfield>
      <subfield code="b">Routledge,</subfield>
      <subfield code="c">2014.</subfield>
    </datafield>
    <datafield tag="300" ind1=" " ind2=" ">
      <subfield code="a">xiv, 198 pages ;</subfield>
      <subfield code="c">24 cm.</subfield>
    </datafield>
    <datafield tag="336" ind1=" " ind2=" ">
      <subfield code="a">text</subfield>
      <subfield code="2">rdacontent</subfield>
    </datafield>
    <datafield tag="337" ind1=" " ind2=" ">
      <subfield code="a">unmediated</subfield>
      <subfield code="2">rdamedia</subfield>
    </datafield>
    <datafield tag="338" ind1=" " ind2=" ">
      <subfield code="a">volume</subfield>
      <subfield code="2">rdacarrier</subfield>
    </datafield>
    <datafield tag="490" ind1="1" ind2=" ">
      <subfield code="a">Routledge studies in anthropology ;</subfield>
      <subfield code="v">15</subfield>
    </datafield>
    <datafield tag="504" ind1=" " ind2=" ">
      <subfield code="a">Includes bibliographical references and index.</subfield>
    </datafield>
    <datafield tag="650" ind1=" " ind2="0">
      <subfield code="a">Animism.</subfield>
    </datafield>
    <datafield tag="650" ind1=" " ind2="0">
      <subfield code="a">Anthropology of religion.</subfield>
    </datafield>
    <datafield tag="650" ind1=" " ind2="0">
      <subfield code="a">Life.</subfield>
    </datafield>
    <datafield tag="830" ind1=" " ind2="0">
      <subfield code="a">Routledge studies in anthropology ;</subfield>
      <subfield code="v">15.</subfield>
    </datafield>
  </record>
</collection>

~~~
{: .output}

### Filter by Element Tag
XPath can be used along with `xmlstarlet` to select the values of a specified element.  This allows for specific pieces of data, from the overall XML document to be filtered and returned.  

In this example, we return just the subject headings from the MARCXML record:


~~~
$ curl -s "https://libapp.library.yale.edu/VoySearch/GetBibMarc?isxn=9780415704953" | xmlstarlet  sel -t -v '//*[@tag="650"]//text()' 
~~~
{: .bash}

~~~
Animism.
Anthropology of religion.
Life.
~~~
{: .output}



   
    

