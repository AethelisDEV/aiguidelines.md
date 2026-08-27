# AI Guidelines for (Project Name) Development

> [!IMPORTANT]
> All AI assistants contributing to (Project Name) MUST read, strictly understand, and unconditionally adhere to these guidelines before proposing, modifying, or executing any code changes.

---

## 1. Zero Unsafe Policy & Modern Memory Safety (MANDATORY)

* **Rule 1.1 (100% Safe Rust Mandate)**: Do NOT use `unsafe` blocks under any circumstances. Memory safety is the highest priority in (Project Name). We prioritize safety and stability over micro-optimizations that bypass the borrow checker. If a task seems to require `unsafe`, find a safe alternative using higher-level abstractions or standard library patterns.
* **Rule 1.2 (Modern Rust Standards & Result Management)**: Always use modern Rust idioms, strict `Option` / `Result` propagation (`?` operator), and robust error handling. Never use `.unwrap()` in production engine logic without mathematically verified invariants.
* **Rule 1.3 (API Accuracy & Deprecated API Prohibition)**: AI MUST verify and strictly adhere to the latest stable documentation of external crates (`wgpu`, `winit`, `egui`, `hecs`, `glam`, etc.). Using deprecated APIs, outdated struct fields, or legacy signatures is strictly forbidden.
* **Rule 1.4 (Breaking Changes & User Confirmation)**: If updating a library or introducing a breaking change across multiple crates is required, the AI MUST inform the user and request explicit confirmation before executing.

---

## 2. Mandatory Triple-Slash (`///`) Documentation & Quality Score Rule (MANDATORY)

* **Rule 2.1 (Mandatory Doc-Comments)**: Every new or modified `pub mod`, `pub struct`, `pub enum`, `pub trait`, `pub fn`, and important public field MUST include triple-slash (`///`) doc-comments.
* **Rule 2.2 (Documentation Quality Score Requirement)**: The AI MUST internally evaluate every `///` doc-comment with a quality score from `0/10` to `10/10`. Any documentation below `8/10` is strictly unacceptable and MUST be improved prior to task completion.
* **Rule 2.3 (Meaningful Technical Descriptions)**: Documentation must explain purpose, responsibility, constraints, failure modes, and integration usage. Merely repeating the item name is forbidden.
* **Rule 2.4 (English-Only Source Code & Code Comments Mandate)**: All source code, identifiers, struct/field names, UI text/labels, error/log strings, and code comments (`///`, `//`) MUST be written exclusively in 100% English. Writing Turkish inside source files, doc-comments, or inline comments is strictly forbidden.
* **Rule 2.5 (Turkish Language Mandate for User Plans & Chat Interaction)**: When communicating directly with the user, generating implementation plans (`implementation_plan.md`), or creating post-execution walkthroughs (`walkthrough.md`), the AI MUST write in Turkish for optimal clarity, alignment, and collaborative user review.

### Documentation Score Standard

```txt
0/10 = Missing documentation.
1/10 = Useless placeholder comment.
2/10 = Extremely vague and not technically helpful.
3/10 = Mostly repeats the item name.
4/10 = Mentions purpose but lacks useful context.
5/10 = Basic explanation, but incomplete.
6/10 = Understandable but missing constraints, behavior, or usage notes.
7/10 = Good enough for humans, but not strong enough for long-term AI context.
8/10 = Acceptable: clear purpose, behavior, and relevant context.
9/10 = Strong: explains purpose, behavior, constraints, and integration context.
10/10 = Excellent: future-proof, precise, technically rich, and useful for AI/human maintenance.

```

---

## 3. Persistent Knowledge Base & Memory Bank Synchronization (MANDATORY - Zero Postponement)

