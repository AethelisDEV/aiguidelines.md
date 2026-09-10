# AI_GUIDELINES

A deterministic rule set and execution harness designed to constrain LLM code generation, prevent context drift, and enforce architectural invariants in large-scale codebases.

---

### Problem & Purpose

LLMs working on complex software projects introduce recurring failure modes: unauthorized refactoring, silent file merging, suppression of compiler warnings, and architectural erosion.

`AI_GUIDELINES.md` acts as an operational boundary. By establishing strict behavioral constraints and structural thresholds, it enables lightweight models to maintain 100k+ LOC systems without code degradation or hallucinated API paths.

### Core Enforcements

* **Memory Safety & Invariants:** Strict ban on `unsafe` blocks; mandatory explicit error propagation via modern idiomatic types.
* **Context Preservation (SRP & Limits):** Hard upper limit of 800 lines per file and single-responsibility decomposition to fit cleanly within model attention windows.
* **Scope Isolation:** Strict prohibition of out-of-scope edits and unrequested file modifications.
* **Warning Suppression Ban:** Complete restriction of `#[allow(...)]` or equivalent linter-silencing attributes; forces root-cause architectural fixes.
* **Deterministic Verification:** Mandatory terminal check execution (`clippy`, `fmt`, `test`) and allocation analysis before accepting output.

### Usage

1. Place `AI_GUIDELINES.md` in the workspace root directory.
2. Adjust environment-specific placeholders (SPDX identifiers, workspace paths, build tooling).
3. Inject the file into the model's system prompt or session initialization context.

---

<details>
<summary>🇹🇷 Türkçe Açıklama</summary>

### Amaç ve Kapsam

Büyük ölçekli projelerde çalışan yapay zeka modelleri zamanla istenmeyen yeniden yapılandırmalar yapma, tekil dosya boyutlarını kontrolsüzce büyütme ve derleyici uyarılarını bastırma eğilimi gösterir.

`AI_GUIDELINES.md`, modele katı mimari sınırlar çizen teknik bir kural kümesidir. Modeli bağımsız bir geliştirici gibi değil, tanımlı sınırlar içinde çalışan bir derleyici asistanı gibi çalışmaya zorlayarak büyük kod tabanlarında bağlam kaybını engeller.

### Temel Kurallar

* **Bellek Güvenliği:** `unsafe` bloklarının koşulsuz yasaklanması ve katı tip kontrolü.
* **Dosya Sınırları:** Bağlam penceresini korumak için dosya başına azami 800 satır sınırı ve Tek Sorumluluk İlkesi (SRP) zorunluluğu.
* **Kapsam İzolasyonu:** Yalnızca hedef dosyalarda değişiklik yapılması, talep edilmeyen modüllerin değiştirilmesinin engellenmesi.
* **Uyarı Bastırma Yasağı:** Hataları halının altına süpüren `#[allow(...)]` gibi etiketlerin yasaklanması; kök nedenin çözülmesi zorunluluğu.
* **Doğrulama Testleri:** Kod çıktısı kabul edilmeden önce linter, formatlayıcı ve birim test çalıştırma zorunluluğu.

### Kurulum

1. `AI_GUIDELINES.md` dosyasını projenin ana dizinine ekleyin.
2. Kendi derleme araçlarınıza ve lisans kurallarınıza göre alanları yapılandırın.
3. Modeli başlatırken dosya içeriğini sistem talimatı olarak sağlayın.
</details>
