OAuth2, JWT, Session vs Token Authentication
"Authentication is about who you are, and authorization is about what you're allowed to do."

This module covers the core concepts used in securing modern web applications and APIs.

✅ Core Terms Defined
Term	What It Means
AuthN	Authentication – verifying who the user is
AuthZ	Authorization – verifying what the user is allowed to do
Session	A server-managed record of a user's login state
Token	A client-side credential used to access protected resources

🏗️ Session vs Token Authentication
Feature	Session-Based	Token-Based (JWT)
Storage	Server-side (e.g., Redis, in-memory)	Client-side (e.g., stored in localStorage)
Scalability	Harder to scale horizontally	Easier to scale (stateless)
State	Stateful – server tracks sessions	Stateless – no server session required
Security	Vulnerable to CSRF	Vulnerable to XSS
Logout	Server can invalidate sessions easily	Hard to revoke tokens before expiry
Performance	Fast lookup in memory	Slightly heavier due to token parsing

🔑 JWT – JSON Web Token
JWTs are compact, self-contained tokens that represent claims.

🔐 JWT Structure:
php-template
Copiar
Editar
<Header>.<Payload>.<Signature>
Example:
json
Copiar
Editar
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJ1c2VySWQiOjEyMywiZXhwIjoxNjQwMDAwMDAwfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Header: Metadata, e.g., algorithm used (HS256)

Payload: Claims – user ID, roles, expiry

Signature: Verifies integrity with a secret key

✅ Use JWT when:

You need stateless auth (e.g., mobile APIs, microservices)

You're dealing with third-party auth providers

🌐 OAuth2 – Delegated Authorization
OAuth2 is a protocol for third-party apps to access user data without sharing passwords.

Used by: Google, Facebook, GitHub, Spotify, etc.

🔁 OAuth2 Roles
Role	Description
User	Resource Owner
Client App	Wants to access data (e.g., Spotify)
Auth Server	Handles login & token generation
Resource Server	Holds user data (e.g., Gmail API)

🔐 OAuth2 Grant Types
Grant Type	Use Case
Authorization Code	Server-side web apps
Implicit	Legacy SPAs (now discouraged)
Client Credentials	Machine-to-machine APIs
Password Grant	Trusted apps with username/password
Device Code	Devices with no browser (e.g., TVs)

🔁 OAuth2 Flow (Authorization Code Grant)
pgsql
Copiar
Editar
[User] → Login on Auth Server
     ↓ (redirect with auth code)
[Client App] → Exchanges code for access_token
     ↓
[Client App] → Access Resource Server with token
Tokens usually include:

Access Token: Used to call APIs

Refresh Token: Used to get a new access token without re-login

🔄 Combining OAuth2 + JWT
Many systems issue JWTs as access tokens in an OAuth2 flow.

E.g., Google issues an id_token (a JWT) that you can use to authenticate.

🧠 Interview Ready Soundbites
“For stateless APIs, I’d use JWT with proper expiration and rotate refresh tokens. For user login via Google or GitHub, I’d integrate OAuth2 using the Authorization Code Flow.”

“JWTs are great for performance and scalability but harder to revoke. Sessions are easier to invalidate but not scalable unless using sticky sessions or Redis.”

🔐 Security Best Practices
Concern	Mitigation
Token theft	Store JWT in HttpOnly cookies or secure storage
Replay attacks	Use short token lifetimes & refresh tokens
Token bloat	Don’t stuff JWTs with too many claims
Logout/revocation	Use refresh token rotation + blacklist

✅ Summary
Sessions = good for classic web apps

JWTs = best for stateless APIs, microservices

OAuth2 = industry-standard for delegated access

Security = always validate, encrypt, and expire

