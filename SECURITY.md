# Security Policy

## Supported versions

This repository contains sample code that demonstrates the
[`openapi-collection-generator`](https://github.com/rspereiratech) ecosystem.
Only the latest revision on the `master` branch is supported. Older commits
will not receive security fixes.

| Version           | Supported |
|-------------------|-----------|
| `master` (latest) | ✅        |
| Older commits     | ❌        |

## Reporting a vulnerability

**Please do not open public GitHub issues for security vulnerabilities.**

If you believe you have found a security issue in the samples (e.g. an
embedded secret, a malicious dependency, or generated output that could harm
a user importing it into Postman/Insomnia), report it privately:

- Open a [private security advisory](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
  on this repository, **or**
- Email the maintainer at **rspereiratech@gmail.com** with the subject
  `[SECURITY] openapi-collection-generator-samples`.

Please include:

- A description of the issue and its impact.
- Steps to reproduce, or a proof-of-concept.
- The commit hash you tested against.
- Any suggested mitigation, if you have one.

## Response process

You can expect:

- An acknowledgement within **5 business days**.
- An initial assessment (confirmed / not reproducible / out of scope) within
  **10 business days**.
- A fix or mitigation plan communicated before any public disclosure.

If the issue affects the generator modules themselves rather than these
samples, it will be forwarded to the appropriate sibling repository and you
will be kept in the loop.

## Scope

In scope:

- The sample OpenAPI spec (`src/main/resources/openapi.yaml`).
- The committed collection files under `collections/`.
- The Maven build configuration (`pom.xml`).

Out of scope:

- Vulnerabilities in upstream dependencies — please report those to the
  respective project.
- Issues in Postman or Insomnia themselves.
- Findings that require an attacker to already control the developer's
  machine.

## Disclosure

We follow a coordinated disclosure model. Once a fix is available, the
reporter will be credited in the release notes (unless they prefer to remain
anonymous).
