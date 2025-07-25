Data Encryption (At-Rest & In-Transit)
"Encryption is not just about secrecy — it’s about trust, compliance, and control."

Modern systems must protect data both while stored and while moving, using encryption techniques to maintain confidentiality, integrity, and compliance.

🧊 1. Encryption At-Rest
Definition: Data is encrypted when it's stored (e.g., on disk, in databases, S3).

🔒 Why it matters:
Prevents attackers from reading raw data if storage is compromised.

Required by compliance (e.g., GDPR, HIPAA, PCI-DSS).

🔧 Common Techniques:
Method	Where Used
File-level encryption	File systems, block storage
Full-disk encryption	Laptops, servers, mobile devices
Database encryption	Transparent Data Encryption (TDE)
Application-level	Encrypt specific fields (e.g., SSN)

✅ Best Practices:
Use strong ciphers (e.g., AES-256).

Leverage cloud-native options (e.g., AWS KMS, GCP Cloud KMS).

Rotate keys regularly.

Encrypt backups and snapshots too.

🌐 2. Encryption In-Transit
Definition: Data is encrypted as it moves between systems (e.g., client ↔ server).

🔒 Why it matters:
Prevents man-in-the-middle attacks, eavesdropping, data tampering.

Protects user credentials, tokens, sensitive data over networks.

🔧 Common Techniques:
Protocol	Description
HTTPS (TLS)	Encrypts web traffic (HTTP over SSL/TLS)
TLS/SSL	Used in APIs, database connections
VPN	Encrypted private network tunnels
SSH	Secure remote login and tunneling

✅ Best Practices:
Enforce HTTPS with valid TLS certificates.

Disable weak cipher suites (e.g., RC4, DES).

Use TLS 1.2+ (1.3 preferred).

Enable HTTP Strict Transport Security (HSTS).

🛠️ Visual Structure
text
Copiar
Editar
+-------------------+           Encrypted via HTTPS           +--------------------+
|     Client        |  ------------------------------------>  |      Server        |
|  (Browser, App)   |           TLS / SSL Encryption          |   (Web/API Server) |
+-------------------+                                         +--------------------+

+--------------------+
|     Encrypted      |
|   Storage Bucket   |
|     (e.g., S3)      |
+--------------------+
🔐 Key Management
Encryption is only as strong as how well you manage the keys.

🔑 Key Management Services (KMS)
Provider	Service
AWS	AWS KMS
GCP	Cloud KMS
Azure	Azure Key Vault

✅ Best Practices:
Never hardcode encryption keys.

Use envelope encryption (KMS wraps your app-generated keys).

Enable key rotation policies.

Restrict key access using IAM policies.

📦 Cloud Provider Encryption Defaults
Cloud	At-Rest Encryption Enabled?	In-Transit Encryption Enabled?
AWS S3	Yes (AES-256, SSE-S3/KMS)	Yes (TLS 1.2+)
GCP Storage	Yes	Yes
Azure Blob	Yes	Yes

🧠 Interview Takeaways
“In my architecture, I ensure all communication between services is encrypted using TLS 1.3 and that all data at rest is encrypted using AES-256 with KMS-managed keys.”

“To secure user data such as passwords or PII, I use application-level field encryption on top of TDE.”

✅ Summary
Encryption Type	Purpose	Example Tools/Tech
At-Rest	Protect stored data	TDE, AES, AWS/GCP KMS
In-Transit	Protect data in motion	TLS, HTTPS, SSH
Key Management	Secure key storage and usage	AWS KMS, GCP KMS, Azure Vault