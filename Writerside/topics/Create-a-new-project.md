# Create a new project

## Overview

In this guide, we'll walk you through the steps to create a new project using the Luoda API. This guide will cover the following sections:

1. **Introduction**
    - Brief overview of the Luoda API project creation endpoint.

2. **Prerequisites**
    - What you need before you start (e.g., API key, authenticated user token, necessary SDKs).

3. **Endpoint Details**
    - Detailed information about the endpoint, including URL, method, headers, and body.

4. **Implementation**
    - Detailed code examples for creating a new project in various programming languages:
        - .NET
        - JavaScript
        - Python
        - PHP

5. **Error Handling and Best Practices**
    - Handling common errors and best practices for creating projects.

6. **Testing Your Setup**
    - Steps to test your project creation setup and ensure it is working correctly.

7. **Conclusion**
    - Summary and additional resources.

---

### 1. Introduction

The Luoda API allows you to create and manage projects programmatically. This guide will walk you through the steps to create a new project using the API. We'll provide code examples in various programming languages to help you get started quickly.

### 2. Prerequisites

Before you begin, ensure you have the following:

- **API Key:** An API key is required for all requests to the Luoda API.
- **Authenticated User Token:** A bearer token for an authenticated user is needed.
- **SDKs:** Necessary SDKs for your chosen programming language.

### 3. Endpoint Details

- **URL:** `https://api.luoda.com/api/v1/projects`
- **Method:** `POST`
- **Headers:**
    - `Authorization: Bearer {user_token}`
    - `XApiKey: {api_key}`
    - `Content-Type: application/json`
- **Body:**
  ```json
  {
    "name": "Project Name",
    "description": "Project Description"
  }
  ```

### 4. Implementation

<tabs group="language">
<tab id=".NET" title=".NET" group-key=".net">

```C#
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;

public async Task CreateProjectAsync(string apiUrl, string apiKey, string userToken, string projectName, string projectDescription)
{
    using var httpClient = new HttpClient();

    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", userToken);
    httpClient.DefaultRequestHeaders.Add("XApiKey", apiKey);

    var project = new
    {
        name = projectName,
        description = projectDescription
    };

    var json = JsonConvert.SerializeObject(project);
    var content = new StringContent(json, Encoding.UTF8, "application/json");

    var response = await httpClient.PostAsync($"{apiUrl}/api/v1/projects", content);

    if (response.IsSuccessStatusCode)
    {
        Console.WriteLine("Project created successfully.");
    }
    else
    {
        Console.WriteLine($"Error creating project: {response.StatusCode}");
    }
}
```

</tab>
<tab id="JS" title="Javascript" group-key="js">

```javascript
const axios = require('axios');

async function createProject(apiUrl, apiKey, userToken, projectName, projectDescription) {
    try {
        const response = await axios.post(`${apiUrl}/api/v1/projects`, {
            name: projectName,
            description: projectDescription
        }, {
            headers: {
                'Authorization': `Bearer ${userToken}`,
                'XApiKey': apiKey,
                'Content-Type': 'application/json'
            }
        });

        console.log('Project created successfully:', response.data);
    } catch (error) {
        console.error('Error creating project:', error.response.data);
    }
}
```

</tab>
<tab id="Python" title="Python" group-key="python">

```python
import requests
import json

def create_project(api_url, api_key, user_token, project_name, project_description):
    headers = {
        'Authorization': f'Bearer {user_token}',
        'XApiKey': api_key,
        'Content-Type': 'application/json'
    }

    data = {
        'name': project_name,
        'description': project_description
    }

    response = requests.post(f'{api_url}/api/v1/projects', headers=headers, data=json.dumps(data))

    if response.status_code == 201:
        print('Project created successfully.')
    else:
        print(f'Error creating project: {response.status_code}')

# Usage
create_project('https://api.luoda.com', 'your_api_key', 'your_user_token', 'Project Name', 'Project Description')
```

</tab>
<tab id="PHP" title="PHP" group-key="php">

```php
<?php

function createProject($apiUrl, $apiKey, $userToken, $projectName, $projectDescription) {
    $url = $apiUrl . '/api/v1/projects';
    $data = array(
        'name' => $projectName,
        'description' => $projectDescription
    );

    $options = array(
        'http' => array(
            'header'  => array(
                "Authorization: Bearer $userToken",
                "XApiKey: $apiKey",
                "Content-Type: application/json"
            ),
            'method'  => 'POST',
            'content' => json_encode($data),
        ),
    );

    $context  = stream_context_create($options);
    $result = file_get_contents($url, false, $context);

    if ($result === FALSE) {
        echo 'Error creating project';
    } else {
        echo 'Project created successfully';
    }
}

// Usage
createProject('https://api.luoda.com', 'your_api_key', 'your_user_token', 'Project Name', 'Project Description');
?>
```

</tab>
</tabs>

### 5. Error Handling and Best Practices

When making API requests, it's essential to handle errors gracefully. Here are some common error scenarios and how to handle them:

- **Invalid API Key:** Ensure that the API key is valid and correctly included in the request headers.
- **Authentication Errors:** Ensure the bearer token is valid and not expired.
- **Validation Errors:** Ensure that the request body is correctly formatted and contains all required fields.

### 6. Testing Your Setup

After implementing the code, test your setup to ensure it works correctly. Make a few test requests to the API and verify that projects are being created successfully.

### 7. Conclusion

In this guide, we've walked through the steps to create a new project using the Luoda API. We've covered the endpoint details, provided code examples in various programming languages, and discussed error handling and best practices. For further assistance, refer to the Luoda API documentation or contact our support team.