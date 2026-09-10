# Permanent AI Agent Instructions for (Project Name)

> [!IMPORTANT]
> Loaded on EVERY session and turn. AI MUST strictly follow all instructions herein and in `knowledge/AI_GUIDELINES.md` without exception.

---

## 1. Zero Unsafe Policy & Modern Memory Safety (MANDATORY)

- **Rule 1.1 (100% Safe Rust Mandate)**: `unsafe` blocks are strictly forbidden under any circumstances. Memory safety is absolute. Use safe abstractions and standard library patterns.
- **Rule 1.2 (Modern Rust Standards & Result Management)**: Use modern idioms, strict `Option`/`Result` propagation (`?`), and robust error handling. Never use `.unwrap()` in production without mathematically verified invariants.
- **Rule 1.3 (API Accuracy & Deprecated API Prohibition)**: Adhere strictly to latest stable docs of external crates (`wgpu`, `winit`, `(sub_project_name)`, `hecs`, `glam`). Deprecated APIs, outdated struct fields, or legacy signatures are forbidden.
- **Rule 1.4 (Breaking Changes & User Confirmation)**: Inform user and request explicit confirmation before introducing breaking changes or upgrading core dependencies.

---

## 2. Mandatory Triple-Slash (`///`) Documentation & Quality Score Rule (MANDATORY)

- **Rule 2.1 (Mandatory Doc-Comments)**: Every new/modified `pub mod`, `pub struct`, `pub enum`, `pub trait`, `pub fn`, and public field MUST include triple-slash (`///`) doc-comments.
- **Rule 2.2 (Documentation Quality Score Requirement)**: Doc-comments must internally score >=8/10. Lower scores are strictly unacceptable.
- **Rule 2.3 (Meaningful Technical Descriptions)**: Comments must explain purpose, constraints, failure modes, and integration usage. Merely repeating item names is forbidden.
- **Rule 2.4 (English-Only Source Code & Code Comments Mandate)**: Source code, identifiers, struct/field names, UI text/labels, logs, and comments (`///`, `//`) MUST be 100% English. Turkish inside source files is forbidden.
- **Rule 2.5 (Turkish Language Mandate for User Plans & Chat Interaction)**: User communication, plans (`implementation_plan.md`), and walkthroughs (`walkthrough.md`) MUST be written in Turkish for optimal clarity.

---

## 3. Persistent Knowledge Base & Memory Bank Synchronization (MANDATORY - Zero Postponement)

- **Rule 3.1 (Dual-Track Targeted Synchronous Documentation Execution)**: Synchronously update targeted docs BEFORE completing response:
  - **Bug Fixes**: Log in `knowledge/history/bug_fixes.md` (Core) or `(sub_project_name)_bug_fixes.md` ((Sub-Project Name)). Never add bug fixes to feature tables.
  - **New Features**: Document in `knowledge/history/engine_features.md` (Core) or `(sub_project_name)_framework_features.md` ((Sub-Project Name)) in categorized tables.
  - **Session Progress**: Synchronize in `memory-bank/activeContext.md` & `progress.md` (Core) or `(sub_project_name)_memory_bank/` ((Sub-Project Name)).
- **Rule 3.2 (Zero-Postponement Policy)**: "Will write later" or batching is strictly forbidden. Apply updates immediately in the same turn.
- **Rule 3.3 (Memory Bank Reading on Task Start)**: Read relevant memory bank and history files before starting any task.
- **Rule 3.4 (Dedicated Feature Documentation)**: Create or update a dedicated descriptive `.md` document under `knowledge/` for major new systems.

---

## 4. Modular Responsibility, SRP & Clean Boundaries (MANDATORY)

