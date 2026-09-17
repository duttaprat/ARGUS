<p align="center">
  <img src="docs/media/argus-symbol.png" alt="ARGUS symbol: an eye combining evidence streams" width="320">
</p>

<h1 align="center">ARGUS</h1>

<p align="center">
  <b>Evidence-Constrained Agentic Reasoning for Noncoding Regulatory Variant Interpretation</b>
</p>

<p align="center">
  <!-- <a href="https://arxiv.org/abs/XXXX.XXXXX"><img src="https://img.shields.io/badge/arXiv-XXXX.XXXXX-b31b1b.svg" alt="arXiv"></a> -->
  <a href="https://huggingface.co/duttaprat/DeepVRegulome"><img src="https://img.shields.io/badge/HuggingFace-DVR%20Models-yellow" alt="HuggingFace"></a>
  <a href="#license"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License"></a>
  <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/python-3.11+-blue.svg" alt="Python 3.11+"></a>
  <!-- <a href="#citation"><img src="https://img.shields.io/badge/NeurIPS%202026-AgenticLS-purple.svg" alt="NeurIPS 2026"></a> -->
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#installation">Installation</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#results">Results</a>
  <!-- • <a href="#citation">Citation</a> -->
</p>

---

## Overview

ARGUS (**A**gentic **R**egulatory **G**enomics for an **U**ncertainty-aware **S**cientist) is a closed-loop agentic framework for interpreting noncoding regulatory variant predictions. Named after Argus Panoptes, the hundred-eyed giant of Greek mythology who watched from every angle simultaneously, ARGUS examines variant predictions through multiple independent evidence sources before forming a verdict.

**The core principle: DVR predicts, ARGUS investigates whether to believe the prediction.**

