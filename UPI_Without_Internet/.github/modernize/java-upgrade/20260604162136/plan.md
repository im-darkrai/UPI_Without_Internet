# Upgrade Plan: UPI Without Internet (20260604162136)

- **Generated**: 2026-06-04 16:22:00
- **HEAD Branch**: main
- **HEAD Commit ID**: N/A

## Available Tools

**JDKs**
- JDK 23.0.2: C:\Program Files\Java\jdk-23\bin (available, used by steps 1, 3, 5)
- JDK 17: not available (baseline will be skipped)

**Build Tools**
- Maven Wrapper: 3.9.9 (defined in `.mvn/wrapper/maven-wrapper.properties`)
- Maven installation: none found locally; using Maven Wrapper for all build steps

## Guidelines

- Upgrade the Java runtime target to the latest LTS version.
- Keep changes minimal and preserve the existing Spring Boot 3.3.5 application behavior.

> Note: You can add any specific guidelines or constraints for the upgrade process here if needed, bullet points are preferred.

## Options

- Working branch: appmod/java-upgrade-20260604162136
- Run tests before and after the upgrade: true

## Upgrade Goals

- Java 21

## Technology Stack

| Technology/Dependency | Current | Min Compatible | Why Incompatible |
| --------------------- | ------- | -------------- | ---------------- |
| Java | 17 | 21 | User requested latest LTS runtime target |
| Spring Boot | 3.3.5 | 3.3.5 | Already compatible with Java 21 |
| Maven Wrapper | 3.9.9 | 3.9.0 | Supported for building with Java 21 |
| `java.version` property | 17 | 21 | Needs to match the requested Java runtime target |

## Derived Upgrades

- Update `pom.xml` `<java.version>` from `17` to `21` to target the requested runtime.
- Use the existing Maven Wrapper 3.9.9; no build tool upgrade is required for Java 21.

## Impact Analysis

### Dependency Changes

| File | Dependency | Current | Action | Target | Reason |
|------|------------|---------|--------|--------|--------|
| pom.xml | `<java.version>` | 17 | upgrade | 21 | User requested latest LTS runtime target |

### Source Code Changes

No source code changes are required for the Java 21 runtime upgrade because the project already uses Spring Boot 3.3.5 and no Jakarta/Java compatibility changes are needed.

### Configuration Changes

No application configuration file changes are required.

### CI/CD Changes

No CI/CD files were detected that require updates for this runtime-only upgrade.

### Risks & Warnings

- **Baseline JDK unavailable**: Java 17 is not installed locally, so the baseline compile/test step must be skipped. Mitigation: use the installed JDK 23 to verify the upgrade path and rely on the existing `java.version` property change with full test execution.
- **JDK 23 build environment**: Building with JDK 23 while targeting Java 21 is acceptable, but any runtime-only behavior differences should be caught by the full test suite in the final validation step.

## Upgrade Steps

- Step 1: Setup Environment
  - **Rationale**: Verify repository cleanliness, Git availability, and that the Maven Wrapper is usable in the local environment. Confirm the available JDK and build tool before making code changes.
  - **Changes to Make**: none
  - **Verification**: `./mvnw -q -version`, JDK 23.0.2 available, Maven Wrapper 3.9.9

- Step 2: Baseline Setup
  - **Rationale**: Attempt to establish a pre-upgrade baseline if Java 17 is available. Since the base JDK is missing, this step will be skipped.
  - **Changes to Make**: none
  - **Verification**: skipped (Java 17 not available)

- Step 3: Update Java Runtime Target to Java 21
  - **Rationale**: Apply the requested runtime upgrade in the Maven build configuration and keep the codebase aligned with the latest LTS target.
  - **Changes to Make**: update `<java.version>` in `pom.xml` from `17` to `21`
  - **Verification**: `./mvnw -q clean test-compile`

- Step 4: CVE Validation & Fix
  - **Rationale**: Scan direct project dependencies for known vulnerabilities and patch any vulnerable dependencies with minimal upgrades.
  - **Changes to Make**: apply patch upgrades only if Vulnerability scan reports issues
  - **Verification**: `./mvnw -q clean test-compile`

- Step 5: Final Validation
  - **Rationale**: Verify the upgrade by running the full test suite and confirm the project compiles and passes all tests.
  - **Changes to Make**: resolve any failures discovered during final validation
  - **Verification**: `./mvnw -q clean test`
