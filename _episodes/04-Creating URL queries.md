---
title: "Creating URL Queries"
teaching:  20
exercises: 20
questions:
- "What are API keys? How do we obtain an API key?"
- "What are URL query strings?"
- "How do you interpret API documentation?"
- "How can you create an API query?"

objectives:
- "Explain URL query strings"
- "Explain how to create API queries"
- "Explain how to read and interpret API documentation"
keypoints:
- "Authentication ensures that only authorised entities can access the API."
- "Authorisation ensures they can only access what they're permitted to."
- "REST APIs use query stings to make requests"
---

# Constructing API Queries
To construct an API query or request, you typically use a combination of endpoints, resources, parameters, and headers, which can be obtained from an APIs documentation. NASA offers an API that exposes much of the data that they make public. In this lesson, we will look at the NASA API portal, `https://api.nasa.gov/`, which makes NASA data, including imagery, eminently accessible to application developers.

# Authentication and identification

Many web APIs restrict access to registered users or applications. This may be
because they are used to control things that are specific to a particular user
account, because different people have different privilege levels and so
different endpoints available, or simply because the API provider wants to
collect statistics on how the API is being used.

Various ways exist for developers to authenticate to an API, including:

## Different methods of Authentication & Authorisation
* Basic Authentication: Username and Password mechanism.
* OAuth: A standard protocol for token-based authentication and authorization.
* API Keys: Tokens to identify and validate the user/application.

One important fact about HTTP is that it is _stateless_: each request is treated
entirely separately, with no memory from one request to the next. This means
that you must present your authentication credentials with every request you
make to the API.

For the vast majority of APIs, there will exist good developer documentation that provides examples of
how to use the token or other identifier that they provide to connect to their
service, including examples.

### NASA API - Example of using an API key
To use the NASA API, you need an API key to identify yourself, but no additional authentication is required beyond this.

Let's try working with the NASA API now. To do this, first we need to generate
our API key by providing our details at [the API home page][nasa-api]. Once that
is done, NASA provide the API key instantly, and send a copy to the email
address you provide. 

# Reading API Documentation
Let's refer to the [NASA API documentation][nasa-api]:

- Look through the endpoints
    - What types of parameters are available for the different endpoints?
    - Do they match what you need?
    - What is the Base URL?
- Look for sample requests & responses
    - What kind of data does it return?
    - Is it what you’re looking for?
- Look for restrictions
    - Is it free?
    - Does it require a key or permissions?
    - Do they impose a limit?

# Components of an API query
Let's analyse this query from the NASA API:
`https://api.nasa.gov/mars-photos/api/v1/rovers/{roverName}/photos?camera=FHAZ&sol=1000&api_key=DEMO_KEY`

Here's how each component plays a role:

### Endpoints
An endpoint is a specific URL at which an API can be accessed. It represents a specific function or resource within the API. To construct an API query, you start with the `base URL` of the API and append the specific `endpoint` that corresponds to the data or functionality you want to access. In the above example:
- **Base URL:** `https://api.nasa.gov/`
- **Endpoint:** `/mars-photos/api/v1/rovers/{roverName}/photos`
Using this endpoint, you can retrieve photos captured by a specified Mars rover. By replacing {roverName} in the URL with the name of an actual rover, you will be able to access the relevant images.

### Parameters
 Parameters are options you can include in your API query to filter, sort, or manipulate the data returned by the API.
- **Types**: There are different types of parameters:
  - **Path Parameters**: Parts of the endpoint URL itself that specify a specific resource or subset of resources. Using the  example, `{roverName}` in `/rovers/{roverName}` is a path parameter.
  - **Query Parameters**: Added to the end of the endpoint URL, usually after a `?`, to filter the results. For example, `/photos?camera=FHAZ` returns the photos taken by the Front Hazard Avoidance Camera	(FHAC) of the rover.

## Creating an API query using the NASA API


### Public API Calls (No API Key Required):
Some APIs/API endpoints, like the NASA Image Search API endpoint, allow users to make requests without an API key. These are often simpler to use and are publicly accessible. For example:
~~~
https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=image&page_size=1
~~~
This URL searches the NASA Image API for images related to “Apollo” with a description of “moon” and limits the result to one image. Anyone can use this without registering for an API key.

### Authenticated API Calls (API Key Required):
Other APIs/endpoints, such as NASA’s Astronomy Picture of the Day (APOD) API endpoint, require an API key to track usage and ensure fair access. This adds a layer of security and allows the provider to monitor or limit the number of requests. 

Let's construct an API query to try, querying the Astronomy Picture of the Day (APOD).
The first step is to read and understand the API documentation. They also provide example queries in which you can see that NASA
expects the API key to be encoded as a query parameter.

~~~
https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY
~~~

Here, the api_key parameter is used to pass the user’s unique key. While DEMO_KEY works for limited testing, registered users receive their own key to access more robust usage.

~~~
{
  "date": "2024-11-03",
  "explanation": "What's that black spot on Jupiter? No one is sure.  During one pass of NASA's Juno over  Jupiter, the robotic spacecraft imaged an usually dark cloud feature informally dubbed the Abyss. Surrounding cloud patterns show the Abyss to be at the center of a vortex. Since dark features on Jupiter's atmosphere tend to run deeper than light features, the Abyss may really be the deep hole that it appears -- but without more evidence that remains conjecture.  The Abyss is surrounded by a complex of meandering clouds and other swirling storm systems, some of which are topped by light colored, high-altitude clouds.  The featured image was captured in 2019 while Juno passed only about 15,000 kilometers above Jupiter's cloud tops.  The next close pass of Juno near Jupiter will be in about three weeks.",
  "hdurl": "https://apod.nasa.gov/apod/image/2411/JupiterAbyss_JunoEichstadt_1080.jpg",
  "media_type": "image",
  "service_version": "v1",
  "title": "Jupiter Abyss",
  "url": "https://apod.nasa.gov/apod/image/2411/JupiterAbyss_JunoEichstadt_1080.jpg"
}
~~~
{: .output}

We can see that this API gives us JSON output including links to two versions
of the picture of the day, and then metadata about the picture including its
title and description.


>## Exercise: Search for Near Earth Objects
> Use the NASA NEO (Near Earth Object) API to search for asteroids. Your task is to:Find asteroids approaching earth in the next 2 days
>
>> ## Solution
>> 1. Construct the API query:
>> https://api.nasa.gov/neo/rest/v1/feed?start_date=2024-06-12&end_date=2024-06-13&api_key=DEMO_KEY
>{: .solution}
{: .challenge}

>## Let’s create a query 
>Run this query: https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=image
>1. What does the result show?
>2. Can you find the URL link for the media type the search returned?
>3. How would you structure a query to search for videos instead of images? (Hint: Check API doc https://api.nasa.gov/ [nasa-api])
>
>>## Solution
>>1. The result from the API query shows data for a specific image related to the Apollo moon missions. It includes metadata like the image’s title (“Apollo Footprint”), a description noting that Buzz Aldrin took the photo of a bootprint on the moon during the Apollo 11 mission. It also provides a link to the thumbnail of the image.
>>2. The URL link for the media type (an image) returned by the search is provided under the links array within the items. The specific link is: `https://images-assets.nasa.gov/image/PIA24439/PIA24439~thumb.jpg`
>>3. To modify the existing query to search for videos instead of images, you would need to change the media_type parameter from image to video. `https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=video&page_size=1`
>{: .solution}
{: .challenge}

[nasa-api]: https://api.nasa.gov