ARGUS wraps [DeepVRegulome](https://github.com/DavuluriLab/DeepVRegulome)'s 458 DNABERT-based TF binding models in a hypothesis-directed investigation loop where:

- A **deterministic classifier** prevents LLM hallucination structurally (not by prompting)
- A **planner** selects evidence sources based on current uncertainty
- A **verifier** deterministically interprets each observation
- **Intermediate results change the investigation path**
- The LLM enters only at reporting, constrained by pre-classified facts it cannot override

<p align="center">
  <img src="docs/figures/ARGUS_architecture.png" alt="ARGUS Architecture" width="800">
</p>

## Installation

### Prerequisites

- Python 3.11+
- [Conda](https://docs.conda.io/en/latest/miniconda.html) (recommended)
- ANTHROPIC_API_KEY (for the LLM planner and reporter)
- Local ADASTRA, JASPAR, and ENCODE cCRE data (see [Data Setup](docs/data_setup.md))

### Create conda environment

```bash
git clone https://github.com/duttaprat/ARGUS.git
cd ARGUS

conda env create -f environment.yml
conda activate argus
```

### Set API keys

```bash
# Required for LLM planner and reporter
export ANTHROPIC_API_KEY="sk-ant-..."

# Optional: for AlphaGenome Atlas cross-reference
export ALPHA_GENOME_API_KEY="your-key"
```

### Verify installation

```bash
# Run module self-tests
python -m dvr_agent.state
python -m dvr_agent.planner
python -m dvr_agent.verifier
python -m dvr_agent.stopping
```

## Quick Start

### Single TF hypothesis investigation

```bash
# Investigate FOXA1 binding at rs6983267 (8q24 colorectal cancer locus)
python -m dvr_agent.run_argus_loop_new \
  --rsid rs6983267 --tf FOXA1 \
  --ref-prob 0.995 --alt-prob 0.995 \
  --label binding_retained \
  --chrom chr8 --pos 127401060 \
  --ref G --alt T \
  --assembly GRCh38 \
  --tools ADASTRA JASPAR ENCODE_cCRE \
  --output results/argus_foxa1.json
```

### Using the LLM planner (requires ANTHROPIC_API_KEY)

```bash
# Create a case file
echo '{"rsid":"rs6983267","tf":"KLF6","ref_prob":0.604,"alt_prob":0.915,
  "label":"binding_strengthened","chrom":"chr8","pos":127401060,
  "ref":"G","alt":"T","assembly":"GRCh38"}' > cases/klf6.json

# Run with the LLM planner
python -m dvr_agent.run_argus_planned \
  --case cases/klf6.json \
  --planner llm \
  --output results/klf6_llm.json
```

### Compare fixed vs. LLM planner

```bash
python -m dvr_agent.run_argus_planned --case cases/klf6.json --planner fixed --output results/klf6_fixed.json
python -m dvr_agent.run_argus_planned --case cases/klf6.json --planner llm   --output results/klf6_llm.json
```

## Architecture

ARGUS consists of five modules with strictly separated responsibilities:

| Module | Role | LLM involved? |
|--------|------|:--------------:|
| `state.py` | Hypothesis state, evidence ledger, trajectory | No |
| `planner.py` | Select next evidence test based on uncertainty | Fixed: No / LLM: Yes (constrained) |
| `verifier.py` | Interpret raw tool output as typed verdict | No |
| `stopping.py` | Assign terminal status when planner abstains | No |
| `reporter.py` | Generate narrative from pre-classified facts | Yes (constrained) |

### Evidence hierarchy

Evidence sources are typed and ranked. The verifier and stopping rule enforce these boundaries:

| Source | Type | Can establish |
|--------|------|--------------|
| ADASTRA | Experimental, direct | Supported, contradicted, rescued |
| JASPAR | Computational, TF-specific | Partial support or opposition |
| ENCODE cCRE | Contextual only | Cannot support any TF-specific claim |

Strong terminal decisions (`supported`, `contradicted`, `rescued`) require admissible direct experimental evidence. cCRE context alone cannot promote a hypothesis. Mixed indirect evidence leads to abstention.

### Investigation loop

```
DVR prediction → HypothesisState (ACTIVE)
    ↓
Planner → selects evidence tool
    ↓
Tool → returns raw observation
    ↓
Verifier → deterministic verdict + status update
    ↓
Planner → reads updated state, selects next tool or abstains
    ↺ (loop until terminal)
    ↓
Stopping rule → final status if planner abstains
    ↓
Reporter → constrained narrative from fixed facts
```

## Results

Evaluated on 2 variants across 2 disease contexts using real evidence from local ADASTRA, JASPAR, and ENCODE cCRE data. All observations are from actual database queries; no results are simulated.

### rs6983267 (8q24, colorectal cancer)

| TF | DVR prediction | Steps | Tools used | Final status | Key finding |
|----|---------------|:-----:|-----------|:------------:|-------------|
| FOXA1 | retained [saturated] | 3 | ADASTRA | **rescued** | ASB confirmed (FDR=0.030, 15 exp., 475 reads) |
| KLF6 | strengthened | 8 | ADASTRA → JASPAR → cCRE | **abstained** | JASPAR contradicts DVR (Δ=-0.147); mixed evidence |
| RAD21 | no binding | 3 | ADASTRA | **contradicted** | Real ASB found (FDR=0.015); model failure |
| SP1 | no binding | 4 | ADASTRA | **abstained** | Classifier prevented hallucination (log-odds=3.02) |

### rs2981578 (FGFR2, breast cancer)

| TF | DVR prediction | Steps | Tools used | Final status | Key finding |
|----|---------------|:-----:|-----------|:------------:|-------------|
| BACH1 | LOF (0.996→0.011) | 8 | ADASTRA → JASPAR → cCRE | **partially supported** | JASPAR concordant with LOF direction |
| FOXA1 | retained [saturated] | 8 | ADASTRA → JASPAR → cCRE | **abstained** | No ADASTRA rescue possible (unlike rs6983267) |
| RBBP5 | weakened | 8 | ADASTRA → JASPAR → cCRE | **abstained** | Context only; no TF-specific evidence |

### AlphaGenome Atlas cross-reference

AlphaGenome Atlas (released September 8, 2026) independently assigns rs6983267 an AVI score of 0.570, confirming high regulatory impact. TF-specific CHIP_TF predictions show the largest change for FOXA1 (|Δ|=0.463), consistent with the ADASTRA rescue.

## Project structure

```
ARGUS/
├── dvr_agent/
│   ├── state.py              # Hypothesis state and trajectory
│   ├── planner.py             # Fixed evidence priority policy
│   ├── planner_llm.py         # LLM-mediated planner (Claude)
│   ├── verifier.py            # Deterministic evidence interpreter
│   ├── stopping.py            # Terminal status assignment
│   ├── run_argus_loop_new.py  # Closed-loop runner (3 tools)
│   ├── run_argus_planned.py   # Runner with planner selection
│   ├── reporter.py            # Constrained LLM narrative generator
│   ├── falsifier.py           # ADASTRA evidence source
│   ├── motif.py               # JASPAR motif scoring
│   ├── encode_ccres.py        # ENCODE cCRE lookup
│   ├── classify.py            # Deterministic binding classifier
│   ├── graph.py               # LangGraph pipeline (DVR stage)
│   └── alphagenome_adapter.py # AlphaGenome Atlas adapter
├── docs/
│   ├── architecture.md
│   └── data_setup.md
├── results/
│   └── README.md
├── environment.yml
├── requirements.txt
└── CITATION.cff
```

<!-- Temporarily hidden from the rendered README; retained for future updates.
## Current limitations

- DVR prediction and investigation are not yet fully integrated from one natural-language prompt
- The fixed planner uses a priority list, not a learned policy
- Evaluated on 2 variants / 7 hypotheses (demonstrates behavior, not predictive accuracy)
- ENCODE cCRE uses locally indexed BED files (SCREEN API unreachable from compute environment)
- AlphaGenome Atlas is a standalone cross-reference, not integrated into the loop
-->

<!-- Citation pending final publication details.
## Citation

```bibtex
@inproceedings{dutta2026argus,
  title={ARGUS: Evidence-Constrained Agentic Reasoning for Noncoding Regulatory Variant Interpretation},
  author={Dutta, Pratik and Davuluri, Ramana V.},
  booktitle={NeurIPS 2026 Workshop on Agentic AI for Biological Discovery (AgenticLS)},
  year={2026},
  url={https://github.com/duttaprat/ARGUS}
}
```
-->

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

DVR model weights are available under CC-BY-NC-4.0 at [HuggingFace](https://huggingface.co/duttaprat/DeepVRegulome). External data resources (ADASTRA, JASPAR, ENCODE) have their own access and redistribution terms.

## Acknowledgments

- [DeepVRegulome](https://github.com/DavuluriLab/DeepVRegulome) for the 458 DNABERT-based TF binding models
- [ADASTRA](https://adastra.autosome.org/) for allele-specific TF binding data
- [JASPAR](https://jaspar.elixir.no/) for TF binding profiles
- [ENCODE](https://www.encodeproject.org/) for candidate cis-regulatory elements
- [AlphaGenome Atlas](https://alphagenome.deepmind.com/) for independent variant impact predictions
- [Anthropic](https://www.anthropic.com/) for Claude API access through the Anthropic for Science program