* **Rule 3.1 (Mandatory Synchronous Documentation Execution)**: Whenever ANY change is made to the codebase (no matter how small), the AI assistant MUST synchronously update the following 3 documentation sets BEFORE completing its response:
1. `knowledge/history/bug_fixes.md`: Chronological append-only post-mortem incident log (`| Hata & İlgili Modül | Kök Neden (Root Cause) | Uygulanan Teknik Çözüm & Koruma |`). Do NOT overwrite past records; document root causes and engineering safeguards for active systems.
2. `knowledge/history/engine_features.md`: Living engine capability specification organized strictly in 8 categorized tables. When upgrading a feature, replace the older entry with the latest capabilities (zero duplication); never write floating text paragraphs outside tables.
3. `memory-bank/activeContext.md` & `memory-bank/progress.md`: Active session context, recent changes, and completed milestones.


* **Rule 3.2 (Zero-Postponement Policy)**: "Will write later", "batching later", or "if user asks" approaches are strictly FORBIDDEN. In every single interaction turn, changes must be written to these files IMMEDIATELY.
* **Rule 3.3 (Memory Bank Reading on Task Start)**: Before starting any task, the AI MUST read all files in `memory-bank/` (`projectbrief.md`, `productContext.md`, `activeContext.md`, `systemPatterns.md`, `techContext.md`, `progress.md`) and relevant historical records under `knowledge/history/`.
* **Rule 3.4 (Dedicated Feature Documentation)**: For any significant new architecture or major system, a dedicated descriptive `.md` document must be created or updated under the `knowledge/` directory.

---

## 4. Modular Responsibility, SRP & Clean Boundaries (MANDATORY)

* **Rule 4.1 (Single Responsibility Per File - SRP)**: Every single `.rs` file must have ONE specific, clearly defined responsibility and execute it exceptionally well. Keep `main.rs` thin and delegate logic to specialized modules.
* **Rule 4.2 (Anti-God Object Mandate)**: No struct or module may exceed a reasonable responsibility scope. Centralized "manager" objects controlling disconnected domains are strictly forbidden. Large structures must be decomposed into focused sub-systems.
* **Rule 4.3 (File & Function Size Limits)**:
* Source files MUST NOT exceed 800 lines. Split large modules logically.
* Functions should remain small and focused (ideally <100 lines). Break monolithic routines into private helper functions.


* **Rule 4.4 (Strict File Boundary Enforcement)**: Existing module boundaries are absolute. Do NOT merge files, collapse modules, or centralize logic into single files unless explicitly instructed.

---

## 5. Scope Isolation, Architecture Preservation & AI Behavior Lock (MANDATORY)

* **Rule 5.1 (Scope Isolation)**: AI must ONLY modify the explicitly requested file or module. Expanding scope or making unrequested "improvements", "refactors", or "optimizations" outside the target area is strictly forbidden.
* **Rule 5.2 (No Implicit Refactoring)**: Reorganizing, restructuring, or rewriting code without explicit instruction is forbidden. The existing architecture is considered STABLE and production-grade.
* **Rule 5.3 (AI Behavior Lock & Uncertainty Resolution)**: AI must NOT assume missing context or take unauthorized initiative. If uncertain or facing architectural ambiguity, the AI MUST ASK the user rather than guessing or making assumptions.

---

## 6. Comprehensive Quality, Formatting & Performance Verification (MANDATORY)

AI assistants MUST NOT declare a task completed simply by saying "code edited" or "test passed". The AI is required to execute a comprehensive suite of static and dynamic checks and report concrete quantitative metrics to the user.

* **Rule 6.1 (Mandatory Multi-Tool Verification Suite)**: Before declaring any task, feature implementation, or bug fix complete, the AI MUST execute:
1. `cargo clippy --workspace --all-targets` (Ensure zero lints, warnings, or idiom violations).
2. `cargo fmt --check` (Ensure code adheres 100% to Rust code style standards).
3. `cargo test --workspace` (Ensure all workspace unit, integration, and doc tests pass 100%).


* **Rule 6.2 (Quantitative Performance & Resource Reporting)**: The AI MUST analyze and report concrete quantitative impacts:
* **Performance & Benchmarks**: Run benchmarks or measure profile metrics when modifying rendering, ECS, physics, or asset pipelines.
* **Memory Allocations**: Explicitly verify zero heap allocations (`Vec::new()`, `Box`, `String`, `HashMap`) in per-frame `update`/`render` hot loops.
* **Binary Size Impacts**: Report binary size impact when adding new external crate dependencies or static assets.


