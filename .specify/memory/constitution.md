<!--
  Sync Impact Report (remove before commit)
  ==========================================
  Version change: (none) → 1.0.0
  Modified principles:
    - "Extreme Minimalism" → "I. Extreme Minimalism" (restructured, wording tightened)
    - "No Bloat" → "II. No Bloat" (restructured, wording tightened)
    - "Single Controller" → "III. Single Controller" (restructured)
    - "Single Endpoint" → "IV. Single Endpoint" (restructured)
    - "Modern Java" → "V. Modern Java" (restructured)
    - "Strict Adherence" → "VI. Strict Adherence" (restructured)
  Added sections:
    - Governance (new — amendment procedure, versioning policy, compliance review)
  Removed sections:
    - (none)
  Deferred items:
    - (none)
-->

# hello-world-sb4 Constitution

## Core Principles

### I. Extreme Minimalism
Generate ONLY what is strictly necessary to run the application. Every file,
class, and configuration entry MUST have a direct, justified purpose. No
speculative code, no premature abstractions, no placeholder content.

### II. No Bloat
The project MUST NOT include:
- Database dependencies or ORM configurations
- Extra services, background workers, or message brokers
- Complex DTOs, mappers, or transformation layers
- Security configurations or authentication mechanisms

Any addition MUST be justified as strictly necessary for the application to
compile and serve its single endpoint.

### III. Single Controller
The application MUST contain exactly one REST Controller class. Additional
controllers MUST NOT be introduced regardless of feature growth. All request
handling MUST route through this single controller.

### IV. Single Endpoint
The application MUST expose exactly one endpoint: `GET /hello`, returning
plain text `"Hello World"`. Additional endpoints MUST NOT be added. The
response content type MUST be `text/plain`.

### V. Modern Java
The project MUST target Java 21. Java 21 features (records, pattern matching,
virtual threads, etc.) SHOULD be used where they improve readability. Code
MUST remain clear and approachable; language features MUST NOT be used solely
to demonstrate novelty.

### VI. Strict Adherence
All code and configuration MUST follow Spring Boot 4 best practices and the
standard Maven project structure. Deviations from Spring Boot conventions
MUST be explicitly justified in code comments or documentation.

## Core Stack

| Component    | Value            |
|-------------|------------------|
| Language     | Java 21          |
| Framework    | Spring Boot 4.x  |
| Build Tool   | Maven            |

The stack MUST NOT be extended with additional frameworks, libraries, or
build plugins unless strictly required for compilation or test execution.

## Deliverables

The project MUST produce exactly the following artifacts:

1. `pom.xml` — Spring Boot 4 dependencies only; no extraneous starters.
2. Main application class — standard `@SpringBootApplication` entry point.
3. Single REST Controller — one class, one `@GetMapping` method.
4. `application.yml` — minimal or empty; no unnecessary configuration keys.

No additional files, configuration classes, or resource files MAY be generated
without explicit justification.

## Governance

This constitution supersedes all ad-hoc practices and prior informal
conventions for the hello-world-sb4 project.

**Amendment procedure:**
1. Propose the change via a pull request that modifies this document.
2. The PR description MUST state the rationale and impact.
3. All existing principles MUST be reviewed for consistency with the change.
4. The version number MUST be incremented per the versioning policy below.

**Versioning policy:**
- MAJOR — backward-incompatible removal or redefinition of a principle.
- MINOR — new principle or section added; material expansion of guidance.
- PATCH — clarifications, wording, typo fixes, non-semantic refinements.

**Compliance review:**
- Every code review MUST verify compliance with the principles above.
- Added complexity MUST be justified against Principle I (Extreme Minimalism)
  and Principle II (No Bloat).

**Version**: 1.0.0 | **Ratified**: 2026-10-15 | **Last Amended**: 2026-10-15
