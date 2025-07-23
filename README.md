# simulation-agent
Minimal property simulation engine using GPT-style output
### main.py
from prompt_builder import build_prompt
from simulator import simulate_response
from formatter import format_output
from version import VERSION

def run_simulation(input_data):
    prompt = build_prompt(input_data["scenario"], input_data["goal"], input_data["constraint"])
    response = simulate_response(prompt)
    return format_output(response)

if __name__ == "__main__":
    input_data = {
        "scenario": "£5.25M Knightsbridge townhouse unsold for 9 months",
        "goal": "Secure offer within 60 days",
        "constraint": "Do not reduce below £4.2M"
    }

    print(f"Simulation Agent v{VERSION}")
    print(run_simulation(input_data))


### prompt_builder.py
def build_prompt(scenario, goal, constraint):
    return f"""
You are a strategic property agent simulator.

Scenario: {scenario}
Goal: {goal}
Constraint: {constraint}

Please diagnose the situation, suggest 2–3 strategic actions to improve the outcome, and rate the simulation's likelihood of success (0–1).
Return the answer as JSON with keys: diagnosis, strategic_actions, simulation_score.
"""


### simulator.py
import random

def simulate_response(prompt):
    return {
        "diagnosis": "The current guide price exceeds buyer tolerance based on local liquidity compression.",
        "strategic_actions": [
            "Reposition guide to £4.25M",
            "Switch to performance-led agent within 14 days",
            "Reframe marketing with withdrawn comp narrative"
        ],
        "simulation_score": round(random.uniform(0.7, 0.75), 2)
    }


### formatter.py
import json

def format_output(response_dict):
    return json.dumps(response_dict, indent=2)


### scorer.py
def score_simulation(response):
    return response.get("simulation_score", 0.5)


### version.py
VERSION = "1.0.0"


### README.md
# 🧐 Simulation Agent

This is a minimal simulation engine that processes structured real estate scenarios and generates GPT-style strategic output. Useful for quick diagnostics, seller strategy refinement, and offer probability forecasting.

## 📥 Input Format

```python
input_data = {
  "scenario": "£5.25M Knightsbridge townhouse unsold for 9 months",
  "goal": "Secure offer within 60 days",
  "constraint": "Do not reduce below £4.2M"
}
```

## 📟 Output Example

```json
{
  "diagnosis": "The current guide price exceeds buyer tolerance based on local liquidity compression.",
  "strategic_actions": [
    "Reposition guide to £4.25M",
    "Switch to performance-led agent within 14 days",
    "Reframe marketing with withdrawn comp narrative"
  ],
  "simulation_score": 0.72
}
```

## 🚀 Run It

```bash
python main.py
```

## 📁 Folder Structure

```
simulation_agent/
├── main.py
├── prompt_builder.py
├── simulator.py
├── formatter.py
├── scorer.py
├── version.py
└── README.md
```

## 🔄 Future Ideas

- OpenAI API Integration
- Streamlit front-end
- CSV batch processor
