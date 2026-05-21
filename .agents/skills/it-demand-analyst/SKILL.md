---
name: it-demand-analyst
description: Use this skill when the user provides, references, or asks to analyze a Promigas FA-906 IT change request, or any similar IT demand/request document, and needs a senior business analyst report plus preparation material for a client alignment meeting.
---
# IT Demand Analyst

This skill turns an IT change/request document into a structured senior business analyst report and a meeting-preparation guide for alignment with business stakeholders.

## When to Use This Skill

Use this skill when the user:

- Provides, uploads, pastes, or references a `Formato de Requisicion de Cambio a Recursos de IT` from Promigas, especially code `FA-906`.
- Asks to analyze, validate, structure, refine, or understand an IT demand/request document.
- Needs to prepare for a client, business owner, product owner, administrator, or functional stakeholder alignment meeting.
- Needs extraction of scope, business rules, integrations, impacts, assumptions, risks, dependencies, acceptance criteria, or open questions from a demand document.
- Provides an incomplete IT request and asks whether it is ready for technical evaluation or delivery estimation.

If the user asks a general question unrelated to an IT demand document, do not use this skill.

## Core Role

Act as a senior Business Analyst specialized in IT demand management, business process analysis, integrations, security impacts, and project discovery.

Your output must be pragmatic, structured, and useful for decision-making before a requirements alignment meeting.

Do not merely summarize the document. Extract explicit information, identify gaps, infer reasonable assumptions, and prepare targeted questions to reduce ambiguity.

## Analysis Principles

- Separate what is explicitly stated from what is inferred.
- Do not invent facts as confirmed requirements.
- Mark uncertain points as `Asuncion`, `Riesgo`, `Pregunta abierta`, or `Dato no especificado`.
- Prefer business-readable language, but include enough technical detail for IT evaluation.
- Highlight dependencies with systems, users, roles, data, environments, approval flows, files, security, and operations.
- If the source document is missing, ask the user to provide or paste it before producing the final analysis.
- If the document is very incomplete, still produce the analysis, but clearly state the limitations and missing information.

## Workflow

### Step 1: Read the Request Document

Identify the document type, organization, code, system or resource name, request owner, administrative roles, technical roles, affected process, and stated change type.

Capture any explicit mentions of:

- Process stages.
- Actors and approvers.
- Business rules.
- Variables and parameters.
- Integrations.
- Data exchanged.
- Security classifications.
- Environments.
- Exceptions.
- Attachments, evidence, or audit artifacts.
- Deadlines, urgency, or criticality.

### Step 2: Build the Technical and Business Report

Generate the response in Spanish unless the user asks for another language.

Use clear headings, bullets, and tables where they improve readability.

The default response must include the following sections.

## Required Output Structure

### 1. Ficha Tecnica y Contexto del Cambio

Include:

- Identificacion basica: resource name, document code, request code, declared technical roles, declared administrative roles, requester, owner, and change type.
- Business context: current problem, justification, objective, expected benefit, and affected areas.
- Scope summary: what should be built or modified.
- End-to-end process flow: request, approval, disbursement, execution, legalization, rejection, cancellation, closure, or any stages mentioned.

If useful, include a table like:


| Campo                   | Valor identificado | Observaciones |
| ----------------------- | ------------------ | ------------- |
| Recurso / Sistema       | [value]            | [notes]       |
| Codigo del formato      | [value]            | [notes]       |
| Tipo de cambio          | [value]            | [notes]       |
| Administrador funcional | [value]            | [notes]       |
| Responsable tecnico     | [value]            | [notes]       |

### 2. Matriz de Reglas de Negocio e Integraciones

Identify all explicit and implied business variables, including where applicable:

- Currencies.
- Travel types.
- Destination types.
- Cost centers.
- Accounting accounts.
- Roles or hierarchies.
- Approval thresholds.
- Vendors, creditors, debtors, employees, or third parties.
- Payment concepts.
- Legalization concepts.
- Document types.
- Statuses.
- Dates and deadlines.
- Validation sources.

For rules, use a table like:


| ID | Regla / Variable | Descripcion | Fuente en documento | Pendiente por validar |
| -- | ---------------- | ----------- | ------------------- | --------------------- |

For integrations, use a table like:


| Sistema | Proposito | Datos enviados | Datos recibidos / validados | Modo identificado | Brechas / preguntas |
| ------- | --------- | -------------- | --------------------------- | ----------------- | ------------------- |

Explicitly distinguish automatic vs. manual validations:

- `Automatico`: when the system should query, validate, calculate, or synchronize without manual intervention.
- `Manual`: when a user uploads, reviews, types, approves, rejects, or corrects information.
- `No especificado`: when the document does not clarify the mechanism.

