#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Wed Jul 23 22:51:27 2025

@ Author: karimelbaroudi

Minimal Simulation Agent
"""

import random
import json
import argparse
import textwrap

# --- version.py ---
VERSION = "1.0.0"

# --- prompt_builder.py ---
def build_prompt(scenario, goal, constraint):
    return f"""
You are a strategic property agent simulator.

Scenario: {scenario}
Goal: {goal}
Constraint: {constraint}

Please diagnose the situation, suggest 2–3 strategic actions to improve the outcome, and rate the simulation's likelihood of success (0–1).
Return the answer as JSON with keys: diagnosis, strategic_actions, simulation_score.
"""

# --- simulator.py ---
def simulate_response(prompt):
    # In a real implementation, this would call OpenAI API with the prompt.
    # Here we simulate a response:
    return {
        "diagnosis": "The current guide price exceeds buyer tolerance based on local liquidity compression.",
        "strategic_actions": [
            "Reposition guide to £4.25M",
            "Switch to performance-led agent within 14 days",
            "Reframe marketing with withdrawn comp narrative"
        ],
        "simulation_score": round(random.uniform(0.7, 0.75), 2)
    }

# --- formatter.py ---
def format_output(response_dict):
    return json.dumps(response_dict, indent=2)

# --- scorer.py ---
def score_simulation(response):
    return response.get("simulation_score", 0.5)

# --- memo_report.py ---
def generate_memo(input_data, simulation):
    diagnosis = simulation.get("diagnosis", "No diagnosis provided.")
    actions = simulation.get("strategic_actions", [])
    score = simulation.get("simulation_score", 0)

    memo = f"""
    Simulation Report Memo
    ======================

    Scenario:
    {input_data['scenario']}

    Goal:
    {input_data['goal']}

    Constraint:
    {input_data['constraint']}

    Diagnosis:
    {diagnosis}

    Strategic Actions:
    """
    for i, action in enumerate(actions, 1):
        memo += f"    {i}. {action}\n"

    memo += f"""

    Forecast:
    The likelihood of securing an offer within the specified constraints is estimated at {score*100:.0f}%.
    This forecast considers local market conditions and buyer sensitivity to price.

    Commentary:
    The seller's resistance to price reduction may limit buyer engagement.
    Adopting a performance-led agent and reframing marketing messaging could shift buyer perception and improve offer probability.
    """

    # Clean indentation for display
    memo = textwrap.dedent(memo).strip()
    return memo

# --- main.py ---
def run_simulation(input_data):
    prompt = build_prompt(input_data["scenario"], input_data["goal"], input_data["constraint"])
    simulation = simulate_response(prompt)
    output_json = format_output(simulation)
    return simulation, output_json

def main():
    parser = argparse.ArgumentParser(description="Minimal Simulation Agent")
    parser.add_argument("--scenario", type=str, default="£5.25M Knightsbridge townhouse unsold for 9 months",
                        help="Property scenario description")
    parser.add_argument("--goal", type=str, default="Secure offer within 60 days",
                        help="Goal for the simulation")
    parser.add_argument("--constraint", type=str, default="Do not reduce below £4.2M",
                        help="Constraints to consider in the simulation")

    args = parser.parse_args()

    input_data = {
        "scenario": args.scenario,
        "goal": args.goal,
        "constraint": args.constraint
    }

    print(f"Simulation Agent v{VERSION}\n")

    simulation, output_json = run_simulation(input_data)

    print("Structured JSON Output:\n")
    print(output_json)

    print("\n" + "="*60 + "\n")

    memo = generate_memo(input_data, simulation)
    print(memo)

if __name__ == "__main__":
    main()
