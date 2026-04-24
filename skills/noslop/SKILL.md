---

name: noslop
description: >
Optimizes code for human comprehension first. Reduces working-memory load,
backtracking, and hierarchical complexity while making domain intent visible
in the local code.
------------------

# No Slop Mode

## Core Rule

Prefer the smallest code shape that makes the next reader understand the intent
without mentally simulating unnecessary detail. Optimize for local clarity,
predictable control flow, and explicit invariants.

## Evidence Tiers

* **Strong evidence:** descriptive names, shallow nesting, explicit state, and beacons/comments that state intent.
* **Moderate evidence:** keeping data near its use, localizing related logic, and using types/assertions to encode assumptions.
* **Heuristic:** immutability, strict top-to-bottom flow, and aggressive abstraction removal.

## Cognitive Principles

* **Descriptive Beacons:** Use domain-specific names that reveal purpose.
* **Low Hierarchical Load:** Prefer guard clauses and shallow nesting.
* **Short Local Data-Flow:** Keep definitions close to first use when that improves readability.
* **Explicit State:** Make hidden assumptions impossible or obvious.
* **Cohesive Abstraction:** Group logic by concept, not by accidental file boundaries.
* **Useful Documentation:** Add comments only when they explain intent, invariants, or non-obvious design choices.

## Patterns

### 1. Naming (The Beacon)

* **Bad:** `data`, `res`, `handler()`, `process()`, `temp`
* **Good:** `tax_retention_rate`, `is_premium_subscriber`, `validate_session_expiry()`

### 2. Flow (The Chain)

* **Bad:** Variables defined at the top of a long scope but used far below.
* **Good:** Variables defined and consumed in the same visual chunk.

### 3. Branching (The Stack)

* **Bad:** Deeply nested `if/else` logic.
* **Good:** **Guard Clauses.** Exit early for edge cases to keep the main logic shallow.

### 4. API Surface Mapping

* **Bad:** Narrow boolean traps such as `?isClosed=false&isPending=true`.
* **Good:** Orthogonal filters mapped to enums or state machines such as `?status=LISTED`.

## Code Review Protocol (NoSlop-Audit)

Analyze the code for the following **cognitive spikes**:

1. **Beacon Quality:** Flag identifiers that force bottom-up line-by-line reading.
2. **Data-Flow Distance:** Flag variables defined far from their consumption point.
3. **Mental Stack Depth:** Flag logic nested more than two levels deep. Suggest guard clauses.
4. **Spatial Disorientation:** Flag ghost abstractions where logic is split across too many files.
5. **Assumptive Gaps:** Identify magic numbers and hidden limits. Suggest explicit encoding through assertions or types.

## Example Review

**User:** Review this code: `if (x) { if (y) { if (z) { do_thing(a); } } }`

**NoSlop-Review:**

> **Cognitive Spike: Mental Stacking.** Nesting depth is 3, which increases working-memory load and makes the execution path harder to track.
>
> **Refactor:** Apply **Guard Clauses** to exit early for `!x`, `!y`, or `!z`. This flattens the execution path and reduces the number of conditions the reader must hold at once.

## Intensity Levels

### Lite — Trim Noise

Semantic naming plus basic guard clauses. Focus on high-level readability.

### Full — Research-Grounded Defaults

* **Variable Proximity:** Keep values close to where they are used when it improves clarity.
* **No Hidden State:** Minimize surprise mutations.
* **Cognitive Audit:** Provide specific notes during review when code creates avoidable load.

### Ultra — Zero Emulation

* **Type-Encoded Rules:** Logic should not run with invalid data.
* **Linear Flow:** Prefer code that reads cleanly from top to bottom.
* **Minimal Surprises:** Use immutability and explicit state only when they reduce cognitive overhead.

## Implementation & Review Checklist

* Is every identifier a domain-specific beacon?
* Does the code use guard clauses to minimize nesting?
* Are variables defined close to their first use?
* Are hidden assumptions enforced by code itself?
* Does the code avoid generic blobs in favor of chunkable types?
* Is the logic localized to prevent spatial disorientation?

## Boundaries

* **Performance:** If an abstraction adds meaningful latency or allocation cost, prioritize the compiler and the runtime.
* **Math:** Keep formulas correct; simplify the surrounding code, not the mathematics.
* **Scope:** Do not force one style everywhere. Optimize for comprehension in the actual context.
* **Instructions:** If the user says `stop noslop`, revert immediately.
