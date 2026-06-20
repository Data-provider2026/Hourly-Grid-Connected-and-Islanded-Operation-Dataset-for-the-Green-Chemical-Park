# 24-Scenario Hourly Dataset of Green-Power Directly Connected Hydrogen-Ammonia Parks

## Overview

This repository contains a comprehensive high-resolution (hourly) dataset detailing the electro-thermal coupled operations and flexible chemical production of green-power directly connected hydrogen-ammonia parks. The foundational meteorological profiles and system baseline limits were directly collected from an actual green hydrogen-ammonia park in Jilin, Northern China. Evaluated across 24 typical meteorological scenarios recorded at the site, the data captures the system's operational performance under the characteristic renewable volatility of this region. It is specifically designed to support the validation of adaptive energy management systems (EMS), multi-energy dispatch algorithms, and safety power supply assessments under strict thermodynamic constraints.

## Repository Structure

To clearly decouple the physical environmental inputs from the complex electro-thermal synergistic responses under different system architectures (grid-connected vs. islanded), the entire dataset is systematically divided into three separate sub-datasets:

1. **Base Scenario Dataset:** `Scenarios_Base_Data.csv`
2. **Grid-Connected Operation Dataset:** `Gridconnected_Park_Operation_Data.csv`
3. **Off-Grid Operation Dataset:** `Offgrid_Park_Operation_Data.csv`

All information is archived in CSV format utilizing UTF-8 encoding, ensuring that the files can be processed safely on any operating system (Windows, macOS, or Linux) and easily imported into programming environments like MATLAB or Python for further algorithmic validation.

---

## 1. Base Scenario Dataset (`Scenarios_Base_Data.csv`)

This sub-dataset provides the fundamental environmental metrics and baseline loads recorded at an hourly resolution across 24 distinct typical wind-solar scenarios. It establishes the physical baseline that drives the subsequent multi-energy dispatch models.

| Variable | Unit | Description |
| :--- | :--- | :--- |
| **Scenario_ID** | - | Identifier for the specific meteorological scenario (1–24), representing 24 different typical wind-solar scenarios. |
| **Hour** | h | Hour of the day in a standard 24-hour format (1–24). |
| **Base_Elec_Load_MW** | MW | The foundational conventional electrical load of the chemical park. |
| **Base_Thermal_Load_MW** | MW | The foundational thermal heating demand of the chemical park. |
| **Wind_Power_MW** | MW | Available wind power generation under the specific scenario. |
| **Solar_Power_MW** | MW | Available solar photovoltaic (PV) power generation under the specific scenario. |

---

## 2. Grid-Connected Operation Dataset (`Gridconnected_Park_Operation_Data.csv`)

This sub-dataset translates the base scenarios into actual electro-thermal synergistic operations under a grid-connected architecture. It records the precise power allocation across multi-state chemical equipment to meet varying daily ammonia production targets.

| Variable | Unit | Description |
| :--- | :--- | :--- |
| **Yield_Target_ton** | tons | The step-wise daily green ammonia production target, decreasing from 72 tons to 36 tons. This variable drives the flexible production dispatch. |
| **Scenario_ID** | - | Identifier for the specific meteorological scenario (1–24). |
| **Hour** | h | Hour of the day in a standard 24-hour format (1–24). |
| **Wind_Power_MW** | MW | Available wind power generation under the specific scenario. |
| **Solar_Power_MW** | MW | Available solar photovoltaic (PV) power generation under the specific scenario. |
| **Total_Elec_Load_MW** | MW | Total system electrical load, which accurately aggregates the base load, electrolyzers, ammonia synthesis units (AMU), and electric boilers (EB). |
| **Grid_Buy_MW** | MW | Power purchased from the main grid to maintain chemical process stability and maximize economic benefits. |
| **Grid_Sell_MW** | MW | Power sold to the main grid. |
| **Thermal_Load_MW** | MW | The physical thermal load requirement of the park. |
| **EB_Power_MW** | MW | Electrical power consumed by the Electric Boiler to generate heat. |
| **WHRU_Heat_MW** | MW | Thermal energy recovered by the Waste Heat Recovery Unit (WHRU) from the electrolysis and exothermic synthesis processes. |
| **ALKEL_Power_MW** | MW | Electrical power allocated to the Alkaline Electrolyzer, reflecting its distinct ramp rate. |
| **PEMEL_Power_MW** | MW | Electrical power allocated to the Proton Exchange Membrane (PEM) Electrolyzer, reflecting its distinct ramp rate and synergistic operation with ALKEL. |
| **AMU_Power_MW** | MW | Power consumed by the flexible Ammonia Synthesis Unit. |

