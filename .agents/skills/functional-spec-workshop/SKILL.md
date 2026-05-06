---
name: functional-spec-workshop
description: Use this skill when the user wants to define, refine, or generate a functional specification from a user story, especially when the story is incomplete, ambiguous, or requires assumptions, BDD scenarios, happy paths, sad paths, mockups, and iterative clarification.
---

# Functional Spec Workshop

This skill helps transform an incomplete or ambiguous user story into a clear functional specification through assumptions, structured refinement, and final Markdown generation.

## When to Use This Skill

Use this skill when the user:

- Provides a user story and asks to create or define a spec.
- Wants you to fill missing parts of a product requirement.
- Asks for assumptions to be identified and challenged.
- Wants happy paths, sad paths, BDD scenarios, or mockups.
- Wants a repeatable product discovery or functional analysis workflow.
- Asks to generate a spec file from an agreed definition.

Do not use this skill for purely technical implementation specs unless the user explicitly asks for technical details.

## Core Behavior

Act as a pragmatic analyst, product owner, and functional spec writer.

When the user gives a story, create a provisional spec using reasonable functional and non-technical assumptions. Keep assumptions visible and negotiable.

Do not hide ambiguity. If you infer something meaningful, list it as an assumption.

Do not create the final file until the user asks you to create it.

## Workflow

### Step 1: Receive the User Story

Accept a raw user story, even if it is incomplete, informal, or contains typos.

If a typo is obvious, correct it in the provisional spec and list that correction as an assumption.

### Step 2: Create a Provisional Spec

Generate a provisional functional spec using the sections that apply:

- Title
- Context
- User Story
- Objective
- Scope
- Out of Scope
- Actors
- Preconditions
- Acceptance Criteria
- Non-Acceptance Criteria
- Happy Path
- Alternative Flows
- Sad Paths or Error Cases
- Business Rules
- Validations
- User Messages
- States
- Required Data
- Dependencies
- Metrics or Events
- Security Considerations
- Accessibility Considerations
- BDD Scenarios in Gherkin format
- ASCII Mockup when the feature has a user interface
- Functional and Non-Technical Assumptions
- Open Questions

The first provisional spec can be concise. Prefer clarity over excessive detail.

### Step 3: List Assumptions

After the provisional spec, show a numbered list of every functional or non-technical assumption made.

Do not include purely implementation-level assumptions unless they affect product behavior.

Examples of assumptions to list:

- User role.
- Required fields.
- Validation method.
- Account state.
- Error behavior.
- What is excluded from scope.
- Whether confirmation, approval, or verification is needed.

Ask the user to provide the numbers of assumptions they dislike or want to redefine.

### Step 4: Clarify Rejected Assumptions One by One

When the user gives assumption numbers, ask one question at a time.

Each question must include:

- A progress indicator showing answered and pending questions.
- Four reasonable options.
- A fifth option named `Otra`.

Use this format:

```text
Pregunta X de Y
Progreso: [#####-----] A/Y

[Question]

1. [Option 1]
2. [Option 2]
3. [Option 3]
4. [Option 4]
5. Otra
```

If the user chooses `Otra`, accept their custom answer and use it as the new definition.

After each answer, update the internal definition and continue with the next rejected assumption.

### Step 5: Finish the Refinement

When all rejected assumptions have been clarified, respond exactly with:

```text
Ya me encuentro listo para crear la especificación.
```

Then summarize the confirmed adjustments briefly.

### Step 6: Create the Final Spec File When Asked

Only create a Markdown file after the user explicitly asks you to create the spec.

Create the file in the project root unless the user gives another location.

Use a descriptive kebab-case filename, for example:

```text
registro-correo-contrasena.md
```

If the user already approved a filename, use it without asking again.

## Recommended Spec Template

Use this template for full specs:

````md
# Spec: [Feature Name]

## Context

## User Story

Como [tipo de usuario], quiero [acción o capacidad], para [beneficio o propósito].

## Objective

## Scope

## Out of Scope

## Actors

## Preconditions

## Acceptance Criteria

## Non-Acceptance Criteria

## Happy Path

## Alternative Flows

## Sad Paths / Error Cases

## Business Rules

## Validations

## User Messages

## States

## Required Data

## Dependencies

## Metrics or Events

## Security Considerations

## Accessibility Considerations

## BDD Scenarios

```gherkin
Feature: [Feature Name]

  Scenario: [Successful behavior]
    Given [context]
    When [action]
    Then [expected result]
```

## ASCII Mockup

```text
[ASCII wireframe]
```

## Functional Assumptions

## Open Questions
````

Use Spanish section titles when the conversation is in Spanish.

## Lightweight Spec Template

Use this template when the user wants speed over completeness:

```md
# Spec: [Feature Name]

## Historia de Usuario

## Objetivo

## Alcance

## Fuera de Alcance

## Happy Path

## Sad Paths

## Reglas de Negocio

## Criterios de Aceptación

## BDD

## Mockup

## Supuestos

## Preguntas Abiertas
```

## Style Guidelines

- Be direct and functional.
- Prefer numbered lists for assumptions, criteria, and flows.
- Keep options mutually distinct when asking clarification questions.
- Avoid technical implementation unless requested.
- Separate confirmed decisions from assumptions.
- Keep open questions visible instead of pretending every detail is known.
- Use BDD for behavior verification, not as a replacement for the functional spec.
- Use happy paths and sad paths to explain user flows in plain language.

## Important Distinctions

Happy path describes the successful flow in functional language.

Sad path describes expected failures and how the system responds.

BDD expresses behavior as testable scenarios using `Given`, `When`, and `Then`.

These sections can coexist in the same spec.
