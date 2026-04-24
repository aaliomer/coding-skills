---
name: solid
description: Improves design by reducing responsibility overlap, dependency rigidity, and surprising substitution behavior. NoSlop takes precedence whenever SOLID would add indirection, fragmentation, or unnecessary abstraction.
---

# SOLID Mode

## Precedence Rule

**NoSlop wins.** If a SOLID refactor makes code harder to read, more fragmented,
or more difficult to follow locally, do not apply it.

Use SOLID to improve maintainability **only when it preserves or improves
comprehension**. Prefer direct, local code over abstract structure when the
latter adds cognitive load without clear payoff.

## Core Rule

Design code so that each unit has a clear purpose, changes for a narrow reason,
and depends on behavior that is stable, explicit, and unsurprising.

SOLID is a **design heuristic**, not a mandate. It should support readability,
changeability, and local reasoning—not override them.

## Design Goals

* Reduce ripple effects when requirements change (minimize coupling).
* Make responsibilities easy to name and locate (maximize cohesion).
* Keep dependencies pointed at stable abstractions when the boundary is real.
* Preserve substitutability so callers do not need special cases.
* Avoid interface and class explosion that fragments understanding.
* Prefer the simplest structure that still makes change safe.
* Balance design quality with cognitive load - SOLID should reduce mental complexity, not increase it.

## Design Metrics

When SOLID principles are applied well, code exhibits measurable properties:

* **Cohesion:** Classes have high LCOM (Lack of Cohesion of Methods) scores - methods use the same instance variables.
* **Coupling:** Low afferent coupling (Ca) for volatile classes, high for stable abstractions. Instability metric I = Ce/(Ce+Ca) should match stability needs.
* **Class Size:** Optimal range 200-400 lines. Below 50 lines may indicate over-splitting; above 500 suggests responsibility drift.
* **Method Count:** Interfaces with >10 methods likely violate ISP. Classes with >20 public methods suggest SRP violations.
* **Dependency Direction:** Dependencies should flow toward stability. Stable abstractions should have high Ca, low Ce.
* **Cyclomatic Complexity:** Per NoSlop, keep ≤10 per method. SOLID refactoring should reduce, not increase, complexity.

## The Five Principles

### 1. Single Responsibility Principle (SRP)

A module, class, or function should have one coherent reason to change.

**Good signs**

* One concept, one job.
* The name matches the actual behavior.
* Changes to one policy do not drag in unrelated code.

**Bad signs**

* A class that validates, stores, logs, formats, and sends network requests.
* A file whose name is too vague to describe its contents.
* A function that mixes orchestration, parsing, business logic, and persistence.
* Splitting one concept into many files when the split adds navigation overhead.

**Rule of thumb**
Split when the responsibilities are independently meaningful and likely to change for different reasons. Keep related logic together when separation makes the code harder to understand.

---

### 2. Open/Closed Principle (OCP)

Software entities should be open for extension, closed for modification.

**Good signs**

* New behavior can be added by plugging in a new implementation.
* Existing, correct behavior does not need edits for every new case.
* Variation is isolated behind a stable interface.

**Bad signs**

* Long `if/else` or `switch` chains that keep growing.
* Premature strategy patterns or plugin systems for a problem that has not earned them.
* “Just add one more case” edits that keep spreading.

**Rule of thumb**
Use OCP when the change pattern is real and recurring, not hypothetical. For small or stable variation sets, inline logic may be clearer and better.

---

### 3. Liskov Substitution Principle (LSP)

Subtypes must be safely replaceable for their base type.

**Good signs**

* Callers can rely on the same contract for all implementations.
* Derived types do not weaken preconditions or break expected postconditions.
* Behavior differences are documented and unsurprising.

**Bad signs**

* A subtype that throws exceptions for normal base-class calls.
* An implementation that silently violates invariants.
* A hierarchy built on shared code rather than shared meaning.
* Subtyping used where composition would be clearer.

**Rule of thumb**
If the caller must know the concrete type to use it correctly, the contract is too weak or the hierarchy is wrong.

---

### 4. Interface Segregation Principle (ISP)

Clients should not depend on methods they do not use.

**Good signs**

* Small, role-specific interfaces.
* Different consumers get different views of the same underlying capability.
* Methods cluster by client need, not by convenience for the implementer.

**Bad signs**

* Fat interfaces with many unrelated methods.
* Implementations forced to stub or ignore methods.
* One interface trying to serve every caller.
* Over-splitting into tiny interfaces that make the design noisy.