- **Rule 4.1 (Single Responsibility Per File - SRP)**: Every `.rs` file must have ONE specific responsibility. Keep `main.rs` thin and delegate logic to specialized modules.
- **Rule 4.2 (Anti-God Object Mandate)**: Centralized "manager" structs controlling disconnected domains are strictly forbidden. Decompose large structures into focused sub-systems.
- **Rule 4.3 (File & Function Size Limits)**: Source files MUST NOT exceed 800 lines. Functions should be small and focused (<100 lines). Break monolithic routines into private helpers.
- **Rule 4.4 (Strict File Boundary Enforcement)**: Existing module boundaries are absolute. Never merge files, collapse modules, or centralize logic without explicit instruction.
- **Rule 4.5 (Module Root Purity & Modern Rust 2018+ Layout Mandate)**: Legacy `mod.rs` files are strictly forbidden across the codebase. Modern Rust 2018+ module layout (`foo.rs` paired with folder `foo/`) MUST be used exclusively. Furthermore, module root/entry files (`foo.rs` or `lib.rs`) MUST strictly serve solely as connection and export points containing ONLY module declarations (`pub mod ...;`) and re-exports (`pub use ...;`); writing implementation logic, functions, structs, or algorithms inside them is strictly forbidden and must be placed in dedicated `.rs` submodules.

---

## 5. Scope Isolation, Architecture Preservation & AI Behavior Lock (MANDATORY)

- **Rule 5.1 (Scope Isolation)**: Only modify the explicitly requested file or module. Unrequested changes or refactoring outside the target area are forbidden.
- **Rule 5.2 (No Implicit Refactoring)**: Reorganizing or rewriting code without instruction is forbidden. Existing architecture is stable and production-grade.
- **Rule 5.3 (AI Behavior Lock & Uncertainty Resolution)**: Never assume missing context or guess. If uncertain or facing ambiguity, ASK the user.

---

## 6. Comprehensive Quality, Formatting & Performance Verification (MANDATORY)

- **Rule 6.1 (Multi-Tool Verification Suite)**: Whenever source code or configs (`.rs`, `.wgsl`, `Cargo.toml`) are modified, execute before completion:
  1. `cargo clippy --workspace --all-targets` (0 warnings / 0 errors).
  2. `cargo fmt --check` (100% compliant).
  3. `cargo test --workspace` (100% passed).
  *(Skip checks if only markdown files (`.md`) or docs are modified).*
- **Rule 6.2 (Quantitative Performance & Resource Reporting)**: When modifying source code, report: benchmarks/profiler metrics, zero heap allocations in per-frame hot loops, and binary size delta.
- **Rule 6.3 (Mandatory Verification Report Block)**: When source code changes are made, final response MUST include:
  - 🛠️ **Cargo Clippy**: 0 warnings / 0 errors
  - 🎨 **Cargo Format**: 100% Compliant (`cargo fmt --check`)
  - 🧪 **Cargo Test**: All tests passed (X passed, 0 failed)
  - ⚡ **Performance Impact**: [Quantitative impact, e.g., "0.0% change"]
  - 🧠 **Memory Allocations**: [Zero allocations in hot loop / Allocation delta]
  - 📦 **Binary Size Impact**: [Impact delta, e.g., "+0 KB"]
- **Rule 6.4 (Mandatory Unit Testing)**: Whenever math (matrices, quaternions, projections, culling, raycasts), state machines, parsers, or non-hardware bug fixes are modified, unit tests (`#[cfg(test)]`) MUST be written to verify invariants and edge cases (zero-division, singularity, empty collections, overflow). POD definitions and GPU render passes are exempt.

---

## 7. Copyright and SPDX Header Rule (MANDATORY)

- **Rule 7.1 (Header Requirement)**: Every project source file (`.rs`, `.wgsl`) MUST begin with the exact two-line header:
  ```rust
  // SPDX-License-Identifier: (License name)
  // Copyright (c) 2026 (Github Name) / (Project Name). All rights reserved.
  ```
- **Rule 7.2 (Preservation & Creation)**: Prepend header to new source files and preserve it during all modifications.

---

## 8. Git Push, Public Exporter & Human-Engineer Commit Standards (MANDATORY)

