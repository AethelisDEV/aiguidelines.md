# AI Guidelines for (Proje Adı) Development

> [!IMPORTANT]
> All AI assistants contributing to this repository MUST read and adhere to these guidelines before proposing or executing any code changes.

## 1. Zero Unsafe Policy
Memory safety is the highest priority in (Proje Adı). 
- **Rule**: Do NOT use `unsafe` blocks under any circumstances.
- **Rationale**: We prioritize safety and stability over micro-optimizations that require bypassing Rust's borrow checker. If a task seems to require `unsafe`, find a safe alternative using higher-level abstractions or libraries.

## 2. Mandatory Documentation (///)
All new code must be self-documenting for both humans and future AI contexts.
- **Rule**: Every new `pub mod`, `pub struct`, `pub enum`, and `pub fn` MUST include triple-slash (`///`) doc-comments.
- **Format**: Turkish/English bilingual comments are allowed, but the technical explanation must be clear and technically descriptive.

## 3. Persistent Knowledge Base (.md)
To maintain architectural continuity across different sessions and AI assistants:
- **Rule**: Bütün eklenen yeni özellikler ve sistemsel geliştirmeler (ufak/büyük fark etmeksizin) ANINDA `knowledge/history/engine_features.md` dosyasına eklenmelidir.
- **Rule**: Bütün hata düzeltmeleri (bug fixes), çökme (crash) çözümleri, kararlılık yamaları ve düzeltmeler ise ANINDA `knowledge/history/bug_fixes.md` dosyasına eklenmelidir.
- **Rule**: Bu bir TAVSİYE DEĞİL, ZORUNLULUKTUR! Yapılan işlemi tamamladıktan sonra `history/engine_features.md` veya `history/bug_fixes.md` dosyalarını (yapılan değişikliğin türüne göre) güncellemezseniz, kural ihlali yapmış olursunuz.
- **Rule**: For every significant feature or refactor, a corresponding `.md` file must be created or updated in the `knowledge/` directory.
- **Rule**: If you modify existing code structures, you MUST update the related documentation in the `knowledge/` folder to reflect the changes immediately.
- **Rule**: Before starting work, the AI must search and read relevant files in the `knowledge/` directory (especially those inside `knowledge/history/`) to understand the existing logic, patterns, and resolved issues.

## 4. Modular Responsibility
(Proje Adı) follows a strict modular architecture.
- **Rule**: Keep `main.rs` thin. 
- **Rule**: Delegate logic to specialized modules (e.g., `engine.rs`, `editor.rs`, `render/`, etc.). 
- **Rule**: Do not create "God Objects" that manage multiple disconnected responsibilities.

## 5. Verification Standards
- **Rule**: Always run `cargo check` after any structural change to ensure zero errors and zero warnings.
- **Rule**: Avoid placeholders. If an asset is needed, generate or use a real representative file.

# (Proje Adı) AI Constitutional Guidelines (v2026)

- **API Accuracy (Strict Enforcement)**: AI, wgpu, winit, egui vb. kullanılan tüm kütüphanelerin internetten (webden) kontrol edilerek elde edilen en güncel kararlı dökümantasyonuna sadık kalmak zorundadır. Eski sürüm API'larını kullanmak veya önermek kesinlikle YASAKTIR.
- **Kırıcı Değişiklikler (Breaking Changes) ve Onay**: Eğer bir kütüphanenin en güncel kararlı sürümüne geçiş, kod tabanında geniş çaplı kırıcı değişiklikler (breaking changes) veya büyük yapısal refaktörler gerektiriyorsa; AI bu güncellemeyi doğrudan uygulamamalıdır. Öncelikle durum hakkında kullanıcıya detaylı bilgi vermeli ve onay istemelidir.
- **Modern Rust Standartları**: Pipeline & Bindings: Kodlar; en güncel kütüphane sürümleriyle gelen en yeni struct alanlarını (örn: depth_slice, multiview_mask) ve zorunlu hale gelen cache yapılarını içermelidir.
- **Memory Safety**: Option sarmalları ve Rust edisyonunun gerektirdiği en güncel modern Result yönetimi kullanılmalıdır.
- **Deprecated Uyarısı**: Eğer kütüphane dokümantasyonunda bir fonksiyon veya struct "deprecated" olarak işaretlenmişse, AI bunu asla kullanmamalı, direkt güncel karşılığına geçmelidir.
- **Doğrulama**: AI, kod üretirken wgpu::RenderPipelineDescriptor veya wgpu::Device gibi tüm kütüphane metodlarının en güncel kararlı imzasıyla eşleştiğini doğrulamakla yükümlüdür. AI, tahmin yürütmek yerine, güncel kütüphane imzasını internetten kontrol ederek yazmalıdır.
- **Documentation**: Yaptığın her köklü değişikliği `///` ile kodun içine ve ilgili .md dosyalarına işle. AI her işe başlarken kendi geçmiş .md raporlarını okumalıdır.
- **Modularization**: Dosyalar 800 satırı geçmemelidir. Mantıksal ayrımı temiz tut.
- **Zero-Error Policy**: Projenin derlenme durumu her zaman 0 error / 0 warning hedefinde tutulmalıdır.

