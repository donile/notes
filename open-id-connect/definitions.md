# OpenID Connect Definitions

### OpenID Connect ID Token

A signed JSON Web Token (JWT) given to the client application alongside the regular OAuth access token.  It is intended to be parsed by the client application.  The ID token includes claims.  It should never be passed to an external service.

| Claim Name | Claim Name (Expanded) | Claim Value                                   |
|------------|-----------------------| ----------------------------------------------|
| iss        | Issuer                | URL of the server that issued the ID token    |
| sub        | Subject               | Unique identifier for the user at identity provider |
| aud        | Audience              | Client ID                                     |
| exp        | Expiration            | Time at which the ID token expires            |
| iat        | Issued At (time)      | The timestamp of when the ID token was issued |

```
{
  {
    // JWT Header
    "type": "JWT",
    "alg": "rsaKey.alg",
    "kid": 
  },
  {
    // Payload
    iss: <idp-host-url>,
    sub: <unique-id>,
    aud: <client-id>,
    iat: <time-id-token-was-issued>,
    exp: <time-id-token-expires>
  },
  {
    
  }
}
```

### UserInfo Endpoint

Accessed by providing the access token, not the ID token.

The response payload is JSON that contains claims about the user.