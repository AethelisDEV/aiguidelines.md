# AI Guidelines for (Proje Adı) Development

> [!IMPORTANT]
> All AI assistants contributing to (proje ismi) MUST read, strictly understand, and unconditionally adhere to these guidelines before proposing, modifying, or executing any code changes.

---

## 1. Zero Unsafe Policy & Modern Memory Safety (ZORUNLU)

- **Rule 1.1 (100% Safe Rust Mandate)**: Do NOT use `unsafe` blocks under any circumstances. Memory safety is the highest priority in Aeon Engine. We prioritize safety and stability over micro-optimizations that bypass the borrow checker. If a task seems to require `unsafe`, find a safe alternative using higher-level abstractions or standard library patterns.
- **Rule 1.2 (Modern Rust Standards & Result Management)**: Always use modern Rust idioms, strict `Option` / `Result` propagation (`?` operator), and robust error handling. Never use `.unwrap()` in production engine logic without mathematically verified invariants.
- **Rule 1.3 (API Accuracy & Deprecated API Prohibition)**: AI MUST verify and strictly adhere to the latest stable documentation of external crates (`wgpu`, `winit`, `egui`, `hecs`, `glam`, etc.). Using deprecated APIs, outdated struct fields, or legacy signatures is strictly forbidden.
- **Rule 1.4 (Breaking Changes & User Confirmation)**: If updating a library or introducing a breaking change across multiple crates is required, the AI MUST inform the user and request explicit confirmation before executing.

---

## 2. Mandatory Triple-Slash (`///`) Documentation & Quality Score Rule (ZORUNLU)

- **Rule 2.1 (Mandatory Doc-Comments)**: Every new or modified `pub mod`, `pub struct`, `pub enum`, `pub trait`, `pub fn`, and important public field MUST include triple-slash (`///`) doc-comments.
- **Rule 2.2 (Documentation Quality Score Requirement)**: The AI MUST internally evaluate every `///` doc-comment with a quality score from `0/10` to `10/10`. Any documentation below `8/10` is strictly unacceptable and MUST be improved prior to task completion.
- **Rule 2.3 (Meaningful Technical Descriptions)**: Documentation must explain purpose, responsibility, constraints, failure modes, and integration usage. Merely repeating the item name is forbidden.

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

## 3. Persistent Knowledge Base & Memory Bank Synchronization (ZORUNLU - Asla Ertelenemez)

- **Rule 3.1 (Mandatory Synchronous Documentation Execution)**: Kodda yapılan EN UFAK değişiklik dahi tamamlandığında, AI asistanı cevabını kullanıcıya sunmadan ÖNCE şu 3 dosya grubunu eşzamanlı olarak güncellemek ZORUNDADIR:
  1. `knowledge/history/bug_fixes.md`: Çözülen her bug, crash, render hatası, UI çakışması, scroll sızıntısı veya fizik/matematik senkronizasyon problemi.
  2. `knowledge/history/engine_features.md`: Eklenen her özellik, UI/UX tasarımı, bileşen kartı, render boru hattı veya mimari refaktör.
  3. `memory-bank/activeContext.md` & `memory-bank/progress.md`: Aktif oturum bağlamı, yapılan son değişiklikler ve tamamlanan kilometre taşları.
- **Rule 3.2 (Zero-Postponement Policy)**: "Sonra yazarım", "toplu yaparım" veya "kullanıcı sorarsa güncellerim" KESİNLİKLE YASAKTIR. Her etkileşim turunda değişiklikler ANINDA bu dosyalara işlenmelidir.
- **Rule 3.3 (Memory Bank Reading on Task Start)**: AI işe başlamadan önce `memory-bank/` dizinindeki tüm dosyaları (`projectbrief.md`, `productContext.md`, `activeContext.md`, `systemPatterns.md`, `techContext.md`, `progress.md`) ve `knowledge/history/` altındaki ilgili geçmiş kayıtları okumakla yükümlüdür.
- **Rule 3.4 (Dedicated Feature Documentation)**: Önemli her yeni mimari veya büyük sistem için `knowledge/` dizini altında açıklayıcı bir `.md` belgesi oluşturulmalı veya güncellenmelidir.

---

## 4. Modular Responsibility, SRP & Clean Boundaries (ZORUNLU)

- **Rule 4.1 (Single Responsibility Per File - SRP)**: Every single `.rs` file must have ONE specific, clearly defined responsibility and execute it exceptionally well. Keep `main.rs` thin and delegate logic to specialized modules.
- **Rule 4.2 (Anti-God Object Mandate)**: No struct or module may exceed a reasonable responsibility scope. Centralized "manager" objects controlling disconnected domains are strictly forbidden. Large structures must be decomposed into focused sub-systems.
- **Rule 4.3 (File & Function Size Limits)**:
  - Source files MUST NOT exceed 800 lines. Split large modules logically.
  - Functions should remain small and focused (ideally <100 lines). Break monolithic routines into private helper functions.