* **Rule 6.3 (Mandatory Verification Report Block)**: Every final response summary MUST include the following verification block:

```markdown
### 📊 Verification & Performance Audit Report
- 🛠️ **Cargo Clippy**: 0 warnings / 0 errors
- 🎨 **Cargo Format**: 100% Compliant (`cargo fmt --check`)
- 🧪 **Cargo Test**: All tests passed (X passed, 0 failed)
- ⚡ **Performance Impact**: [Quantitative impact, e.g., "0.0% change" or "-8.2% degradation detected"]
- 🧠 **Memory Allocations**: [Zero allocations in hot loop / Allocation delta]
- 📦 **Binary Size Impact**: [Impact delta, e.g., "+0 KB"]

```

---

## 7. Copyright and SPDX Header Rule (MANDATORY)

* **Rule 7.1 (Header Requirement)**: Every project source file (e.g., `.rs`, `.wgsl` files) MUST begin with the following exact two-line header:
```rust
// SPDX-License-Identifier: (License name)
// Copyright (c) 2026 (Github Name) / (Project Name). All rights reserved.

```


* **Rule 7.2 (Preservation & Creation)**: When creating a new `.rs` or `.wgsl` source file, this header must be prepended at the very top. AI assistants and developers MUST preserve this header during any modifications or refactoring. If a file is updated in a subsequent year, update the copyright year range accordingly.

---

## 8. Git Push, Public Exporter & Human-Engineer Commit Standards (MANDATORY)

* **Rule 8.1 (AI-Free Human Engineer Commit Messages)**: All git commit messages MUST be completely free of marketing adjectives, hype, or AI-generated phrasing. Messages must be written in a concise, technical, human-engineer format (`feat(scope): ...`, `fix(scope): ...`).
* **Rule 8.2 (Private Repo Workflow)**: After verifying changes with tests and receiving explicit user approval to push, commit and push changes to the private repository via standard git workflow.
* **Rule 8.3 (Public Export Workflow)**: ONLY when the user explicitly commands a public export/release ("push to public", "public release", "public github"), execute `python scripts/export_public.py --push`. This script automatically strips internal guidelines, memory banks, personal local paths, and test scenes before pushing to the public repository.
* **Rule 8.4 (Private File Confidentiality in Commits)**: Commit messages MUST NEVER mention internal-only files that are excluded from the public repository (`knowledge/`, `memory-bank/`, `AI_GUIDELINES.md`, `bug_fixes.md`, `engine_features.md`, test scenes, etc.). Commit messages must describe only public source code changes.

---

## 9. Strict Redundant Clone Prevention Rule (MANDATORY)

Unnecessary `.clone()` calls made for convenience or to easily bypass the borrow checker are STRICTLY FORBIDDEN in (Project Name).

* **Rule 9.1 (No Unnecessary Clones)**: Cloning objects simply to appease the borrow checker is forbidden. Every `.clone()` call must have an explicit technical necessity.
* **Rule 9.2 (Ownership & Move Semantics First)**: In scenarios where data can be moved (`move`) or referenced (`&`/`&mut`), cloning is prohibited. When clearing or consuming collections, use `std::mem::take`, `std::mem::replace`, or `.into_iter()` / `.into_par_iter()`.
* **Rule 9.3 (No Clone on Copy Types)**: Calling `.clone()` on types implementing `Copy` (`Vertex`, `[f32; N]`, primitive types, etc.) is strictly forbidden (`clippy::clone_on_copy`).
* **Rule 9.4 (Hot Path Heap Allocation Lock)**: Cloning `Vec`, `String`, `Box`, or large data structures inside hot loops (per-frame `update`, `render`, mesh generation, or texture/mipmap processing) is a direct rule violation.

---

## 10. Absolute Prohibition of Hardcoded Magic String Heuristics (MANDATORY)

