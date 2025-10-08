# Repository Guidelines

## Project Structure & Module Organization
- Java sources live in `src/main/java`, with runtime resources (dialect service descriptors, properties) under `src/main/resources`.
- Tests live in `src/test/java`, split into unit suites (`.../dialect`, `.../source`, `.../sink`) and integration suites (`.../integration`) that depend on Testcontainers and embedded databases.
- Sample connector configurations for local runs are in `config/`, Maven assemblies in `src/assembly/`, and release tooling in `scripts/`.
- Contribution assets such as the PR template reside in `docs/`, and build metadata sits in `pom.xml`, `service.yml`, and `sonar-project.properties`.

## Build, Test, and Development Commands
- `mvn clean package` — compile, run the full test suite, and assemble the Kafka Connect plugin under `target/`.
- `mvn test` — execute fast unit tests only; ideal for iterating on logic in `src/main/java`.
- `mvn verify -Pintegration-tests` — run integration tests that exercise live database dialects; expect Docker/Testcontainers requirements.
- `mvn checkstyle:check` — enforce the shared Confluent Checkstyle rules defined by the parent POM.

## Coding Style & Naming Conventions
- Java sources follow the Confluent Checkstyle configuration (4-space indentation, brace-on-same-line, camelCase identifiers).
- Type and constant names should match existing patterns (`JdbcSourceTask`, `TABLES_FETCHED`) and stay consistent across source/sink packages.
- Prefer descriptive class/package names aligned with connector roles (e.g., `...source.TableQuerier`, `...sink.metadata.SchemaPair`).
- Run `mvn formatter:format` if the parent introduces formatting helpers; otherwise rely on IDE settings that respect Checkstyle.

## Testing Guidelines
- Unit tests use JUnit 4/5, EasyMock, and Mockito; place fixtures beside the code under test and name classes `<ClassUnderTest>Test`.
- Integration tests rely on the Maven Failsafe plugin plus Testcontainers/embedded databases; gate them behind the `integration-tests` profile.
- JaCoCo thresholds are enforced at verify time (80% line, 78% instruction, etc.); ensure new logic is covered before opening a PR.
- Capture new edge cases in deterministic unit tests before adding integration coverage.

## Commit & Pull Request Guidelines
- Write commits in the imperative mood (`Fix overflow in TimestampIncrementingOffset`) and keep related changes together; avoid mixing refactors with features.
- Pull requests should complete the template in `docs/pull_request_template.md`, outlining the problem, solution, test evidence, and release plan.
- Link GitHub issues when applicable and document manual validation for risky database changes.
- Include configuration updates or migration notes in the PR description whenever new connector properties are added.