## 6. Strict File Boundary Enforcement

- **Rule**: Existing module boundaries are ABSOLUTE and MUST NOT be changed.
- **Rule**: DO NOT merge files, collapse modules, or move logic between modules unless explicitly instructed.
- **Rule**: Each module has a SINGLE responsibility and must remain isolated.
- **Rule**: If a task requires changes across multiple modules, modify them individually — NEVER combine them.

- **Violation Condition**:
If an AI merges modules, combines files, or centralizes logic into a single file, the solution is INVALID.

## 7. Scope Isolation Rule

- **Rule**: AI must ONLY modify the explicitly requested file or module.
- **Rule**: Expanding scope beyond the requested area is FORBIDDEN.
- **Rule**: No "improvements", "refactors", or "optimizations" outside the given scope.

- **Example**:
If asked to fix a bug in `translate.rs`:
→ ONLY modify `translate.rs`
→ DO NOT touch `render.rs`, `input.rs`, or other modules

- **Violation Condition**:
If AI modifies unrelated modules, the answer is INVALID.

## 8. No Implicit Refactor Rule

- **Rule**: Refactoring is ONLY allowed when explicitly requested.
- **Rule**: AI must NOT reorganize, restructure, or rewrite code unless instructed.

- **Forbidden Actions**:
- Combining multiple modules into one
- Moving logic "for readability"
- Changing architecture without explicit permission

- **Allowed**:
- Minimal, localized fixes

## 9. Architecture Preservation Rule

- **Rule**: The current architecture is considered STABLE and must be preserved.
- **Rule**: AI must treat the codebase as a production system, not a prototype.

- **Priority Order**:
1. Stability
2. Modularity
3. Readability
4. Performance

- **Rule**:
If a change risks breaking architecture, it must NOT be applied.

## 10. Small Function Rule

- **Rule**: Functions should remain small and focused (ideally <100 lines).
- **Rule**: If a function grows too large, split it into smaller helper functions.

- **Forbidden**:
- Large monolithic functions
- Multi-responsibility functions

- **Goal**:
AI-friendly, low-context code.

## 11. Multi-Module Coordination Rule

- **Rule**: When working across multiple modules:
    1. Analyze all modules
    2. Propose a plan
    3. Wait for confirmation
    4. Then implement

- **Rule**: Direct multi-module changes without planning are FORBIDDEN.
## 12. Anti-God Object Rule

- **Rule**: No struct may exceed a reasonable responsibility scope.
- **Rule**: Large structs must be decomposed into smaller sub-systems.

- **Forbidden**:
- Centralized "manager" objects controlling everything
- Overloaded state containers (God Objects)

- **Goal**:
Composable systems with clear boundaries.

## 13. AI Behavior Lock

- AI must NOT assume missing context.
- AI must NOT "improve" unspecified areas.
- AI must NOT take initiative beyond instructions.

- If uncertain:
→ ASK instead of modifying code.

## 14. Single Responsibility Per File (SRP)

- **Rule**: Every single `.rs` file must have ONE specific, clearly defined responsibility and execute it exceptionally well.
- **Rule**: If a file manages pipelines, it should NOT manage uniform buffers or input. 
- **Goal**: Strict adherence to SOLID principles, ensuring maximum maintainability and predictability. The "One File, One Task" architecture ensures that bugs are deeply localized.

## 15. Memory Bank Rule (ZORUNLU)

Aeon Engine uses a persistent `memory-bank/` folder as the long-term project memory for all AI assistants.

