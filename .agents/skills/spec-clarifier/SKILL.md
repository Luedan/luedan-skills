---
name: spec-clarifier
description: Use this skill whenever the user wants to define, refine, validate, or generate a functional specification from a user story — especially when the story is incomplete, ambiguous, contains typos, or requires assumptions, BDD scenarios, happy/sad paths, mockups, or iterative clarification. Trigger on phrases like "redacta el spec", "define la historia", "convierte esto en requerimiento", "haz un análisis funcional", "qué falta en esta historia", or any request to turn a raw story into a structured product requirement. Use this even if the user only says "tengo esta historia" and pastes one.
---
# Spec-clarifier

Transforms an incomplete or ambiguous user story into a clear, traceable, foolproof functional specification through a pre-flight check, classified assumptions, systematic sad-path discovery, and a quality gate before file creation.

## When to Use This Skill

- User provides a user story and asks to define, refine, or create a spec.
- User wants missing parts filled with explicit assumptions.
- User asks for happy paths, sad paths, BDD scenarios, or mockups.
- User asks for a repeatable product discovery or functional analysis workflow.
- User asks to generate a spec file from an agreed definition.

## When NOT to Use This Skill

- The user wants a purely technical implementation spec (architecture, schemas, API contracts) without product behavior.
- The user wants code, not a document.
- The user has a fully-formed spec already and only wants formatting cleanup.

## Core Principles

1. **No silent gaps.** If something is inferred, it appears as an assumption. If something cannot be inferred, it appears as an open question.
2. **Assumptions are classified.** Not all assumptions are equal — surface critical ones first.
3. **Everything is traceable.** Every Acceptance Criterion, BDD scenario, and Sad Path has an ID. Sections cross-reference each other.
4. **Sad paths are systematic.** Run a discovery checklist; do not rely on imagination alone.
5. **Quality gate before delivery.** Never write the final file without passing the checklist.
6. **Language follows the user.** Detect the conversation language and use it consistently.

## Workflow Overview

```text
[1] Pre-flight check on the story
[2] Generate provisional spec (with IDs and traceability)
[3] Run sad-path discovery checklist
[4] List assumptions classified by risk + open questions
[5] Clarification loop (one or grouped questions)
[6] Quality gate (DoD for the spec itself)
[7] Announce readiness
[8] Create the file when explicitly asked
```

---

## Step 1: Pre-flight Check

Before generating anything, scan the story for **critical gaps**:


| Critical element    | What to look for                                          |
| ------------------- | --------------------------------------------------------- |
| Actor               | Is there a "Como [rol]"?                                  |
| Action / capability | Is there a "quiero [acción]"?                            |
| Outcome / benefit   | Is there a "para [beneficio]"? Is the outcome measurable? |
| Scope signal        | Is it clear what is in or out?                            |
| Trigger / context   | When does this happen? What state precedes it?            |

**Rules:**

- If **0–2 critical elements** are missing, proceed to Step 2 and capture the missing ones as assumptions.
- If **3 or more critical elements** are missing, do **not** generate a full provisional spec yet. Ask 1–3 high-leverage questions first using the format from Step 5. Then proceed.
- Correct obvious typos and list the correction as an assumption.
- If the story actually contains multiple features bundled together, point this out and ask whether to split it into separate specs before continuing.

## Step 2: Generate the Provisional Spec

Use the **Recommended Template** (full) by default. Use the **Lightweight Template** if the user asks for speed or the feature is small.

**Mandatory rules for IDs and traceability:**

- Number every Acceptance Criterion: `AC-1`, `AC-2`, `AC-3`...
- Number every BDD scenario: `BDD-1`, `BDD-2`... and reference the AC it covers (e.g., `BDD-3 → AC-2`).
- Number every Sad Path: `SP-1`, `SP-2`... and reference the AC it protects when applicable.
- Number every assumption: `S-1`, `S-2`...
- Number every open question: `Q-1`, `Q-2`...

Acceptance Criteria must be **atomic and testable**. Each one should be expressible as `Given / When / Then` without splitting it. If you cannot phrase it that way, split it.

**For UI features**, the provisional spec must include at minimum these states: empty, loading, success, error, permission-denied. If the visual layout matters, prefer ASCII; if ASCII is insufficient, offer to render a visual mockup with `show_widget`.

## Step 3: Run the Sad-Path Discovery Checklist

Before listing assumptions, walk through this checklist and add a numbered Sad Path for every item that plausibly applies. Do **not** skip the walk-through — explicitly note "no aplica" for items that don't apply.


