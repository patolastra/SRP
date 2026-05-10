# SRP PARSER OUTPUT CONTRACT v1

## PURPOSE

Define the mandatory parser output contract for SRP.

This contract establishes:
- minimum parser output structure
- required fields
- parser behavior expectations
- ambiguity handling
- resilience policy
- warning policy
- category classification behavior

This document exists to ensure:
- deterministic parser behavior
- backend consistency
- machine validation readiness
- regression stability
- SRP ↔ GPT ↔ Notion interoperability

---

# REQUIRED ROOT STRUCTURE

Parser output MUST ALWAYS return a valid JSON object.

Required root fields:

- schema_version
- fixture_name
- courses
- warnings
- parser_confidence

Example minimal root structure:

```json
{
  "schema_version": "SRP_SCHEMA_v3",
  "fixture_name": "fixture_name_here",
  "courses": [],
  "warnings": [],
  "parser_confidence": 0.0
}
```

---

# ROOT FIELD DEFINITIONS

## schema_version

Required.
String.

Must contain:

```plaintext
SRP_SCHEMA_v3
```

---

## fixture_name

Required.
String.

Must match originating fixture.

---

## courses

Required.
Array.

Can be empty ONLY if parsing catastrophically fails.

---

## warnings

Required.
Array.

Can be empty.

Warnings NEVER replace extracted items.

---

## parser_confidence

Required.
Number.

Range:

```plaintext
0.0 → 1.0
```

Parser confidence MUST ALWAYS exist.

---

# COURSE OBJECT CONTRACT

Each course object MUST contain:

Required:
- course
- detected_categories
- items

Optional:
- participation_tracking
- student_support_tracking
- students_pending
- metadata

Example:

```json
{
  "course": "SEGUNDO",
  "detected_categories": [],
  "items": []
}
```

---

# COURSE FIELD DEFINITIONS

## course

Required.
String.

Must use normalized uppercase course names.

Examples:
- PRIMERO
- SEGUNDO
- TERCERO
- CUARTO
- QUINTO
- SEXTO
- SEPTIMO
- OCTAVO
- GLOBAL

---

## detected_categories

Required.
Array of strings.

Must contain normalized SRP category names.

---

## items

Required.
Array.

Can be empty ONLY if parser confidence is very low.

---

# ITEM OBJECT CONTRACT

Each item object MUST contain:

Required:
- category
- text

Optional:
- status
- priority
- metadata
- confidence
- relations

Example:

```json
{
  "category": "🎵 pendientes_sala",
  "text": "Reforzar trabajo de ritmo"
}
```

---

# ITEM FIELD DEFINITIONS

## category

Required.
String.

Must match official SRP categories EXACTLY.

---

## text

Required.
String.

Must preserve semantic meaning of original source text.

Parser MAY normalize:
- spelling
- punctuation
- spacing

Parser MUST NOT:
- invent pedagogy
- invent actions
- invent students
- invent repertoire

---

# WARNING OBJECT CONTRACT

Warnings are REQUIRED structural entities.

Each warning object MUST contain:

Required:
- type
- severity
- text

Example:

```json
{
  "type": "ambiguous_reference",
  "severity": "warning",
  "text": "Student reference incomplete"
}
```

---

# WARNING SEVERITY ENUM

Allowed values:

- info
- warning
- critical

---

# PARSER RULES

Parser MUST:

- always return valid JSON
- always return parser_confidence
- preserve multicourse separation
- preserve semantic continuity
- preserve extracted pending actions
- preserve category integrity

Parser MUST NOT:

- silently delete ambiguous text
- collapse multiple courses into one
- invent missing structure
- discard warnings
- replace items with warnings

---

# AMBIGUITY POLICY

Ambiguity MUST generate warnings.

Examples:
- incomplete student references
- uncertain course ownership
- contradictory instructions
- fragmented thoughts
- unresolved references

Ambiguous content MAY still produce items.

---

# GLOBAL COURSE POLICY

If course ownership cannot be determined:

```plaintext
course = GLOBAL
```

GLOBAL is a valid fallback course entity.

---

# CATEGORY CLASSIFICATION POLICY

Parser MUST classify content into official SRP categories only.

Official categories:

- 📝 revision
- 🎵 pendientes_sala
- 🧠 pendientes_planificacion
- 🎒 pendientes_materiales
- 🏠 pendientes_casa
- 📋 pendientes_administrativos
- 💬 pendientes_mensajes
- 👥 pendientes_jefatura
- 💻 pendientes_apps
- 🎼 posible_repertorio

Unknown categories MUST generate warnings.

---

# RESILIENCE POLICY

Parser MUST tolerate:

- broken markdown
- malformed bullets
- fragmented speech
- transcription errors
- abrupt topic changes
- multiline chaos
- duplicated thoughts
- punctuation inconsistency
- spacing inconsistency

Parser MUST remain operational under noisy input conditions.

---

# MULTICOURSE POLICY

Parser MUST:

- separate courses correctly
- preserve cross-course continuity
- avoid category leakage between courses

---

# VALIDATION POLICY

Parser output MUST be:

- machine-readable
- deterministic
- schema-compatible
- backend-safe
- serialization-safe

Parser output MUST NEVER:
- contain invalid JSON
- contain undefined categories
- omit required root fields

---

# CONFIDENCE POLICY

parser_confidence represents:
- structural confidence
- semantic confidence
- classification confidence

Suggested ranges:

```plaintext
0.90 - 1.00 = high confidence
0.70 - 0.89 = acceptable confidence
0.50 - 0.69 = unstable parsing
0.00 - 0.49 = unreliable parsing
```

---

# FUTURE COMPATIBILITY

This contract is designed to support:

- parser_v1
- SRP backend
- Notion sync
- search indexing
- longitudinal persistence
- conflict resolution
- regression testing
- batch parsing
- GPT interoperability

---

# FINAL CONTRACT RULE

Parser output MUST prioritize:

1. semantic preservation
2. structural validity
3. parser resilience
4. deterministic output
5. backend compatibility

over:
- visual formatting
- markdown aesthetics
- stylistic cleanliness