- **Rule**: Before starting any task, the AI MUST read all existing files inside the `memory-bank/` folder.
- **Rule**: `projectbrief.md` is the source of truth for project scope, goals, and core requirements.
- **Rule**: `activeContext.md` MUST reflect the current focus, recent changes, next steps, active decisions, and important learnings.
- **Rule**: `progress.md` MUST track what works, what is left to build, current status, known issues, and the evolution of project decisions.
- **Rule**: `systemPatterns.md` MUST document architecture, technical decisions, design patterns, component relationships, and critical implementation paths.
- **Rule**: `techContext.md` MUST document technologies, dependencies, development setup, technical constraints, and tool usage patterns.
- **Rule**: `productContext.md` MUST document why the project exists, what problems it solves, how it should work, and user experience goals.

### Required Memory Bank Structure

The following files are required:

```txt
memory-bank/
  projectbrief.md
  productContext.md
  activeContext.md
  systemPatterns.md
  techContext.md
  progress.md
```
## 16. Documentation Quality Score Rule (ZORUNLU)

Aeon Engine does not accept low-quality `///` documentation comments. Documentation must be useful for humans, future AI assistants, and long-term maintenance.

- **Rule**: Every new or modified `pub mod`, `pub struct`, `pub enum`, `pub trait`, `pub fn`, and important public field MUST have a documentation quality score.
- **Rule**: The AI MUST internally evaluate every `///` doc-comment with a score from `0/10` to `10/10`.
- **Rule**: Any documentation below `8/10` is considered unacceptable and MUST be improved before the task is completed.
- **Rule**: The AI MUST prefer clear technical explanations over vague comments.
- **Rule**: Documentation must explain purpose, responsibility, constraints, and usage when relevant.
- **Rule**: Documentation must NOT simply repeat the item name.

### Documentation Score Meaning

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

## 17. Copyright and SPDX Header Rule (ZORUNLU)

- **Rule**: Every project source file (e.g., `.rs`, `.wgsl` files) MUST begin with the following exact two-line header:
  ```rust
  // SPDX-License-Identifier: (Lisans Türü)-only
  // Copyright (c) 2026 (Github Adı) / (Proje Adı). All rights reserved.
  ```
- **Rule**: When creating a new `.rs` or `.wgsl` source file, this header must be prepended at the very top.
- **Rule**: AI assistants and developers MUST preserve this header during any modifications or refactoring.
- **Rule**: If a file is updated in a subsequent year (e.g., 2027, 2028, etc.), the copyright year in the header should be updated to reflect the active year or year range (e.g., `2026-2027` or `2026-2028`).

  
## 18. Comprehensive Quality, Formatting, and Performance Verification Rule (ZORUNLU)

AI assistants MUST NOT declare a task completed simply by saying "code edited" or "test passed". The AI is required to execute a comprehensive suite of static and dynamic checks and report concrete quantitative metrics to the user.

- **Rule 18.1 (Mandatory Multi-Tool Verification Suite)**: Before declaring any task, feature implementation, or bug fix complete, the AI MUST execute:
  1. `cargo clippy --workspace --all-targets` (Ensure zero lints, warnings, or idiom violations).
  2. `cargo fmt --check` (Ensure code adheres 100% to Rust code style standards).
  3. `cargo test --workspace` (Ensure all workspace unit, integration, and doc tests pass 100%).

- **Rule 18.2 (Quantitative Performance & Resource Metric Reporting)**: The AI MUST NOT give vague or generic summaries. When introducing or modifying systems (rendering pipelines, ECS queries, physics step, audio math, asset management, UI processors), the AI MUST analyze and report:
  - **Performance & Benchmarks**: Run benchmarks (`cargo bench` or runtime profiler metrics) and report exact percentage changes (e.g., *"This change improved raycast throughput by +12%"* or *"Warning: This change degraded render execution time by 8%"*).
  - **Memory Allocations**: Explicitly report whether the change introduces heap allocations (`Vec::new()`, `Box`, `String`, `HashMap`) inside hot inner loops (per-frame `update` or `render`), or if allocations remain 0-allocation / stack-bound.
  - **Binary Size Impacts**: Measure and report binary size impact when adding new external crate dependencies or large static assets (e.g., *"Binary size delta: +14 KB"*).