| #  | Failure category   | Question to ask yourself                                                                  |
| -- | ------------------ | ----------------------------------------------------------------------------------------- |
| 1  | Invalid input      | What if a required field is missing, malformed, or out of range?                          |
| 2  | Authorization      | What if the actor lacks permission?                                                       |
| 3  | Authentication     | What if the session is expired or the user is anonymous?                                  |
| 4  | Resource not found | What if the referenced entity doesn't exist or was deleted?                               |
| 5  | Concurrency        | What if two actors modify the same resource simultaneously?                               |
| 6  | Idempotency        | What if the action is submitted twice (double-click, retry)?                              |
| 7  | Network / timeout  | What if the request fails or times out?                                                   |
| 8  | Dependency down    | What if an external service or database is unavailable?                                   |
| 9  | Rate limit / quota | What if the actor hits a limit?                                                           |
| 10 | Empty state        | What does the first-time user see when there's no data yet?                               |
| 11 | Boundary values    | What about zero, max, negative, very long inputs, special characters?                     |
| 12 | State conflict     | What if the entity is in a state that doesn't allow this action (e.g., a closed account)? |
| 13 | Partial success    | What if part of a multi-step operation succeeds and part fails?                           |
| 14 | Cancel / undo      | Can the action be cancelled or reversed? What happens then?                               |

Each applicable item becomes a `SP-N` entry with: trigger, system response, user-facing message, and AC reference if applicable.

## Step 4: List Assumptions Classified by Risk

After the provisional spec, present every functional or non-technical assumption made, **classified by risk**:

- **[CRÍTICO]** — Changing this materially changes scope, actors, or core behavior.
- **[MEDIO]** — Changing this affects details, validations, or specific flows.
- **[BAJO]** — Default behavior; easy to change without rewriting the spec.

Present them in order: all `[CRÍTICO]` first, then `[MEDIO]`, then `[BAJO]`. Number them `S-1`, `S-2`, etc.

Then list **Open Questions** (`Q-1`, `Q-2`...) for things that could not reasonably be inferred and require user input. Open questions are different from assumptions — they have no provisional answer.

End with this prompt (adapt to conversation language):

> Indícame los números de los supuestos que quieres redefinir (puedes listar varios, separados por coma) o responde "todos bien" si los aceptas como están. También responde las Preguntas Abiertas si tienes la información.

If the user responds "todos bien" / "all good" / similar, skip Step 5 but record in the spec a note: *"Supuestos aceptados sin modificación por el usuario."*

## Step 5: Clarification Loop

For each rejected assumption (or related group):

- If the user rejected **3+ assumptions on the same topic** (e.g., all about authentication), group them into a single multi-part question.
- Otherwise, ask **one question at a time**.

Use this format:

```text
Pregunta X de Y
Progreso: [#####-----] A/Y

[Pregunta clara]

1. [Opción 1]
2. [Opción 2]
3. [Opción 3]
4. [Opción 4]
5. Otra (describe la respuesta)
```

After each answer:

1. Update the internal definition.
2. **Check for contradictions** with previously confirmed answers or with other sections of the spec. If a contradiction appears, surface it: *"Esto entra en conflicto con S-3 que confirmamos antes (X). ¿Cuál prevalece?"*
3. Continue with the next pending item.

If the user picks `Otra`, accept the custom answer and use it verbatim as the new definition.

## Step 6: Quality Gate (Definition of Done for the Spec)

Before announcing readiness, run this checklist silently. If anything fails, fix it or surface a new open question — **do not** announce readiness with hidden gaps.

- [ ]  Every `AC-N` is atomic and expressible as Given/When/Then.
- [ ]  Every `AC-N` has at least one `BDD-N` covering it.
- [ ]  At least one Sad Path exists for: invalid input, authorization denied, system/dependency error.
- [ ]  Every error case has a user-facing message defined under "User Messages".
- [ ]  No section contradicts another (cross-check Business Rules vs ACs vs Sad Paths).
- [ ]  If the feature has UI: empty, loading, success, error, and permission-denied states are described.
- [ ]  If the feature handles user data: validation rules are listed.
- [ ]  NFRs considered when relevant: performance expectation, accessibility level, i18n/locales, mobile vs desktop.
- [ ]  Open Questions list reflects every genuine unknown — nothing has been silently guessed.
- [ ]  Filename has been agreed (or will be proposed in Step 8).

## Step 7: Announce Readiness

Once the gate passes, respond with the readiness phrase **in the conversation language**:

- Spanish: `Ya me encuentro listo para crear la especificación.`
- English: `I'm ready to create the specification.`

Follow with a brief summary of confirmed adjustments (max 5 bullets) and the proposed filename.

## Step 8: Create the Final Spec File When Asked

Only create the Markdown file after the user explicitly asks for it ("créalo", "create it", "go ahead", etc.).

- Default location: project root.
- Filename: descriptive kebab-case (e.g., `registro-correo-contrasena.md`). If already approved, do not ask again.
- Append a **Traceability Matrix** at the end of the spec (see template).
- Include a version line at the top: `Versión: 1 · Fecha: YYYY-MM-DD`.

After creating the file, present it with `present_files`.

---

## Recommended Spec Template (Full)

````md
# Spec: [Feature Name]

> Versión: 1 · Fecha: YYYY-MM-DD

## Contexto

## Historia de Usuario

Como [tipo de usuario], quiero [acción o capacidad], para [beneficio o propósito].

## Objetivo

## Alcance

## Fuera de Alcance

## Actores

## Precondiciones

## Criterios de Aceptación

