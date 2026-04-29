<p align="center">
  <h1 align="center">🌾 AgriIR</h1>
  <p align="center">
    <strong>Configurable RAG Framework for Domain-Specific Knowledge Retrieval</strong>
  </p>
  <p align="center">
    <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/python-3.10+-3776ab.svg" alt="Python"></a>
    <a href="https://ollama.ai"><img src="https://img.shields.io/badge/LLM-Ollama-white.svg" alt="Ollama"></a>
    <a href="https://github.com/facebookresearch/faiss"><img src="https://img.shields.io/badge/vector--db-FAISS-orange.svg" alt="FAISS"></a>
    <a href="https://huggingface.co/datasets/ShuvBan/AgriIR_dataset"><img src="https://img.shields.io/badge/Dataset-HuggingFace-yellow?style=flat" alt="HuggingFace Dataset"></a>
    <a href="https://doi.org/10.1007/978-3-032-21324-2_37"><img src="https://img.shields.io/badge/DOI-Springer-green?style=flat" alt="DOI"></a>
    <a href="https://arxiv.org/abs/2604.16353"><img src="https://img.shields.io/badge/arXiv-2604.16353-red?style=flat" alt="arXiv"></a>
  </p>
  <p align="center">
    <a href="https://ecir2026.eu/calls/call-for-ir-for-good-papers"><img src="https://img.shields.io/badge/ECIR%202026-IR%20for%20Good%20✓-brightgreen.svg" alt="ECIR 2026"></a>
    <a href="https://bit.ly/AgriIR"><img src="https://img.shields.io/badge/Demo-Video-red.svg" alt="Demo"></a>
  </p>
</p>

<br/>

