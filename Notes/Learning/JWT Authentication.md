## Definition
JWT (JSON Web Token) is a *standard* that defines *a way for securely transmitting information between parties* as a JSON object. The information can be *trusted* because it is digitally *signed*. JWTs can be *signed* using a **secret** (with the HMAC Algorithm) or a **public/private key** pair using **RSA** or **ECDSA**. 

## When to use
- **Authorization:** Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token.
- **Information Exchange:** JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are.

## Structure
JWT  consist of three parts separated by dots (.), which are :
- Header
- Payload
- Signature
Therefore, a JWT typically looks like the following :
`xxxx.yyyy.zzzz`
### Header
The header _typically_ consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.

Example:
```json
{ 
  "alg": "HS256", 
  "typ": "JWT" 
}
```
### Payload
The payload contains the **claim**, which are statements about an entity (typically, the user) and additional data.

Example:
```json
{ 
   "sub": "1234567890", 
   "name": "John Doe", 
   "admin": true 
}
```
### Signature
To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For example if you want to use the HMAC SHA256 algorithm, the signature will be created in the following way:
```typescript
HMACSHA256( 
  base64UrlEncode(header) + "." + 
  base64UrlEncode(payload), 
  secret)
```

## How do JWT works
In authentication, when the user successfully logs in using their credentials, a JSON Web Token will be returned. Since tokens are credentials, great care must be taken to prevent security issues. In general, you should not keep tokens longer than required.

Whenever the user wants to access a protected route or resource, the user agent should send the JWT, typically in the **Authorization** header using the **Bearer** schema. The content of the header should look like the following:
`Authorization: Bearer <token>`

This can be, in certain cases, a stateless authorization mechanism. The server's protected routes will check for a valid JWT in the `Authorization` header, and if it's present, the user will be allowed to access protected resources. If the JWT contains the necessary data, the need to query the database for certain operations may be reduced, though this may not always be the case.

## Another resource about JWT
- [What Is JWT and Why Should You Use JWT](https://www.youtube.com/watch?v=7Q17ubqLfaM)
- [JWT Authentication Tutorial - Node.js](https://www.youtube.com/watch?v=mbsmsi7l3r4)