- **Rule 18.3 (Mandatory Verification Report Block)**: Every final response summary MUST include a **Verification & Performance Audit Report** block formatted as follows:

  ```markdown
  ### 📊 Verification & Performance Audit Report
  - 🛠️ **Cargo Clippy**: 0 warnings / 0 errors
  - 🎨 **Cargo Format**: 100% Compliant (`cargo fmt --check`)
  - 🧪 **Cargo Test**: All tests passed (X passed, 0 failed)
  - ⚡ **Performance Impact**: [Quantitative impact, e.g., "0.0% change" or "-8.2% degradation detected"]
  - 🧠 **Memory Allocations**: [Zero allocations in hot loop / Allocation delta]
  - 📦 **Binary Size Impact**: [Impact delta, e.g., "+0 KB"]
  ```
## 19. Strict Redundant Clone Prevention Rule (Gereksiz `.clone()` Kullanımı Yasaktır - (ZORUNLU)

Kolaycılığa kaçılarak yapılan gereksiz `.clone()` çağrıları Aeon Engine kod tabanında KESİNLİKLE YASAKTIR.

- **Rule 19.1 (No Unnecessary Clones)**: Sırf borçlanma denetleyicisini (borrow checker) tatmin etmek veya kolayca geçiştirmek için nesneleri klonlamak kesinlikle yasaktır. Her `.clone()` çağrısının teknik bir zorunluluğu olmak zorundadır.
- **Rule 19.2 (Ownership & Move Semantics First)**: Taşınabilecek (`move`) veya referans (`&`/`&mut`) verilebilecek durumlarda klonlama yapılmamalıdır. Bir vektör veya veri yapısı kullanım sonrasında temizleniyorsa veya tüketiliyorsa `std::mem::take`, `std::mem::replace` veya `.into_iter()` / `.into_par_iter()` kullanılmalıdır.
- **Rule 19.3 (No Clone on Copy Types)**: `Copy` niteliğine sahip türler (`Vertex`, `[f32; N]`, ilkel türler vb.) üzerinde `.clone()` çağrısı yapılması yasaktır (`clippy::clone_on_copy`).
- **Rule 19.4 (Hot Path Heap Allocation Lock)**: Sıcak döngülerde (per-frame `update`, `render`, mesh üretimi veya mipmap/kaplama işleme) gereksiz `Vec`, `String`, `Box` veya büyük veri yapısı klonlaması yapmak doğrudan kural ihlalidir.


---
## 20. Strict Prohibition of Linter Warning Suppression Attributes (`#[allow(clippy::...)]` Yasağı - ZORUNLU)

Derleyici veya linter uyarılarını (özellikle Clippy) kolay yoldan susturmak amacıyla koda `#[allow(clippy::...)]` veya `#[allow(...)]` nitelikleri eklemek Aeon Engine kod tabanında **KESİNLİKLE YASAKTIR**.

- **Rule 20.1 (Universal Ban on Clippy Suppressions)**: `#[allow(clippy::too_many_arguments)]`, `#[allow(clippy::type_complexity)]`, `#[allow(clippy::needless_range_loop)]`, `#[allow(clippy::manual_clamp)]`, `#[allow(clippy::if_same_then_else)]` ve benzeri uyarı susturma niteliklerinin kullanımı istisnasız yasaktır.
- **Rule 20.2 (Refactor Over Suppression)**: Bir fonksiyon veya veri yapısı linter uyarısı veriyorsa, bu bir mimari koku (code smell) işaretidir. Uyarıyı susturmak yerine kodun mimarisi temiz bir şekilde yeniden düzenlenmelidir (refactor):
  - **Too Many Arguments (>7 parametre)**: Fonksiyon parametreleri mantıksal ve temiz bir Context / Parameters / Descriptor struct'ı (`RenderParams`, `GizmoInputParams`, `GeometryParseOptions` vb.) altında toplanmalıdır.
  - **Type Complexity (Karmaşık Tipler)**: Uzun tuple'lar veya karmaşık closure/tip tanımları için anlamlı `type` takma adları (type alias) veya özel struct/veri yapıları oluşturulmalıdır.
  - **Diğer Clippy Uyarıları**: Idiomatic Rust çözümleri (`clamp()`, `zip()`, `if-let` zincirleri vb.) uygulanmalıdır.
- **Rule 20.3 (Root-Cause Engineering)**: Linter uyarıları motorun kod kalitesini, okunabilirliğini ve güvenliğini koruyan birer rehberdir; halının altına süpürülemez. Her uyarı kök nedeninde çözülmelidir.





---