> **🏅 Accepted at [ECIR'26 IR for Good Track](https://ecir2026.eu/calls/call-for-ir-for-good-papers)** · [Watch Demo](https://bit.ly/AgriIR)

<br/>

## What is AgriIR?

AgriIR is a 6-stage retrieval-augmented generation pipeline for agricultural Q&A. It combines local vector search (FAISS) with web retrieval, applies domain-specific agents, and inserts deterministic citations into responses.

<br/>

## Pipeline

```mermaid
graph LR
    %% Global Styles
    classDef stage fill:#90caf9,stroke:#01579b,stroke-width:2px,color:#000,font-weight:bold;
    classDef config fill:#b2ebf2,stroke:#006064,stroke-width:1px,color:#000;
    classDef storage fill:#fff3e0,stroke:#e65100,stroke-width:2px,stroke-dasharray: 5 5,color:#000,font-weight:bold;
    classDef external fill:#ef9a9a,stroke:#b71c1c,stroke-width:1px,color:#000;
    classDef governance fill:#d1c4e9,stroke:#4527a0,stroke-width:1px,color:#000;
    classDef terminal fill:#e3f2fd,stroke:#01579b,stroke-width:2px,color:#000;

    %% Entry and Exit
    Start([User Query]) --> S1
    S6 --> End([Cited Answer])

    %% Main Pipeline Stages
    subgraph Pipeline [Core RAG Pipeline]
        S1[Stage-1<br/>Query Refine<br/>T=0.1]
        S2[Stage-2<br/>Sub-Query<br/>T=0.5]
        S3[Stage-3<br/>Parallel Retrieval]
        S4[Stage-4<br/>Agent Enhance]
        S5[Stage-5<br/>Synthesis<br/>T=0.2]
        S6[Stage-6<br/>Citation Insert]

        S1 --> S2
        S2 --> S3
        S3 --> S4
        S4 --> S5
        S5 --> S6
    end

    %% External Logic & Retrieval
    subgraph Retrieval_Layer [Knowledge Retrieval]
        AE[Adaptive Embedding] -.-> VDB
        VDB[(Vector DB<br/>FAISS<br/>C2)]
        
        LLMF[LLM Filtering] -.-> WS
        WS[(Web Search<br/>Duck Duck Go<br/>C2)]
    end

    %% Configuration & Governance
    subgraph Control_Plane [System Configuration]
        TP[Temperature<br/>Profiles<br/>C1]
        MR[Model<br/>Registry<br/>C1]
        AC[Agent<br/>Catalogue<br/>C3]
        OBS[Observability<br/>C4]
        DEP[Deployment<br/>C4]
    end

    %% Connections
    VDB & WS --> S3
    
    %% Config Connections
    TP -.->|Config| S1 & S2 & S6
    MR -.->|Config| S1 & S2 & S6
    AC -.->|Config| S4
    OBS -.->|Monitor| S5
    DEP -.->|Govern| S6

    %% Applying Classes
    class S1,S2,S3,S4,S5,S6 stage;
    class TP,MR config;
    class VDB,WS storage;
    class AE,LLMF external;
    class AC,OBS,DEP governance;
    class Start,End terminal;
```




| Stage | Operation | Details |
|:-----:|:----------|:--------|
| 1 | Query Refinement | Remove ambiguity (Gemma3:1B) |
| 2 | Decomposition | Split into 3-5 sub-queries |
| 3 | Retrieval | FAISS + DuckDuckGo in parallel |
| 4 | Enhancement | Domain agent keyword injection |
| 5 | Synthesis | Generate answer (configurable model) |
| 6 | Citation | Insert sources where similarity ≥ 0.75 |

<br/>

## Dataset

The AgriIR dataset containing 15,247 agricultural knowledge entries is available on HuggingFace:

[![HuggingFace](https://img.shields.io/badge/Dataset-HuggingFace-yellow?style=flat-square)](https://huggingface.co/datasets/ShuvBan/AgriIR_dataset)

- **Direct Link**: https://huggingface.co/datasets/ShuvBan/AgriIR_dataset
- **Format**: JSONL (JSON Lines)
- **Size**: ~1.9GB (compressed ~678MB)
- **Entries**: 15,247 curated agricultural knowledge entries

## Citation

If you use AgriIR, please cite our paper. The Springer/ECMIR version is the official peer-reviewed publication.

### Springer/Conference Citation (Recommended for academic publications)

```bibtex
@InProceedings{10.1007/978-3-032-21324-2_37,
  author="Banerji Seal, Shuvam and Poddar, Aheli and Mishra, Alok and Roy, Dwaipayan",
  editor="Campos, Ricardo and Jatowt, Adam and Lan, Yanyan and Aliannejadi, Mohammad and Bauer, Christine and MacAvaney, Sean and Anand, Avishek and Ren, Zhaochun and Verberne, Suzan and Bai, Nan and Mansoury, Masoud",
  title="AgriIR: A Scalable Framework for Domain-Specific Knowledge Retrieval",
  booktitle="Advances in Information Retrieval",
  year="2026",
  publisher="Springer Nature Switzerland",
  address="Cham",
  pages="489--504",
  doi={10.1007/978-3-032-21324-2_37},
  isbn="978-3-032-21324-2"
}
```

### arXiv Citation

```bibtex
@misc{BanerjiSeal2026AgriIR,
  title={AgriIR: A Scalable Framework for Domain-Specific Knowledge Retrieval},
  author={Shuvam Banerji Seal and Aheli Poddar and Alok Mishra and Dwaipayan Roy},
  year={2026},
  eprint={2604.16353},
  archivePrefix={arXiv},
  primaryClass={cs.IR},
  url={https://arxiv.org/abs/2604.16353}
}
```

<br/>

## Quick Start

```bash
git clone https://github.com/Shuvam-Banerji-Seal/AgriIR.git && cd AgriIR
./install_agriir.sh
./start_agriir.sh
# Open http://localhost:5000
```

<br/>

## Database Architecture

<div id="header" align="center">
  <img align="center" width=800px height=400px alt="side_sticker" src="https://github.com/XAheli/AgriIR/blob/main/Images/database_pipeline.png" />

</div>

## Installation

### Requirements

| | Minimum | Recommended |
|:--|:-------:|:-----------:|
| **Python** | 3.10 | 3.11+ |
| **RAM** | 8 GB | 32 GB |
| **GPU** | — | 8GB+ VRAM |


### Options

<details>
<summary><strong>Standard Installation</strong></summary>

```bash
./install_agriir.sh
```
</details>

<details>
<summary><strong>With Pre-built Embeddings (~40GB)</strong></summary>

```bash
./install_agriir.sh --download-embeddings
```
</details>

<details>
<summary><strong>Web Search Only (No Local DB)</strong></summary>

```bash
./install_agriir.sh --no-rag
```
</details>

<details>
<summary><strong>Manual Setup</strong></summary>

```bash
python3 -m venv agriir_env && source agriir_env/bin/activate
pip install -r requirements.txt
curl -fsSL https://ollama.ai/install.sh | sh
ollama pull gemma3:1b && ollama pull llama3.2:3b
```
</details>

### Models

| Model | VRAM | Role |
|:------|:----:|:-----|
| `gemma3:1b` | 1.6 GB | Query refinement |
| `llama3.2:3b` | 2 GB | Default synthesis |
| `gemma3:27b` | 16 GB | High-quality synthesis |
| `Qwen3-Embedding-8B` | 5 GB | Embeddings |

<br/>

## Usage

### Server Modes

| Command | Mode |
|:--------|:-----|
| `./start_agriir.sh` | Auto-detect |
| `python3 agri_bot_searcher/src/enhanced_web_ui.py` | Full RAG |
| `python3 agri_bot_searcher/src/enhanced_voice_web_ui.py` | Voice enabled |
| `python3 agri_bot_searcher/src/fallback_web_ui.py` | Web search only |

### API

```python
import requests

r = requests.post('http://localhost:5000/api/query',
                  json={'query': 'Wheat cultivation in Punjab'})
print(r.json())
```

| Endpoint | Method | Description |
|:---------|:------:|:------------|
| `/api/query` | POST | Submit query |
| `/api/status` | GET | System status |
| `/api/health` | GET | Health check |

<br/>


## Configuration

Files in `agri_bot_searcher/`:

| File | Purpose |
|:-----|:--------|
| `config.yml` | Main settings |
| `config/enhanced_rag_config.yaml` | RAG parameters |
| `.env.example` | Environment template |

<details>
<summary><strong>Example config.yml</strong></summary>

```yaml
ollama:
  base_port: 11434
  model: gemma3:1b

agents:
  max_agents: 6
  roles:
    - crop_specialist
    - disease_expert
    - economics_analyst
    - climate_researcher

search:
  agriculture_domains:
    - extension.org
    - fao.org
    - usda.gov
```
</details>

<br/>


## Contributing

We welcome contributions! Here's how to get started

1. **Fork** the repository
2. **Create a branch:** `git checkout -b feature/your-feature`
3. **Commit changes:** `git commit -am 'Add your feature'`
4. **Push to branch:** `git push origin feature/your-feature`
5. **Open a Pull Request**

> 💡 Please open an issue first to discuss major changes before submitting a PR.

<br/>
