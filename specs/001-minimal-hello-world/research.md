# Research: Minimal Hello World

**Feature**: 001-minimal-hello-world
**Date**: 2026-10-15

## Unknowns Resolved

### 1. Build Tool: Maven (Confirmed)

**Context**: Feature specification (FR-010) initially stated "Gradle with Kotlin
DSL", but user clarified that Maven is the correct build tool, aligning with
constitution v1.0.0 Core Stack.

**Decision**: Use **Maven** as the build tool.

**Rationale**:
- Constitution v1.0.0 specifies Maven in Core Stack table.
- User explicitly confirmed Maven is correct.
- Aligns with Principle VI (Strict Adherence to Spring Boot conventions).

**Alternatives Considered**:
- Gradle Kotlin DSL: Rejected — contradicts constitution and user clarification.
- Gradle Groovy DSL: Rejected — same reason.

**Impact**: Specification FR-010 requires update from "Gradle with Kotlin DSL"
to "Maven". This should be done after plan completion.

---

### 2. Testing Framework

**Context**: Specification does not explicitly define a testing framework.

**Decision**: Use **JUnit 5** (Jupiter) with Spring Boot Test.

**Rationale**:
- JUnit 5 is the standard testing framework for modern Java applications.
- Spring Boot Test provides convenient integration testing support.
- Aligns with Principle I (Extreme Minimalism) — no additional testing
  frameworks or layers.
- Industry standard for Spring Boot 4.x projects.

**Alternatives Considered**:
- TestNG: Rejected — less common in Spring Boot ecosystem, adds no value over
  JUnit 5.
- Spock: Rejected — requires Groovy, violates minimalism principle.
- No tests: Rejected — violates best practices and Principle VI (Strict
  Adherence to Spring Boot best practices).

---

### 3. Response Format Details

**Context**: Specification states response is "Hello World" plain text, but
does not clarify encoding or trailing characters.

**Decision**: Response is exactly `Hello World` (11 bytes, UTF-8 encoded, no
trailing newline or whitespace).

**Rationale**:
- Specification FR-002 states "exact string, no trailing whitespace or
  newline".
- UTF-8 is the standard encoding for HTTP responses.
- Minimalism principle requires no unnecessary bytes.

**Alternatives Considered**:
- Include trailing newline: Rejected — violates FR-002.
- JSON response: Rejected — violates FR-003 (must be text/plain).

---

### 4. Server Port Configuration

**Context**: Specification does not mention port configuration.

**Decision**: Use default Spring Boot port **8080**. No custom configuration.

**Rationale**:
- Assumption in spec.md: "Default server port (8080) is acceptable".
- Aligns with Principle I (Extreme Minimalism) — no unnecessary configuration.
- Standard convention for Spring Boot applications.

**Alternatives Considered**:
- Custom port via application.yml: Rejected — violates minimalism, no
  requirement for custom port.
- Environment variable override: Rejected — violates FR-009 (no external
  configuration required).

---

### 5. Logging Configuration

**Context**: Specification does not mention logging requirements.

**Decision**: Use **Spring Boot default logging** (Logback, INFO level). No
custom configuration.

**Rationale**:
- Assumption in spec.md: "No logging configuration beyond the framework
  default is required".
- Aligns with Principle I (Extreme Minimalism).
- Default logging is sufficient for a minimal application.

**Alternatives Considered**:
- Custom logback.xml: Rejected — violates minimalism, no requirement.
- Disable logging: Rejected — violates best practices, makes debugging
  impossible.

---

## Best Practices Researched

### Spring Boot 4.x Minimal Project Structure

**Decision**: Follow standard Maven project layout:
```
src/
├── main/
│   ├── java/
│   │   └── com/example/hello/
│   │       ├── HelloApplication.java
│   │       └── HelloController.java
│   └── resources/
│       └── application.yml (empty or minimal)
└── test/
    └── java/
        └── com/example/hello/
            └── HelloControllerTest.java
```

**Rationale**:
- Standard Spring Boot convention (Principle VI).
- Minimal file count (5 source files, per SC-003).
- Clear separation of main and test code.

---

### REST Controller Best Practices

**Decision**: Single controller with single `@GetMapping` method.

**Rationale**:
- Complies with Principle III (Single Controller) and Principle IV (Single
  Endpoint).
- `@RestController` annotation is idiomatic for Spring Boot REST APIs.
- Return type `String` with `text/plain` content type is the simplest approach.

**Implementation Pattern**:
```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

---

### Testing REST Endpoints in Spring Boot

**Decision**: Use `@WebMvcTest` for controller slice testing.

**Rationale**:
- `@WebMvcTest` loads only the web layer, not the full application context.
- Aligns with minimalism — no database, security, or other layers loaded.
- Fast test execution (no full startup).
- Industry standard for Spring Boot controller testing.

**Test Pattern**:
```java
@WebMvcTest(HelloController.class)
class HelloControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    void hello() throws Exception {
        mockMvc.perform(get("/hello"))
               .andExpect(status().isOk())
               .andExpect(content().string("Hello World"))
               .andExpect(content().contentType("text/plain")));
    }
}
```

---

## Summary of Decisions

| Topic                  | Decision                          | Rationale                                    |
|------------------------|-----------------------------------|----------------------------------------------|
| Build Tool             | Maven                             | Constitution + user confirmation             |
| Testing Framework      | JUnit 5 + Spring Boot Test        | Industry standard, minimal                   |
| Response Format        | "Hello World" (11 bytes, UTF-8)   | Per FR-002, no trailing whitespace           |
| Server Port            | 8080 (default)                    | Per assumption, minimalism                   |
| Logging                | Spring Boot default (Logback)     | Per assumption, minimalism                   |
| Project Structure      | Standard Spring Boot layout       | Convention (Principle VI)                    |
| Controller Pattern     | @RestController + @GetMapping     | Idiomatic, minimal                           |
| Testing Pattern        | @WebMvcTest + MockMvc             | Fast, minimal, industry standard             |

## Follow-up Actions

1. **Spec Update**: FR-010 requires update from "Gradle with Kotlin DSL" to
   "Maven" to align with constitution and user clarification.
2. **Proceed to Phase 1**: Generate data-model.md, contracts/, quickstart.md.