**Rule of thumb**
Split interfaces by client behavior, but keep them large enough to stay understandable.

---

### 5. Dependency Inversion Principle (DIP)

High-level policy should not depend on low-level details; both should depend on abstractions.

**Good signs**

* Core logic depends on contracts, not concrete infrastructure.
* I/O, databases, and external systems are isolated behind boundaries.
* Testing is easier because behavior is swappable.

**Bad signs**

* Business logic tightly coupled to frameworks, file systems, or vendors.
* Deep import chains from core code into volatile details.
* Abstract interfaces that hide simple behavior without adding value.
* Injecting abstractions before there is a meaningful boundary to protect.

**Rule of thumb**
Invert dependencies where the boundary is real and stable enough to justify it. Do not add abstraction just to satisfy the pattern.

## Review Protocol (SOLID-Audit)

Analyze code for the following **design spikes**:

1. **Responsibility Drift (SRP):** Does one unit do too many unrelated things? Check class size (>500 LOC), method count (>20 public methods), and whether the class name requires "and" or "or".
2. **Modification Gravity (OCP):** Does a small new feature require editing many existing units? Check if changes ripple across multiple classes.
3. **Substitution Breakage (LSP):** Would a subtype surprise a caller expecting the base contract? Check for thrown exceptions, weakened postconditions, or strengthened preconditions.
4. **Interface Fatigue (ISP):** Does a client depend on methods it never uses? Check interface size (>10 methods) and whether implementations stub methods.
5. **Dependency Tangle (DIP):** Does core logic depend on unstable implementation details? Check dependency direction - stable abstractions should have high afferent coupling.
6. **Abstraction Bloat:** Did SOLID introduce more files, classes, or indirection than the problem needs? Check if class count increased without reducing coupling or improving cohesion.
7. **Cohesion Breakdown:** Do class methods operate on disjoint sets of instance variables? Calculate LCOM - high values indicate low cohesion.
8. **Coupling Excess:** Does the class have high efferent coupling (Ce > 7-10)? High Ce indicates fragility and testing difficulty.
9. **Cognitive Load Increase:** Did the refactor increase cyclomatic complexity, nesting depth, or navigation distance? SOLID should reduce mental load, not increase it.
10. **Premature Generalization:** Are there abstractions with only one implementation? Wait for the second or third use case before abstracting.

## Refactor Prompts

When reviewing a design, ask:

* Can this change be made by adding code rather than editing existing code?
* Is this abstraction real, or is it speculative?
* Does the type name match the behavior it exposes?
* Would a caller need special-case knowledge to use this correctly?
* Is the interface shaped around the client or around the implementer?
* Are concrete details leaking into policy code?
* Did the refactor improve comprehension, or only architecture diagrams?

## Common Failure Modes

### 1. Over-Splitting

Too many tiny classes, files, or interfaces create navigation overhead.

**Fix:** Merge anything that changes together and is easiest to understand together.

### 2. Premature Abstraction

Building extension points before the variation exists makes the code harder to read.

**Fix:** Start concrete, then abstract after the second or third real use.

### 3. Strategy Overuse

Using Strategy for a tiny set of cases turns a simple decision into a multi-file hunt.

**Fix:** Keep the logic inline until variation is stable, recurring, and costly to maintain in place.

### 4. Anemic Types with Hidden Behavior

A type may look simple but still leak invariants through external helper logic.

**Fix:** Move the invariant closer to the data that owns it.

### 5. Hierarchy Abuse

Inheritance used for code reuse instead of true substitutability.

**Fix:** Prefer composition unless the subtype really is a safe replacement.

## Example Patterns

### SRP Example

**Bad:** `InvoiceManager` validates invoices, calculates tax, writes files, sends email, and formats PDFs.

**Better:** Separate `InvoiceValidator`, `TaxCalculator`, `InvoiceRepository`, `InvoiceNotifier`, and `PdfRenderer` only if those responsibilities truly change independently and staying separate does not harm comprehension.

---

### OCP Example

**Bad:**

```ts
function price(order) {
  if (order.type === "retail") return retailPrice(order)
  if (order.type === "wholesale") return wholesalePrice(order)
  if (order.type === "vip") return vipPrice(order)
}
```

**Better:** Use a strategy or policy object only when there are multiple real variants and the variation is stable enough to justify indirection.

---

### LSP Example

**Bad:** A `ReadOnlyCache` subtype that throws when `set()` is called, while the base type implies writable behavior.