- **Rule 4.4 (Strict File Boundary Enforcement)**: Existing module boundaries are absolute. Do NOT merge files, collapse modules, or centralize logic into single files unless explicitly instructed.

---

## 5. Scope Isolation, Architecture Preservation & AI Behavior Lock (ZORUNLU)

- **Rule 5.1 (Scope Isolation)**: AI must ONLY modify the explicitly requested file or module. Expanding scope or making unrequested "improvements", "refactors", or "optimizations" outside the target area is strictly forbidden.
- **Rule 5.2 (No Implicit Refactoring)**: Reorganizing, restructuring, or rewriting code without explicit instruction is forbidden. The existing architecture is considered STABLE and production-grade.
- **Rule 5.3 (AI Behavior Lock & Uncertainty Resolution)**: AI must NOT assume missing context or take unauthorized initiative. If uncertain or facing architectural ambiguity, the AI MUST ASK the user rather than guessing or making assumptions.

---

## 6. Comprehensive Quality, Formatting & Performance Verification (ZORUNLU)

AI assistants MUST NOT declare a task completed simply by saying "code edited" or "test passed". The AI is required to execute a comprehensive suite of static and dynamic checks and report concrete quantitative metrics to the user.

- **Rule 6.1 (Mandatory Multi-Tool Verification Suite)**: Before declaring any task, feature implementation, or bug fix complete, the AI MUST execute:
  1. `cargo clippy --workspace --all-targets` (Ensure zero lints, warnings, or idiom violations).
  2. `cargo fmt --check` (Ensure code adheres 100% to Rust code style standards).
  3. `cargo test --workspace` (Ensure all workspace unit, integration, and doc tests pass 100%).
- **Rule 6.2 (Quantitative Performance & Resource Reporting)**: The AI MUST analyze and report concrete quantitative impacts:
  - **Performance & Benchmarks**: Run benchmarks or measure profile metrics when modifying rendering, ECS, physics, or asset pipelines.
  - **Memory Allocations**: Explicitly verify zero heap allocations (`Vec::new()`, `Box`, `String`, `HashMap`) in per-frame `update`/`render` hot loops.
  - **Binary Size Impacts**: Report binary size impact when adding new external crate dependencies or static assets.
- **Rule 6.3 (Mandatory Verification Report Block)**: Every final response summary MUST include the following verification block:

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

## 7. Copyright and SPDX Header Rule (ZORUNLU)

- **Rule 7.1 (Header Requirement)**: Every project source file (e.g., `.rs`, `.wgsl` files) MUST begin with the following exact two-line header:
  ```rust
  // SPDX-License-Identifier: (Lisans Adı)
  // Copyright (c) 2026 (Kullanıcı Adı) / (Proje Adı). All rights reserved.
  ```
- **Rule 7.2 (Preservation & Creation)**: When creating a new `.rs` or `.wgsl` source file, this header must be prepended at the very top. AI assistants and developers MUST preserve this header during any modifications or refactoring. If a file is updated in a subsequent year, update the copyright year range accordingly.

---

## 8. Strict Redundant Clone Prevention Rule (Gereksiz `.clone()` Yasağı - ZORUNLU)

Kolaycılığa kaçılarak yapılan gereksiz `.clone()` çağrıları (Proje Adı) kod tabanında KESİNLİKLE YASAKTIR.

- **Rule 9.1 (No Unnecessary Clones)**: Sırf borçlanma denetleyicisini (borrow checker) tatmin etmek veya kolayca geçiştirmek için nesneleri klonlamak kesinlikle yasaktır. Her `.clone()` çağrısının teknik bir zorunluluğu olmak zorundadır.
- **Rule 9.2 (Ownership & Move Semantics First)**: Taşınabilecek (`move`) veya referans (`&`/`&mut`) verilebilecek durumlarda klonlama yapılmamalıdır. Bir vektör veya veri yapısı kullanım sonrasında temizleniyorsa veya tüketiliyorsa `std::mem::take`, `std::mem::replace` veya `.into_iter()` / `.into_par_iter()` kullanılmalıdır.
- **Rule 9.3 (No Clone on Copy Types)**: `Copy` niteliğine sahip türler (`Vertex`, `[f32; N]`, ilkel türler vb.) üzerinde `.clone()` çağrısı yapılması yasaktır (`clippy::clone_on_copy`).
- **Rule 9.4 (Hot Path Heap Allocation Lock)**: Sıcak döngülerde (per-frame `update`, `render`, mesh üretimi veya mipmap/kaplama işleme) gereksiz `Vec`, `String`, `Box` veya büyük veri yapısı klonlaması yapmak doğrudan kural ihlalidir.

