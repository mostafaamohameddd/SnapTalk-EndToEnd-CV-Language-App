# SnapTalk: End-to-End Multimodal AI Language Learning Platform

SnapTalk is a production-ready, interactive pipeline that transforms real-world environments into language-learning experiences. It leverages state-of-the-art Computer Vision (YOLOv8, MobileSAM) and Vision-Language Models (Qwen2-VL/GPT-4o) to automate flashcard generation and provide phoneme-level pronunciation feedback



## Technical Architecture (Modular AI Pipeline)

### The project is built on a modular service architecture, ensuring scalability and high maintainability. Each stage of the pipeline is a dedicated service:

1. Detection & Recognition: Class-agnostic object detection using YOLOv8m followed by semantic labeling via configurable VLM Providers (Qwen2-VL-2B or GPT-4o).

2. Segmentation: Precise object isolation using MobileSAM to generate transparent crops and artifact overlays.

3. Linguistic Enrichment: Automated flashcard generation including translations, IPA phonetics, and example sentences through a robust fallback chain (DeepL, Google Cloud, Ollama).

4. Speech Synthesis: High-fidelity audio generation via Edge-TTS.

5. Pronunciation Lab: A hybrid scoring system using Whisper and Wav2Vec2 for detailed, per-phoneme alignment and evaluation.



## Model & Engine Inventory

### Task & Engine / Model 

Object Detection   YOLOv8m (yolov8m.pt)

Segmentation   MobileSAM (mobile_sam.pt)

Vision-Language   Qwen2-VL-2B-Instruct / GPT-4o

Translation   DeepL / Google Cloud / Ollama Fallback

Audio Synthesis   Edge-TTS

Pronunciation   OpenAI Whisper & facebook/wav2vec2-base-960h



## Key Features & Performance

Modular Entry Points: Supports both a FastAPI REST server for production and an Interactive CLI for local experimentation.

Production Safeguards: Implements network security utilities, byte-limit enforcement, and content-type validation.

Translation Memory: Integrated SQLite cache to optimize latency and cost for recurring translations.

Runtime Verification: Automated auditing of deployment topology, latency benchmarks, and credential presence.



## Project Structure

snaptalk/
├── app/                        # Core FastAPI application
│   ├── main.py                 # Application entry point & route wiring
│   ├── core/                   # Configuration & global settings
│   ├── routers/                # Dedicated API endpoints (VLM, TTS, etc.)
│   ├── services/               # Modular AI logic
│   │   ├── detection/          # YOLOv8 implementation
│   │   ├── segmentation/       # MobileSAM services
│   │   ├── recognition/        # VLM provider orchestration
│   │   ├── translation/        # Multi-engine translation logic
│   │   ├── tts/                # Edge-TTS synthesis
│   │   └── pronunciation/      # Whisper & Wav2Vec2 scoring
│   └── schemas/                # Pydantic models for API validation
├── scripts/                    # CLI Tools (Snap & Learn flow, Pronunciation Lab)
├── docs/                       # Technical documentation & Runtime evidence
├── tests/                      # Comprehensive API and Flow testing
├── data/                       # Seed translations & artifact storage
└── requirements.txt            # Project dependencies


## Quick Start

### Prerequisites
Python 3.9+

Model weights (yolov8m.pt, mobile_sam.pt) in root

### Installation

git clone https://github.com/your-username/snaptalk.git
cd snaptalk
python -m venv .venv
source .venv/bin/activate  # Or .venv\Scripts\Activate.ps1 for Windows
pip install -r requirements.txt

### Execution
Run CLI Flow: python scripts/snap_learn.py --image "path/to/image.jpg"

Run API Server: uvicorn app.main:app --host 127.0.0.1 --port 8000



## Testing & Reliability
We maintain high reliability through rigorous testing:

Integration Tests: Validating the entire flow from image upload to pronunciation scoring.

Security Checks: Ensuring zero exposure of legacy endpoints and validating remote fetch safety.

Performance: Benchmarked health and translation baselines documented in docs/RUNTIME_VERIFICATION.md
