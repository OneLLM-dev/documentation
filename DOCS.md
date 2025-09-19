# OneLLM.dev API documentation
Welcome to the official OneLLM.dev API documentation! Here you will find:

## Table of Contents
- [Authentication](#authentication)  
- [API Endpoint](#api-endpoint)  
- [Request Body](#request-body)  
- [Example](#example)  
- [Supported Models](#models)  
- [Error Handling](#error-handling)  
- [Important Notes](#important-notes)  
 
It is advised to properly read and understand the documentation before going ahead and making your product. Best of luck!

--------------
# Authentication
All the API requests require an **API Key**. Get your API key at [OneLLM dashboard](https://onellm.dev/) after logging in.

Include the key in the `Authorization` header as a Bearer token:
```http
Authorization: Bearer $APIKEY
```
> An API key is a unique identifier used to authenticate and authorize a user or application when accessing an API (Application Programming Interface). It acts like a password, allowing the API to verify that the request is coming from a legitimate source.

 
------ 
# API Endpoint
The BASE_URL for the API is `https://onellm.dev/api`.
This is the primary endpoint for interacting with the language models. It allows you to send a chat conversation and receive a response from the specified model.
`POST https://onellm.dev/api`
**Note:** Streaming responses are **not supported**. Do not include a `stream` parameter.
## Request Body
All the requests must be a JSON object with the following fields:

| Parameter | Type | Required | Description|
| -------------|---------------|--------------------|-------------|
| Model | String | Yes | The model ID to use for the request. See [Supported Models](#models) for model information. |
| Messages | Array | Yes | Conversation history as an array of message objects. Each object requires `role` and `content`. See an [example](#example). |
| Temperature | Number | No | Controls randomness. 0-2, with 0 being too deterministic and 2 being very random. Default is 1. | 
| Max_tokens | Integer | No | Maximum number of tokens to generate. Server adjusts automatically if it exceeds your balance. |
| top_p | Number | No | Nucleus sampling probability (0–1). Controls diversity. |
| stop_sequences | Array | No | Strings that will cause the model to stop generating tokens. |

To view more parameters, please go visit the [OpenAPI specifications](https://github.com/OneLLM-dev/documentation/blob/main/openapi.yaml).

# Example
Example request:
```json
{
  "model": "GPT-4.1",
  "messages": [
    { "role": "user", "content": "Tell me a joke about computers." }
  ],
  "max_tokens": 50
}
```
Example response:
```json
{
  "provider": "openai",
  "model": "GPT-4.1",
  "role": "assistant",
  "content": "Why did the computer show up at work late? It had a hard drive!",
  "usage": {
    "input_tokens": 7,
    "output_tokens": 16,
    "total_tokens": 23
  },
  "finish_reason": "stop"
}
```
# Models

Available model IDs (use in `model` field):

-   **GPT Series**: GPT-5, GPT-5-Mini, GPT-5-Nano, GPT-5-Chat-Latest, GPT-4.1, GPT-4.1-Mini, GPT-4.1-Nano, GPT-o3, GPT-o3-pro, GPT-o3-DeepResearch, GPT-o3-Mini, GPT-o4-mini, GPT-4o, GPT-4o-mini, GPT-o1, GPT-o1-Mini
    
-   **Anthropic (Claude-like)**: Opus-4, Sonnet-4, Haiku-3.5, Opus-3, Sonnet-3.7, Haiku-3
    
-   **DeepSeek**: DeepSeek-Reasoner, DeepSeek-Chat
    
-   **Flash/Preview**: 2.5-Flash-preview, 2.5-Pro-preview, 2.0-Flash, 2.0-Flash-lite, 1.5-Flash, 1.5-Flash-8B, 1.5-Pro
    
-   **Mistral & Variants**: Mistral-Medium-3, Magistral-Medium, Codestral, Devstral-Medium, Mistral-Saba, Mistral-Large, Pixtral-Large, Ministral-8B-24.10, Ministral-3B-24.10, Mistral-Small-3.2, Magistral-Small, Devstral-Small, Pixtral-12B, Mistral-NeMo, Mistral-7B, Mixtral-8x7B, Mixtral-8x22B

 
# Error Handling 

The API may return different error codes. Common ones are:

| Status Code | Meaning | Reason |
| ---------|------------------|----------|
| 400 | Bad Request | Invalid JSON or missing required fields. |
| 401 | Unauthorized | API key missing or invalid. | 
|403 | Forbidden | You don't have access to the requested resource |
| 404 | Not Found | Invalid Endpoint. | 
| 405 | Method not allowed | Endpoint does not support the HTTP method used. |
| 422 | Unprocessable Entity | Invalid type or format in the request body. |
| 429 | Too Many Requests | Rate limit exceeded. Try again later. |
| 500 | Internal Server Error | Something went wrong on the server side. |

Please be sure to check the response body for extra error detail. If you encounter a error you cannot debug, please be sure to join our [Discord Support server](https://discord.gg/4xY47qP88Z).

# Important Notes
-   **Streaming is not supported.** Always send complete requests.
    
-   `max_tokens` will **auto-adjust** if it’s larger than your allowed balance.
    
-   A **minimum balance of $0.10** is required to make API requests.
    
-   The response includes `usage` info:
    
    -   `input_tokens`: number of tokens sent in the request.
        
    -   `output_tokens`: number of tokens generated by the model.
        
    -   `total_tokens`: combined count of both.
        






