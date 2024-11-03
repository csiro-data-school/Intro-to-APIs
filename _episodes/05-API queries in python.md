---
title: "API queries in python"
teaching: 15
exercises: 5
questions:
- "What python libraries can we use to make API requests?"
- "How do we store and access API keys securely?"
- "How do we use the Python libraries to make an API request?"
objectives:
- "Understand the purpose of storing API keys using environmental variables."
- "Understand requests library in Python."
- "Understand the json library in Python."
keypoints:
- "The requests and json libraries in Python are important tools to make API calls."
- "Environmental variables are a secure way to store and access API keys from."
- "Some services offer a demo key for testing with limited functionality."
---

# Making API requests in Python
## Essential Libraries

- **`requests`**:
The requests library in Python is a popular and user-friendly HTTP library that simplifies the process of making HTTP requests. It abstracts away much of the complexity involved in sending HTTP requests and handling responses.
The requests.get function is used to send a GET request to a specified URL.

### Example of an HTTP request
Let's make the same request we observed in shell
```python
import requests

# URL you want to make the request to
url = "https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=image&page_size=1"

# Making the GET request
response = requests.get(url)

# Checking if the request was successful
if response.status_code == 200:
    # The request was successful; you can now process the response
    print("Request successful!")
    # Printing the content of the response; assuming it's text-based like HTML or JSON
    print(response.text)
else:
    # The request failed
    print(f"Request failed with status code: {response.status_code}")

```

- **`json`**:
One of the primary uses of the `json` module is to parse JSON data received from an API into Python dictionaries, allowing for easy access and manipulation of the data within a Python program. JSON is easy for humans to read and write and easy for machines to parse and generate.

- `json.loads()`: This function is used to parse a JSON string, converting it into a Python dictionary. `loads` stands for "load string."

- `json.dumps()`: The `json.dumps()` function provides parameters like `indent` and `sort_keys` to format the JSON output, making it more readable. This function takes a Python object (like a dictionary) and returns a JSON string.

#### Example of Parsing and Formatting JSON:

```python
import requests
import json

# URL you want to make the request to
url = "https://images-api.nasa.gov/search?q=apollo&description=moon&media_type=image&page_size=1"

# Making the GET request
response = requests.get(url)

# Checking if the request was successful
if response.status_code == 200:
    # The request was successful; you can now process the response
    print("Request successful!")
    # Printing the content of the response; assuming it's text-based like HTML or JSON
    print(response.text)
else:
    # The request failed
    print(f"Request failed with status code: {response.status_code}")

data_dict = json.loads(response.text)
print(data_dict) 

json_data = json.dumps(data_dict,indent=4, sort_keys=True)

print(json_data) 
```

### Structuring the HTTP request in Python
 To structure the HTTP request in a more organized way, especially for complex APIs, you can break down the request into components such as URL, parameters, and headers. This approach enhances readability and maintainability, particularly when dealing with APIs that require authentication, accept various parameters, or depend on specific header values.

This structured approach is particularly useful in scenarios where you might need to modify only one aspect of the request, such as changing a parameter or adding an authentication token to the headers. It keeps the request flexible and your code clean.

```python
import requests

# Base URL of the API endpoint
base_url = "https://images-api.nasa.gov/search"

# Parameters to be sent with the request
params = {
    'q': 'apollo',
    'description': 'moon landing',
    'media_type': 'image',
    'page_size': '1'
}

# Headers for the request
headers = {
    'Accept': 'application/json'  # Assuming you expect a JSON response
    # 'Authorization': 'Bearer YOUR_API_KEY'  # Uncomment and replace if authentication is required
}

# Making the GET request
response = requests.get(base_url, params=params, headers=headers)

# Checking the response status and handling the data
if response.status_code == 200:
    # Request was successful, process the data
    data = response.json()  # Parsing the response as JSON
    print("Data received from the API:")
    print(data)
else:
    # Handling request errors
    print(f"Request failed with status code: {response.status_code}")
```


# Making API requests that require an API key
## Storing API keys

Using environment variables to store API keys is a safer alternative that keeps sensitive information like API keys out of your source code, making your application more secure. 

Let's see how to do this by querying the Astronomy Picture of the Day (APOD) endpoint of th NASA API: https://api.nasa.gov

### Setting the Environment Variable

First, you'll need to set the environment variable in your operating system.

#### On Windows:

1. Open the Start Search, type in "env", and choose "Edit the system environment variables."
2. In the System Properties window, click on the "Environment Variables…" button.
3. In the Environment Variables window, click on the "New…" button under the "User variables" section.
4. Set the variable name as `NASA_API_KEY` and its value to your actual NASA API key.

#### On macOS/Linux:

1. Open your terminal.
2. Use the `export` command to set an environment variable. Add this line to your `~/.bashrc`, `~/.zshrc`, or `~/.profile` file to make it persistent across sessions:

   ```bash
   export NASA_API_KEY='your_nasa_api_key_here'
   ```

