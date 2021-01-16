# Authorization Code Flow with Proof Key for Code Exchange (PKCE)

User clicks sign-in link on browser  
SPA generates `code_verifier` and `code_challenge`  
SPA submits http `GET` request to authorization server's `/authorize` endpoint with query parameters `client_id`, `response_type`, `scope`, `redirect_uri`, `state`, `challenge_method`, and `code_challenge`    
Authorization server returns http response that contains html login page  
User enters their credentials and the permissions they wish to provide the SPA  
Browser submits http `POST` request with user's credentials and selected permissions to authorization server's `/token` endpoint  
Authorization server validates user's credentials  
If user's credentials are valid, authorization server returns http redirect to app server with `code` as query parameter  
SPA extracts `code` query parameter from the `redirect_uri` (usually handled by a JavaScript library)  
SPA submits http `GET` request with `code` and `code_verifier` to authorization server  
Authorization server returns http response with `access_token` and `id_token` to SPA  
SPA can send token in http requests to other API endpoints to retrieve data  