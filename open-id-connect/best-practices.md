# OpenId Connect Best Practices

Use the authorization code flow so the client (relying party) only accepts tokens received directly from the authorization server.

Use the `state` parameter to provide an unguessable value.  The client should assign `state` a random value and verify it is the same when the response from from the authorization server is received.

## Client Best Practices

* Parse the ID token
  * Ensure it's valid JWT
  * Collect claims
* Validate signature against Identity Provider's public key
* Ensure the ID token was issued by a trusted Identity Provider
* Ensure client's ID is included in the audience (aud)
* Confirm timing is correct (expiration, issued-at and not-before)
* Verify the nonce matches
* Validate the hashes for the authorization code or access token

## Authorization Code Flow

HTTP request to OpenId Connect authorization endpoint

```
http://idp.hostname.com/connect/authorize
  ?client_id=<client-id>
  &redirect_uri=http://client.app.com/endpoint
  &scope=openid profile
  &response_type=code
  &response_mode=form_post
  &nonce=<random-bytes>
```

Response Types -> Authorization Flow
| Response Type       | Flow               |
|---------------------|--------------------|
| code                | Authorization Code |
| id_token            | Implicit           |
| id_token token      | Implicit           |
| code id_token       | Hybrid             |
| code token          | Hybrid             |
| code id_token token | Hybrid             |