---
name: skill-graphify-query
trigger: automatic — runs before ANY code suggestion or modification
description: "Before suggesting any code change, queries the Graphify graph to understand the blast radius. Never suggest without checking."
---

# Skill: Graphify Query

## When this runs
This skill runs AUTOMATICALLY before:
- Any suggestion to modify an existing file
- Any new endpoint that relates to existing entities
- Any refactor proposal
- Any "what breaks if I change X?" question

## Steps
1. Identify the target node (class, function, or file in question)
2. Query Graphify: `graphify query --node [target] --depth 2`
3. Read the output: direct dependents + transitive dependents
4. Report the blast radius BEFORE suggesting anything:
   "Modifying [X] affects: [list]. Proceed?"
5. Only after blast radius is understood: suggest the change

## If Graphify is not installed
Say: "Graphify is not installed in this repo.
     Run: `graphify claude install && graphify`
     from inside Tila_BackEnd or Tila_Frontend.
     Until then, I will read files directly but with reduced precision."

## Rule
NEVER suggest a change to an existing class, endpoint, or component
without first checking what depends on it. In a medical application,
a broken dependency is not a bug — it is a patient safety risk.
