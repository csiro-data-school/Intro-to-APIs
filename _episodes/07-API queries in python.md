---
title: "API queries in python"
teaching: 15
exercises: 5
questions:
- "What is the difference between authentication and authorization?"
- "What are API keys?"
- "How do we obtain an API key?"
objectives:
- "Understand the purpose of authentication and authorisation."
- "Understand the different methods of authentication."
- "Be able to obtain and use an API key to access data."
keypoints:
- "Authentication ensures that only authorised entities can access the API."
- "Authorisation ensures they can only access what they're permitted to."
---


### Essential Libraries

- **`requests`**:
The requests library in Python is a popular and user-friendly HTTP library that simplifies the process of making HTTP requests. It abstracts away much of the complexity involved in sending HTTP requests and handling responses.
The requests.get function is used to send a GET request to a specified URL.

#### Example of an HTTP request
Let's make the same request we observed in shell
```python
import requests

# URL you want to make the request to
url = "https://libapp.library.yale.edu/VoySearch/GetBibItem?isxn=9780415704953"

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

 To structure the HTTP request in a more organized way, especially for complex APIs, you can break down the request into components such as URL, parameters, and headers. This approach enhances readability and maintainability, particularly when dealing with APIs that require authentication, accept various parameters, or depend on specific header values.

### 1. Base URL
The base URL is the root address of the API endpoint you're accessing.

```python
base_url = "https://libapp.library.yale.edu/VoySearch/GetBibItem"
```

### 2. Parameters
Parameters are often used in GET requests to filter data, specify queries, etc. They are appended to the URL as a query string.

```python
params = {
    'isxn': '9780415704953'
}
```

### 3. Headers
Headers can include metadata such as content type, authentication tokens, and other information about the request or the desired response.

```python
headers = {
    'Accept': 'application/json',  # Assuming the API returns JSON data
    # 'Authorization': 'Bearer YOUR_API_KEY',  # Uncomment if you need authentication
}
```

### 4. Making the Request
With the URL, parameters, and headers defined, you can make the request using the `requests` library. The `params` and `headers` arguments in the `requests.get()` method allow you to pass the parameters and headers defined earlier.

```python
import requests

response = requests.get(base_url, params=params, headers=headers)
```

### 5. Checking the Response and Handling Data
After making the request, check the response status and handle the data accordingly.

```python
if response.status_code == 200:
    # The request was successful; handle the data
    data = response.json()  # Parse JSON response
    print(data)
else:
    # Handle errors (e.g., display a message or log the error)
    print(f"Request failed with status code: {response.status_code}")
```

### Example Use Case
This structured approach is particularly useful in scenarios where you might need to modify only one aspect of the request, such as changing a parameter or adding an authentication token to the headers. It keeps the request flexible and your code clean.

```python
import requests

# Base URL of the API endpoint
base_url = "https://libapp.library.yale.edu/VoySearch/GetBibItem"

# Parameters to be sent with the request
params = {
    'isxn': '9780415704953'
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

- **`json`**:
One of the primary uses of the `json` module is to parse JSON data received from an API into Python dictionaries, allowing for easy access and manipulation of the data within a Python program. JSON is easy for humans to read and write and easy for machines to parse and generate.

- `json.loads()`: This function is used to parse a JSON string, converting it into a Python dictionary. `loads` stands for "load string."

#### Example of Parsing JSON:

```python
import json

# Sample JSON data received from an API
json_data = '{"name": "John", "age": 30, "city": "New York"}'

# Parse JSON data into a Python dictionary
data_dict = json.loads(json_data)

print(data_dict) 
```

### Formatting the Output

- **`json.dumps`**:
The `json.dumps()` function provides parameters like `indent` and `sort_keys` to format the JSON output, making it more readable. This function takes a Python object (like a dictionary) and returns a JSON string.


#### Example of Converting to JSON:

```python
import json

# A Python dictionary
data_dict = {
    "name": "Jane",
    "age": 25,
    "city": "Los Angeles"
}

# Convert the Python dictionary to a JSON string

json_data = json.dumps(data_dict,indent=4, sort_keys=True)

print(json_data) 
```
## API keys

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

### Step 2: Accessing the Environment Variable in Python

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