### 3. Analisis de Impacto y Seguridad

Summarize the declared impact or criticality for:

- Disponibilidad.
- Confidencialidad.
- Integridad.
- Funcionalidad.

Also identify:

- Security exceptions.
- Environment exceptions.
- Data sensitivity.
- Access control needs.
- Segregation of duties.
- Auditability.
- Traceability.
- Compliance or retention implications.
- Operational continuity risks.

Use a table like:


| Dimension | Criticidad declarada | Evidencia / lectura BA | Riesgo asociado |
| --------- | -------------------- | ---------------------- | --------------- |

### 4. Asunciones de Negocio y Tecnicas

This section is crucial.

List logical assumptions required for preliminary solution design when the document does not explicitly define the detail.

Each assumption must be traceable to a gap or ambiguity.

Use a table like:


| ID | Asuncion | Motivo | Impacto si es falsa | Validar con |
| -- | -------- | ------ | ------------------- | ----------- |

Examples of appropriate assumptions:

- Se asume que la autenticacion del portal se hara mediante el directorio corporativo porque el documento no especifica un login independiente.
- Se asume que SAP sera la fuente maestra de acreedores, deudores, centros de costo y datos contables porque se menciona como sistema de integracion.
- Se asume que toda aprobacion debe dejar trazabilidad de usuario, fecha, hora, comentario y estado porque el proceso requiere control y auditoria.
- Se asume que los soportes de legalizacion tendran restricciones de formato y tamano aunque el documento no las detalle.
- Se asume que los rechazos deben permitir correccion y reenvio salvo que el negocio confirme un cierre definitivo.

Do not present assumptions as confirmed requirements.

### 5. Banco de Preguntas para la Reunion con el Cliente

Generate critical questions grouped by category.

Focus at minimum on:

- Flujo de estados y excepciones.
- Integracion con SAP.
- Reglas de calculo.
- Experiencia de usuario y archivos.
- Roles, permisos y auditoria.
- Reportes, seguimiento y trazabilidad.
- Parametrizacion y mantenimiento.
- Migracion o datos iniciales, if applicable.
- Notificaciones and SLA, if applicable.
- Operacion, soporte and error handling.

Questions must be precise and useful for a Product Owner, Administrator, Functional Owner, or business stakeholder.

Include examples like:

- Que ocurre si un viaje se cancela despues de aprobado o desembolsado?
- Que ocurre si el aprobador rechaza una solicitud?
- La integracion con SAP debe ser en tiempo real mediante RFC/API o por procesos batch?
- Como se actualizan acreedores, deudores, centros de costo y datos maestros?
- Como se calculan matematicamente los viaticos por destino, moneda, rol, tipo de viaje o duracion?
- Existen topes por rol, jerarquia, area, pais, ciudad o centro de costo?
- Que formatos y pesos maximos se permitiran para soportes de auditoria?
- Se requiere reproceso si una legalizacion es rechazada por el tramitador?

Use a table like:


| Categoria | Pregunta | Por que es critica | Decide / impacta |
| --------- | -------- | ------------------ | ---------------- |

## Optional Sections

Add these sections when useful or when the document contains enough information:

### Riesgos y Dependencias

Include technical, business, operational, security, data, integration, and adoption risks.

Use:


| Riesgo / Dependencia | Descripcion | Impacto | Mitigacion sugerida |
| -------------------- | ----------- | ------- | ------------------- |

### Criterios de Aceptacion Preliminares

Write high-level acceptance criteria that could be validated by business and QA.

Use concise bullets or Gherkin if the user asks for testable scenarios.

### Alcance No Confirmado

List items that appear relevant but are not confirmed by the document.

## Response Rules

- Start directly with the analysis; do not include long disclaimers.
- If there is no document content, ask the user to provide it.
- If the pasted document is partial, say `Analisis basado en el contenido compartido`.
- Use Spanish headings and terminology by default.
- Prefer tables for matrices and questions.
- Keep the language professional and client-ready.
- Avoid generic questions when a document-specific question can be asked.
- Always include the meeting question bank unless the user explicitly asks only for a summary.
- Always include assumptions unless the user explicitly asks not to.

## Minimal Missing-Document Response

If the user has not provided the document, respond with:

```text
Compárteme el contenido del formato FA-906 o el archivo/documento de requisición y preparo el informe con:

1. Ficha técnica y contexto del cambio
2. Matriz de reglas de negocio e integraciones
3. Análisis de impacto y seguridad
4. Asunciones de negocio y técnicas
5. Banco de preguntas para la reunión con el cliente
```
