# EDCA Architecture

## EDCA Code and Foundations

- EDCA runs on Python
- Data is in parquets
- Based on various other tools
- Various packages are listed all over
- Things are in .py files
- Data is in .parquet files
- Optional user inputs can be added in .csv files
- Key hardcoded information (schemas, code specs, properties) are in .yml files
- Filetypes

## EDCA Structure

### Inputs

- User-Edited Registries
- User-Submitted Data (CSVs)
- Occupancies
- Systems
- Parsers

### Properties (Hardcoded)

- Units and Constants
- ASCE/EC Combinations
- Metadata (Registries and Data)

### Core Code

- Parse.py
- Loads.py
- Systems.py
- Spans.py
- Takeoff.py
- Carbon.py
- Rank.py
- Reporting.py
- Run_edca.py

### Additional Code

- Code Checks

### Testing/Validation

- Examples
- Testing

## Flow

- Data procession
- Parser Functions
- CSV -> Pydantic -> Parquet

### Key Decisions

### Configurations

### Future Improvements



