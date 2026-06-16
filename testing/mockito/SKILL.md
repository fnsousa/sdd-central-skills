---
name: mockito
description: Use Mockito to isolate Java unit and application-service tests in a spec driven development workflow with Speckit. Use when mocking outbound ports, stubbing collaborators, verifying interactions, capturing arguments, testing failures, avoiding brittle mocks, or combining Mockito with JUnit 5.
---

# Mockito

Use this skill to isolate collaborators while keeping tests focused on behavior from the Speckit spec.

## Workflow

1. Mock only dependencies outside the behavior under test, usually outbound ports.
2. Prefer real domain objects and value objects over mocks.
3. Stub only calls required by the scenario.
4. Assert the returned result or state first; verify interactions only when the interaction is part of the contract.
5. Keep mocks at architecture boundaries. Do not mock the class under test.

## JUnit 5 Setup

```java
@ExtendWith(MockitoExtension.class)
class CreateOrderServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @Mock
    private PaymentGateway paymentGateway;

    @InjectMocks
    private CreateOrderService service;
}
```

Use constructor injection in production code so `@InjectMocks` remains simple and predictable.

## Stubbing

- Use `when(...).thenReturn(...)` for straightforward queries.
- Use `when(...).thenThrow(...)` for dependency failures that the spec requires handling.
- Use `doThrow(...).when(mock).voidMethod(...)` for void methods.
- Avoid broad matchers unless the value is irrelevant to the behavior.
- Do not overuse lenient stubbing; remove unused stubs instead.

## Verification

- Verify outbound calls when the spec requires an external effect, such as publishing an event or saving a record.
- Use `ArgumentCaptor` when the produced object matters.
- Prefer verifying meaningful method calls over `verifyNoMoreInteractions`, unless strict absence is part of the behavior.
- Avoid verifying implementation choreography that can change without breaking the spec.

## Boundary Guidance

- In hexagonal architecture, mock outbound ports in application-service tests.
- Use fake implementations instead of Mockito when the fake makes scenarios clearer, especially for in-memory repositories.
- Do not mock JDK value types, records, collections, optionals, or domain entities.
- For adapter tests, prefer framework test support or integration tests over mocking every framework class.

## Quality Checklist

- The test fails if the Speckit behavior is broken.
- The test does not fail because an internal helper method was renamed or reordered.
- Stubs and verifications use business-relevant values.
- Each mock has a clear architectural reason to exist.
