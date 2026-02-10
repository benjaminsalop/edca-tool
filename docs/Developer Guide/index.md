```md
- `edca/edca_tool/data/constants/rebar_properties.py`
  - This file creates a data class for rebar specifications, as well as two dictionaries — one for ASTM, and one for Eurocode rebar properties. For each rebar type, the data class includes the specification, bar type, diameter, spacing, and units. It then calculates bar area and number of bars in one linear unit of a slab for future calculations.
  - These properties are stored within `METRIC_REBAR_BY_SPEC` and `IMPERIAL_REBAR_BY_SPEC`, where a sample row looks like:

    ```python
    "H6@250": RebarSpec(
        spec="H6@250",
        bar_type="H",
        diameter=6.0,
        diameter_unit="mm",
        spacing=250.0,
        spacing_unit="mm",
        bar_number=4.0,
        bar_number_unit="per_m",
    ),
    ```

  - The featured documentation of this code looks like:

    ```text
    This module defines standard rebar properties for both metric and imperial systems. It includes a RebarSpec
    dataclass to encapsulate rebar specifications, and dictionaries to map rebar specs to their properties.

    The data in this module is drawn from standard engineering references, including ASTM, ACI 318-19 and CRSI
    manuals. However, there are small discrepancies between sources — especially the rough heuristic associating
    US bar sizes with diameters measured in eighths of an inch. As such, users should verify values against
    their specific requirements.

    A sample use of this function would appear as follows:

        from edca_tool.constants.rebar import lookup_rebar, UnitSystem
        bar = lookup_rebar("H6@300", UnitSystem.METRIC)
        print(bar.area_single_bar_mm2)           # mm^2 of one bar
        print(bar.area_per_m_width_m2())         # m^2 of steel per meter of width
        print(bar.area_over_width_m2(2.5))       # area in m^2 for 2.5 m width

    This function will be primarily adopted in the edca_tool.code_checks module for reinforcement calculations
    during EC or ACI code checks.
    ```

---

## Inputs

### CSVs

- `edca_tool/inputs/source/presets/csvs/materials.csv`

  ```csv
  material_id,family,concrete_psi,steel_ksi,density_kg_per_m3,ec_a1a3_per_m3,ec_a1a3_per_kg,ec_a4_per_ton_km,transport_km,ec_a5_per_kg,cost_per_m3,cost_per_kg
  concrete,concrete,4000,(blank),2400,330,0.138,0.06,50,0.005,120,0.05
  ```

  - This file has materials properties, flags, and carbon and cost data.
  - Note that the prices and unit values are all metric — this may be changed in the future.
  - Carbon and cost values are in 2025 EUR and kg/m² (which will be converted based on contemporary exchange rates).

- `edca_tool/inputs/source/presets/csvs/occupancies.csv`

  ```csv
  use,unit,sdl,sdl_partition,ll,code,notes
  Residential_US,imperial,0,15,40,ASCE,"ASCE 7-22 Table 4.3-1 L=40 psf; SDL per actual materials (not prescribed)."
  ```

  - The occupancies spreadsheet features the loading prescribed by the US and European building codes corresponding to various occupancy types.
  - Units are standardized based on the code preference (thus psf or kg/m²), and are called by the functions in early stages of the tool.
  - Including roof loading, not weather loading.

  **ASCE Loading (psf)**
  - Residential — 40
  - Office — 50
  - Assembly
    - Fixed Seats — 60
    - Lobbies — 100
    - Movable Seats — 100
    - Other — 100
  - Corridors (Including Occupied Roofs) — 100
    - Higher Floor Corridors — 80
    - Balconies/Decks — 1.5LL up to 100
  - Garages — 40
  - Hospitals (Laboratories) — 60
  - Libraries — 60–80
  - Roofs (Unoccupied) — 20

  **EC Loading (kN/m²)**
  - Residential (A) — 1.5–2
  - Office (B) — 2–3
  - Congregation (C)
    - Tables (C1) — 2–3
    - Fixed Seats (C2) — 3–4
    - Corridors (C3) — 3–5
    - Movable Seats (C4) — 4.5–5
    - Lobbies/Other (C5) — 5–7.5
  - Shopping (D) — 4–5

- `edca_tool/inputs/source/presets/csvs/systems.csv`

  ```csv
  component,system_id,category,type,span_behavior,manufacturer,unit,length,width,slab_depth,beam_depth,screed_depth,steel_depth,swt,sdl,ll,max_span,material_concrete_id,material_pt_id,material_steel_id,material_timber_id,ebc_mm,beam_ref,concrete_volume,steel_volume,floor
  bison_bb_ij1,precast,beam_block,one-way,bison,metric,0.125,0.15,0.1,1.86,1.8,1.5,4.1,concrete,pt,(blank),(blank),525,IJ1,0.0775,0.0372,
  ```

  - This file is the primary source of structural data for the larger edca tool.
  - It contains the tool’s “spine” — made of `system_id`, `type`, and `category` — which are used to categorize and classify each row into certain groupings.
  - In addition, `systems.csv` holds data on the properties of each system brand and type, including depths, weights, and maximum spans.
  - The material flags link to specific materials used for each, and volume.
  - This file is called by various functions within the tool, and thus should be altered as little as possible.

---

### Load YAML files

- `edca_tool/inputs/source/presets/yaml/load_combinations.yaml`
  - All load combinations featured in EC1990 or ASCE7-22 are featured in this file, given the alternatives and variations between combinations (in addition to the need to consider variations between each when various wind/snow/rain/other loads are included).

- `edca_tool/inputs/source/presets/yaml/load_values.yaml`
  - The `load_values` file is an essential component of Ultimate and Serviceability State Design under Eurocode-governed structures, while being optional but useful for ASCE design.
  - This file includes combination factors (gamma values) and load reduction factors (psi values) which will be used to create load combinations based on occupancy load cases.
  - It also includes area reduction parameters based on EN1990.
  - While the load case factor sets for US code design are also included in this file, they are not necessary due to simultaneously being hard-coded into the `load_cominations.yaml` file.
  - In future updates, live-load reduction files will be added to this set.
```