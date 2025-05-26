# 📄 Vulnerability Report - WebGoat

## 🔍 Εντοπισμένα Alerts

| Severity | Είδος Ευπάθειας     | Αρχείο                          | Περιγραφή                                      | Link στο CVE |
|----------|---------------------|----------------------------------|------------------------------------------------|----------------|
| Critical | [1] XML External Entity Attack| webgoat-lessons/xxe/src/main/java/org/owasp/webgoat/xxe/Comments.java:101 | Parsing untrusted XML files with a weak-configured XML parseyer, results to a XXE attack | [CWE Alert](https://cwe.mitre.org/data/definitions/611.html)     |
| Critical | [2] Server-side request forgery | webgoat-container/src/main/java/org/owasp/webgoat/WebSecurityConfig.java:72 | Input into an HTTP request without validating the input can facilitate server-side request forgery (SSRF) attacks. In these attacks, the server may be tricked into making a request and interacting with an attacker-controlled server. | [CWE Alert](https://cwe.mitre.org/data/definitions/918.html)     | 
| Critical | [3] Deserialization of user-controlled data     | webgoat-lessons/insecure-deserialization/src/main/java/org/owasp/webgoat/deserialization/InsecureDeserializationTask.java:56   | Deserializing untrusted data using any deserialization framework that allows the construction of arbitrary serializable objects is easily exploitable and in many cases allows an attacker to execute arbitrary code.| [CWE Alert](https://cwe.mitre.org/data/definitions/502.html)     |
| High     | [4] Disabled Spring CSRF protection | webwolf/src/main/java/org/owasp/webwolf/WebSecurityConfig.java:56 | Cross-site request forgery (CSRF) is a type of vulnerability in which an attacker is able to force a user to carry out an action that the user did not intend. | [CWE Alert](https://cwe.mitre.org/data/definitions/352.html)   |
| High     | [5] Insecure randomness | webgoat-lessons/jwt/src/main/java/org/owasp/webgoat/jwt/JWTRefreshEndpoint.java:91 | If you use a cryptographically weak pseudo-random number generator to generate security-sensitive values, such as passwords, attackers can more easily predict those values.   | [CWE Alert](https://cwe.mitre.org/data/definitions/330.html)     |
| High     | [6] Missing JWT signature check | webgoat-lessons/jwt/src/main/java/org/owasp/webgoat/jwt/JWTRefreshEndpoint.java:129 | A JSON Web Token (JWT) consists of three parts: header, payload, and signature. The io.jsonwebtoken.jjwt library is one of many libraries used for working with JWTs. Therefore it is necessary to provide the JwtParser with a key that is used for signature validation. Unfortunately the parse method accepts a JWT whose signature is empty although a signing key has been set for the parser. This means that an attacker can create arbitrary JWTs that will be accepted if this method is used. | [CWE Alert](https://cwe.mitre.org/data/definitions/347.html) |

---

## 🛡️ Προτεινόμενα Μέτρα Αντιμετώπισης

### [1] XXE Attack
- Disabling the parsing of any Document Type Declarations (DTDs) in untrusted data.
- Protection against denial of service attacks may also be implemented by setting entity expansion limits, which is done by default in recent JDK and JRE implementations

### [2] Server-side request forgery
- avoid putting user-provided input directly into a request URL
- maintain a list of authorized URLs on the server
- requests constructed from user input are limited to a particular host or more restrictive URL prefix.

### [3] Deserialization of user-controlled data
- Avoid deserialization of untrusted data if at all possible
- If the architecture permits it then use other formats instead of serialized objects, for example JSON or XML
- lternatively, a tightly controlled whitelist can limit the vulnerability of code, but be aware of the existence of so-called Bypass Gadgets, which can circumvent such protection measures.
- Usage of something like: 
    - FastJson - `com.alibaba:fastjson`
    - FasterXML - `com.fasterxml.jackson.core:jackson-databind`
    - Kryo - `com.esotericsoftware:kryo and com.esotericsoftware:kryo5`
 
  
### [4] Disabled Spring CSRF protection
- When you use Spring, Cross-Site Request Forgery (CSRF) protection is enabled by default. Spring's recommendation is to use CSRF protection for any request that could be processed by a browser client by normal users.

### [5] Insecure randomness
- The `java.util.Random` random number generator is not cryptographically secure. Use a secure random number generator such as `java.security.SecureRandom` instead.
- Use a cryptographically secure pseudo-random number generator if the output is to be used in a security-sensitive context. As a general rule, a value should be considered "security-sensitive" if predicting it would allow the attacker to perform an action that they would otherwise be unable to perform. For example, if an attacker could predict the random password generated for a new user, they would be able to log in as that new user.

### [6] Missing JWT signature check 
- Always verify the signature by using either the parseClaimsJws and parsePlaintextJws methods or by overriding the onPlaintextJws or onClaimsJws of JwtHandlerAdapter.


---

## 🔁 Κατάσταση μετά τη Διόρθωση

| Ευπάθεια | Κατάσταση | Σχόλιο |
|----------|-----------|--------|
| [1] XXE Attack | ✅ Pending | ???  |
| [2] Command Injection | ✅ Fixed | Αφαιρέθηκε η χρήση `Runtime.exec`. |
| [3] Path Traversal | ✅ Fixed | Προστέθηκε έλεγχος με canonical path. |
| [4] XSS | ✅ Fixed | Εφαρμόστηκε HTML encoding. |
| [5] Hardcoded Credentials | ✅ Fixed | Μεταφέρθηκαν σε αρχείο `.env`. |

---
