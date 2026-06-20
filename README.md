# 24-Scenario Hourly Dataset of Green-Power Directly Connected Hydrogen-Ammonia Parks

## Overview

This repository contains a comprehensive high-resolution (hourly) dataset detailing the electro-thermal coupled operations and flexible chemical production of green-power directly connected hydrogen-ammonia parks. Evaluated across 24 extreme meteorological scenarios, the data captures the system's multi-stage operational evolution. It is specifically designed to support the validation of adaptive energy management systems (EMS), multi-energy dispatch algorithms, and safety power supply assessments under strict thermodynamic constraints.

## Repository Structure

To clearly separate the physical environmental baselines from the specific operational architectures (grid-tied vs. islanded), the repository is structured into three primary CSV files:

1. **Base Scenario Dataset:** `Scenarios_Base_Data.csv`
2. **Grid-Connected Operation Dataset:** `Gridconnected_Park_Operation_Data.csv`
3. **Off-Grid Operation Dataset:** `Offgrid_Park_Operation_Data.csv`

---

## 1. Base Scenario Dataset (`Scenarios_Base_Data.csv`)

This sub-dataset provides the fundamental environmental metrics and baseline loads recorded at an hourly resolution across 24 distinct weather scenarios.

| Variable | Unit | Description |
| :--- | :--- | :--- |
| **Scenario_ID** | - | Identifier for the specific meteorological scenario (1–24). |
| **Hour** | h | Hour of the day in a 24-hour format (1–24). |
| **Base_Elec_Load_MW** | MW | The foundational conventional electrical load of the chemical park. |
| **Base_Thermal_Load_MW** | MW | The foundational thermal heating demand of the chemical park. |
| **Wind_Power_MW** | MW | Available wind power generation based on the specific scenario profile. |
| **Solar_Power_MW** | MW | Available solar photovoltaic power generation based on the specific scenario profile. |

---

## 2. Grid-Connected Operation Dataset (`Gridconnected_Park_Operation_Data.csv`)

This sub-dataset translates the base scenarios into actual electro-thermal synergistic operations under a grid-connected architecture. It records the precise power allocation across multi-state equipment to meet varying daily ammonia production targets.

| Variable | Unit | Description |
| :--- | :--- | :--- |
| **Yield_Target_ton** | tons | The step-wise daily green ammonia production target (36 to 72 tons). |
| **(Time/Space)** | - | Shares the exact indexing columns (`Scenario_ID`, `Hour`, `Wind_Power_MW`, `Solar_Power_MW`) as the base dataset. |
| **Total_Elec_Load_MW** | MW | Total system electrical load including base load, electrolyzers, AMU, and electric boilers. |
| **Grid_Buy_MW** | MW | Power purchased from the main grid to maintain chemical stability. |
| **Grid_Sell_MW** | MW | Excess renewable power sold back to the main grid. |
| **Thermal_Load_MW** | MW | The physical thermal load requirement of the park. |
| **EB_Power_MW** | MW | Power consumed by the Electric Boiler to generate heat. |
| **WHRU_Heat_MW** | MW | Thermal energy recovered by the Waste Heat Recovery Unit from electrolysis and synthesis processes. |
| **ALKEL_Power_MW** | MW | Power allocated to the Alkaline Electrolyzer. |
| **PEMEL_Power_MW** | MW | Power allocated to the Proton Exchange Membrane (PEM) Electrolyzer. |
| **AMU_Power_MW** | MW | Power consumed by the flexible Ammonia Synthesis Unit. |

---

## 3. Off-Grid Operation Dataset (`Offgrid_Park_Operation_Data.csv`)

This sub-dataset characterizes the system's resilience and safety power supply during completely islanded operations. Constrained by a fixed Battery Energy Storage System (BESS), it captures the dynamic yield maximization strategy under extreme weather.

| Variable | Unit | Description |
| :--- | :--- | :--- |
| **(Time/Space)** | - | Shares indexing columns (`Scenario_ID`, `Hour`, `Total_Elec_Load_MW`, `Thermal_Load_MW`, `EB_Power_MW`, `WHRU_Heat_MW`). |
| **RE_Gen_MW** | MW | Total renewable energy generated (Wind + Solar). |
| **BESS_Discharge_MW** | MW | Power discharged from the 195 MWh Battery Energy Storage System to support the chemical load. |
| **BESS_Charge_MW** | MW | Surplus renewable power charged into the BESS. |
| **BESS_SOC_Percentage** | % | State of Charge (SOC) of the battery system. |
| **Curtailment_MW** | MW | Excess renewable power that could not be stored or used (curtailed energy). |
| **Power_Deficit_MW** | MW | Unserved load/power deficit, utilized to assess safety power supply boundaries and system resilience. |

---

## 4. Usage Guidelines for Engineers

The operational metrics provided in these datasets are **physically bounded**. All equipment power allocations (`ALKEL_Power_MW`, `PEMEL_Power_MW`, `AMU_Power_MW`) strictly adhere to real-world chemical ramp rates, variable condition energy consumption coefficients, and thermodynamic conservation laws (e.g., heat dissipation mapped to `WHRU_Heat_MW`).

* **EMS Algorithm Validation:** Researchers can utilize the `Offgrid_Park_Operation_Data.csv` to train and validate adaptive Energy Management Systems (EMS). The `Power_Deficit_MW` and `BESS_SOC_Percentage` columns serve as direct benchmarks for evaluating the resilience and self-healing capabilities of deep-learning or reinforcement-learning dispatch algorithms.
* **Flexible Production & Sizing:** The `Gridconnected_Park_Operation_Data.csv` provides a robust baseline for economic capacity sizing. By analyzing the interaction between `Yield_Target_ton` and `Grid_Buy_MW`/`Grid_Sell_MW`, engineers can evaluate optimal hybrid sizing (Wind/Solar/Electrolyzer ratios) without needing to reconstruct complex electro-thermal physical models from scratch.
