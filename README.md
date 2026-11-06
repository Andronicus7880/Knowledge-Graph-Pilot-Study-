# LLM-Assisted Knowledge Graph for Wheat Stress Response Literature

> A pilot workflow that uses a large language model (DeepSeek) to extract genes, QTLs, stress conditions, phenotypes and pathways from wheat stress-response abstracts and assemble them into an interactive knowledge graph.



## Overview

Wheat (*Triticum aestivum* L.) productivity is threatened by drought, salinity, heat and pathogens, and the literature describing stress-responsive genes and regulatory pathways is growing faster than anyone can manually review. This project tests whether an LLM can turn unstructured abstracts into structured, queryable biological relationships.

The pipeline retrieves records from Web of Science, extracts entities and relationships with an LLM, validates the output, and produces a graph that can be explored interactively, with every relationship traceable back to its supporting sentence.

## Key Results (pilot, n = 10 abstracts)

| Metric | Value |
| --- | --- |
| Web of Science records retrieved (2016–2026) | 3,213 |
| Papers analysed | 10 |
| Papers yielding usable graph output | 5 (R2, R3, R4, R9, R10) |
| Nodes | 26 (22 unique) |
| Edges | 22 |
| Mean / median edge confidence | 0.84 / 0.85 |
| Confidence range | 0.70 – 0.95 |

**Biological themes captured**

- **Drought tolerance:** *TaFBX361* (F-box gene; confidence 0.95) and a predicted lncRNA–miRNA–mRNA link with *XR_006461531* (0.70)
- **Salinity response:** *CHX2*, *PK2*, *PUBs* and *NTP10*, linked to salt stress and germination performance via GWAS/QTL associations
- **Arbuscular mycorrhizal symbiosis:** associated with drought tolerance (0.90), with QTLs on chromosomes 3D and 7D (0.85)

> **Note:** This is a proof of concept, not a validated resource. See [Limitations](#limitations).

## Workflow

1. **Literature retrieval:** Web of Science search combining wheat identifiers, abiotic stress terms and omics/genetics keywords (2016–2026); top 10 ranked records exported for the pilot.
2. **Abstract processing:** abstracts extracted and standardised.
3. **LLM extraction:** DeepSeek configured to identify six entity classes and the relationships between them.
4. **Validation:** JSON/schema checks, plus manual comparison of extracted relationships against source abstracts.
5. **Storage and visualisation:** results saved as JSON, Excel and interactive HTML network files.

### Extracted entity types

Genes · Proteins · Stress conditions · QTLs · Phenotypes · Biological pathways

### Extracted relationship types

- `ASSOCIATED_WITH`: statistical, GWAS or QTL-mapping associations
- Regulatory interactions: activation or inhibition of genes and pathways
- Functional relationships: stress tolerance or susceptibility

Each edge keeps the model-assigned confidence score and its supporting sentence.

### Output formats

| Format | Purpose |
| --- | --- |
| JSON | Primary storage; preserves the full hierarchical LLM response and allows recovery from interrupted runs |
| Excel (`.xlsx`) | Relational querying, QC, validation and manual review |
| HTML (PyVis) | Interactive graph exploration |



## Limitations

- **Small sample:** only 10 papers were analysed and only 5 produced relationships, so patterns may not generalise.
- **Single database:** Web of Science only; relevant work indexed elsewhere is missing.
- **Abiotic bias:** biotic stress is in scope, but no biotic-stress relationships reached the final graph (a *Fusarium* node appeared in a record that failed to parse).
- **Truncated outputs:** R6 and R8 failed JSON validation because outputs were cut off at roughly 2,000 characters, likely losing relationships (e.g. *TaKIN* and hormone-related drought/heat pathways).
- **Empty extractions:** R1, R5 and R7 returned valid JSON with no relationships; these have not been checked for missed extractions.
- **Unvalidated confidence scores:** scores are the model's own estimates, not checked against expert curation or a reference set.
- **Information loss:** reducing abstracts to single-sentence triples can lose direction (e.g. down-regulation recorded as "induced by") and context.
- **Schema inconsistency:** truncated outputs contained attributes absent from successful records.

## Roadmap

- [ ] Expand sources beyond Web of Science to PubMed and Scopus
- [ ] Scale from 10 to 10,000+ abstracts
- [ ] Include biotic stress topics (rust, powdery mildew, *Fusarium*)
- [ ] Fix output truncation and standardise extraction/validation schemas
- [ ] Compare multiple LLMs
- [ ] Validate against expert-curated datasets and report precision and recall



## Acknowledgements

[Dr Mengyuan Wang, Original LLM pipeline source]

