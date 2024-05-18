# Examples

**Sample Request to Retrieve All Projects**

```http
GET /api/v1/projects HTTP/1.1
Host: api.luoda.com
XApiKey: your_api_key
Authorization: Bearer your_bearer_token
```

**Sample Response**

```json
[
    {
        "id": 1,
        "name": "Project 1",
        "description": "Description of Project 1"
    }
]
```

**Sample Request to Create a Project**

```http
POST /api/v1/projects HTTP/1.1
Host: api.luoda.com
XApiKey: your_api_key
Authorization: Bearer your_bearer_token
Content-Type: application/json

{
    "name": "New Project",
    "description": "Description of the new project"
}
```

**Sample Response**

```json
{
    "id": 2,
    "name": "New Project",
    "description": "Description of the new project"
}
```