---

## 9. Absolute Prohibition of Hardcoded Magic String Heuristics (Bütünsel Veri Odaklı Mimari - ZORUNLU)

(Proje Adı)'in **HİÇBİR modülünde, katmanında, sisteminde veya algoritmasında** (Render, Fizik, ECS, Animasyon, Ses, UI, Editör, İçe Aktarma / Asset Pipeline, Sahne Yönetimi, Scripting, Eklenti / Plugin Sistemi vb.) kolaycılığa kaçılarak yapılan sihirli dize (magic string / string pattern matching / substring contains) filtrelemeleri ve ad temelli tahmin yürütme yaklaşımları **KESİNLİKLE VE İSTİSNASIZ YASAKTIR**.

- **Rule 10.1 (Universal Ban on String-Based Subsystem Logic)**: Motorun hiçbir yerinde değişken, nesne, materyal, düğüm, kemik, ses, dosya veya bileşen isimlerine bakılarak (`name.contains(...)`, `str.starts_with(...)`, hardcoded ad filtreleri vb.) sistemsel, mantıksal, fiziksel veya grafiksel kararlar **VERİLEMEZ**.
- **Rule 10.2 (100% Data-Driven & Explicit Type System Mandate)**: Bütün kararlar ve davranışlar; doğrudan **tip güvenli Rust enum'ları, struct alanları, matematiksel/fiziksel veri analizleri, formatların resmi standartları (glTF 2.0, FBX, WAV, PNG, WGSL vb.) veya açıkça tanımlanmış ECS bileşenleri (`Component`)** üzerinden yürütülmelidir.
- **Rule 10.3 (Asset, Language & Localization Independence)**: Motor, dünyanın herhangi bir yerinden yüklenen veya herhangi bir dilde (İngilizce, Türkçe, Japonca, Almanca vb.) isimlendirilmiş her türlü varlık ile %100 kusursuz ve kararlı çalışmak zorundadır. İsimlendirmeye dayalı varsayımlar motoru kırıcı mimari kusurlar olarak kabul edilir.
- **Rule 10.4 (Engine-Wide Industrial Solution Mandate)**: Asla geçici string kontrolleri ("quick-and-dirty hack") yazılmamalıdır. Çözüm HER ZAMAN motorun mimarisini güçlendiren, genel, modüler ve endüstri standardı bir sistem tasarımı olarak uygulanmalıdır.

---

## 10. Strict Prohibition of Linter Warning Suppression Attributes (`#[allow(...)]` Yasağı - ZORUNLU)

Derleyici veya linter uyarılarını (özellikle Clippy) kolay yoldan susturmak amacıyla koda `#[allow(clippy::...)]` veya `#[allow(...)]` nitelikleri eklemek Aeon Engine kod tabanında **KESİNLİKLE YASAKTIR**.

- **Rule 11.1 (Universal Ban on Clippy Suppressions)**: `#[allow(clippy::too_many_arguments)]`, `#[allow(clippy::type_complexity)]`, `#[allow(clippy::needless_range_loop)]`, `#[allow(clippy::manual_clamp)]`, `#[allow(clippy::if_same_then_else)]` ve benzeri uyarı susturma niteliklerinin kullanımı istisnasız yasaktır.
- **Rule 11.2 (Refactor Over Suppression)**: Bir fonksiyon veya veri yapısı linter uyarısı veriyorsa, bu bir mimari koku (code smell) işaretidir. Uyarıyı susturmak yerine kodun mimarisi temiz bir şekilde yeniden düzenlenmelidir:
  - **Too Many Arguments (>7 parametre)**: Fonksiyon parametreleri mantıksal bir Context / Parameters / Descriptor struct'ı (`RenderParams`, `GizmoInputParams`, `GeometryParseOptions` vb.) altında toplanmalıdır.
  - **Type Complexity (Karmaşık Tipler)**: Uzun tuple'lar veya karmaşık closure/tip tanımları için anlamlı `type` takma adları (type alias) veya özel struct/veri yapıları oluşturulmalıdır.
  - **Diğer Clippy Uyarıları**: Idiomatic Rust çözümleri (`clamp()`, `zip()`, `if-let` zincirleri vb.) uygulanmalıdır.
- **Rule 11.3 (Root-Cause Engineering)**: Linter uyarıları halının altına süpürülemez; her uyarı kök nedeninde çözülmelidir.

---
