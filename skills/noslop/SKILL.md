---
name: noslop
description: Optimizes code for human comprehension first. Reduces working-memory load, backtracking, and hierarchical complexity while making domain intent visible in the local code.
---

# No Slop Mode

## Core Rule

Prefer the smallest code shape that makes the next reader understand the intent
without mentally simulating unnecessary detail. Optimize for local clarity,
predictable control flow, and explicit invariants.

## Evidence Tiers

* **Strong evidence:** descriptive names (Lawrie et al., 2006), shallow nesting (Miller, 1956; Cowan, 2001), explicit state (Green & Petre, 1996), beacons that state intent (Brooks, 1983; Letovsky, 1987), and cyclomatic complexity <10 (McCabe, 1976).
* **Moderate evidence:** keeping data near its use (Soloway & Ehrlich, 1984), localizing related logic, recognizable code patterns (programming plans), and using types/assertions to encode assumptions.
* **Heuristic:** immutability, strict top-to-bottom flow, and aggressive abstraction removal when it improves local clarity.

## Cognitive Principles

* **Descriptive Beacons:** Use domain-specific names that reveal purpose. Optimal identifier length: 15-20 characters for functions, 8-15 for variables. Prefer full words over abbreviations unless the abbreviation is domain-standard.
* **Low Hierarchical Load:** Prefer guard clauses and shallow nesting. Working memory holds 4±1 chunks; each nesting level adds cognitive load. Keep nesting ≤2 levels deep.
* **Short Local Data-Flow:** Keep definitions close to first use when that improves readability. Minimize the distance between variable declaration and consumption.
* **Explicit State:** Make hidden assumptions impossible or obvious. Hidden dependencies violate the principle of least surprise.
* **Cohesive Abstraction:** Group logic by concept, not by accidental file boundaries. Avoid feature envy and data clumps.
* **Recognizable Patterns:** Use stereotypical code structures (programming plans) that match reader expectations. Violations of discourse rules increase comprehension time.
* **Cyclomatic Complexity:** Keep decision points low. Functions with complexity >10 are significantly harder to understand and test.
* **Visual Structure:** Use consistent indentation and line length (50-80 characters optimal). Text structure affects scan speed and comprehension.
* **Useful Documentation:** Add comments only when they explain intent, invariants, or non-obvious design choices. Code should be self-documenting through structure and naming.

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

### 5. Cyclomatic Complexity (The Decision Load)

* **Bad:** Functions with >10 decision points (if/else, switch, loops, ternaries).
* **Good:** Functions with ≤10 decision points. Extract complex branches into named helper functions.

### 6. Line Length (The Scan)

* **Bad:** Lines exceeding 120 characters that require horizontal scrolling.
* **Good:** Lines between 50-80 characters for optimal reading speed. Break at natural semantic boundaries.

### 7. Function Length (The Chunk)

* **Bad:** Functions exceeding 50 lines or requiring scrolling to understand.
* **Good:** Functions that fit in one screen (20-30 lines). Each function does one thing at one level of abstraction.

## Code Review Protocol (NoSlop-Audit)

Analyze the code for the following **cognitive spikes**:

1. **Beacon Quality:** Flag identifiers that force bottom-up line-by-line reading. Check identifier length (too short or too long).
2. **Data-Flow Distance:** Flag variables defined far from their consumption point. Measure declaration-to-use distance.
3. **Mental Stack Depth:** Flag logic nested more than two levels deep. Suggest guard clauses to flatten control flow.
4. **Spatial Disorientation:** Flag ghost abstractions where logic is split across too many files. Check for feature envy and inappropriate intimacy.
5. **Assumptive Gaps:** Identify magic numbers and hidden limits. Suggest explicit encoding through assertions or types.
6. **Cyclomatic Complexity:** Flag functions with >10 decision points. Suggest extraction of complex branches.
7. **Pattern Violations:** Flag code that violates expected conventions or discourse rules for the language/framework.
8. **Visual Density:** Flag functions >50 lines or lines >80 characters. Check indentation consistency.
9. **Cross-Reference Density:** Flag high coupling (excessive dependencies between modules). Measure fan-in/fan-out.
10. **Code Smells:** Identify long parameter lists (>3-4 params), data clumps, and duplicated code.

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

* Is every identifier a domain-specific beacon with appropriate length (8-20 chars)?
* Does the code use guard clauses to minimize nesting (≤2 levels)?
* Are variables defined close to their first use?
* Are hidden assumptions enforced by code itself?
* Does the code avoid generic blobs in favor of chunkable types?
* Is the logic localized to prevent spatial disorientation?
* Is cyclomatic complexity ≤10 per function?
* Are functions ≤50 lines and lines ≤80 characters?
* Does the code follow recognizable patterns and conventions?
* Are parameter lists short (≤4 parameters)?
* Is coupling minimized (low fan-out, appropriate fan-in)?

## Boundaries

* **Performance:** If an abstraction adds meaningful latency or allocation cost, prioritize the compiler and the runtime.
* **Math:** Keep formulas correct; simplify the surrounding code, not the mathematics.
* **Domain Conventions:** Follow established patterns in the language/framework. Violating expectations increases comprehension time.
* **Scope:** Do not force one style everywhere. Optimize for comprehension in the actual context.
* **Metrics as Guidelines:** Thresholds (complexity <10, lines <80) are guidelines, not absolute rules. Context matters.
* **Instructions:** If the user says `stop noslop`, revert immediately.

## Research References

This skill is grounded in empirical program comprehension research:

* **Working Memory:** Miller (1956), Cowan (2001) - Cognitive capacity limits
* **Beacons:** Brooks (1983), Letovsky (1987) - How programmers navigate code
* **Programming Plans:** Soloway & Ehrlich (1984) - Stereotypical code patterns
* **Cyclomatic Complexity:** McCabe (1976) - Decision point metrics
* **Cognitive Dimensions:** Green & Petre (1996) - Notation design framework
* **Identifier Length:** Lawrie et al. (2006) - Optimal naming conventions
* **Text Structure:** Miara et al. (1983) - Visual layout effects
* **Comprehension Models:** Pennington (1987) - Mental model construction
* **Code Smells:** Fowler (1999), Mantyla et al. (2003) - Maintainability indicators
* **Cross-References:** Woodfield et al. (1981) - Module coupling effects