---

## 3. Off-Grid Operation Dataset (`Offgrid_Park_Operation_Data.csv`)

This sub-dataset characterizes the system's resilience and safety power supply capabilities during completely islanded operations. Constrained by a fixed 195 MWh Battery Energy Storage System (BESS), it captures the dynamic yield maximization strategy under extreme weather.

| Variable | Unit | Description |
| :--- | :--- | :--- |
| **Scenario_ID** | - | Identifier for the specific meteorological scenario (1–24), representing different typical wind-solar scenarios. |
| **Hour** | h | Hour of the day in a standard 24-hour format (1–24). |
| **Wind_Power_MW** | MW | Available wind power generation under the specific scenario. |
| **Solar_Power_MW** | MW | Available solar photovoltaic (PV) power generation under the specific scenario. |
| **Base_Elec_Load_MW** | MW | The foundational conventional electrical load of the chemical park. |
| **ALKEL_Power_MW** | MW | Electrical power consumed by the Alkaline Electrolyzer. |
| **PEMEL_Power_MW** | MW | Electrical power consumed by the Proton Exchange Membrane (PEM) Electrolyzer. |
| **AMU_Power_MW** | MW | Electrical power consumed by the flexible Ammonia Synthesis Unit. |
| **BESS_Charge_MW** | MW | Power charged into the BESS to buffer renewable volatility and support the continuous chemical load. |
| **BESS_Discharge_MW** | MW | Power discharged from the BESS to buffer renewable volatility and support the continuous chemical load. |
| **Curtailment_MW** | MW | Excess renewable power that could not be stored or utilized by the flexible chemical processes. |
| **Power_Deficit_MW** | MW | Unserved load or power deficit. This is a critical metric utilized to assess safety power supply boundaries, revealing instances where the islanded system fails to sustain operations. |
| **BESS_SOC_Percentage** | % | State of Charge (SOC) of the battery system, reflecting the real-time energy reserve level. |
| **Thermal_Load_MW** | MW | The physical thermal load requirement of the park. |
| **EB_Power_MW** | MW | Electrical power consumed by the Electric Boiler to generate heat. |
| **WHRU_Heat_MW** | MW | Thermal energy recovered by the Waste Heat Recovery Unit (WHRU) from the electrolysis and exothermic synthesis processes. |

---

## 4. Usage Guidelines for Engineers

The operational metrics provided in these datasets are **physically bounded**. All equipment power allocations (`ALKEL_Power_MW`, `PEMEL_Power_MW`, `AMU_Power_MW`) strictly adhere to real-world chemical ramp rates, variable condition energy consumption coefficients, and thermodynamic conservation laws (e.g., heat dissipation mapped to `WHRU_Heat_MW`). By enforcing rigorous physical boundaries and thermodynamic limits throughout the modeling process, these datasets effectively eliminate invalid operational states, providing a highly reliable engineering benchmark.

- **EMS Algorithm Validation:** Researchers can utilize the `Offgrid_Park_Operation_Data.csv` to train and validate adaptive Energy Management Systems (EMS). The `Power_Deficit_MW` and `BESS_SOC_Percentage` columns serve as direct benchmarks for evaluating the resilience and self-healing capabilities of deep-learning or reinforcement-learning dispatch algorithms under islanded conditions.

- **Flexible Production & Sizing:** The `Gridconnected_Park_Operation_Data.csv` provides a robust baseline for economic capacity sizing. By analyzing the interaction between `Yield_Target_ton` and `Grid_Buy_MW`/`Grid_Sell_MW`, engineers can evaluate optimal hybrid sizing (Wind/Solar/Electrolyzer ratios) without needing to reconstruct complex electro-thermal physical models from scratch.

- **Data Source Authenticity:** The foundational meteorological data and baseline operational parameters were collected from a real-world green-power directly connected hydrogen-ammonia chemical park located in Jilin, Northern China. This region is characterized by abundant wind-solar resources but is highly prone to extreme winter weather, providing a unique environment for data collection.
