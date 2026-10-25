# Open4DP 🚴‍♂️🤖

**Open4DP** is an open-source, subscription-free, AI-driven adaptive training engine for cyclists. It bridges the gap between static workout plans (such as Zwift templates, TrainerDay, or raw `.zwo` files) and dynamic daily readiness tracking (similar to JOIN.cc or TrainerRoad Adaptive Training)—while scaling workouts using **Four-Dimensional Power (4DP)** metrics instead of traditional FTP-only percentages.

---

## 🎯 Core Features

- **4DP Multi-Ceiling Scaling**: Scales workout targets independently across **Neuromuscular (NM)**, **Anaerobic Capacity (AC)**, **Maximal Aerobic Power (MAP)**, and **Functional Threshold Power (FTP)**.
- **Daily Readiness Adaptation**: Ingests daily wellness data (HRV, Sleep, RHR, Fatigue) to modify planned workouts dynamically.
- **Provider-Agnostic AI Engine**: Supports local LLMs via **Ollama** (e.g., `llama3.2`, `qwen2.5`) for 100% offline execution, or cloud providers (Google Gemini API, OpenAI).
- **Direct Head Unit Sync**: Pushes dynamically generated workouts to **Intervals.icu**, which automatically syncs with **Wahoo ELEMNT**, **Garmin Edge**, or indoor software like **MyWhoosh** and **GoldenCheetah**.
- **100% Free & Open Source**: Zero subscription fees or proprietary lock-ins.

---

## 🏗️ System Architecture

```
┌───────────────────────────────┐
│  Intervals.icu / Health API   │  (Fetches 7-day HRV, Sleep, RHR, eFTP)
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│     AI Coach Layer (Local)    │  (Ollama / Gemini API evaluates readiness
└───────────────┬───────────────┘   and outputs daily intensity ceiling)
                │
                ▼
┌───────────────────────────────┐
│   Deterministic 4DP Engine    │  (Parses workout template & calculates
└───────────────┬───────────────┘   watts against personal 4DP array)
                │
                ▼
┌───────────────────────────────┐
│    Intervals.icu Calendar     │ ──► Pushes .zwo / structured text to
└───────────────┴───────────────┘     Wahoo ELEMNT / Garmin / MyWhoosh
```

---

## 📐 How 4DP Scaling Works

Standard FTP scaling assumes fixed population ratios (e.g., $VO_2\text{ max} = 120\%\text{ FTP}$). **Open4DP** decouples intensity zones to hit your physiological limits without under- or over-training:

| Target Zone | Duration | Primary Metric | Target Calculation |
| :--- | :--- | :--- | :--- |
| **FTP** (Tempo/Threshold) | $\ge 5\text{ min}$ | **FTP** | $\text{Target} = \% \text{FTP} \times \text{Personal FTP}$ |
| **MAP** ($VO_2\text{ max}$) | $2\text{ to } 5\text{ min}$ | **MAP** | $\text{Target} = \frac{\% \text{FTP}}{1.15} \times \text{Personal MAP}$ |
| **AC** (Anaerobic) | $15\text{s to } 2\text{ min}$ | **AC** | $\text{Target} = \frac{\% \text{FTP}}{1.40} \times \text{Personal AC}$ |
| **NM** (Sprint) | $< 15\text{s}$ | **NM** | $\text{Target} = \frac{\% \text{FTP}}{3.00} \times \text{Personal NM}$ |

---

## 🚀 Quickstart

### 1. Requirements

- Python 3.10+
- [Ollama](https://ollama.com/) (if using local AI execution)
- A free [Intervals.icu](https://intervals.icu) account

### 2. Configuration (`config.json`)

Create a local configuration file:

```json
{
  "athlete": {
    "ftp": 250,
    "map": 320,
    "ac": 400,
    "nm": 800
  },
  "ai": {
    "provider": "OLLAMA",
    "model": "llama3.2"
  },
  "intervals_icu": {
    "athlete_id": "iXXXXX",
    "api_key": "YOUR_API_KEY"
  }
}
```

### 3. Execution

```bash
# Clone the repository
git clone https://github.com/yourusername/Open4DP.git
cd Open4DP

# Install dependencies
pip install -r requirements.txt

# Run daily sync script
python main.py
```

---

## 🗺️ Roadmap

- [x] Core 4DP transformation logic for standard FTP percentage workouts.
- [x] Integration with local Ollama runtime and JSON response parsing.
- [x] Syncing rescaled events via Intervals.icu REST API.
- [ ] Auto-updating 4DP profile metrics from rolling 90-day peak power curves.
- [ ] Direct `.zwo` / `.fit` file parser for local workout directory execution.

---

## 📝 License

Distributed under the GPLv3.0 License. See `LICENSE` for details.