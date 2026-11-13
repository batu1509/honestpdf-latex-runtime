# honestpdf-latex-runtime

The files behind the [LaTeX to PDF](https://www.gethonestpdf.com/latex-to-pdf) tool on HonestPDF.
Documents are compiled on the visitor's device; this repository only holds the archive that the
site's build downloads and serves from the site itself.

## What a release contains

Each release has one archive, `latex-runtime-<id>.tar`, where `<id>` is `tl2026-` and the first
16 hex characters of a hash of its contents. The release tag is the same id. Inside:

- `wasm/`: the TeX engine, BusyTeX compiled to WebAssembly, in gzipped parts.
- `bundles/`: the base TeX Live files, file indexes, and the pdfLaTeX and XeLaTeX formats. The
  formats are built with the engine itself and hold the hyphenation patterns of every language
  TeX Live has.
- `api/texlive/`: TeX Live packages, one archive each, fetched by the page when a document needs
  them. They come from the TeX Live snapshot of 2026-05-27
  (<https://texlive.info/tlnet-archive/2026/05/27/tlnet/>) and are checked against its database.
- `worker.js`, `xzwasm.js`: the engine's worker and the decompressor for package archives.
- `SOURCES.txt`: where everything comes from.

## How the site uses it

The site's build reads the archive name, URL and SHA-256 from its `runtime.lock.json`, downloads
the archive, checks the hash and unpacks it under `/latex/releases/<id>/`. Visitors' browsers
download the files from the site, never from GitHub.

## Sources and licences

- TeX engine: BusyTeX, GPL. Source of the build used here:
  <https://github.com/SiglumProject/busytex/tree/c2c008ef28f8058fa6888853dbaf55fa91b19eda>
- Base runtime: Siglum 0.2.0 (<https://github.com/SiglumProject/siglum/releases/tag/v0.2.0>).
- Engine JavaScript: `@siglum/engine`, MIT (<https://github.com/SiglumProject/siglum>).
- TeX Live packages: each keeps its own licence (LPPL, GPL, OFL, public domain and others), as
  recorded for it in TeX Live.
