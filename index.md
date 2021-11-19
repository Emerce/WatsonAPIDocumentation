Documentation for usage of the WatsonAPI

## Introduction
Watson is the name of the application on which account.emerce.nl runs. On the front it provides a public platform for Emerce Users to edit their settings, personal data, preferences etc. On the back it connects to Sherlock, the main back-office system for EMERCE, to proces updates submitted by Users, fetch personal data to be used on the EMERCE websites, manage subscriptions, etc.

## RESTful API
WatsonAPI is a RESTful API. 

## Accept: application/json
All responses are in application/json MIME type. Setting the "Accept: application/json" is mandatory for all requests.

## Authorization
API clients need to authorize each request by setting a "Authorization" header containing a 60 character long Bearer token.

Example usage of authorization header:
```
curl --location --request POST 'https://account.emerce.nl/api/authenticate' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
[...]
```

Example response of an unsuccesful authorization attempt:
```
HTTP/2 401 
server: nginx/1.18.0 (Ubuntu)
content-type: application/json
cache-control: no-cache, private
date: Fri, 19 Nov 2021 13:41:36 GMT
x-ratelimit-limit: 6000
x-ratelimit-remaining: 5999

{"message":"Unauthenticated."}
```

## Endpoints
The WatsonAPI provides endpoints to facilitate processes initiated by both Sherlock and (currently the only public client) Emerce.nl. Below you will find a list of all available endpoints, a description of their usecases and documentation on how to appraoch them.

### /authenticate
- **Purpose:**
  - Used to authenticate a user providing an email address and password. 
- **Type:**
  - POST
- **Full URL:**
  - https://account.emerce.nl/api/authenticate
- **Params:**
  - *email* - The User's email address
  - *password* - The User's password
- **Response types:**
  - *404 (Not found)* - Email address is not found or invalid login credentials
  - *303 (See other)* - User must reset password. Please redirect User to included "url"
  - *200 (OK)* - Reponse provides User token
  
**Example request**
```
curl --location --request POST 'https://account.emerce.nl/api/authenticate' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
--form 'email="bastiaan@superinteractive.com"' \
--form 'password="Welkom01"'
```

**Example responses**
```
HTTP/2 404 
server: nginx
content-type: application/json
cache-control: no-cache, private
date: Fri, 19 Nov 2021 12:26:12 GMT

{"status":"error","message":"User not found or invalid login credentials"}%                                                                                                                                                                                                                    
```

```
HTTP/2 200 
server: nginx
content-type: application/json
cache-control: no-cache, private
date: Fri, 19 Nov 2021 12:27:00 GMT

{"token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"}
```

```
HTTP/2 303 
server: nginx
content-type: application/json
cache-control: no-cache, private
date: Fri, 19 Nov 2021 13:40:29 GMT

{"message":"User must reset password","url":"https:\/\/loc.account.emerce.nl\/password\/reset"}                                                                                                                                                                                             
```

### /user
- **Purpose:**
  - Get a specific User's profile data using their unique API token
- **Type:**
  - POST
- **Full URL:**
  - https://account.emerce.nl/api/user
- **Params:**
  - *token* - The User's unique API token
- **Response types:**
  - *404 (Not found)* - Invalid token
  - *200 (OK)* - Reponse provides User's profile data

**Example request**
curl --location --request POST 'https://account.emerce.nl/api/user' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
--form 'token="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"'

**Example responses**
```
HTTP/2 404 
server: nginx
content-type: application/json
cache-control: no-cache, private
date: Fri, 19 Nov 2021 13:50:52 GMT

{"status":"error","message":"Invalid token"}
```

```
HTTP/2 200 
server: nginx/1.18.0 (Ubuntu)
content-type: application/json
cache-control: no-cache, private
date: Fri, 19 Nov 2021 13:51:56 GMT

{"user":{"id":7102,"first_name":"Bastiaan","last_name_prefix":"van","last_name":"Dreunen","position":"Creative & Developer, Owner","email":"bastiaan@superinteractive.com","subscription_type":1,"registration_subscription_type":1}}
```
