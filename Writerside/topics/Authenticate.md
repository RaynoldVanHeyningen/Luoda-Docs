# Authenticate with Luoda

## Introduction

### Purpose of the Guide
This guide aims to provide a comprehensive understanding of how to implement and manage authentication for the Luoda API. Whether you are developing in .NET, JavaScript, or PHP, this guide will offer step-by-step instructions and examples to help you secure your API endpoints effectively.

### Importance of Authentication
Authentication is a crucial aspect of API security, ensuring that only authorized users can access the resources and perform actions. It helps protect sensitive data and maintain the integrity of the application.

### Overview of Authentication Methods
The Luoda API requires a `XApiKey` header to be present, with the supplied API key. Next token-based authentication using JWT (JSON Web Tokens) is required. This method allows clients to securely authenticate users and maintain session state without requiring server-side storage of session data.

---

## Getting Started

### Prerequisites
Before you begin, ensure you have the following:
- Basic understanding of API authentication
- A development environment set up for your preferred programming language (.NET, JavaScript, PHP, etc.)

### Setting Up User Login Flow with Clerk
To authenticate users, you need to set up a user login flow with Clerk. This allows users to log in to your application using Clerk, after which you can obtain their bearer access token.

1. **Integrate Clerk into Your Application**:
    - Follow the Clerk integration guide for your specific platform (e.g., .NET, JavaScript, PHP).
    - Set up the login and registration flows as per Clerk's documentation.

2. **Obtain Bearer Access Token**:
    - After users log in, obtain the bearer access token from Clerk. This token will be used to authenticate API requests.

### Obtaining API Credentials (XApiKey)
Every request to the Luoda API must include the `XApiKey` header. To obtain your API key:

1. **Register for an API Key**:
    - Contact Luoda support or use the Luoda developer portal to request an API key.

2. **Store API Key Securely**:
    - Ensure that you store your API key securely and do not expose it in client-side code.

---

Great! Let's move on to the next section: **Authentication Flow**.

---

## Authentication Flow

In this section, we will cover the overall authentication process, providing a step-by-step guide and a flow diagram to help you understand how to authenticate users with the Luoda API.

### Overview of the Authentication Process

The authentication process involves the following steps:

1. **User Login**:
    - The user logs in to your application using Clerk.
    - Clerk handles the authentication and provides a bearer access token upon successful login.

2. **Obtain Bearer Token**:
    - Retrieve the bearer access token from Clerk.
    - This token will be used to authenticate API requests.

3. **API Request with Bearer Token and API Key**:
    - Make a request to the Luoda API.
    - Include the bearer token in the `Authorization` header and the API key in the `XApiKey` header.

4. **Token Validation**:
    - The Luoda API validates the bearer token and the API key.
    - If valid, the API processes the request and returns the appropriate response.

5. **Handle API Response**:
    - Process the response from the API in your application.
    - Handle any errors or required actions based on the response.

### Step-by-Step Flow Diagram

Below is a flow diagram illustrating the authentication process:

```
+---------------------------+
|      User Login Flow      |
+---------------------------+
           |
           v
+---------------------------+
| User logs in via Clerk    |
|                           |
| - User enters credentials |
| - Clerk authenticates user|
+---------------------------+
           |
           v
+---------------------------+
|    Obtain Bearer Token    |
+---------------------------+
           |
           v
+---------------------------+
|        API Request        |
+---------------------------+
| - Include Bearer Token in |
|   Authorization header    |
| - Include API Key in      |
|   XApiKey header          |
+---------------------------+
           |
           v
+---------------------------+
| Token Validation          |
+---------------------------+
| - API validates Bearer    |
|   Token and API Key       |
+---------------------------+
           |
           v
+---------------------------+
| Handle API Response       |
+---------------------------+
| - Process API response    |
| - Handle errors           |
+---------------------------+
```

### Detailed Steps

1. **User Login**:
    - Integrate Clerk's authentication flow into your application.
    - Upon successful login, Clerk provides a bearer access token.

2. **Obtain Bearer Token**:
    - Retrieve the bearer access token from Clerk after user login.
    - Store the token securely for use in API requests.

