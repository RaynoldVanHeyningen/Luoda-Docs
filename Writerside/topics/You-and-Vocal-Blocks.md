# You and Vocal Blocks

## Overview

This guide provides a detailed explanation of how to work with Vocal Blocks in the API. Vocal Blocks are sections of a project that can contain lyrics, categories, and other information. This guide will cover the following topics:

1. **Prerequisites**
2. **Understanding Vocal Blocks in Luoda**
3. **Creating a Vocal Block**
4. **Retrieving Vocal Blocks**
5. **Updating Vocal Blocks**
6. **Deleting Vocal Blocks**

## Prerequisites

Before you can start working with Vocal Blocks, ensure you have the following:

- An API key to authenticate your requests.
- A valid Bearer token for user authentication.
- The project ID where the Vocal Blocks will be managed.

## 1. Understanding Vocal Blocks in Luoda

In Luoda, Vocal Blocks represent distinct sections of a song, such as verses, choruses, bridges, etc. Each Vocal Block can contain lyrics and is categorized to indicate its role within the song.

When a Vocal Block is created, an event is triggered in Azure. This event is monitored by a dedicated service that generates an AI vocal for the Vocal Block. Once the AI vocal is generated, it is stored in an external storage, and the Vocal Block record in Luoda is updated with the status and the URL of the generated vocal.

### Workflow

1. **Creating a Vocal Block**: When a new Vocal Block is created, an event is sent to an Azure event grid.
2. **Event Processing**: A background service listens for these events and processes each one to generate an AI vocal.
3. **AI Vocal Generation**: The service generates the AI vocal and stores the resulting file in a storage service.
4. **Updating Status**: The service updates the Vocal Block record in Luoda with the status and the URL of the generated vocal.
5. **Polling for Updates**: Consumers can poll the API to check the status of their Vocal Blocks until the process is complete and the URL is available.

### Polling for Updates

Consumers should periodically poll the API to check the status of their Vocal Blocks. The status field in the response will indicate whether the AI vocal generation is complete. Once the status is "Completed", the consumer can retrieve the URL of the generated vocal.

## 2. Creating a Vocal Block

### Endpoint {id="endpoint_1"}

```
POST /api/v1/projects/{projectId}/vocalblocks
```

### Sample Request {id="sample-request_1"}

```json
{
  "name": "Chorus",
  "description": "This is the chorus of the song.",
  "category": "Chorus",
  "lyrics": "These are the lyrics of the chorus."
}
```

### Example in .NET {id="example-in-net_1"}

```C#
var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_BEARER_TOKEN");
client.DefaultRequestHeaders.Add("XApiKey", "YOUR_API_KEY");

var content = new StringContent(JsonConvert.SerializeObject(new
{
    name = "Chorus",
    description = "This is the chorus of the song.",
    category = "Chorus",
    lyrics = "These are the lyrics of the chorus."
}), Encoding.UTF8, "application/json");

var response = await client.PostAsync("https://yourapiurl/api/v1/projects/1/vocalblocks", content);
response.EnsureSuccessStatusCode();
```

### Expected Response {id="expected-response_1"}

```json
{
  "id": 1,
  "name": "Chorus",
  "description": "This is the chorus of the song.",
  "category": "Chorus",
  "lyrics": "These are the lyrics of the chorus."
}
```

## 3. Retrieving Vocal Blocks

### Retrieve All Vocal Blocks

#### Endpoint {id="endpoint_2"}

```
GET /api/v1/projects/{projectId}/vocalblocks
```

#### Example in .NET {id="example-in-net_2"}

```C#
var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_BEARER_TOKEN");
client.DefaultRequestHeaders.Add("XApiKey", "YOUR_API_KEY");

var response = await client.GetAsync("https://yourapiurl/api/v1/projects/1/vocalblocks");
response.EnsureSuccessStatusCode();

var content = await response.Content.ReadAsStringAsync();
var vocalBlocks = JsonConvert.DeserializeObject<List<VocalBlockDto>>(content);
```

### Retrieve Specific Vocal Block

#### Endpoint {id="endpoint_3"}

```
GET /api/v1/projects/{projectId}/vocalblocks/{vocalBlockId}
```

#### Example in .NET {id="example-in-net_3"}

```C#
var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_BEARER_TOKEN");
client.DefaultRequestHeaders.Add("XApiKey", "YOUR_API_KEY");

var response = await client.GetAsync("https://yourapiurl/api/v1/projects/1/vocalblocks/1");
response.EnsureSuccessStatusCode();

var content = await response.Content.ReadAsStringAsync();
var vocalBlock = JsonConvert.DeserializeObject<VocalBlockDto>(content);
```

## 4. Updating Vocal Blocks

### Full Update

#### Endpoint {id="endpoint_4"}

```
PUT /api/v1/projects/{projectId}/vocalblocks/{vocalBlockId}
```

### Sample Request {id="sample-request_4"}

```json
{
  "name": "Updated Chorus",
  "description": "This is the updated chorus of the song.",
  "category": "Chorus",
  "lyrics": "These are the updated lyrics of the chorus."
}
```

### Example in .NET {id="example-in-net_4"}

```C#
var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_BEARER_TOKEN");
client.DefaultRequestHeaders.Add("XApiKey", "YOUR_API_KEY");

var content = new StringContent(JsonConvert.SerializeObject(new
{
    name = "Updated Chorus",
    description = "This is the updated chorus of the song.",
    category = "Chorus",
    lyrics = "These are the updated lyrics of the chorus."
}), Encoding.UTF8, "application/json");

var response = await client.PutAsync("https://yourapiurl/api/v1/projects/1/vocalblocks/1", content);
response.EnsureSuccessStatusCode();
```

### Partial Update

#### Endpoint {id="endpoint_5"}

```
PATCH /api/v1/projects/{projectId}/vocalblocks/{vocalBlockId}
```

### Sample Request {id="sample-request_5"}

```json
[
  {
    "op": "replace",
    "path": "/name",
    "value": "Updated Chorus Name"
  }
]
```

### Example in .NET {id="example-in-net_5"}

```C#
var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_BEARER_TOKEN");
client.DefaultRequestHeaders.Add("XApiKey", "YOUR_API_KEY");

var content = new StringContent(JsonConvert.SerializeObject(new[]
{
    new
    {
        op = "replace",
        path = "/name",
        value = "Updated Chorus Name"
    }
}), Encoding.UTF8, "application/json-patch+json");

var response = await client.PatchAsync("https://yourapiurl/api/v1/projects/1/vocalblocks/1", content);
response.EnsureSuccessStatusCode();
```

## 5. Deleting Vocal Blocks

### Endpoint {id="endpoint_6"}

```
DELETE /api/v1/projects/{projectId}/vocalblocks/{vocalBlockId}
```

### Example in .NET {id="example-in-net_6"}

```C#
var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_BEARER_TOKEN");
client.DefaultRequestHeaders.Add("XApiKey", "YOUR_API_KEY");

var response = await client.DeleteAsync("https://yourapiurl/api/v1/projects/1/vocalblocks/1");
response.EnsureSuccessStatusCode();
```

### Expected Response {id="expected-response_6"}

```
204 No Content
```

## Conclusion

This guide covered how to create, retrieve, update, and delete vocal blocks in the API. Additionally, it explained the process of AI vocal generation triggered by creating a Vocal Block and how consumers should poll the API for status updates. By following these steps, you can manage vocal blocks efficiently within your projects. Remember to always authenticate your requests with a valid Bearer token and API key.

---