In **ANY module, layer, system, or algorithm** of (Project Name) (Renderer, Physics, ECS, Animation, Audio, UI, Editor, Asset Pipeline, Scene Management, Scripting, Plugin System, etc.), heuristic string pattern matching, substring checks (`contains`), or name-based guessing are **STRICTLY AND UNCONDITIONALLY FORBIDDEN**.

* **Rule 10.1 (Universal Ban on String-Based Subsystem Logic)**: Systemic, logical, physical, or graphical decisions MUST NEVER be made by inspecting variable, object, material, node, bone, sound, file, or component names (`name.contains(...)`, `str.starts_with(...)`, hardcoded name filters, etc.).
* **Rule 10.2 (100% Data-Driven & Explicit Type System Mandate)**: All decisions and behaviors must be driven directly by **type-safe Rust enums, struct fields, mathematical/physical data analysis, official format specifications (glTF 2.0, FBX, WAV, PNG, WGSL, etc.), or explicitly declared ECS components (`Component`)**.
* **Rule 10.3 (Asset, Language & Localization Independence)**: The engine must function flawlessly with assets named in any language or character set (English, Turkish, Japanese, German, etc.). Assumptions based on naming are considered breaking architectural defects.
* **Rule 10.4 (Engine-Wide Industrial Solution Mandate)**: Never write temporary string checks ("quick-and-dirty hacks") for a specific test case. Solutions MUST ALWAYS be implemented as generic, modular, and industry-standard system designs that strengthen engine architecture.

---

## 11. Strict Prohibition of Linter Warning Suppression Attributes (`#[allow(...)]` - MANDATORY)

Adding `#[allow(clippy::...)]` or `#[allow(...)]` attributes to suppress compiler or linter warnings is **STRICTLY FORBIDDEN** in (Project Name).

* **Rule 11.1 (Universal Ban on Clippy Suppressions)**: Attributes such as `#[allow(clippy::too_many_arguments)]`, `#[allow(clippy::type_complexity)]`, `#[allow(clippy::needless_range_loop)]`, `#[allow(clippy::manual_clamp)]`, `#[allow(clippy::if_same_then_else)]`, and similar suppression tags are universally banned without exception.
* **Rule 11.2 (Refactor Over Suppression)**: A linter warning indicates a code smell. Instead of suppressing it, refactor the underlying code architecture:
* **Too Many Arguments (>7 parameters)**: Group parameters into a logical Context / Parameters / Descriptor struct (`RenderParams`, `GizmoInputParams`, `GeometryParseOptions`, etc.).
* **Type Complexity**: Create meaningful type aliases (`type`) or dedicated data structs for complex tuples and closures.
* **Other Clippy Warnings**: Apply idiomatic Rust patterns (`clamp()`, `zip()`, `if-let` chains, etc.).


* **Rule 11.3 (Root-Cause Engineering)**: Linter warnings must never be swept under the rug; each warning must be resolved at its root cause.

---

## 12. Neutral & Brand-Free Technical Documentation Standard (MANDATORY)

In (Project Name) source code comments (`///`, `//`), commit messages, and technical documentation, mentioning commercial brand names (Unity, Unreal, Blender, Godot, etc.) or using hyperbolic marketing adjectives ("AAA", "legendary", "ultra", "flawless", "revolutionary", etc.) is **STRICTLY FORBIDDEN**.

* **Rule 12.1 (No Third-Party Software / Brand Name-Dropping)**: Never reference third-party commercial software by name in comments, commits, or documentation (phrases like "Unreal-style", "Unity standard", "Blender-like" are forbidden). Describe the concrete technical pattern or algorithm directly (e.g., "Tree-based split docking layout", "Hierarchical scene graph", "Quaternion slerp interpolation").
* **Rule 12.2 (No Hype or Superlative Adjectives)**: Subjective marketing words such as "AAA", "perfect", "flawless", "legendary", "world-class" must never be used.
* **Rule 12.3 (Factual, Concise & Objective Engineering Language)**: All documentation and comments must describe only what the code does, its inputs and outputs, its mathematical/physical principles, and its architectural purpose in clear, concise, and objective engineering language.