3. **API Request with Bearer Token and API Key**:
    - For every API request, include the bearer token in the `Authorization` header.
    - Also include your API key in the `XApiKey` header.
    - Example request headers:
      ```http
      Authorization: Bearer {BearerToken}
      XApiKey: {YourAPIKey}
      ```

4. **Token Validation**:
    - The Luoda API validates the provided bearer token and API key.
    - If the token and key are valid, the API processes the request.

5. **Handle API Response**:
    - Process the API response in your application.
    - Handle any errors or required actions based on the response.

---

Great! Let's move on to the implementation section where we'll provide detailed code examples for different programming languages to show how to implement the authentication flow.

## Implementation

In this section, we will provide code examples for different programming languages to demonstrate how to implement the authentication flow described earlier. We'll start with .NET, and then move on to JavaScript, Python, and PHP.

---

<tabs group="language">
<tab id=".NET-authenticate" title=".NET" group-key=".net">

```C#
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;
    
    public class ApiService
    {
        private readonly HttpClient _httpClient;
        private readonly string _apiKey;
    
        public ApiService(HttpClient httpClient, string apiKey)
        {
            _httpClient = httpClient;
            _apiKey = apiKey;
        }
    
        public async Task<string> GetProtectedResource(string bearerToken)
        {
            _httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
            _httpClient.DefaultRequestHeaders.Add("XApiKey", _apiKey);
    
            var response = await _httpClient.GetAsync("https://api.yourdomain.com/protected-endpoint");
    
            response.EnsureSuccessStatusCode();
    
            return await response.Content.ReadAsStringAsync();
        }
    }
    
    // Usage example
    public class Program
    {
        public static async Task Main(string[] args)
        {
            var apiKey = "YourAPIKey";
            var bearerToken = "BearerTokenFromClerk";
    
            var httpClient = new HttpClient();
            var apiService = new ApiService(httpClient, apiKey);
    
            var result = await apiService.GetProtectedResource(bearerToken);
    
            Console.WriteLine(result);
        }
    }
```

</tab>
<tab id="JS-authenticate" title="JS" group-key="js">

```javascript
    const axios = require('axios');
    
    async function getProtectedResource(apiKey, bearerToken) {
        try {
            const response = await axios.get('https://api.yourdomain.com/protected-endpoint', {
                headers: {
                    'Authorization': `Bearer ${bearerToken}`,
                    'XApiKey': apiKey
                }
            });
    
            console.log(response.data);
        } catch (error) {
            console.error('Error fetching protected resource:', error);
        }
    }
    
    // Usage example
    const apiKey = 'YourAPIKey';
    const bearerToken = 'BearerTokenFromClerk';
    
    getProtectedResource(apiKey, bearerToken);
```

</tab>
<tab id="Python-authenticate" title="Python" group-key="python">

```python
    import requests
    
    def get_protected_resource(api_key, bearer_token):
    headers = {
    'Authorization': f'Bearer {bearer_token}',
    'XApiKey': api_key
    }
    response = requests.get('https://api.yourdomain.com/protected-endpoint', headers=headers)
    
        if response.status_code == 200:
            print(response.json())
        else:
            print(f'Error: {response.status_code}, {response.text}')
    
    # Usage example
    api_key = 'YourAPIKey'
    bearer_token = 'BearerTokenFromClerk'
    
    get_protected_resource(api_key, bearer_token)
```

</tab>
<tab title="PHP">

```PHP
    <?php        
    function get_protected_resource($api_key, $bearer_token) {
        $curl = curl_init();
    
        curl_setopt_array($curl, array(
            CURLOPT_URL => "https://api.yourdomain.com/protected-endpoint",
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_HTTPHEADER => array(
                "Authorization: Bearer $bearer_token",
                "XApiKey: $api_key"
            ),
        ));
    
        $response = curl_exec($curl);
        $err = curl_error($curl);
    
        curl_close($curl);
    
        if ($err) {
            echo "cURL Error #:" . $err;
        } else {
            echo $response;
        }
    }
    
    // Usage example
    $api_key = 'YourAPIKey';
    $bearer_token = 'BearerTokenFromClerk';
    
    get_protected_resource($api_key, $bearer_token);            
    ?>
```

</tab>
</tabs>