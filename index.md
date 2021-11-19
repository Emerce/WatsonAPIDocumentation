## Documentation for usage of the WatsonAPI

### Introduction
Watson is the name of the application on which account.emerce.nl runs. On the front it provides a public platform for Emerce Users to edit their settings, personal data, preferences etc. On the back it connects to Sherlock, the main back-office system for EMERCE, to proces updates submitted by Users, fetch personal data to be used on the EMERCE websites, manage subscriptions, etc.

### RESTful API
WatsonAPI is a RESTful API. Responses are in JSON format.

### Authorization
API clients need to authorize each request by setting a "Authorization" header containing a Bearer token.

Example:
```
curl --location --request POST 'https://account.emerce.nl/api/authenticate' \
--header 'Accept: application/json' \
*--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \*
--form 'email="bastiaan@superinteractive.com"' \
--form 'password="Welkom01"'
```

### Endpoints
The WatsonAPI provides endpoints to facilitate processes initiated by both Sherlock and (currently the only public client) Emerce.nl. Below you will find a list of all available endpoints, a description of their usecases and documentation on how to appraoch them.

#### /authenticate

Purpose: Used to authenticate 
