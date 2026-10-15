# Quickstart Validation Guide: Minimal Hello World

**Feature**: 001-minimal-hello-world
**Date**: 2026-10-15

## Prerequisites

- Java 21 JDK installed and available in PATH
- Maven 3.8+ installed and available in PATH
- HTTP client (curl, Postman, or browser)

## Build & Run

### 1. Build the project

```bash
mvn clean package
```

**Expected outcome**: Build succeeds with no errors. JAR file is created in
`target/` directory.

---

### 2. Start the application

```bash
mvn spring-boot:run
```

**Expected outcome**: Application starts successfully, logs show Spring Boot
banner and "Started HelloApplication in X.XXX seconds". No errors or warnings
about missing configuration.

---

### 3. Verify the endpoint

```bash
curl -i http://localhost:8080/hello
```

**Expected response**:
```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 11

Hello World
```

**Validation checklist**:
- [ ] Status code is `200 OK`
- [ ] Content-Type header is `text/plain`
- [ ] Response body is exactly `Hello World` (11 characters, no trailing newline)

---

## Test Scenarios

### Scenario 1: Correct endpoint and method

**Action**: Send GET request to `/hello`

```bash
curl http://localhost:8080/hello
```

**Expected**: Response body is `Hello World`

---

### Scenario 2: Wrong HTTP method

**Action**: Send POST request to `/hello`

```bash
curl -X POST http://localhost:8080/hello
```

**Expected**: Status code `405 Method Not Allowed`

---

### Scenario 3: Wrong path

**Action**: Send GET request to `/goodbye`

```bash
curl http://localhost:8080/goodbye
```

**Expected**: Status code `404 Not Found`

---

### Scenario 4: Trailing slash

**Action**: Send GET request to `/hello/`

```bash
curl http://localhost:8080/hello/
```

**Expected**: Status code `404 Not Found` (only exact path `/hello` is valid)

---

### Scenario 5: Run automated tests

```bash
mvn test
```

**Expected outcome**: All tests pass. Test report shows:
- `HelloControllerTest` executed
- Tests verify status 200, body "Hello World", content-type "text/plain"

---

## Success Criteria Validation

| Criterion                | How to Verify                                    | Target       |
|--------------------------|--------------------------------------------------|--------------|
| SC-001: Response < 1s    | Measure time with `curl -w "%{time_total}"`      | < 1 second   |
| SC-002: Startup < 10s    | Observe startup log timestamp                    | < 10 seconds |
| SC-003: ≤ 5 source files | Count files in `src/`                            | ≤ 5 files    |
| SC-004: 100% success     | Send 100 requests, verify all return 200         | 100%         |
| SC-005: Dev setup < 3min | Time from clone to first successful request      | < 3 minutes  |

---

## Troubleshooting

### Application fails to start

- **Symptom**: Error about missing database configuration
- **Cause**: Unnecessary dependencies in `pom.xml`
- **Fix**: Remove database-related dependencies (H2, JPA, etc.)

### Endpoint returns 404

- **Symptom**: `curl http://localhost:8080/hello` returns 404
- **Cause**: Controller not properly annotated or wrong path
- **Fix**: Verify `@GetMapping("/hello")` annotation on controller method

### Response includes extra whitespace

- **Symptom**: Response body is `"Hello World\n"` or `"Hello World "`
- **Cause**: Controller returns string with trailing characters
- **Fix**: Ensure controller returns exactly `"Hello World"` with no concatenation

### Content-Type is application/json

- **Symptom**: Response header shows `Content-Type: application/json`
- **Cause**: Controller annotated incorrectly or return type is object
- **Fix**: Ensure controller returns `String` and is annotated with `@RestController`

---

## Cleanup

Stop the application:
```bash
# Press Ctrl+C in the terminal where mvn spring-boot:run is running
```

Clean build artifacts:
```bash
mvn clean
```
