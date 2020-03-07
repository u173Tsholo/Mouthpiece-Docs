# Mouthpiece API

All components in the Mouthpiece system will interact with each other strictly through the Mothpiece API.
The following document describes the structure of the API, as well as how requests and responses need to be structured.
In order to make requests you will be required to follow [RESTful](https://restfulapi.net/resource-naming/) conventions.

# Table of Contents

1. [Endpoints](#endpoints)  
		1.1		[User Endpoints](#user-endpoints)  
		1.2		[Notifications Endpoints](#notifications-endpoints)  
		1.3		[Neural Network Endpoints](#neural-network-endpoints)  
		1.4		[Template Sharing Endpoints](#template-sharing-endpoints)  
2. [Requesting New Endpoints](#requesting-new-endpoints)  
3. [Response Codes](#response-codes)  
4. [Authorization](#authorization)  
5. [Users](#users)  
		5.1		[Create User](#create-user)  
		5.2		[Authenticate](#authenticate)  
		5.3		[Get User](#get-user)  
    5.4		[Get Templates](#get-templates)  
    5.5		[Get Template](#get-template)  
    5.6		[Update Password](#update-password)  
    5.7		[Update Email](#update-email)  
    5.8		[Update Theme](#update-theme)  
    5.9		[Update Mode](#update-mode)  
    5.10	[Delete User](#delete-user)  
6. [Notifications](#notifications)  
7. [Neural Network](#neural-network)  
8. [Template Sharing](#template-sharing)  

# Endpoints

## User Endpoints

| Method		| Path															| Used for								|
|-----------|-----------------------------------|-------------------------|
| `POST`		| `/api/users`      								| Create new user					|
| `GET`			| `/api/users/{id}` 								| Get user								|
| `DELETE`	| `/api/users/{id}` 								| Delete user							|
| `GET`			| `/api/users/{id}/templates` 			| Get user's templates		|
| `GET`			| `/api/users/{id}/templates/{id}`	| Get user's template			|
| `PUT`			| `/api/users/{id}/username`				| Update user's username	|
| `PUT`			| `/api/users/{id}/password` 				| Update user's password	|
| `PUT`			| `/api/users/{id}/theme`						| Update user's theme			|
| `PUT`			| `/api/users/{id}/formant`					| Update user's formant		|

## Notifications Endpoints

| Method		| Path															| Used for								|
|-----------|-----------------------------------|-------------------------|
| `-`				| `-`								 								| -												|

## Neural Network Endpoints

| Method		| Path															| Used for								|
|-----------|-----------------------------------|-------------------------|
| `-`				| `-`								 								| -												|

## Template Sharing Endpoints

| Method		| Path															| Used for								|
|-----------|-----------------------------------|-------------------------|
| `-`				| `-`								 								| -												|

# Requesting New Endpoints

The endpoints provided below are by no means the only endpoints that will be available.
If your team requires a new endpoint your **team leader** will need to submit an issue on our [GitHub Repository](https://github.com/).
Once the issue has been closed, you will be able to make use of the new endpoint.

>**NOTE:** You will need to fully justify why you require the endpoint in the issue.

# Response Codes
There are a number of global response codes that apply to all requests made and a number of response codes which are specific to each endpoint, the table below describes all of the global response codes.

| Code	| Description																																																		|
|-------|---------------------------------------------------------------------------------------------------------------|
| `200` | Valid Request, a body is returned in the response																															|
| `201` | Valid Request, no body is returned in the response																														|
| `204` | Valid Request, no body is returned in the response																														|
| `400` | Invalid Request, check if the request body contains all of the required fields and if the body is valid JSON	|
| `401`	| Unauthorized request due to missing or invalid `Authorization` header																					|

>**NOTE:** A response will only return a body if a status code of `200` is returned.

# Authorization
Each group will be provided with a [Bearer Authentication](https://dzone.com/articles/four-most-used-rest-api-authentication-methods) token that will be used for authorization purposes.
Everytime a request is made to the API, this token must be provided or else the API will reject the request immediately and will return a `401` status code.
The token should be sent using the *Authorization* header in the request.
An example of how this token should be sent is given below:

`Authorization: Bearer <token>`

# Users

## Create User

This endpoint is used to create an account for a user.

##### HTTP Request

`POST /api/users`

##### Request Body

The request body requires the following fields:

```json
{
	"name": "John Doe",
	"username": "john@doe.com",
	"password": "KN0FiiO3WWUI4aDY9F10jeXQEnl7QAI3"
}
```

##### Response Body

This endpoint will return the `id` of the newly created user, as well as a `token` which must be used for all further requests.

```json
{
	"id": 1234,
	"token": "4dffdfd0f9d0f9df0"
}
```

##### Response Status Codes

>**NOTE:** This endpoint does not adhere to the global response codes provided above, for this endpoint a status code of `200` will never be returned but rather it will return `201` along with a body if the request is valid.

| Status Code | Description																														  |
|-------------|-------------------------------------------------------------------------|
| `201`       | User created																													  |
| `403`       | Invalid `username` (must be a valid email address)											|
| `409`       | User already exists																										  |

## Authenticate

This endpoint is used to authenticate a user based off of their `password`.

##### HTTP Request

`GET /api/users/authenticate`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"username": "john@doe.com",
	"password": "KN0FiiO3WWUI4aDY9F10jeXQEnl7QAI3"
}
```

##### Response Body

This endpoint will return the `id` of the user, as well as a `token` which must be used for all further requests.

```json
{
	"id": 12345,
	"token": "4dffdfd0f9d0f9df0"
}
```

##### Response Status Codes

| Status Code | Description											                                          |
|-------------|---------------------------------------------------------------------------|
| `200`       | User `id` returned																											  |
| `404`       | Incorrect `username` or `password`										                    |

## Get User

This endpoint retrieves a specific users details.

##### HTTP Request

`GET /api/users/{id}`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0"
}
```

##### Response Body

```json
{
	"name": "Jane Doe",
	"username": "jane@doe.com",
	"theme": "dark",
	"mode": "formant"
}
```

##### Response Status Codes

| Status Code | Description										                                        |
|-------------|-----------------------------------------------------------------------|
| `200`       | User returned																												  |
| `403`       | Invalid `token`																                        |
| `404`       |	Invalid user `id`																	                    |

## Get Templates

This endpoint retrieves all of a user's templates.

##### HTTP Request

`GET /api/users/{id}/templates`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0"
}
```

##### Response Body

The response will return an array of templates, each template will have an `id`,
a boolean indicating whether or not it has been shared,
as well as an array of urls for each formant representation of the template:

```json
{
	"templates": [
		{
			"id": 54321,
			"shared": true,
			"urls": [
				"https://www.image1.com",
				"https://www.image2.com"
			]
		}
	]
}
```

##### Response Status Codes

| Status Code | Description									                                        |
|-------------|---------------------------------------------------------------------|
| `200`       | User `templates` returned																						|
| `403`       | Invalid `token`																                      |
| `404`       |	Invalid user `id`															                      |

## Get Template

This endpoint retrieves a specific template of a user.

##### HTTP Request

`GET /api/users/{id}/templates/{id}`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0"
}
```

##### Response Body

```json
{
	"id": 54321,
	"shared": true,
	"urls": [
		"https://www.image1.com",
		"https://www.image2.com"
	]
}
```

##### Response Status Codes

| Status Code | Description											                                            |
|-------------|-----------------------------------------------------------------------------|
| `200`       | User `template` returned																								    |
| `403`       | Invalid `token`																                              |
| `404`       |	Invalid user `id` or template `id`									                        |

## Update Password

This endpoint updates a user's password.

##### HTTP Request

`PUT /api/users/{id}/password`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0",
	"password": "IEJy4axvQKi3CcKQZdaG3YWIc9EvILi0"
}
```

##### Response Body

This endpoint will not return a body.

##### Response Status Codes

| Status Code | Description											                                        |
|-------------|-------------------------------------------------------------------------|
| `204`				| Password changed																											  |
| `403`       | Invalid `token`																                          |
| `404`       |	Invalid user `id`																	                      |

## Update Email

This endpoint updates a user's email address.

##### HTTP Request

`PUT /api/users/{id}/username`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0",
	"username": "john.adam@doe.com"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description							                                        |
|-------------|-----------------------------------------------------------------|
| `204`				| Username changed																							  |
| `403`       | Invalid `token`												                          |
| `404`       | Invalid user `id` or `username` (must be a valid email address)	|

## Update Theme

This endpoint updates a user's theme.

##### HTTP Request

`PUT /api/users/{id}/theme`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0",
	"theme": "dark"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description							                                        |
|-------------|-----------------------------------------------------------------|
| `204`				| Theme changed																									  |
| `403`       | Invalid `token`												                          |
| `404`       | Invalid user `id` or `theme`																		|

## Update Voice Mode

This endpoint updates a user's voice mode.

##### HTTP Request

`PUT /api/users/{id}/mode`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "4dffdfd0f9d0f9df0",
	"mode": "formant"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description											                                          |
|-------------|---------------------------------------------------------------------------|
| `204`				| Listening mode changed																								    |
| `403`       | Invalid `token`																                            |
| `404`       | Invalid user `id` or `mode`																			  |

## Delete User

This endpoint is used to delete a user's account.

##### HTTP Request

`DELETE /api/users/{id}`

##### Request Body

The request body requires the following fields:

```json
{
	"token": "4dffdfd0f9d0f9df0"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description																														  |
|-------------|-------------------------------------------------------------------------|
| `204`				| User deleted																													  |
| `403`       | Invalid `token`																                          |
| `404`       | Invalid user `id`																											  |

# Notifications

Under Construction

# Neural Network

Under Construction

# Template Sharing

Under Construction
