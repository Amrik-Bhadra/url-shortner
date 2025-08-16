# URL Shortener
    Spring Boot Application to short long urls
---
## Spring Boot Project Setup
- Spring Initializr is used with following configurations:
    - Build Tool: **Maven**
    - Language: **Java**
    - Spring Boot: **v3.5.4**
    - Project Metadata:
        - Group: com.url
        - Artifact: url-shortner-sb
        - Name: url-shortner-sb
        - Package Name: com.url.shortener
        - Packaging: **Jar**
        - Java Version: **21**
    - Dependencies:
        - Spring Web
        - Lombok
        - Spring Data JPA
        - MySQL Driver

---
## Database Setup
### EDA
![url-shortner-sb-eda](https://github.com/user-attachments/assets/10ad6511-2a31-406c-97da-a7466851ff82)

### Add properties in application.properties
- go to src > main > resources > application.properties
- add following properties
  - `spring.datasource.url=jdbc:mysql://localhost:3306/urlshortenerdb`
  - `spring.datasource.username=dbtester`
  - `spring.datasource.password=dbtester@123`
  - `spring.jpa.hibernate.ddl-auto=update`
  - `spring.jpa.show-sql=true`
  - `spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect`

---
## Authentication Setup

### How are Tokens Sent to Server?
- Tokens are sent using HTTP Authorization Header
- **Format:** `Authorization: Bearer<token>`
- **Header name:** Authorization
- **Sample Token:**
  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30

### JWT (JSON Web Tokens)
- has three parts: header.payload.signature
- What we will need:
  1. **JwtUtils:** 
     - contains utility methods for generating, parsing, and validating JWT
     - include generating token from username, validating a JWT, and extracting username from the token
  
  2. **JwtAuthenticationFilter:**
     - filters incoming request to check for a valid JWT in the header, setting the authentication context if the token is valid
     - Extracts JWT from request header, validates it, and configures the Spring Security context with user details if the token is valid

  3. **JwtAuthenticationResponse:**
      - DTO for JWT Authentication Response

  4. **SecurityConfig:**
      - Configures Spring Security filters and rules for the application
      - sets up the security filter chain, permitting or denying access based on paths and roles

### Spring Security
- Spring Security provides the tools and structure to authenticate and authorize users
- sprint security has its own inbuilt implementation of User which would work, but in production grade application, we build our own custom file

    #### UserDetailServiceImpl
  - This is a service that loads User data from database in security context when request is valid (token is valid)
  - Bridges the gap between the database (User entity) and Spring Security (UserDetails Interface)

   #### UserDetailImpl
  - This is a custom implementation of Spring Security's UserDetail interface
  - Needed to represent the authenticated user in Spring Security

### Working Steps:
1. JwtAuthenticationFilter:
   - Intercepts HTTP requests and extracts JWT token
2. JwtUtils:
   - Generated, validate tokens and provides helper methods
3. UserDetailsServiceImpl:
   - Fetch User details from the database
4. UserDetailsImpl:
   - Provides spring security-compatible representation of the user for authentication and authorization

### JWT implementation
1. Add Following Dependency in pom.xml

    ```xml
    <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    
    <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```
2. Add JWT  
    Go to this link: https://github.com/jwtk/jjwt  
    in Readme, go to installation > JDK Projects > Maven
    
    ```xml
   <dependencies>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-api</artifactId>
            <version>0.12.7</version>
        </dependency>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
             <artifactId>jjwt-impl</artifactId>
            <version>0.12.7</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-jackson</artifactId> <!-- or jjwt-gson if Gson is preferred -->
            <version>0.12.7</version>
            <scope>runtime</scope>
        </dependency>
   </dependencies>
    ```
3. 
---
