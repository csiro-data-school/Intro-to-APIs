---
title: "Creating URL Queries"
teaching:  20
exercises: 20
questions:
- "What are URL query strings?"
- "How can you make API requests?"
- "How do you interpret API documentation?"
objectives:
- "Explain URL query strings"
- "Explain how to create API queries"
- "Explain how to read and interpret API documentation"
keypoints:
- "REST APIs use query stings to make requests"
- "URL queries are found all across the web"
---


### URL query String: Youtube

 **[https://www.youtube.com/watch?v=s7wmiS2mSXY&t=1m45s](https://www.youtube.com/watch?v=s7wmiS2mSXY&t=1m45s)**
![youtube URL](../assets/img/youtubeAPI.png)

## Constructing API Queries
To construct an API query or request, you typically use a combination of endpoints, resources, parameters, and headers, which can be obtained from an APIs documentation. NASA offers an API that exposes much of the data that they make public. In this lesson, we will look at the NASA API portal, `https://api.nasa.gov/`, which makes NASA data, including imagery, eminently accessible to application developers.


### Reading API Documentation

- Look through the params
    - What types of search parameters are available?
    - Do they match what you need?
    - What is the Endpoint (Base URL)?
- Look for sample requests & responses
    - What kind of data does it return?
    - Is it what you’re looking for?
- Look for restrictions
    - Is it free?
    - Does it require a key or permissions?
    - Do they impose a limit?



## Authentication and identification

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

## NASA API - Example of using an API key
To use the NASA API, you need an API key to identify yourself, but no additional authentication is required beyond this.

Let's try working with the NASA API now. To do this, first we need to generate
our API key by providing our details at [the API home page][nasa-api]. Once that
is done, NASA provide the API key instantly, and send a copy to the email
address you provide. 


Let's construct an API query to try, querying the Astronomy Picture of the Day (APOD).
The first step is to read and understand the API documentation. They also provide example queries in which you can see that NASA
expects the API key to be encoded as a query parameter.

~~~
$ curl -i "https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY"
~~~
{: .language-bash}

~~~
HTTP/2 200 
date: Thu, 08 Feb 2024 02:39:35 GMT
content-type: application/json
content-length: 1265
vary: Accept-Encoding
access-control-allow-origin: *
access-control-expose-headers: X-RateLimit-Limit, X-RateLimit-Remaining
age: 1
strict-transport-security: max-age=31536000; includeSubDomains; preload
vary: Accept-Encoding
via: https/1.1 api-umbrella (ApacheTrafficServer [cMsSf ])
x-api-umbrella-request-id: cdgqarbvpj8g36vie310
x-cache: MISS
x-ratelimit-limit: 40
x-ratelimit-remaining: 39
x-vcap-request-id: 029df11c-5e1d-4b6b-6389-b62bf72c9566
x-frame-options: DENY
x-content-type-options: nosniff
x-xss-protection: 1; mode=block

{"copyright":"\nKent E. Biggs\n","date":"2024-02-07","explanation":"Are these two galaxies really attracted to each other? Yes, gravitationally, and the result appears as an enormous iconic heart -- at least for now. Pictured is the pair of galaxies cataloged as NGC 4038 and NGC 4039,known as the Antennae Galaxies.  Because they are only 60 million light years away, close by intergalactic standards, the pair is one of the best studied interacting galaxies on the night sky. Their strong attraction began about a billion years ago when they passed unusually close to each other.  As the two galaxies interact, their stars rarely collide, but new stars are formed when their interstellar gases crash together.  Some new stars have already formed, for example, in the long antennae seen extending out from the sides of the dancing duo. By the time the galaxy merger is complete, likely over a billion years from now, billions of new stars may have formed.   Open Science: Browse 3,300+ codes in the Astrophysics Source Code Library","hdurl":"https://apod.nasa.gov/apod/image/2402/Antennae_Biggs_3840.jpg","media_type":"image","service_version":"v1","title":"The Heart Shaped Antennae Galaxies","url":"https://apod.nasa.gov/apod/image/2402/Antennae_Biggs_960.jpg"}
~~~
{: .output}

We can see that this API gives us JSON output including links to two versions
of the picture of the day, and then metadata about the picture including its
title, description, and copyright. The headers also give us some information
about our API usage&mdash;our rate limit is 40 requests per day, and we have
39 of these remaining.

>## Let’s create a query 
>Run this query: https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=image
>1. What does the result show?
>2. Can you find the URL link for the media type the search returned?
>3. How would you structure a query to search for videos instead of images? (Hint: Check API doc https://api.nasa.gov/ [nasa-api])
>
>>## Solution
>>1. The result from the API query shows data for a specific image related to the Apollo moon missions. It includes metadata like the image’s title (“Apollo Footprint”), a description noting that Buzz Aldrin took the photo of a bootprint on the moon during the Apollo 11 mission. It also provides a link to the thumbnail of the image.
>>2. Yes, the URL link for the media type (an image) returned by the search is provided under the links array within the items. The specific link is: `https://images-assets.nasa.gov/image/PIA24439/PIA24439~thumb.jpg`
>>3. To modify the existing query to search for videos instead of images, you would need to change the media_type parameter from image to video. `https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=video&page_size=1`
>{: .solution}
{: .challenge}

[nasa-api]: https://api.nasa.gov