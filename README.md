<!-- repo-header:start -->
<img src="https://github.com/dcondrey.png?size=160" alt="C2PA Content Credential Provenance Chains for NLP Experiments logo" width="120" align="left">

<h1>C2PA Content Credential Provenance Chains for NLP Experiments</h1>

<p><strong>C2PA content credential provenance chains for reproducible NLP experiments</strong></p>

<br clear="left">

[![Best Practices Evidence](https://img.shields.io/badge/best%20practices-evidence%20reviewed-6a4c93?style=flat-square&labelColor=20232a)](.bestpractices.json) [![C2PA](https://img.shields.io/badge/standard-C2PA%20related-6a4c93?style=flat-square&labelColor=20232a)](https://c2pa.org/) [![GitHub Sponsors](https://img.shields.io/badge/GitHub%20Sponsors-Sponsor-EA4AAA?style=flat-square&labelColor=20232a)](https://github.com/sponsors/dcondrey)
<!-- repo-header:end -->

> **Content Authenticity for Reproducible NLP: Provenance Chains as Experimental Infrastructure**
> David Condrey, WritersLogic Inc. CLEF 2026.

## Manifest Chain

The `manifests/` directory contains a five-stage provenance chain with real SHA-256 hashes from our [PAN@CLEF 2026 Reasoning Trajectory Detection](https://github.com/dcondrey/trajectory-detection-clef2026) submission (1st place source detection, 0.85 macro F1):

| Manifest | Stage | Key Artifact Hash (prefix) |
|----------|-------|---------------------------|
| `m1_data_ingestion.json` | Training data (87,696 samples) | `60f6ef33...` |
| `m2_feature_extraction.json` | Feature code (30 features) | `46ec38f7...` |
| `m3_model_training.json` | 5-seed LightGBM ensemble | `ed353e7c...` (seed42) |
| `m4_inference.json` | Submitted predictions | `fba7d9cf...` (S1) |
| `m5_evaluation.json` | Official results | 0.85 F1 (S1) |

Each manifest references the previous stage as a C2PA *ingredient*, forming a cryptographically bound chain. Modifying any artifact invalidates all downstream hashes.

## How It Works

The methodology uses three existing C2PA mechanisms with no specification changes:

1. **Ingredient chaining** links pipeline stages (the same mechanism used for photo editing chains)
2. **Custom assertions** (`org.writerslogic.experiment.*`) embed experiment metadata
3. **The attestation feature** (dormant in spec, activation in progress) provides the integration point for process evidence

## Generating Your Own Chain

```bash
# Compute hash for your training data
DATA_HASH=$(cat data/train/*.jsonl | shasum -a 256 | cut -d' ' -f1)

# Generate manifest (adapt m1_data_ingestion.json as template)
# Sign with c2patool
c2patool your_artifact.pdf --manifest your_manifest.json --output signed_artifact.pdf
```

## Process Evidence (PoSME)

The paper also describes how [Proofs of Sequential Memory Execution](https://datatracker.ietf.org/doc/draft-condrey-cfrg-posme/) (PoSME) constrain the "honest liar" problem by preventing parallelization and bounding time-compression to ~2x the memory-latency floor. Process evidence connects to C2PA via the attestation feature but operates as an architecturally independent verification system.

## License

CC BY 4.0
