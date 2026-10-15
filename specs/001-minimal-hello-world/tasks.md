# Tasks: Minimal Hello World

**Input**: Design documents from `specs/001-minimal-hello-world/`

**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Included — spec.md implies testing via success criteria and Spring Boot best practices (Principle VI).

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/`, `src/test/` at repository root
- Standard Maven layout: `src/main/java/`, `src/main/resources/`, `src/test/java/`

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [x] T001 Create Maven project structure per implementation plan (`pom.xml`, `src/main/java/`, `src/main/resources/`, `src/test/java/`)
- [x] T002 Initialize Maven project with Spring Boot 4.x dependencies in `pom.xml` (spring-boot-starter-web, spring-boot-starter-test, junit-jupiter)
- [x] T003 [P] Create base package structure `com.example.hello` in `src/main/java/com/example/hello/`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T004 Create main application class `HelloApplication.java` in `src/main/java/com/example/hello/HelloApplication.java` with `@SpringBootApplication` annotation
- [x] T005 Create minimal `application.yml` in `src/main/resources/application.yml` (empty or with comment only)

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Retrieve Greeting (Priority: P1) 🎯 MVP

**Goal**: A user (or automated client) sends an HTTP GET request to the `/hello` endpoint and receives the plain-text response "Hello World". This is the sole interaction the application supports.

**Independent Test**: Can be fully tested by sending a single GET request to `/hello` and verifying the response body equals "Hello World" with content type `text/plain`. Delivers the entire value of the application.

### Tests for User Story 1

> **NOTE: Write these tests FIRST, ensure they FAIL before implementation**

- [x] T006 [P] [US1] Create controller test `HelloControllerTest.java` in `src/test/java/com/example/hello/HelloControllerTest.java` with `@WebMvcTest(HelloController.class)` annotation
- [x] T007 [US1] Add test method `hello_ReturnsOkStatus_WithHelloWorldBody()` in `HelloControllerTest.java` that verifies GET `/hello` returns status 200, body "Hello World", content-type "text/plain"
- [x] T008 [P] [US1] Add test method `hello_Returns405_WhenPostMethodUsed()` in `HelloControllerTest.java` that verifies POST `/hello` returns status 405
- [x] T009 [P] [US1] Add test method `hello_Returns404_WhenWrongPathRequested()` in `HelloControllerTest.java` that verifies GET `/goodbye` returns status 404

### Implementation for User Story 1

- [x] T010 [US1] Create REST controller `HelloController.java` in `src/main/java/com/example/hello/HelloController.java` with `@RestController` annotation
- [x] T011 [US1] Implement `hello()` method in `HelloController.java` with `@GetMapping("/hello")` that returns exact string "Hello World" (11 characters, no trailing whitespace)

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - Application Startup (Priority: P1)

**Goal**: A developer starts the application and it becomes ready to serve requests without errors, without external dependencies, and without manual configuration.

**Independent Test**: Can be tested by launching the application and observing that it starts successfully, binds to a port, and responds to the first request without requiring database connections, environment variables, or external service configuration.

### Tests for User Story 2

- [x] T012 [US2] Create integration test `HelloApplicationTests.java` in `src/test/java/com/example/hello/HelloApplicationTests.java` with `@SpringBootTest` annotation
- [x] T013 [US2] Add test method `contextLoads()` in `HelloApplicationTests.java` that verifies application context starts without errors

### Implementation for User Story 2

- [x] T014 [US2] Verify application starts successfully by running `mvn spring-boot:run` and confirming no startup errors or missing configuration warnings
- [x] T015 [US2] Verify first request to `/hello` returns immediately without lazy-initialization delays

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [x] T016 [P] Run `mvn clean test` and verify all tests pass
- [x] T017 [P] Run `mvn clean package` and verify JAR builds successfully
- [x] T018 Run quickstart.md validation scenarios (build, run, curl tests)
- [x] T019 Verify source file count ≤ 5 (SC-003): pom.xml, HelloApplication.java, HelloController.java, application.yml, HelloControllerTest.java
- [x] T020 Code cleanup and removal of any unnecessary comments or whitespace

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-4)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (both are P1, so order doesn't matter)
- **Polish (Phase 5)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P1)**: Can start after Foundational (Phase 2) - Independent of US1, but US1 provides the endpoint that US2 validates

### Within Each User Story

- Tests MUST be written and FAIL before implementation
- Controller before endpoint method
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel (T003)
- All tests for User Story 1 marked [P] can run in parallel (T006, T008, T009)
- Polish tasks marked [P] can run in parallel (T016, T017)

---

## Parallel Example: User Story 1

```bash
# Launch all tests for User Story 1 together:
Task: "Create controller test HelloControllerTest.java" (T006)
Task: "Add test method hello_Returns405_WhenPostMethodUsed()" (T008)
Task: "Add test method hello_Returns404_WhenWrongPathRequested()" (T009)

# Then implement controller:
Task: "Create REST controller HelloController.java" (T010)
Task: "Implement hello() method" (T011)
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently with `curl http://localhost:8080/hello`
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Verify startup behavior
4. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1 (endpoint implementation)
   - Developer B: User Story 2 (startup validation)
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Both user stories are P1 — they can be implemented in any order, but US1 is the core functionality
