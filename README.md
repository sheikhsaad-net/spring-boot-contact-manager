# Contact Management

A small Java 11 and Spring Boot application for storing and managing contacts. It exposes a REST API and includes a Thymeleaf HTML contact form template.

## Features

- Contact records with name, company, phone number, and email.
- REST endpoints to list, retrieve, create, and delete contacts.
- Spring Data JPA persistence.
- H2 in-memory database configuration for local development.
- A Thymeleaf contact form template.
- Maven Wrapper and a Dockerfile.

## Technology

- Java 11
- Spring Boot 2.5
- Maven
- Spring Web, Spring Data JPA, Thymeleaf, and Bean Validation
- H2 database
- Docker

## Run locally

### Requirements

Install a Java 11 JDK. Maven is optional because the repository includes Maven Wrapper scripts.

### Start the application

Clone the repository and enter its directory:

```sh
git clone https://github.com/sheikhsaad-net/contact-management.git
cd contact-management
```

On Linux or macOS:

```sh
./mvnw spring-boot:run
```

On Windows:

```bat
mvnw.cmd spring-boot:run
```

The API uses port `8080` by default.

## REST API

All API paths are relative to `http://localhost:8080`.

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/contacts` | List contacts |
| `GET` | `/contacts/{id}` | Retrieve a contact |
| `POST` | `/contacts` | Create a contact |
| `DELETE` | `/contacts/{id}` | Delete a contact |

Example create request:

```sh
curl -X POST http://localhost:8080/contacts \
  -H "Content-Type: application/json" \
  -d '{"name":"Ada Lovelace","company":"Example Ltd","number":"+1-555-0100","email":"ada@example.com"}'
```

## HTML form

The form template is at `src/main/resources/templates/index.html` and submits to `POST /saveContact`. **The current controllers do not define a `GET /` handler to render that template**, despite the earlier README describing a working form at the application root. Add a view route before relying on the browser form.

## Database and privacy

The default datasource is an in-memory H2 database, so contact records are temporary and will not survive an application restart. The checked-in `schema.sql` creates `CONTACT_MODEL`, while the JPA entity maps to `Contacts`; review this naming mismatch before changing database configuration.

The CRUD endpoints currently have no authentication or authorization. Do not expose this service publicly with real contact details until access control and persistent database configuration are in place.

## Docker

Build the application JAR first, then build and run the image:

```sh
./mvnw clean package
docker build -t contact-management .
docker run --rm -p 8080:8080 contact-management
```

On Windows, replace `./mvnw` with `mvnw.cmd`. The Dockerfile expects `target/contact-0.0.1-SNAPSHOT.jar`; update the Dockerfile if the Maven project version changes.

## Tests and CI

Run the Maven test suite with:

```sh
./mvnw test
```

The GitHub Actions workflow also runs Maven checks on pushes and pull requests to `master`. It requires the `COVERALLS_REPO_TOKEN` repository secret for its Coveralls reporting step.

## License

There is no top-level `LICENSE` file. Confirm the intended license before redistributing this project.
