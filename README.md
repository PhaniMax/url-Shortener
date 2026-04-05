# URL Shortening Service

A backend web application built using **Spring Boot** that converts long URLs into compact **7-character short links**, similar to services like **TinyURL** and **Bitly**.

This project provides URL shortening, redirection, expiration management, and persistent storage using **Spring Data JPA** and **H2 Database**.

---

## Features

- Convert long URLs into unique **7-character short links**
- Uses **MurmurHash (Guava)** for efficient short URL generation
- Supports **multiple requests for the same original URL**
- Generates a **new unique short URL every time**
- Automatic **redirection to the original URL**
- **Customizable expiration time** for each shortened URL
- Automatically removes expired links
- REST API based architecture
- Structured error handling with DTO responses

---

## Tech Stack

- **Java**
- **Spring Boot**
- **Spring Web**
- **Spring Data JPA**
- **H2 Database**
- **Google Guava**
- **Apache Commons Lang**
- **Maven**

---

## Hashing Logic Used

This project uses **Guava’s MurmurHash3 (32-bit)** hashing algorithm to generate compact short links.

### Implementation

```java
String encodeUrl = "";
LocalDateTime time = LocalDateTime.now();

encodeUrl = Hashing.murmur3_32()
        .hashString(url.concat(time.toString()), StandardCharsets.UTF_8)
        .toString();

return encodeUrl;
```

### How it Works

- The original URL is concatenated with the **current timestamp**
- The combined string is passed to **MurmurHash3**
- A compact hash string is generated
- The first **7 characters** are used as the short URL code

This approach ensures:

- **fast hashing performance**
- **unique short links**
- **new short link generation even for the same original URL**
- **low collision probability**

By combining the URL with `LocalDateTime.now()`, even repeated requests for the same URL generate **different short links**.

---

## Project Structure

```text
src
└── main
    ├── java
    │   └── com.phanimax.UrlShorteningService
    │       ├── controller
    │       ├── model
    │       ├── repository
    │       ├── service
    │       └── UrlShorteningServiceApplication
    └── resources
        └── application.properties
```

---

## API Endpoints

### Generate Short URL

```http
POST /generate
```

### Request Body

```json
{
  "url": "https://www.example.com/very/long/url",
  "expirationDate": "2026-04-06T10:30:00"
}
```

### Response

```json
{
  "originalUrl": "https://www.example.com/very/long/url",
  "shortLink": "a1b2c3d",
  "expirationDate": "2026-04-06T10:30:00"
}
```

---

### Redirect to Original URL

```http
GET /{shortLink}
```

Example:

```http
GET /a1b2c3d
```

Redirects the user to the original URL if the link exists and is not expired.

---

## Expiration Handling

Each shortened URL contains an **expiration timestamp**.

When a short URL is accessed:

- system checks whether it exists
- validates whether it is expired
- redirects if valid
- deletes and invalidates if expired

This expiration duration is fully customizable.

---

## Database

The application uses **H2 in-memory database** for storing URL mappings.

Access console:

```text
http://localhost:8080/h2-console
```

Example credentials:

```text
JDBC URL: jdbc:h2:mem:testdb
Username: sa
Password: password
```

---

## How to Run

Clone the repository:

```bash
git clone https://github.com/your-username/UrlShorteningService.git
```

Move into the project:

```bash
cd UrlShorteningService
```

Run application:

```bash
mvn spring-boot:run
```

Server runs at:

```text
http://localhost:8080
```

---

## Learning Outcomes

This project demonstrates practical implementation of:

- REST API development
- hashing-based URL generation
- MurmurHash algorithm usage
- backend service architecture
- database persistence
- URL redirection
- expiration lifecycle management
- DTO-based response handling
- clean layered Spring Boot architecture

---

## Author

**Phanindra Mekala**  