3. After adding the line, run `source ~/.bashrc`, `source ~/.zshrc`, or `source ~/.profile` to reload the configuration and apply the changes.

### Accessing the Environment Variable in Python

Now, in your Python script, use the `os` module to access the environment variable:

```python
import requests
import os

base_url = "https://api.nasa.gov"
endpoint = f"{base_url}/planetary/apod/"
headers = {"accept": "application/json"}

# Access the API key from environment variables
api_key = os.getenv("NASA_API_KEY")

if not api_key:
    raise ValueError("No API Key found. Please set the NASA_API_KEY environment variable.")

params = {"api_key": api_key, "count": 2}

response = requests.get(endpoint, params=params, headers=headers)
print(response.json())
```

# Handling Environment Variables in Kubeflow
The Kubeflow environment behaves differently from typical terminal environments on Windows, Mac, or Linux machines. While it does offer a bash terminal option, setting and accessing environment variables can be more complex. This can be an issue, especially when trying to securely manage sensitive information like API keys.

To avoid hardcoding your API key in your scripts, you can use Python’s getpass module to securely input it at runtime. Here’s an example:
```python
import getpass
api_key = getpass.getpass(prompt="Enter your API key: ")
```
This approach prompts the user for their API key and stores it in the api_key variable without displaying it on the screen, enhancing security and flexibility.

>## Let’s retrieve some photos of Mars!
> Use the NASA API to retrieve pictures from the curiosity rover in Mars
>>## Solution
>>```python
>>import requests
>>
>># Uncomment the line below if you want to use the default DEMO_KEY
>>api_key = "DEMO_KEY"
>>
>># Uncomment the line below if you are storing your API key as an environment variable. Remember to add 'import os' in the code
>>#api_key = os.getenv("NASA_API_KEY")
>>
>># The API key is stored in api_key if you have used the getpass method
>>
>># The Martian solar day (sol) you're interested in
>>sol = "1000"
>>
>># The base URL for the NASA Mars Rover Photos API
>>base_url = "https://api.nasa.gov/mars-photos/api/v1/"
>>
>># The endpoint for retrieving photos from the Curiosity rover
>>endpoint = f"{base_url}/rovers/curiosity/photos"
>>
>># The parameters for the request, including the sol and your API key
>>parameters = {
>>    "sol": sol,
>>    "api_key": api_key
>>}
>># Make the GET request to the NASA API, passing the parameters separately
>>response = requests.get(endpoint, params=parameters)
>>
>># Check if the response status code is 200 (OK)
>>if response.status_code == 200:
>>    # Parse the JSON response to get the photos
>>    photos = response.json()['photos']
>>    # Iterate through the photo entries and print the URL of each image
>>    for photo in photos:
>>        print(photo['img_src'])
>>else:
>>    # Handle errors if the request was not successful
>>    print("Failed to retrieve photos. Status Code:", response.status_code)
>>```
>{: .solution}
{: .challenge}

>## Let’s retrieve some information on asteroids
> Use the NASA API to check which potentially hazardous asteroids are approaching Earth in March 2024
>>## Solution
>>```python
>>import requests
>>import json
>>
>># Replace DEMO_KEY with your actual API key
>>api_key = "DEMO_KEY"
>>
>># Set the date range you're interested in
>>start_date = "2024-03-01"
>>end_date = "2024-03-07"
>>
>># The base URL for the NEO feed
>>url = f"https://api.nasa.gov/neo/rest/v1/feed?start_date={start_date}&end_date={end_date}&api_key={api_key}"
>>
>># Make a GET request to the NASA API
>>response = requests.get(url)
>>
>># Check if the request was successful
>>if response.status_code == 200:
>>    # Parse the JSON response
>>    data = response.json()
>>    
>>    # Iterate through the asteroid data
>>    for date in data["near_earth_objects"]:
>>        for asteroid in data["near_earth_objects"][date]:
>>            if asteroid["is_potentially_hazardous_asteroid"]:
>>                # Print details about potentially hazardous asteroids
>>                print(f"Asteroid {asteroid['name']} (ID: {asteroid['id']})")
>>                print(f"Potentially Hazardous: {asteroid['is_potentially_hazardous_asteroid']}")
>>                print(f"Close Approach Date: {asteroid['close_approach_data'][0]['close_approach_date']}")
>>                print(f"Miss Distance (kilometers): {asteroid['close_approach_data'][0]['miss_distance']['kilometers']}")
>>                print(f"Velocity (km/s): {asteroid['close_approach_data'][0]['relative_velocity']['kilometers_per_second']}\n")
>>else:
>>    print("Failed to retrieve data:", response.status_code)
>>
>>```
>{: .solution}
{: .challenge}

