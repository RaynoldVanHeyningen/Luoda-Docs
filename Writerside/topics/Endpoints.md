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
    "id": "59065bc4-b318-4955-8dfc-c8ed5705b229",
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
  "id": "59065bc4-b318-4955-8dfc-c8ed5705b229",
  "name": "Project Test",
  "description": "Famous project.",
  "numberOfVocalBlocks": 0,
  "vocalBlocks": [],
  "bpm": 0,
  "musicalKey": null
}
```