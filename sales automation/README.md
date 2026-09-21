# AI Sales Army

Four agents, one Apollo CSV, one master sheet. Streamlit is the workshop UI. The file pipeline in `sales-agent/` is the source of truth.

Order: **Scout → Research → Strategy → Drafting → master_output.csv**

Nothing is emailed or sent on WhatsApp.

## Run it

1. Copy `.env.example` to `.env` and paste a Groq API key.
2. Install and start the UI:

```
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

3. Open http://localhost:8501

CLI, same pipeline:

```
python -m pipeline.run --step preflight
python -m pipeline.run --step scout
python -m pipeline.run --all
```

## What each agent writes

| Agent | New fields only |
|---|---|
| Scout | dated company and person signals, sources, `scout_confidence` |
| Research | pain point, evidence, opportunity area, technology maturity |
| Strategy | matched service, sales angle, channel, opener type |
| Drafting | email always; LinkedIn or WhatsApp only for the selected channel |

Channel rule: executive titles → LinkedIn, sales/marketing → email, **10,000+ employees → email**, otherwise WhatsApp. `Qualify Contact = No` still runs and is stored as `apollo_qualify_flag`.

## Layout

```
sales-agent/
  IMPLEMENTATION_PLAN.md
  inputs/apollo_leads.csv
  inputs/company_info.md
  agents/01_scout/instructions.md
  agents/02_research/instructions.md
  agents/03_strategy/instructions.md
  agents/04_drafting/instructions.md
  outputs/
pipeline/          Python runner used by Streamlit and the CLI
app.py             workshop UI
```

Walkthrough: [BUILD_GUIDE.md](BUILD_GUIDE.md)

Technical write-up of this repo: [docs/TECHNICAL.md](docs/TECHNICAL.md)

Original pipeline specification (PDF): [docs/AI-Sales-Agent-Pipeline-Technical-Guide.pdf](docs/AI-Sales-Agent-Pipeline-Technical-Guide.pdf)
