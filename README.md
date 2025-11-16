# Emergency Triage Agent

**Track:** Agents for Good  
**Author:** Malik Kolawole Lanlokun  
**Course:** 5 Day AI Agents Intensive with Google – Capstone Project

---

## Overview

The Emergency Triage Agent is a multi-agent conversational system that helps users describe symptoms clearly and receive structured, non-diagnostic guidance. It turns free text into a simple symptom profile, applies rule-based risk scoring, optionally finds nearby health facilities, and explains the result in friendly language.

It does not replace clinicians. It is a learning project that demonstrates safe agent design, tool use, and multi-step reasoning for public health awareness and wellness support.

---

## Problem Statement

People often struggle to understand if a symptom is urgent or not. They search online or ask general chatbots that are not designed for triage. This can cause:

- Delays in seeking urgent care.
- Unnecessary visits to clinics and emergency rooms.
- Anxiety and confusion during stressful situations.

There is a need for a structured assistant that:

- Helps users describe their symptoms in a consistent way.
- Applies simple and transparent rules.
- Clearly communicates if the situation looks non-urgent, urgent, or emergency level.
- Avoids making diagnoses.

---

## Solution Summary

The Emergency Triage Agent uses a **sequential multi-agent pipeline** called `EmergencyTriagePipeline`. Each agent handles one part of the triage flow:

1. Collect structured symptoms.
2. Score risk with a custom tool.
3. Optionally find nearby hospitals using OpenStreetMap.
4. Decide escalation and support human approval for emergencies.
5. Explain everything back to the user in clear language.

This fits the Agents for Good track because it supports safer decisions and reduces confusion in moments of uncertainty. It also shows how agents can break a complex task into reliable, testable steps.

---

## Architecture

The system uses `SequentialAgent` orchestration and shared state. Agents read and write keys like `symptom_profile`, `risk_summary`, `location_data`, and `escalation_result` in session state.

### 1. Symptom Intake Agent

- Type: `LlmAgent`
- Role: Triage intake nurse.
- Input: Free text from the user.
- Output key: `symptom_profile`

It converts user text into JSON with this schema:

```json
{
  "main_symptom": "...",
  "onset": "...",
  "severity": "mild|moderate|severe",
  "location": "...",
  "risk_factors": [],
  "additional_notes": ""
}
