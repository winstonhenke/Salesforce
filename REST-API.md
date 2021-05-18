# Salesforce REST APIs

Documentation on using Salesforces REST API

Resources

- [REST API Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm)
- [Authorize Apps with OAuth](https://help.salesforce.com/articleView?id=sf.remoteaccess_authenticate.htm&type=5)
- [Authentication using JWT](https://salesforce.stackexchange.com/questions/201636/authentication-using-jwt/201648)

---

## Creating Connected App

I'm unsure how much of the Connacted Apps configuration depends on the type of OAuth Flow being used. I'm just testing `OAuth 2.0 Username-Password Flow` here which is pretty basic.

- `Setup -> Apps -> App Manager -> New Connected App`
  - Fill in name and contact email
  - Enable OAuth Settings
    - Callback URL: `https://login.salesforce.com/services/oauth2/callback`
      - I'm unsure how much this matters when using the `OAuth 2.0 Username-Password Flow`
    - Selected OAuth Scopes: `Full access`
      - Obviously don't always use this
    - Save the `Consumer Key` & `Consumer Secret`
- `Setup -> Apps -> Connected Apps -> Manage Connected Apps`
  - Edit the Connected App just created
  - Permitted Users: `Admin approved users are pre-authorized`
  - IP Relaxation: `Relax IP restrictions`
- `Setup -> Apps -> Connected Apps -> Manage Connected Apps`
  - Open the Connected App
  - Under the Profiles section add a profile (I'm using System Admin for this example)

## Using the Connected App

Examples using `curl`

- Request an Access Token

  ```bash
  curl -v https://login.salesforce.com/services/oauth2/token -d 'grant_type=password' -d 'client_id=<your_consumer_key>' -d 'client_secret=<your_consumer_secret>' -d 'username=<your_username>' -d 'password=<password_for_username>' -H 'X-PrettyPrint:1'
  ```

  - Example Response

    ```json
    {
      "access_token" : "<access_token>",
      "instance_url" : "https://yourInstance.salesforce.com",
      "id" : "https://login.salesforce.com/id/<orgID>/<userId>",
      "token_type" : "Bearer",
      "issued_at" : "1621352591988",
      "signature" : "<signature>"
    }
    ```

- See what API versions the org supports: `curl -i https://yourInstance.salesforce.com/services/data/`
- View authenticated user info: `curl -i https://login.salesforce.com/id/<orgID>/<userId> -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1'`
  - For `orgId` & `userId` make sure to use the values from the Access Token. These IDs seem to have a few extra characters than what's shown in the Salesforce UI.
- Lists available resources for the specified API version: `curl -i https://yourInstance.salesforce.com/services/data/v51.0/ -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1'`
- Make a request
  - **WARNING**: Pay attention to the subdomain. Needs to math `instance_url` in the Access Token response. `force.com` will not work.

  ```bash
  # GET Account
  curl -i https://yourInstance.salesforce.com/services/data/v51.0/sobjects/Account/<account_ID> -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1'
  # PATCH Account
  curl -i https://yourInstance.salesforce.com/services/data/v51.0/sobjects/Account/<account_ID> -H "Content-Type: application/json" -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1' --request PATCH --data '{"Name" : "Test Account", "ShippingCity" : "Milwaukee"}'
  # POST Account
  curl -i https://yourInstance.salesforce.com/services/data/v51.0/sobjects/Account/ -H "Content-Type: application/json" -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1' --request POST --data '{"Name" : "New Test Account", "ShippingCity" : "Milwaukee"}'

  # GET Case
  curl -i https://yourInstance.salesforce.com/services/data/v51.0/sobjects/Case/<case_ID> -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1'
  # PATCH Case
  curl -i https://yourInstance.salesforce.com/services/data/v51.0/sobjects/Case/<case_ID> -H "Content-Type: application/json" -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1' --request PATCH --data '{"Status" : "New", "Origin" : "Web", "Subject" : "Test Case", "Description" : "This is a test Case created from the REST API"}'
  # POST Case
  curl -i https://yourInstance.salesforce.com/services/data/v51.0/sobjects/Case/ -H "Content-Type: application/json" -H 'Authorization: Bearer <access_token>' -H 'X-PrettyPrint:1' --request POST --data '{"Status" : "New", "Origin" : "Web", "Subject" : "New Test Case", "Description" : "This is a new test Case created from the REST API"}'
  ```

---

## Access Tokens

Descriptions of **some of** the attributes on the returned Access Token

```json
{
  "access_token" : "OAuth token that a connected app uses to request access to a protected resource on behalf of the client application. Additional permissions in the form of scopes can accompany the access token.",
  "instance_url" : "A URL indicating the instance of the user’s org.",
  "id" : "An identity URL that can be used to identify the user and to query for more information about the user. See Identity URLs.",
  "token_type" : "A Bearer token type, which is used for all responses that include an access token.",
  "issued_at" : "Time stamp of when the signature was created in milliseconds.",
  "signature" : "Base64-encoded HMAC-SHA256 signature signed with the client_secret. The signature can include the concatenated ID and issued_at value, which you can use to verify that the identity URL hasn’t changed since the server sent it."
}
```

---
