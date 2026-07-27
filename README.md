# ClimateOps-Risk-Monitor
# ClimateOps Risk Monitor

Automated climate hazard and supply chain disruption monitoring pipeline, built solo in **n8n**. Monitors a 15-supplier portfolio daily and flags which suppliers face elevated climate/disaster risk — before a shipment is late, not after.

🎥 **[Watch the 5-minute walkthrough](https://www.loom.com/share/4b4a3e868b29460abe2fa61166b28e75)**

![Architecture diagram](ClimateOps_Architecture_Diagram.svg)

---

## The Problem

Most supply chain teams find out a supplier's operations were disrupted by a flood, storm, or heatwave *after* a shipment is already delayed. This pipeline closes that gap: it checks live disaster and weather data against each supplier's exact location every day, and routes findings by urgency automatically.

## What It Does

Every day, on a schedule, the pipeline:

1. Pulls live disaster alerts from **GDACS** (earthquakes, floods, storms, wildfires) and 30-day weather forecasts from **Open-Meteo**, matched to each supplier's coordinates
2. Scores each supplier using a **catastrophe-modeling framework**: `risk = hazard × vulnerability`
3. Classifies physical climate risk using **TCFD** categories (acute vs. chronic) — the standard used in climate financial disclosure
4. Routes suppliers by risk level:
   - **High** → immediate email alert with recommended next steps
   - **Medium** → bundled into a digest to avoid alert fatigue
   - **Low** → logged only
5. Archives every supplier, every run, for trend analysis over time

No manual input required once scheduled.

## Tech Stack

| Layer | Tool |
|---|---|
| Orchestration | n8n (Cloud) |
| Data store | Google Sheets |
| Alerts | Gmail API |
| Scoring logic | JavaScript (n8n Code nodes) |
| Disaster data | GDACS API |
| Weather data | Open-Meteo API |

## Scoring Methodology

```
risk_score = (temp_severity + rain_severity + wind_severity + gdacs_severity) × vulnerability
vulnerability = criticality_multiplier × lead_time_factor
```

Full formula detail and thresholds are documented in [`ClimateOps_Rebuild_Guide.md`](ClimateOps_Rebuild_Guide.md).

## Skills Demonstrated

- Workflow automation & orchestration (n8n)
- API integration (REST, no-auth public APIs)
- Applied climate risk methodology (catastrophe modeling, TCFD classification)
- JavaScript for data transformation and scoring logic
- Technical documentation and architecture design
- End-to-end ownership: problem framing → build → testing → presentation

## Repo Contents

- `ClimateOps_Risk_Monitor.json` — importable n8n workflow
- `ClimateOps_Rebuild_Guide.md` — full node-by-node build reference
- `ClimateOps_Architecture_Diagram.svg` — pipeline architecture visual
- `README.md` — this file

## Status & Next Steps

Pipeline is fully built and tested end-to-end. Planned next: calibrate HIGH/MEDIUM/LOW thresholds against several weeks of real data, and explore NGFS climate scenario tagging for high-risk suppliers.

---

*Built as a self-paced upskilling project. Questions or feedback welcome — feel free to open an issue.*