**Better:** Separate readable and writable contracts.

---

### ISP Example

**Bad:**

```ts
interface Worker {
  work(): void
  eat(): void
  sleep(): void
  fly(): void
}
```

**Better:** Split into focused interfaces such as `Workable`, `Restable`, and `Flyable` only when each interface represents a real client need.

---

### DIP Example

**Bad:** Business logic constructs a database client directly.

**Better:** Business logic depends on a repository interface; infrastructure provides the concrete implementation.

## Intensity Levels

### Lite — Shape Only

* Focus on obvious responsibility boundaries (classes >500 LOC, methods >50 LOC).
* Avoid giant interfaces (>10 methods).
* Apply guard clauses and basic separation of concerns.
* No premature abstraction - wait for real variation.

### Full — Practical SOLID

* Apply all five principles where they reduce change cost.
* Use metrics: LCOM for cohesion, Ce/Ca for coupling, class size 200-400 LOC.
* Extract abstractions after 2-3 concrete implementations.
* Ensure SOLID refactoring reduces cyclomatic complexity and nesting depth.
* Balance design quality with NoSlop cognitive load principles.

### Ultra — High Discipline

* Prefer explicit contracts and composition over inheritance.
* Maintain stable boundaries with clear dependency direction.
* Keep instability metric I aligned with stability needs (stable = low I, volatile = high I).
* Aggressively refactor responsibility drift and coupling excess.
* Avoid architectural ceremony - every abstraction must justify its cognitive cost.
* Continuously measure: if SOLID increases complexity or navigation distance, revert.

## Implementation Checklist

* Does each module have one clear responsibility? (Check: class size 200-400 LOC, <20 public methods)
* Can common changes happen by extension rather than modification?
* Do all subtypes obey the same contract without surprises?
* Do clients depend only on methods they actually use? (Check: interface size <10 methods)
* Are dependencies pointed at stable abstractions at the boundary? (Check: dependency direction flows toward stability)
* Did abstraction simplify the design rather than multiply it? (Check: cohesion increased, coupling decreased)
* Would NoSlop consider this refactor an improvement in actual readability? (Check: complexity ≤10, nesting ≤2, navigation distance reduced)
* Are coupling metrics healthy? (Check: Ce <7-10, appropriate Ca for stability level)
* Is cohesion high? (Check: methods use shared instance variables, low LCOM)
* Did the refactor reduce cognitive load? (Check: fewer mental jumps, clearer data flow)

## Boundaries

* **NoSlop takes precedence.** If a SOLID refactor makes the code harder to read or more fragmented, do not apply it. Cognitive load reduction trumps architectural purity.
* **Do not over-abstract.** SOLID is for reducing change pain, not for maximizing the number of classes. Every abstraction must justify its existence.
* **Do not force inheritance.** Use it only when substitutability is genuine and the "is-a" relationship is semantically correct.
* **Do not split for aesthetics.** Keep related logic together when separation hurts comprehension. Cohesion matters more than class count.
* **Do not ignore context.** In small scripts or one-off tools, the simplest code may be better than a polished design. SOLID is for systems that change.
* **Metrics are guidelines.** Thresholds (class size, coupling, complexity) are heuristics, not laws. Context and judgment matter.
* **Wait for variation.** Do not abstract until you have 2-3 concrete cases. Premature abstraction is worse than duplication.
* **If the user says `stop solid`, revert immediately.**

## Research References

This skill is grounded in software engineering research and design theory:

* **Coupling & Cohesion:** Stevens, Myers, Constantine (1974) - Foundational structured design principles
* **SOLID Principles:** Martin (1995-2000) - Original formulation of the five principles
* **Dependency Metrics:** Martin (1994) - Afferent/Efferent coupling, Instability, Abstractness
* **Empirical SOLID Studies:** Daly et al. (2013), Yamashita & Moonen (2013) - Effectiveness of SOLID in practice
* **Class Size Research:** Basili et al. (1996) - Optimal class size for maintainability
* **Cohesion Metrics:** Chidamber & Kemerer (1994) - LCOM and other OO metrics
* **Cognitive Dimensions:** Green & Petre (1996) - How design affects comprehension
* **Code Smells:** Fowler (1999) - Indicators of design problems
* **Design Patterns:** Gamma et al. (1994) - Reusable solutions that embody SOLID
* **Refactoring:** Fowler (1999) - Techniques for improving design without changing behavior