- **AC-1**: ...
- **AC-2**: ...

## Criterios de No Aceptación

## Happy Path

## Flujos Alternativos

## Sad Paths / Casos de Error

- **SP-1** (cubre AC-1): ...
  - Disparador: ...
  - Respuesta del sistema: ...
  - Mensaje al usuario: ...
- **SP-2** (cubre AC-2): ...

## Reglas de Negocio

## Validaciones

## Mensajes al Usuario

| ID | Contexto | Mensaje |
|---|---|---|
| MSG-1 | SP-1 | ... |

## Estados

## Datos Requeridos

## Dependencias

## Métricas / Eventos

| Evento | Cuándo se dispara | Propiedades |
|---|---|---|
| `feature_action_completed` | Tras AC-1 exitoso | `user_id`, `timestamp` |

## Consideraciones de Seguridad

## Consideraciones de Accesibilidad

## Consideraciones No Funcionales

- Performance: ...
- Internacionalización: ...
- Mobile vs Desktop: ...

## Escenarios BDD

```gherkin
Feature: [Feature Name]

  # BDD-1 → AC-1
  Scenario: [Comportamiento exitoso descrito en AC-1]
    Given [contexto]
    When [acción]
    Then [resultado esperado]

  # BDD-2 → AC-2
  Scenario: ...
```

## Mockup ASCII

```text
[wireframe ASCII]
```

## Supuestos Funcionales

- **S-1** [CRÍTICO]: ...
- **S-2** [MEDIO]: ...
- **S-3** [BAJO]: ...

## Preguntas Abiertas

- **Q-1**: ...

## Matriz de Trazabilidad

| AC | BDD | Sad Paths | Mensajes |
|---|---|---|---|
| AC-1 | BDD-1 | SP-1 | MSG-1 |
| AC-2 | BDD-2 | SP-2 | — |
````

Use Spanish section titles when the conversation is in Spanish, English titles otherwise.

## Lightweight Spec Template

For small features or when the user prioritizes speed. Still requires IDs and the quality gate, but fewer sections.

```md
# Spec: [Feature Name]

> Versión: 1 · Fecha: YYYY-MM-DD

## Historia de Usuario

## Objetivo

## Alcance / Fuera de Alcance

## Criterios de Aceptación

- **AC-1**: ...

## Happy Path

## Sad Paths

- **SP-1** (cubre AC-1): ...

## Reglas de Negocio

## BDD

```gherkin
# BDD-1 → AC-1
Scenario: ...
```

## Mockup

## Supuestos

- **S-1** [CRÍTICO]: ...

## Preguntas Abiertas

- **Q-1**: ...

```

---

## Style Guidelines

- Be direct and functional.
- Prefer numbered lists for assumptions, criteria, flows, and sad paths.
- Keep clarification options mutually distinct.
- Avoid technical implementation details unless requested.
- Separate confirmed decisions from assumptions visually.
- Keep open questions visible — never pretend every detail is known.
- BDD verifies behavior; it does not replace happy/sad paths in plain language.
- User messages must be user-friendly: avoid technical jargon, suggest a next step, never blame the user.

## Important Distinctions

- **Happy path**: successful flow in functional language.
- **Sad path**: expected failures and how the system responds, including the user-facing message.
- **BDD**: testable scenarios using `Given / When / Then`. Each maps back to an `AC-N`.
- **Assumption**: a provisional answer to an inferable gap, classified by risk.
- **Open Question**: a gap that cannot reasonably be inferred and requires the user's input.

## Mini Example (Reference)

User says: *"Como usuario quiero registrarme con mi correo"*

**Pre-flight check** detects: actor ✓, action ✓, outcome ✗ (missing benefit), scope ✗, trigger ✗ → 3 critical gaps → ask 1–2 high-leverage questions first.

After user clarifies (purpose: access personal dashboard; scope: web only), generate provisional spec with:

- `AC-1`: Usuario registra con correo y contraseña válidos y recibe acceso a su dashboard.
- `AC-2`: Sistema valida formato de correo y fortaleza de contraseña antes de crear la cuenta.
- `AC-3`: Sistema previene registros duplicados con el mismo correo.
- `SP-1` (cubre AC-2): Correo malformado → mostrar `MSG-1`.
- `SP-2` (cubre AC-3): Correo ya registrado → mostrar `MSG-2`, ofrecer enlace a login.
- `SP-3` (cubre AC-1): Servicio de email para verificación caído → registrar usuario en estado pendiente, mostrar `MSG-3`.
- `S-1` [CRÍTICO]: No requiere verificación por correo antes de habilitar la cuenta.
- `S-2` [MEDIO]: Contraseña mínima 8 caracteres con al menos una letra y un número.
- `S-3` [BAJO]: Mensaje de éxito redirige al dashboard tras 2 segundos.
- `Q-1`: ¿La contraseña debe cumplir requisitos adicionales por política de seguridad de la empresa?

User rejects `S-1`. Skill asks one question with 5 options about email verification, updates spec, runs quality gate, announces readiness, and only then creates the file when asked.
```
