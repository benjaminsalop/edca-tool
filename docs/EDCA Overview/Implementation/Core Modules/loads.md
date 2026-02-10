# Carbon Intensity Calculations (loads.py)

## Purpose

- create relevant load combos
- determine which code is relevant
- apply all the mathy math that can be extrapolated

### Inputs

- parse.py control file dataclass
- specific code flag
- loads from occupancy.csv

### Exports

- load combo and specific gamma values and final load values
- per floor

### Code Pipeline

- happens after parse.py
- used in systems.py
- passes back through for code_checks.py

## Structure

### Functions

### Dataclasses

## Constraints

### Assumptions

### Heuristics

### Areas for Improvement

### Edge Cases