# Implementation Plan: Minimal Hello World

**Branch**: `001-minimal-hello-world` | **Date**: 2026-10-15 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `specs/001-minimal-hello-world/spec.md`

## Summary

Build a minimal Spring Boot 4 web application exposing a single REST endpoint
(`GET /hello`) that returns the plain-text string "Hello World". The project
uses Maven as the build tool, targets Java 21, and contains no database,
security, DTOs, or unnecessary services. Total source file count is 5 or fewer.

## Technical Context

**Language/Version**: Java 21

**Primary Dependencies**: Spring Boot 4.x (spring-boot-starter-web)

**Storage**: N/A (no database)

**Testing**: JUnit 5 + Spring Boot Test (@WebMvcTest + MockMvc)

**Target Platform**: JVM (Linux/Windows/macOS server)

**Project Type**: Web service (minimal REST API)

**Performance Goals**: Response within 1 second (SC-001); startup within 10 seconds (SC-002)

**Constraints**: ≤ 5 source files (SC-003); no database, security, DTOs, or background workers (Principles I, II)

**Scale/Scope**: Single endpoint, single controller, stateless

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle                  | Status | Notes                                                        |
|----------------------------|--------|--------------------------------------------------------------|
| I. Extreme Minimalism      | PASS   | Only essential files: pom.xml, Application, Controller, test |
| II. No Bloat               | PASS   | No DB, no security, no DTOs, no workers                      |
| III. Single Controller     | PASS   | Exactly one `@RestController` class                          |
| IV. Single Endpoint        | PASS   | Only `GET /hello` exposed                                    |
| V. Modern Java             | PASS   | Target Java 21; no unnecessary language features             |
| VI. Strict Adherence       | PASS   | Standard Spring Boot layout and conventions                  |

**Post-Phase 1 Re-check**: All principles still pass. Design introduces no
additional complexity beyond what the specification requires.

## Project Structure

### Documentation (this feature)

```text
specs/001-minimal-hello-world/
├── plan.md              # This file
├── spec.md              # Feature specification
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/
│   └── openapi.yaml     # Phase 1 output — API contract
├── checklists/
│   └── requirements.md  # Spec quality checklist
└── tasks.md             # Phase 2 output (via /speckit.tasks)
```

### Source Code (repository root)

```text
src/
├── main/
│   ├── java/
│   │   └── com/example/hello/
│   │       ├── HelloApplication.java      # @SpringBootApplication entry point
│   │       └── HelloController.java       # @RestController, single @GetMapping
│   └── resources/
│       └── application.yml                # Empty or minimal (no config keys)
└── test/
    └── java/
        └── com/example/hello/
            └── HelloControllerTest.java   # @WebMvcTest controller slice test

pom.xml                                    # Maven build — spring-boot-starter-web only
```

**Structure Decision**: Standard single-project Spring Boot layout. No
multi-module structure, no frontend, no mobile — not applicable for this
feature. Five source files total (pom.xml, HelloApplication, HelloController,
application.yml, HelloControllerTest), meeting SC-003.

## Complexity Tracking

> No constitution violations. All gates pass.

## Follow-up Actions

1. Run `/speckit.tasks` to generate `tasks.md` from this plan.
2. Update spec FR-010 to replace "Gradle with Kotlin DSL" with "Maven" (user
   confirmed Maven as the correct build tool).
