## Documentation for usage of the WatsonAPI

### Introduction
Watson is the name of the application on which account.emerce.nl runs. On the front it provides a public platform for Emerce Users to edit their settings, personal data, preferences etc. On the back it connects to Sherlock, the main back-office system for EMERCE, to proces updates submitted by Users, fetch personal data to be used on the EMERCE websites, manage subscriptions, etc.

### RESTful API
WatsonAPI is a RESTful API. 

### Accept: application/json
All responses are in application/json MIME type. Setting the "Accept: application/json" is mandatory for all requests.

### Authorization
API clients need to authorize each request by setting a "Authorization" header containing a 60 character long Bearer token.

Example:
```
curl --location --request POST 'https://account.emerce.nl/api/authenticate' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
[...]
```

### Endpoints
The WatsonAPI provides endpoints to facilitate processes initiated by both Sherlock and (currently the only public client) Emerce.nl. Below you will find a list of all available endpoints, a description of their usecases and documentation on how to appraoch them.

#### /authenticate
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



