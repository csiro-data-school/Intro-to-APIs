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

With a [URL query](https://csiro-data-school.github.io/Intro-to-APIs/04-Creating%20URL%20queries/index.html) in hand, the [Unix shell](https://librarycarpentry.org/lc-shell/) provides a powerful command line driven interface for processing and interacting with large amounts of text data.  As the response from many APIs will be in a text-based format, such as JSON or XML, various shell tools will provide us with the functionality  to acquire data and process the response, allowing for easy use of the large existing set of Unix tools.


## Shell Tools  
Building on the skill from [Unix shell](https://librarycarpentry.org/lc-shell/) lesson, we add the following command line programs for working with APIs

- `curl`   - A command line tool for transferring data from URLs
- `jq`  - A command line JSON processor
- `xmlstarlet`  - A command line XML parser

In this lesson we will use `curl` command for interacting with our API endpoints and either `jq` or `xmlstarlet` to process the return based on the type response that is given by the endpoint.  In both cases we use the Unix shell pipe `|` to redirect the output of the `curl` command into the next for processing.
The full manual of usage options are available on the command line by entering `man curl`

## Using the `-G` Option in `curl`

The `-G` option in `curl` is used to make a GET request with data appended to the URL query string. When you use `-G`, any data specified with the `-d` or `--data-urlencode` option is appended to the URL as query parameters rather than being sent as the body of a POST request.

~~~
$ curl -G [URL] [options]
~~~
{: .bash}

 
### Using the `--data-urlencode` Option in `curl`

The `--data-urlencode` option in `curl` is used to URL-encode data before sending it in an HTTP request. This is particularly useful when the data contains special characters or spaces that need to be encoded properly for the request.

~~~
$ curl -G https://images-api.nasa.gov/search --data-urlencode "q=apollo" --data-urlencode "description=moon landing" --data-urlencode "media_type=image" --data-urlencode "page_size=1"
~~~
{: .bash}

This command generates the following URL:

`https://images-api.nasa.gov/search?q=apollo&description=moon%20landing&media_type=image&page_size=1`


# Pretty printing with `json.tool` Module in Python

You can use `python -m json.tool` to pretty-print JSON data, making it easier to read and inspect:

~~~
$ curl -G https://images-api.nasa.gov/search --data-urlencode "q=apollo" --data-urlencode "description=moon landing" --data-urlencode "media_type=image" --data-urlencode "page_size=1" | python -m json.tool
~~~
{: .bash}

~~~
{
    "collection": {
        "version": "1.0",
        "href": "http://images-api.nasa.gov/search?q=apollo&description=moon+landing&media_type=image&page_size=1",
        "items": [
            {
                "href": "https://images-assets.nasa.gov/image/7995383/collection.json",
                "data": [
                    {
                        "center": "MSFC",
                        "title": "Saturn Apollo Program",
                        "nasa_id": "7995383",
                        "media_type": "image",
                        "keywords": [
                            "Montage",
                            "Insignia",
                            "Logo",
                            "Patch",
                            "Emblem",
                            "Apollo 7",
                            "Apollo 8",
                            "Apollo 9",
                            "Apollo 10",
                            "Apollo 11",
                            "Apollo 12",
                            "Apollo 13",
                            "Apollo 14",
                            "Apollo 15",
                            "Apollo 16",
                            "Apollo 17"
                        ],
                        "date_created": "1979-05-01T00:00:00Z",
                        "description": "This montage depicts the flight crew patches for the manned Apollo 7 thru Apollo 17 missions.  The Apollo 7 through 10 missions were basically manned test flights that paved the way for lunar landing missions. Primary objectives met included the demonstration of the Command Service Module (CSM) crew performance; crew/space vehicle/mission support facilities performance and testing during a manned CSM mission; CSM rendezvous capability; translunar injection demonstration; the first manned Apollo docking, the first Apollo Extra Vehicular Activity (EVA), performance of the first manned flight of the lunar module (LM); the CSM-LM docking in translunar trajectory, LM undocking in lunar orbit, LM staging in lunar orbit, and manned LM-CSM docking in lunar orbit. Apollo 11 through 17 were lunar landing missions with the exception of Apollo 13 which was forced to circle the moon without landing due to an onboard explosion. The craft was,however, able to return to Earth safely. Apollo 11 was the first manned lunar landing mission and performed the first lunar surface EVA. Landing site was the Sea of Tranquility. A message for mankind was delivered, the U.S. flag was planted, experiments were set up and 47 pounds of lunar surface material was collected for analysis back on Earth.  Apollo 12, the 2nd manned lunar landing mission landed in the Ocean of Storms and retrieved parts of the unmanned Surveyor 3, which had landed on the Moon in April 1967. The Apollo Lunar Surface Experiments Package (ALSEP) was deployed, and 75 pounds of lunar material was gathered. Apollo 14, the 3rd lunar landing mission landed in Fra Mauro. ALSEP and other instruments were deployed, and 94 pounds of lunar materials were gathered, using a hand cart for first time to transport rocks. Apollo 15, the 4th lunar landing mission landed in the Hadley-Apennine region. With the first use of the Lunar Roving Vehicle (LRV), the crew was bale to gather 169 pounds of lunar material. Apollo 16, the 5th lunar landing mission, landed in the Descartes Highlands for the first study of highlands area. Selected surface experiments were deployed, the ultraviolet camera/spectrograph was used for first time on the Moon, and the LRV was used for second time for a collection of 213 pounds of lunar material. The Apollo program came to a close with Apollo 17, the 6th and final manned lunar landing mission that landed in the Taurus-Littrow highlands and valley area. This mission hosted the first scientist-astronaut, Schmitt, to land on the Moon. The 6th automated research station was set up, and 243 ponds of lunar material was gathered using the LRV.  "
                    }
                ],
                "links": [
                    {
                        "href": "https://images-assets.nasa.gov/image/7995383/7995383~thumb.jpg",
                        "rel": "preview",
                        "render": "image"
                    }
                ]
            }
        ],
        "metadata": {
            "total_hits": 1022
        },
        "links": [
            {
                "rel": "next",
                "prompt": "Next",
                "href": "http://images-api.nasa.gov/search?q=apollo&description=moon+landing&media_type=image&page_size=1&page=2"
            }
        ]
    }
}

~~~
{: .output}

The API response can be written to a file using the angled bracket `>` followed by a file name:

~~~
$ curl -G https://images-api.nasa.gov/search --data-urlencode "q=apollo" --data-urlencode "description=moon landing" --data-urlencode "media_type=image" --data-urlencode "page_size=1" > file.txt
~~~
{: .bash}

#### Pretty-print with `jq .` from the command line
Be default, the JSON returned in most API calls has the white-space removed, while this is more efficient for transfer and makes no operational difference, it makes the data difficult for humans to read.  The `jq` tool can be used to pretty-print, or format the JSON in a human-readable way, using the `.` command.

~~~
$ curl -G https://images-api.nasa.gov/search --data-urlencode "q=apollo" --data-urlencode "description=moon landing" --data-urlencode "media_type=image" --data-urlencode "page_size=1"  | jq '.'
~~~
{: .bash}
