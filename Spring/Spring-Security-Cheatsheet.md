### **📌 Important Classes & Interfaces in Spring Security 6+** 🚀  

Spring Security 6+ has undergone significant changes, especially with the removal of the default `WebSecurityConfigurerAdapter`. Below is a list of the most important classes and interfaces categorized by their purpose.  

---

### **1️⃣ Core Security Configuration**
| Class / Interface | Description |
|------------------|------------|
| **`SecurityFilterChain`** | Replaces `WebSecurityConfigurerAdapter` for configuring security in Spring Boot 3+. |
| **`SecurityConfigurerAdapter<T, B>`** | Base class for custom security configurations. |
| **`AuthenticationManager`** | Responsible for authenticating users. |
| **`UserDetailsService`** | Interface to load user-specific security details (e.g., from a database). |
| **`PasswordEncoder`** | Interface for encoding and verifying passwords (e.g., `BCryptPasswordEncoder`). |

---

### **2️⃣ Authentication & Authorization**
| Class / Interface | Description |
|------------------|------------|
| **`UsernamePasswordAuthenticationToken`** | Represents an authentication request (username & password). |
| **`JwtAuthenticationToken`** | Used for JWT-based authentication. |
| **`AbstractAuthenticationToken`** | Base class for authentication tokens. |
| **`GrantedAuthority`** | Represents a user’s role or privilege (e.g., `ROLE_ADMIN`). |
| **`SimpleGrantedAuthority`** | Concrete implementation of `GrantedAuthority`. |
| **`SecurityContext`** | Holds the currently authenticated user details. |
| **`SecurityContextHolder`** | Stores `SecurityContext` for the current thread. |

---

### **3️⃣ User Management**
| Class / Interface | Description |
|------------------|------------|
| **`UserDetails`** | Represents a user in Spring Security. |
| **`User`** | Default implementation of `UserDetails`. |
| **`UserDetailsManager`** | Interface for managing users (e.g., `JdbcUserDetailsManager`). |

---

### **4️⃣ Filters & Middleware**
| Class / Interface | Description |
|------------------|------------|
| **`OncePerRequestFilter`** | Ensures a filter runs only once per request (useful for JWT filters). |
| **`BasicAuthenticationFilter`** | Processes HTTP Basic authentication. |
| **`BearerTokenAuthenticationFilter`** | Extracts and processes bearer tokens (JWT). |
| **`CorsFilter`** | Handles Cross-Origin Resource Sharing (CORS). |
| **`CsrfFilter`** | Protects against Cross-Site Request Forgery (CSRF) attacks. |
| **`ExceptionTranslationFilter`** | Converts authentication exceptions into HTTP responses. |

---

### **5️⃣ OAuth2 & JWT**
| Class / Interface | Description |
|------------------|------------|
| **`OAuth2LoginAuthenticationFilter`** | Handles OAuth2 login flows. |
| **`OAuth2AuthorizationRequestRedirectFilter`** | Redirects users to the OAuth2 provider (Google, GitHub, etc.). |
| **`JwtDecoder`** | Decodes JWT tokens. |
| **`JwtEncoder`** | Encodes JWT tokens. |
| **`OAuth2UserService`** | Loads user details from an OAuth2 provider. |

---

### **6️⃣ Security Annotations**
| Annotation | Description |
|-----------|------------|
| **`@EnableWebSecurity`** | Enables Spring Security in the application. |
| **`@EnableMethodSecurity`** | Enables method-level security annotations like `@PreAuthorize`. |
| **`@PreAuthorize("hasRole('ADMIN')")`** | Restricts access to methods based on roles. |
| **`@PostAuthorize`** | Evaluates access after method execution. |
| **`@Secured("ROLE_ADMIN")`** | Alternative to `@PreAuthorize`, but less flexible. |
| **`@RolesAllowed("ROLE_USER")`** | JSR-250 annotation for role-based access control. |

---

### **📌 Summary**
- **Security Configuration**: `SecurityFilterChain`, `AuthenticationManager`
- **Authentication & Authorization**: `UserDetails`, `GrantedAuthority`
- **Filters**: `OncePerRequestFilter`, `BasicAuthenticationFilter`
- **OAuth2 & JWT**: `JwtDecoder`, `OAuth2UserService`
- **Security Annotations**: `@PreAuthorize`, `@RolesAllowed`

