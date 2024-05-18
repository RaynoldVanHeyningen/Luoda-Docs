# Getting Started

**Authentication and Authorization**

The Luoda API uses two layers of authentication:
1. **API Key Authentication**: You need to include your API key in the `XApiKey` header for every request.
2. **Bearer Token Authentication**: Each request must also include a bearer token in the `Authorization` header. This token is obtained from Clerk after authenticating the user.

**Setting Up Your Environment**

1. **Install Dependencies:**
   Ensure you have the necessary tools and libraries installed.

   ```bash
   dotnet add package Newtonsoft.Json
   dotnet add package Microsoft.Extensions.Configuration
   dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
   ```

2. **Configure Your API Key and Bearer Token:**
   Set your API key in your configuration file or environment variables.

3. **User Authentication Using Clerk:**
   To authenticate users, you will need to use Clerk. Our Clerk environment runs on `https://probable-caiman-75.clerk.accounts.dev`. You may want to include this URL directly in your documentation or provide it to consumers upon approval.

**Authenticating Users with Clerk**

To authenticate a user, direct them to your Clerk authentication page and obtain the user's access token. Include this access token in all API requests as a bearer token.

**Example Authentication Configuration:**

```C#
var clerkDomain = builder.Configuration["Authentication:Clerk:Domain"];
var audience = builder.Configuration["Authentication:Clerk:Audience"];
var jwksUri = $"{clerkDomain}{builder.Configuration["Authentication:Clerk:JWKSUri"]}";

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(o =>
    {
        o.Authority = clerkDomain;
        o.Audience = audience;
        o.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidIssuer = clerkDomain,
            ValidateAudience = false,
            ValidAudience = audience,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            IssuerSigningKeyResolver = (token, securityToken, kid, validationParameters) =>
            {
                // Retrieve JWKS keys from Clerk
                var client = new HttpClient();
                var result = client.GetAsync(jwksUri).Result;
                var keys = new JsonWebKeySet(result.Content.ReadAsStringAsync().Result);

                return keys.GetSigningKeys();
            }
        };
    });
```

**Making Your First API Call**

Here’s an example of how to retrieve all projects using the Luoda API:

```C#
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

public class Program
{
    private static async Task Main(string[] args)
    {
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("XApiKey", "your_api_key");
        client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", "your_bearer_token");

        var response = await client.GetAsync("https://api.luoda.com/api/v1/projects");
        var content = await response.Content.ReadAsStringAsync();

        Console.WriteLine(content);
    }
}
```