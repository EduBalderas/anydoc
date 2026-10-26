# anydoc

## META
- id: anydoc
- owner: EduBalderas
- visibility: public
- idioma: en
- estado: N/A
- tipo: fork
- nivel: L4
  - L0=gobernanza/política
  - L1=marca/metodología
  - L2=flujo/arquitectura
  - L3=producto ejecutable
  - L4=herramienta
- familia: ninguna
- origen: upstream:https://github.com/firecrawl/anydoc
- experimento: no
- nacido_de: firecrawl/anydoc
- orquestado_por: N/A
- brief_desde: N/A

## OBJETIVO
Convertir documentos de oficina, EPUB, CSV y PDF a Markdown (GitHub Flavored) con la biblioteca Rust `anydoc` 0.2.4 y bindings para Node.js, Python y WebAssembly.

## QUE_CONTIENE
- Crate Rust `anydoc` 0.2.4 (`Cargo.toml`, `src/`): detección de formato, parsers, modelo de documento y serializador a Markdown. `rust-version` 1.88, edition 2024. Licencia MIT.
- Bindings Node.js `@firecrawl/anydoc` (`node/`, Node >= 20) con CLI `anydoc` (`node/cli.js`).
- Bindings Python `firecrawl-anydoc` (`python/`, Python >= 3.10); el módulo de import es `anydoc`.
- Bindings WebAssembly `@firecrawl/anydoc-wasm` (`wasm/`) y la página de demo en `wasm/www`.
- Skill de agente `skills/convert-documents-to-markdown` (autor declarado: firecrawl).
- Ejemplos `examples/convert.rs`, `examples/convert.py` y `examples/convert.mjs`.
- Fixtures y snapshots en `tests/`, objetivos cargo-fuzz en `fuzz/` (excluido del workspace) y harness de benchmark en `bench/`.
- Workflows `.github/workflows/ci.yml`, `pages.yml` y `release.yml`. Versión alineada en `Cargo.toml`, `node/package.json`, `python/Cargo.toml` y `wasm/Cargo.toml` (`scripts/check-versions.sh`).
- `LICENSE`: MIT, copyright 2026 Sideguide Technologies Inc.

## CARACTERISTICAS
- Formatos en `Format` (`src/lib.rs`): Word (`.doc`, `.docx`, `.docm`), PowerPoint (`.ppt`, `.pps`, `.pot`, `.pptx`, `.pptm`, `.ppsx`, `.ppsm`), Excel (`.xls`, `.xlsx`, `.xlsm`, `.xlsb`), OpenDocument (`.odt`, `.ods`, `.odp`), RTF, EPUB, CSV y PDF.
- La detección usa marcadores del contenido (`Format::from_bytes`): cabecera PDF, grupo de apertura RTF, nombres de stream OLE y mimetype/content types del ZIP. CSV no tiene firma; se nombra por extensión o formato explícito.
- Los formatos de oficina pasan por un modelo compartido (`src/model/`) y un serializador Markdown (`src/render/markdown/`). El PDF de texto sale por la dependencia `pdf-inspector` 1.14.2, sin ese modelo (`to_document` no aplica a PDF).
- Ecuaciones de Word/PowerPoint (OMML), OpenDocument/EPUB (MathML) y RTF se convierten a matemática GitHub Flavored (`$...$` y bloques `$$`), según el README de upstream.
- Páginas de PDF escaneadas o solo imagen devuelven `ConvertError::NeedsOcr`. CLI, Node y Python pueden optar por OCR alojado (`--ocr hosted`, `ocr: 'hosted'`, `ocr="hosted"`) hacia Firecrawl Parse. El crate Rust no expone `ocr` y no hace llamadas de red.
- API pública del crate: `to_markdown`, `to_markdown_bytes`, `to_document`.

## FUERA_DE_ALCANCE
- OCR local: la conversión no incluye un motor de OCR; el opt-in alojado existe en CLI, Node y Python, no en el crate Rust.
- `to_document` para PDF: la conversión de PDF produce Markdown directo.
- WASM no convierte desde ruta de archivo; no hay sistema de archivos y la entrada son bytes (`wasm/README.md`).
- El corpus del benchmark no está en el repo (no redistribuible). `bench/README.md` limita el harness a documentos de oficina, texto y presentaciones; los PDF quedan fuera de ese harness.
- El servicio alojado Firecrawl Parse no forma parte de este repositorio.

## RELACIONES
- Fork público de [firecrawl/anydoc](https://github.com/firecrawl/anydoc). El campo `repository` del crate, de npm y de PyPI apunta a ese upstream.
- Homepage del fork y demo: https://firecrawl.github.io/anydoc/ (`wasm/www`, workflow `pages.yml`).
- Paquetes nombrados en el árbol: crates.io `anydoc`, npm `@firecrawl/anydoc`, PyPI `firecrawl-anydoc`, npm `@firecrawl/anydoc-wasm` (`publish = false` en crates.io para el crate wasm).
- Dependencia `pdf-inspector` (https://github.com/firecrawl/pdf-inspector).
- OCR opcional documentado contra [Firecrawl Parse](https://firecrawl.dev/parse). El README de upstream indica que anydoc alimenta ese producto.
- Skill instalable, según el README de upstream, con `npx skills add firecrawl/anydoc`.

## ETIQUETAS
- markdown
- docx
- pdf
- pptx
- xlsx
- epub
- converter
- document
- rust
- mit

## TOPICS_GITHUB
- N/A
