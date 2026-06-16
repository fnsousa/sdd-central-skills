---
name: hexagonal
description: Design, review, or refactor Java/Spring applications using hexagonal architecture in a spec driven development workflow with Speckit. Use when implementing use cases, ports, adapters, domain boundaries, dependency direction, package structure, REST/database/messaging integrations, or keeping specs aligned with architecture decisions.
---

# Hexagonal Architecture

Use this skill to keep implementation driven by the spec while preserving clean boundaries between domain, application, and infrastructure.

## Workflow

1. Read the Speckit spec first: clarify actors, commands, queries, business rules, external systems, persistence needs, and acceptance criteria.
2. Identify the application use case before writing adapters. Name it with business language, not framework language.
3. Place business decisions in the domain/application core. Keep Spring, HTTP, database, messaging, serialization, and vendor SDK details outside the core.
4. Define ports from the core's perspective:
   - inbound ports expose use cases.
   - outbound ports describe dependencies the core needs.
5. Implement adapters around those ports:
   - inbound adapters translate external input into commands/queries.
   - outbound adapters translate port calls into persistence, API, messaging, file, or time interactions.
6. Verify every acceptance criterion with focused tests at the right boundary.

## Package Shape

Prefer a feature-oriented shape when the system is business-rich:

```text
com.example.<boundedcontext>.<feature>
  domain/
  application/
    port/in/
    port/out/
    service/
  adapter/in/
    web/
    messaging/
  adapter/out/
    persistence/
    client/
  config/
```

For smaller services, a layer-oriented shape is acceptable if dependency direction remains clear.

## Boundary Rules

- Domain must not depend on Spring, JPA, Jackson, HTTP, messaging, database, or generated client classes.
- Application services orchestrate use cases and transactions; they should not contain infrastructure mapping noise.
- Ports should express business needs, not infrastructure operations. Prefer `FindCustomerByDocument` semantics over generic repository leakage when the spec is business-specific.
- Adapters may depend on frameworks and mappers, but they must not introduce business rules that belong in the core.
- DTOs and entities from external layers must be mapped before entering the core.
- Keep time, UUIDs, randomness, and external identity providers behind ports when business behavior depends on them.

## Implementation Heuristics

- Start with command/query records for use case input when Java 21 is available.
- Return domain results or explicit output models from use cases; avoid exposing persistence entities.
- Model validation close to the rule: syntactic validation at adapters, business validation in domain/application.
- Use small ports. Split read/write ports when it improves testability or business clarity.
- Keep mapper code at adapter edges. Do not make domain objects know how to serialize themselves for external systems.
- Prefer dependency inversion over shared utility classes that couple layers.

## Testing Strategy

- Domain tests cover business rules without Spring.
- Application tests cover use case orchestration with fake or mocked ports.
- Adapter tests cover framework integration, mapping, persistence queries, HTTP contracts, and message formats.
- End-to-end or slice tests should trace the most important acceptance paths from the Speckit spec.

## Spec Alignment Checklist

- Every use case maps to at least one explicit spec behavior.
- Every external dependency is represented by an outbound port.
- Every adapter exists because a spec scenario needs an external interaction.
- Every business rule has a domain or application test.
- Error paths from the spec are represented as domain/application outcomes and translated by adapters.
