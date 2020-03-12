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
    5.4		[Update Password](#update-password)  
    5.5		[Update Email](#update-email)  
    5.6		[Update Theme](#update-theme)  
    5.7		[Update Listening Mode](#update-listening-mode)  
    5.8   [Delete User](#delete-user)  
6. [Notifications](#notifications)  
7. [Neural Network](#neural-network)  
8. [Template Sharing](#template-sharing)  
    8.1   [Create Template](#create-template)  
    8.2   [Get Templates](#get-templates)  
    8.3   [Get Template](#get-template)  
    8.4   [Share Template](#share-template)  
    8.5   [Delete Template](#delete-template)  
    
# Endpoints

## User Endpoints

| Method		| Path																				| Used for																|
|-----------|---------------------------------------------|-----------------------------------------|
| `POST`		| `/api/users`																| Create new user													|
| `GET`			| `/api/users/authenticate`										| Authenticate user												|
| `GET`			| `/api/users/:userid`												| Get user																|
| `DELETE`	| `/api/users/:userid` 												| Delete user															|
| `PUT`			| `/api/users/:userid/password` 							| Update user's password									|
| `PUT`			| `/api/users/:userid/username`								| Update user's username									|
| `PUT`			| `/api/users/:userid/theme`									| Update user's theme preference					|
| `PUT`			| `/api/users/:userid/mode`										| Update user's listening mode preference	|

## Notifications Endpoints

| Method		| Path															| Used for								|
|-----------|-----------------------------------|-------------------------|
| `-`				| `-`								 								| -												|

## Neural Network Endpoints

| Method		| Path															| Used for								|
|-----------|-----------------------------------|-------------------------|
| `-`				| `-`								 								| -												|

## Template Sharing Endpoints

| Method		| Path															                | Used for			                          					|
|-----------|---------------------------------------------------|---------------------------------------------------|
| `POST`    | `/api/users/:userid/templates`                    | Create user's template                            |
| `GET`			| `/api/users/:userid/templates`							      | Get user's templates				          						|
| `GET`			| `/api/users/:userid/templates/:templateid`	      | Get user's template	          										|
| `PUT`			| `/api/users/:userid/templates/:templateid/shared`	| Share/unshare user's template											|
| `DELETE`  | `/api/users/:userid/templates/:templateid`      	| Delete user's template								      			|

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
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
}
```

##### Response Status Codes

| Status Code | Description																														  |
|-------------|-------------------------------------------------------------------------|
| `201`       | User created																													  |
| `403`       | Invalid `username` (must be a valid email address)											|
| `409`       | User already exists																										  |

[Back to Table of Contents](#table-of-contents)

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
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
}
```

##### Response Status Codes

| Status Code | Description											                                          |
|-------------|---------------------------------------------------------------------------|
| `200`       | User `id` returned																											  |
| `404`       | Incorrect `username` or `password`										                    |

[Back to Table of Contents](#table-of-contents)

## Get User

This endpoint retrieves a specific users details.

##### HTTP Request

`GET /api/users/:userid`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
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
| `404`       |	Invalid `:userid`																	                    |

[Back to Table of Contents](#table-of-contents)

## Delete User

This endpoint is used to delete a user's account.

##### HTTP Request

`DELETE /api/users/:userid`

##### Request Body

The request body requires the following fields:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description																														  |
|-------------|-------------------------------------------------------------------------|
| `204`				| User deleted																													  |
| `403`       | Invalid `token`																                          |
| `404`       | Invalid `:userid`																											  |

[Back to Table of Contents](#table-of-contents)

## Update Password

This endpoint updates a user's password.

##### HTTP Request

`PUT /api/users/:userid/password`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1",
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
| `404`       |	Invalid `:userid`																	                      |

[Back to Table of Contents](#table-of-contents)

## Update Username

This endpoint updates a user's username.

##### HTTP Request

`PUT /api/users/:userid/username`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1",
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
| `404`       | Invalid `:userid` or `username` (must be a valid email address)	|

[Back to Table of Contents](#table-of-contents)

## Update Theme

This endpoint updates a user's theme preference.

##### HTTP Request

`PUT /api/users/:userid/theme`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1",
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
| `404`       | Invalid `:userid` or `theme`																		|

[Back to Table of Contents](#table-of-contents)

## Update Listening Mode

This endpoint updates a user's listening mode preference.

##### HTTP Request

`PUT /api/users/:userid/mode`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1",
	"mode": "formant"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description							                                          |
|-------------|-------------------------------------------------------------------|
| `204`				| Listening mode changed																				    |
| `403`       | Invalid `token`												                            |
| `404`       | Invalid `:userid` or `mode`																			  |

[Back to Table of Contents](#table-of-contents)

# Notifications

Under Construction

# Neural Network

Under Construction

# Template Sharing

## Create Template

This endpoint is used to create mouth template for a user.

##### HTTP Request

`POST /api/users/:userid/templates`

##### Request Body

The following fields must be provided in the request body. 
The token required is the token which was retrieved when authenticating
the user and is not linked to the actual template. Note that the images
sent will be Base64 encoded. The full length of an image is not demonstrated
below only, a substring of how it may look is shown.

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1",
	"shared": true,
	"images": [
		"aGVsbG8gd29ybGQIxA==",
		"dGVzdGluZyAxIDIgMw==",
		"aGVsbG8gZGFya25lc3==",
		"d2UgY2FuIGRvIHRoaM=="
	]
}
```

##### Response Body

```json
{
	"id": 54321
}
```

##### Response Status Codes

| Status Code | Description																														  |
|-------------|-------------------------------------------------------------------------|
| `201`       | Template created																												|
| `403`       | Invalid image provided in `images` or invalid `shared` value						|

[Back to Table of Contents](#table-of-contents)

## Get Templates

This endpoint retrieves all of a user's templates.

##### HTTP Request

`GET /api/users/:userid/templates`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
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

| Status Code | Description									                                                      |
|-------------|-----------------------------------------------------------------------------------|
| `200`       | User `templates` returned															              							|
| `403`       | Invalid `token`		              														                      |
| `404`       |	Invalid `:userid` or `:templateid`	       									                      |

[Back to Table of Contents](#table-of-contents)

## Get Template

This endpoint retrieves a specific template of a user.

##### HTTP Request

`GET /api/users/:userid/templates/:templateid`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
}
```

##### Response Body

```json
{
	"id": 54321,
	"shared": true,
	"urls": [
		"https://www.formant1image.com",
		"https://www.formant2image.com",
		"https://www.volume1image.com"
		"https://www.volume2image.com"
	]
}
```

##### Response Status Codes

| Status Code | Description											                                            |
|-------------|-----------------------------------------------------------------------------|
| `200`       | User `template` returned																								    |
| `403`       | Invalid `token`																                              |
| `404`       |	Invalid `:userid` or `:templateid`	       									                |

[Back to Table of Contents](#table-of-contents)

## Share Template

This endpoint either shares or unshares a specific template of a user. If the user's template `shared` field is 
currently `true` this request will set it to `false` and *vice versa*.

##### HTTP Request

`PUT /api/users/:userid/templates/:templateid`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description											                                                  |
|-------------|-----------------------------------------------------------------------------------|
| `204`       | User `template` shared/unshared																								    |
| `403`       | Invalid `token`	              															                      |
| `404`       |	Invalid `:userid` or `:templateid`	       									                      |

[Back to Table of Contents](#table-of-contents)

## Delete Template

This endpoint deletes a specific template of a user. 

##### HTTP Request

`DELETE /api/users/:userid/templates/:templateid`

##### Request Body

The following fields must be provided in the request body:

```json
{
	"token": "i58b540dae2a49b5f7f752a8b84037fb1"
}
```

##### Response Body

This endpoint will not return any fields in the response body.

##### Response Status Codes

| Status Code | Description						                                                            |
|-------------|-----------------------------------------------------------------------------------|
| `204`       | User `template` deleted		          																					    |
| `403`       | Invalid `token`	              															                      |
| `404`       |	Invalid `:userid` or `:templateid`	       									                      |

[Back to Table of Contents](#table-of-contents)
