# Guides

**How to Authenticate Using API Keys and Bearer Tokens**

To authenticate with the Luoda API, include your API key in the `XApiKey` header and the user's bearer token in the `Authorization` header of your HTTP requests. You can obtain the bearer token by authenticating the user through Clerk.

**Managing Projects**

To create a new project, send a POST request to `/api/v1/projects` with the project details in the request body. Ensure the request includes both the API key and the bearer token.

Example:
```json
{
    "name": "New Project",
    "description": "Description of the new project"
}
```

**Working with Vocal Blocks**

To retrieve all vocal blocks for a project, send a GET request to `/api/v1/projects/{projectId}/vocalblocks`. Ensure the request includes both the API key and the bearer token.