---

name: solid
description: >
Improves design by reducing responsibility overlap, dependency rigidity, and
surprising substitution behavior. NoSlop takes precedence whenever SOLID
would add indirection, fragmentation, or unnecessary abstraction.
-----------------------------------------------------------------

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

* Reduce ripple effects when requirements change.
* Make responsibilities easy to name and locate.
* Keep dependencies pointed at stable abstractions when the boundary is real.
* Preserve substitutability so callers do not need special cases.
* Avoid interface and class explosion that fragments understanding.
* Prefer the simplest structure that still makes change safe.

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

1. **Responsibility Drift:** Does one unit do too many unrelated things?
2. **Modification Gravity:** Does a small new feature require editing many existing units?
3. **Substitution Breakage:** Would a subtype surprise a caller expecting the base contract?
4. **Interface Fatigue:** Does a client depend on methods it never uses?
5. **Dependency Tangle:** Does core logic depend on unstable implementation details?
6. **Abstraction Bloat:** Did SOLID introduce more files, classes, or indirection than the problem needs?

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

Focus on obvious responsibility boundaries and avoiding giant interfaces.

### Full — Practical SOLID

Apply SOLID where it reduces change cost without adding unnecessary layers.

### Ultra — High Discipline

Prefer explicit contracts, composition over inheritance, and stable boundaries.
Avoid architectural ceremony that makes simple code harder to understand.

## Implementation Checklist

* Does each module have one clear responsibility?
* Can common changes happen by extension rather than modification?
* Do all subtypes obey the same contract?
* Do clients depend only on methods they actually use?
* Are dependencies pointed at stable abstractions at the boundary?
* Did abstraction simplify the design rather than multiply it?
* Would NoSlop consider this refactor an improvement in actual readability?

## Boundaries

* **NoSlop takes precedence.** If a SOLID refactor makes the code harder to read or more fragmented, do not apply it.
* **Do not over-abstract.** SOLID is for reducing change pain, not for maximizing the number of classes.
* **Do not force inheritance.** Use it only when substitutability is genuine.
* **Do not split for aesthetics.** Keep related logic together when separation hurts comprehension.
* **Do not ignore context.** In small scripts or one-off tools, the simplest code may be better than a polished design.
* **If the user says `stop solid`, revert immediately.**