- **Rule 8.1 (AI-Free Human Engineer Commit Messages)**: Commit messages must be concise, technical, human-engineer format (`feat(scope): ...`, `fix(scope): ...`), completely free of marketing adjectives or AI phrasing.
- **Rule 8.2 (Private Repo Workflow)**: After verifying with tests and receiving explicit approval (e.g. "githuba yolla", "push to github", "kural 8"), commit and push to private repository (`git add .`, `git commit -m "..."`, `git push`).
- **Rule 8.3 (Mandatory Dual Synchronous Public Export Workflow)**: Immediately after pushing to private repo, automatically execute `python scripts/export_public.py --push` in the same turn. This sanitizes code and strips internal docs before pushing to public repo (`https://github.com/(Github Name)/(Project Name).git`).
- **Rule 8.4 (Private File & Memory Bank Confidentiality in Commits)**: Commits, PRs, and releases MUST NEVER mention internal-only files or memory banks (`knowledge/`, `memory-bank/`, `AI_GUIDELINES.md`, etc.). Only public technical changes may be described.

---

## 9. Strict Redundant Clone Prevention Rule (MANDATORY)

- **Rule 9.1 (No Unnecessary Clones)**: Cloning merely to appease borrow checker is forbidden. Every `.clone()` must have explicit technical necessity.
- **Rule 9.2 (Ownership & Move Semantics First)**: Use `move` or `&`/`&mut` references wherever possible. When consuming collections, use `std::mem::take`, `std::mem::replace`, or `.into_iter()`.
- **Rule 9.3 (No Clone on Copy Types)**: Calling `.clone()` on `Copy` types is strictly forbidden (`clippy::clone_on_copy`).
- **Rule 9.4 (Hot Path Heap Allocation Lock)**: Cloning `Vec`, `String`, `Box`, or large structs inside per-frame `update`/`render` hot loops is a direct rule violation.

---

## 10. Absolute Prohibition of Hardcoded Magic String Heuristics (MANDATORY)

- **Rule 10.1 (Universal Ban on String-Based Subsystem Logic)**: Engine decisions MUST NEVER inspect variable, object, node, bone, sound, or component names (`name.contains(...)`, `str.starts_with(...)`, hardcoded filters).
- **Rule 10.2 (100% Data-Driven & Explicit Type System Mandate)**: Decisions must be driven directly by type-safe Rust enums, struct fields, math data, official format specs (glTF 2.0, FBX, WAV, PNG, WGSL), or explicit ECS components.
- **Rule 10.3 (Asset, Language & Localization Independence)**: Engine must function flawlessly with assets named in any language/character set (English, Turkish, Japanese, German, etc.).
- **Rule 10.4 (Engine-Wide Industrial Solution Mandate)**: Never write temporary string checks for specific test cases. Solutions must be generic, modular, and industry-standard architecture.

---

## 11. Strict Prohibition of Linter Warning Suppression Attributes (`#[allow(...)]` - MANDATORY)

- **Rule 11.1 (Universal Ban on Clippy Suppressions)**: Attributes like `#[allow(clippy::too_many_arguments)]`, `#[allow(clippy::type_complexity)]`, `#[allow(clippy::needless_range_loop)]`, `#[allow(clippy::manual_clamp)]`, `#[allow(clippy::if_same_then_else)]`, and all other suppression tags are universally banned without exception.
- **Rule 11.2 (Refactor Over Suppression)**: Refactor underlying architecture: group parameters (>7) into descriptor structs, use type aliases/structs for type complexity, and apply idiomatic Rust patterns.
- **Rule 11.3 (Root-Cause Engineering)**: Resolve warnings at root cause; never sweep under the rug.

---

## 12. Neutral & Brand-Free Technical Documentation Standard (MANDATORY)

- **Rule 12.1 (No Third-Party Software / Brand Name-Dropping)**: Never reference third-party commercial software by name in comments, commits, or docs ("Unreal-style", "Unity standard", "Blender-like" are forbidden). Describe technical patterns directly.
- **Rule 12.2 (No Hype or Superlative Adjectives)**: Subjective marketing words ("AAA", "perfect", "flawless", "legendary", "world-class") are forbidden.
- **Rule 12.3 (Factual, Concise & Objective Engineering Language)**: Documentation and comments must describe only what the code does, inputs/outputs, math/physics principles, and architectural purpose in objective engineering language.
