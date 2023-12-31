# Metabase Overview

[Metabase](https://www.metabase.com/) is an open-source tool used for creating data dashboards and visualizations. It simplifies the process of exploring and understanding your data. By providing a user-friendly interface, it allows users to query data without needing extensive SQL knowledge. Metabase supports a variety of databases and has a robust API.


# [Metabase API: An Introduction](https://www.metabase.com/docs/latest/api-documentation)

Metabase is a powerful open-source tool for creating data dashboards and visualizations. It offers a RESTful API, allowing programmatic access and modification of many of its features.

To make use of this script, a local Metabase server needs to be set up. This process involves Docker, which needs to be installed and configured on your system.

## Getting Started 

Follow these steps to set up Metabase locally.

### Prerequisites

Before setting up Metabase locally, you need to have Docker installed on your system as we will be using it for the setup.

- If Docker is not already installed on your system, you can download it from the official Docker website. Here are the links:
	- [Docker for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)
    - [Docker for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
    - [Docker for Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
    - [Docker for other Linux distributions](https://docs.docker.com/engine/install/)

- After installing Docker, you can check whether it's been installed correctly by opening a terminal and running the following command:


```shell
docker --version
```

### Steps For Metabase Setup

1. **Pull the Metabase image from Docker Hub**
    - Open a terminal and run the following command: `docker pull metabase/metabase`

2. **Run the Metabase Docker container**
    - Run the command: `docker run -d -p 3000:3000 --name metabase metabase/metabase`

3. **Access Metabase**
    - Open your browser and visit `http://localhost:3000`
    - You will be greeted with the Metabase setup wizard. Follow the prompts to set up an account and connect your data sources.

4. **Create and Save a Native Question**
    - In Metabase, create a new 'question' (a query or report), and save it to your 'Personal Collection' or any other collection.

    **Ex:** Fetch users details for specific emails from `people` table of Sample Database 

    ```sql
    select 
	    email, password, name
	from people 
	where (
	    email in (
	        'borer-hudson@yahoo.com', 
	        'williamson-domenica@yahoo.com'
	    )
	    [[and email = {{email}}]] -- email parameter is optional
	)
    ```

 5. **Run Query**
    - Once you've run the query, you should see the results as shown in the image below

    ![Image description](images/metabase_native_question_example_1.png)

### Installing `metabase-api-python`

Before you begin, make sure you have Python3 and pip3 installed on your system. If not, refer to the official Python [installation guide](https://www.python.org/downloads/) and pip [installation guide](https://pip.pypa.io/en/stable/installation/).


Follow the steps below to install the `metabase-api-python` package:

> **NOTE:** We have not yet handled all error cases, and we have only added a few functions. More functions and scripts will be added over time. Please keep this in mind while using this module.


1. Open a terminal.

2. If you're using a virtual environment (which is a good practice), make sure to activate it. If you're not, that's perfectly fine too.

3. Use pip to install the `metabase-api-python` package from PyPi:

```shell
pip3 install metabase-api-python
```

4. Now, you can use the `metabase-api-python` package in your Python scripts. Here's a basic example:

```python
from metabase_api_python import MetabaseAPI

metabase_api = MetabaseAPI(
	base_url="http://localhost:3000", 
	user_name="YOUR_USERNAME", 
	password="YOUR_PASSWORD"
)

print("Access Token is : ", metabase_api.access_token)
```

Please replace `YOUR_USERNAME` and `YOUR_PASSWORD` with your actual Metabase credentials. 

Remember, always keep these credentials secure. Do not expose them in any places. 

Note: The installation process might vary slightly depending on your operating system and the specific configuration of your Python environment.


## Avalable Functions
- get_data_from_question
- archive_question
- delete_question

### **get_data_from_question**

**Ex:** Get the question data without parameters 
	
```python
from metabase_api_python import MetabaseAPI

	metabase_api = MetabaseAPI(
		base_url="http://localhost:3000", 
		user_name="xxxxxx@gmail.com", 
		password="xxxxxx"
	)

	# Get Question Reponse
	question_response = metabase_api.get_data_from_question(
		question_id=1 
	)
	print(question_response)
```

Note: Usually, the question_id for the first question you create in Metabase is 1. You can verify this by checking the URL when you navigate to the question in your local Metabase interface. For example, if your URL is http://localhost:3000/question/1-your-question-name, the question_id is 1.

**Ex:** Get the question data with parameters

```python
payload =  [
    {
        "type": "text",
        "value": "borer-hudson@yahoo.com",
        "target": ["variable", ["template-tag", "email"]]
        
    }
]

question_response = metabase_api.get_data_from_question(
	question_id=1,params=payload
)
print(question_response)
```

### **archive_question**

**Ex:** Archive Question

```python

# Archive Question
metabase_api.archive_question(
	question_id=1
)

```


