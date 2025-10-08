# TIMES-CZ v03.0 Documentation
**Authors:** Lukáš Rečka, Dali Laxton, Lukáš Vorel  
**Date:** October 2025  
**Model Version:** 3.0

---

## Executive Summary

The TIMES-CZ v03.0 model is a comprehensive energy system optimization model for Czechia, designed to analyze energy system pathways and inform energy and climate policy decisions. The model generates least-cost energy system configurations to meet future energy service demands while adhering to technical, environmental, economic, and policy constraints.

### Key Model Capabilities
- **Spatial Coverage**: 14 NUTS 3 regions plus national aggregate for transport and agriculture
- **Temporal Scope**: 2019-2050 with seasonal and daily time resolution
- **Sectoral Coverage**: Complete energy balance divided into 7 major sectors including power generation, industry, buildings, and transport
- **Technology Detail**: ETS facilities at individual level, 300+ transport technologies, and comprehensive representation of energy conversion processes up to energy service level
- **Policy Applications**: Carbon pricing, renewable energy targets, decarbonization pathways, impact assessment

### Main Applications
The model is primarily used for:
- Climate policy impact assessment (EU ETS, carbon taxation, renewable targets)
- GHG and air pollutant emission projections
- National energy strategy development and evaluation
- Technology pathway analysis for decarbonization
- Transport sector transformation scenarios

### Key Limitations
- Based on perfect foresight and competitive market assumptions
- Contains proprietary data (EU ETS reports, ADSEND questionnaire) preventing open-access sharing
- No endogenous  behavior demand response
- Macroeconomic feedback only through links with macro-econometric models (E3ME, CGE)

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Model Architecture Overview](#2-model-architecture-overview)
3. [Sectoral Structure and Coverage](#3-sectoral-structure-and-coverage)
4. [Input Data Sources](#4-input-data-sources)
5. [User Constraints](#5-user-constraints)
6. [References](#references)

---

## 1. Introduction

This documentation provides a technical overview of the TIMES-CZ v03.0 model, including its structure, sectors, and constraints. The documentation focuses on model structure and general characteristics, providing the list of data sources needed to compile the model. Base year data and scenario-specific inputs are described in relevant model applications (see  Ščasný et al. 2025 and [Rečka, Ščasný, Laxton 2023](https://www.mdpi.com/1996-1073/16/21/7380)).

The TIMES-CZ model is mostly based on publicly available data but includes individual data from EU ETS reports and ADSEND questionnaire that cannot be shared. Due to these proprietary data sources, the model cannot be shared as open-access.

### 1.1 Model Overview

The TIMES-CZ model generates energy system pathways for Czechia under given assumptions. It identifies the most cost-effective configuration of energy fuels and technologies to meet future energy service demands while adhering to technical, environmental, economic, social, and policy constraints. Key inputs include availability and costs of primary energy resources, technical and cost evolution of new technological options, and maximum feasible uptake rates of new technologies. Additionally, TIMES-CZ can evaluate implications of specific policies such as regulatory measures, technology targets, taxes, and subsidies (e.g., energy taxation,
subsidies for renewable sources, or targets for electric vehicles).

### 1.2 The TIMES Framework

**TIMES** (The Integrated MARKAL-EFOM System) is a bottom-up optimization model generator for analyzing energy-environment systems. It is developed and maintained by the [Energy Technology Systems Analysis Programme (ETSAP)](https://iea-etsap.org/), a Technology Collaboration Programme (TCP) of the International Energy Agency (IEA). TIMES models can encompass single or multiple regions and are typically detailed in technology, used for medium- to long-term energy system analysis and planning at regional, national, or global levels.

TIMES is a linear optimization, techno-economic, partial equilibrium model generator that assumes perfectly competitive markets and perfect foresight. The standard objective function maximizes net total surplus (sum of producers' and consumers' surpluses), which in a perfect market with perfect foresight equates to maximizing the net present value (NPV) of the entire energy system, thereby maximizing societal welfare. It determines the energy system configuration that minimizes discounted total energy system costs over the model time horizon, including investments, fixed and variable costs, fuel import costs, and export revenues for all modeled processes, less potential salvage values of investments with lifetimes extending beyond the model time horizon.

#### Key Inputs Required

| Input Category | Description |
|---|---|
| **Reference Energy System (RES)** | Process-flow architecture of sectors and energy flows |
| **Technology Parameters** | Techno-economic data for current and future technologies |
| **Energy Service Demands** | Physical services driving energy demand |
| **Supply Curves** | Primary resource availability and costs |
| **User Constraints** | Policy and technical limitations |

TIMES outputs optimal investment and operation levels of all energy technologies that meet future energy service demands at least cost while respecting user constraints. The model also produces corresponding energy flows, emissions, and marginal prices of energy and emission flows.

For detailed TIMES documentation, see [IEA-ETSAP Optimization Modeling Documentation](https://iea-etsap.org/index.php/documentation).

---

## 2. Model Architecture Overview

TIMES-CZ model describes the complete Czech energy balance. Maintained by the **[Charles University Environment Centre](https://czp.cuni.cz/en/)**, the model has grown in complexity over time. Initially based on the Czech region of the Pan-European TIMES PanEu model developed by the Institute of Energy Economics and Rational Energy Use at the University of Stuttgart, it has undergone significant enhancements. The model was expanded to include detailed power sector representation and individual ETS facilities (TIMES-CZ v01), and detailed transportation and fuel production modules (TIMES-CZ v02).

Version 3.0 introduces disaggregated representation of Czechia comprising 14+1 regions, each characterized by distinct energy service demand profiles and biomass availability potential. Regional delineation follows the CZ-NUTS classification system at NUTS 3 administrative level. The additional "+1" region represents the Czech Republic in its entirety and primarily encompasses the transport sector and agriculture, which remain non-regionalized.

The model incorporates greenhouse gas emissions and air pollutant emissions (including nitrogen oxides, sulfur oxides, and particulate matter) from both fuel combustion and industrial processes. However, greenhouse gas emissions from land use, land-use change and forestry (LULUCF), waste management, and fluorinated gases (F-gases) fall outside the model's scope.

The model uses **2019** as its reference year, with all energy flows, emission inventories, and energy technology stock assessments calibrated against the Czech energy balance for that year. The year 2020 was not selected to avoid bias caused by the COVID-19 pandemic. The standard energy unit used throughout the model is the petajoule (PJ).

### 2.1 Model Characteristics

| Characteristic | Description |
|---|---|
| **Spatial Coverage** | 14 NUTS 3 regions + 1 national aggregate |
| **Base Year** | 2019 |
| **Time Horizon** | 2019-2050 |
| **Time Structure** | 2 two-year + 6 five-year periods |
| **Time Slices** | 12 slices (4 seasons × 3 daily parts) |
| **Discount Rate** | 7.5% (general), technology-specific rates available |

### 2.2 Temporal Resolution

#### Modeling Periods
- **2019-2021, 2021-2023** (2-year periods)
- **2025, 2030, 2035, 2040, 2045, 2050** (5-year periods; milestone year represents period center)

#### Seasonal Definition

| Season | Start Date | End Date | Characteristics |
|---|---|---|---|
| **Spring** | March 15 | May 31 | Moderate temperatures |
| **Summer** | June 1 | August 30 | Zero heating demand, High PV generation |
| **Autumn** | August 31 | November 15 | Moderate temperatures |
| **Winter** | November 16 | March 14 | Peak heating demand |

#### Daily Time Structure
The model distinguishes 3 parts of day within each period:
- Day = 11 hours
- Night = 12 hours
- Peak = 1 hour with the highest electricity demand within a day.

#### Time Slice Structure

| Time Slice | Fraction of Year |
|---|---|
| **Spring - Day** | 0.097 | 
| **Spring - Night** | 0.106 | 
| **Spring - Peak** | 0.009 |
| **Summer - Day** | 0.114 | 
| **Summer - Night** | 0.125 | 
| **Summer - Peak** | 0.010 | 
| **Autumn - Day** | 0.097 | 
| **Autumn - Night** | 0.106 | 
| **Autumn - Peak** | 0.009 | 
| **Winter - Day** | 0.151 | 
| **Winter - Night** | 0.164 |
| **Winter - Peak** | 0.014 |

### 2.3 Discount Rates

The discount rate is a key parameter in the TIMES objective function, representing the degree to which future values are discounted to the base year. We distinguish a general discount rate (representing a social discount rate) and technology-specific discount rates (hurdle rates). A social discount rate reflects society's view of present versus future costs and benefits and is lower than the financial discount rate applied to firm-level investment decisions. Technology-specific discount rates capture the investor's perspective and risk associated with specific technologies, representing the weighted average cost of capital (WACC).

Generally, within an energy system model, higher discount rates tend to delay decarbonization efforts and favor technologies requiring lower capital investment.

Unless specified otherwise, the TIMES-CZ model uses a **general discount rate of 7.5%**. This value is at the upper end of the range recommended by Steinbach & Staniaszek (2015) for EU Member States.

Unless otherwise specified, technology-specific discount rates are adopted from the JRC-TIMES model (JRC 2018):

**Technology-Specific Discount Rates**

| Technology Category | Rate (2025-2050) |
|---|---|
| **Electricity Production** | 9.0% |
| **Electricity with CCS** | 12.0% |
| **Geothermal Electricity** | 12.0% |
| **Electricity Storage** | 9.0% |
| **Combined Heat & Power** | 12.0% |
| **District Heating** | 10.0% → 8.5% |
| **Industry** | 12.0% |
| **Commercial** | 10.0% → 8.5% |
| **Residential** | 10.0% |
| **Personal Vehicles** | 11.0% |
| **Buses** | 8.0% |
| **Transmission/Distribution** | 7.0% |
| **Biofuels Production** | 8.0% |

### 2.4 Model Structure Overview

The energy flows within the model are depicted in the following simplified diagram:

**Simplified structure of TIMES-CZ model**
![TIMES CZ_scheme](TIMES-CZ_scheme.png)

> **Note:** All energy carriers with base year trade have future import/export optimization potential. Electricity trade varies by time slice with different prices and volumes.

---

## 3. Sectoral Structure and Coverage

The model is divided into **seven sectors** with detailed regional disaggregation (14 NUTS 3 regions, except transport and agriculture). The structure follows energy balance structure and nature of main processes. **ETS facilities** in each sector are modeled at individual level, except iron and steel production which is modeled at technology process level.

### 3.1 Primary Energy & Fuels Transformation

**Scope:** Extraction, processing, import & export, and transformation of primary energy resources into secondary energy carriers

**Key Processes:** Mines, biomass potential, refineries, coking plants, other secondary energy transformation (except heat and power)

**Regional Coverage:** 14 regions

### 3.2 Public Heat & Power Generation

**Scope:** Public power plants, Combined heat and power plants (CHPs), heating plants, and renewable energy electricity production

**Electricity Voltage Levels:** High, Medium, Low 

**Grid Losses:** Modeled explicitly but simplified 

**Demand Drivers:** Electricity demand from all sectors with seasonal and intra-daily variations. Derived heat demand from all sectors with seasonal variations.

**Regional Coverage:** 14 regions

#### 3.2.1 Electricity trade
There are five price bands for electricity imports and exports: 0–25, 25–50, 50–75, 75–100 and over 100 €/MWh. The potential for importing and exporting electricity in each price band is determined for each timeslice based on the output from the hourly PLEXOS model (provided by ČEPS).

### 3.3 Industry Sector

**Scope:** Industrial energy consumption and material production processes, including both energy-intensive industries with physical production tracking and service industries with energy service demands.

The industry sector is split into 15 subsectors according to the energy balance. Energy-intensive subsectors with relatively homogeneous products (iron and steel, cement) are modeled up to demand for products in physical units (Mt). Demand in other subsectors is tracked in energy units (PJ) and represents energy required for energy services such as machine drive or steam. This means that, except for electricity and delivered heat, demand is lower than the subsector's final energy consumption because it represents energy after transformation within the subsector (e.g., heat output of a boiler).

**Demand Drivers:** 
- Physical production demands (Mt) for homogeneous products (steel, cement, lime, glass, bricks, ceramics, paper)
- Energy service demands (PJ) for machine drive, steam, high-temperature heat, and other processes
- Non-energy use demands for feedstocks (PJ)

**Regional Coverage:** 14 regions with some subsectors partially aggregated at national level due to limited data availability. EU ETS facilities modeled individually with specific locations and characteristics.

**Subsector Structure:**

| Subsector | Demand Unit | Modeling Approach |
|---|---|---|
| **Iron & Steel** | Mt | Physical production |
| **Non-Ferrous Metals** | PJ | Energy services |
| **Chemical Industry** | PJ | Energy services |
| **Cement** | Mt | Physical production |
| **Lime** | Mt | Physical production |
| **Glass** | Mt | Physical production |
| **Bricks & Ceramics** | Mt | Physical production |
| **Pulp & Paper** | Mt | Physical production |
| **Food & Tobacco** | PJ | Energy services |
| **Machinery & Transport Equipment** | PJ | Energy services |
| **Wood & Wood Products** | PJ | Energy services |
| **Textile** | PJ | Energy services |
| **Other Industries** | PJ | Energy services |
| **Non-Energy Use - Chemical/Petrochemical** | PJ | Final demand |
| **Non-Energy Use - Other** | PJ | Final demand |

### 3.4 Residential Sector

**Scope:** Final energy consumption of households satisfying 11 energy service demands.

**Energy Service Demands and Drivers:** The model satisfies 11 distinct energy service demands:
- Space Heating (largest component; dependent on temperature, household numbers & energy efficiency improvements)
- Space Cooling (growing demand; weather-dependent)
- Hot water (relatively stable; population-dependent)
- Lighting (relatively stable; dependent on household numbers)
- Cooking (relatively stable; population-dependent)
- Refrigeration (relatively stable; dependent on household numbers)
- Cloth Washing (relatively stable; population-dependent)
- Cloth Drying (slowly growing; dependent on household numbers and increasing equipment adoption)
- Dish Washing (slowly growing; dependent on household numbers and increasing equipment adoption)
- Other Electric services (electronics, IT equipment; GDP-dependent)
- Other Energy services (other thermal uses; GDP-dependent)

**Regional Coverage:** 14 regions

### 3.5 Commercial Sector

**Scope:** Energy services in tertiary sector buildings including offices, retail, hospitality, public buildings, and other service sector activities.

**Energy Service Demands and Drivers:** 9 main energy service demands:
- Space Heating (dependent on temperature, building size & number, and energy efficiency improvements)
- Space Cooling (increasing demand; dependent on tertiary sector output, usually linked to E3ME or CGE model)
- Hot water (Constant)
- Lighting (dependent on tertiary sector output, usually linked to E3ME or CGE model)
- Refrigeration (Constant)
- Cooking (restaurant and hospitality sector; dependent on tertiary sector output)
- Public lighting (dependent on household numbers)
- Other Electric services (IT, office equipment; Constant)
- Other Energy services (other thermal uses; Constant)

**Regional Coverage:** 14 regions

### 3.6 Agriculture Sector

**Structure:** Final energy consumption in generic energy consumption processes

**Regional Coverage:** National aggregate only

### 3.7 Transport Sector

**Scope:** All motor-based transport modes within Czech borders including passenger and freight transport, with detailed technology representation for road transport and simplified representation for rail, marine, and aviation.

**Key Features:**
- **Road transport:** 135+ vehicle technologies in base year covering different powertrains (diesel, gasoline, hybrid, plug-in hybrid, battery electric), 3 vehicle sizes of passangers cars, and [COPERT](https://www.emisia.com/utilities/copert/) emission standard classification (Euro)
- **Rail transport:** Electric and diesel trains for passenger and freight services
- **Inland navigation:** Domestic shipping
- **Aviation:** Domestic, International intra EU, International extra EU flights
- **Supporting infrastructure:** Biofuel production facilities, charging infrastructure considerations
- **COPERT methodology** for air pollutant emissions
- **Vehicle markets:** New and used vehicle segments (as a new registration in CZ) with vintage tracking

**Demand Drivers:** Transport demand expressed in passenger-kilometers (pkm) and tonne-kilometers (tkm):

| Mode | Subcategories | Demand Unit |
|---|---|---|
| **Personal Road** | Small, Medium, Large vehicles; Motorcycles | pkm |
| **Freight Road** | N1 (<3.5t), N2 (3.5-12t), N3 (>12t) | tkm |
| **Personal Rail** | Heavy (trains), Light (metro/trams) | pkm |
| **Freight Rail** | Heavy freight | tkm |
| **Inland navigation** | Domestic shipping | Final energy demand |
| **Aviation** |Domestic, International intra EU, International extra EU flights | Final energy demand|

**Regional Coverage:** National aggregate only

---

## 4. Input Data Sources

Exogenous model inputs are divided into five categories:

1. **Reference Energy System (RES):** Process-flow architecture of economic sectors and energy flows between technologies that consume and produce energy, energy services, and other commodities such as environmental emissions. Includes techno-economic parameters of existing technologies.

1. **Techno-economic parameters** of potential future energy technologies.

1. **Energy service demands:** Physical services required by economy and society for mobility, heating, industrial production, etc., which drive energy demand.  Energy service demands can be replaced by final energy demand if necessary.

1. **Energy supply curves:** Quantities of primary energy resources  (e.g. lignite, biomass and wind power) or imported commodities  (e.g. oil, gas and electricity) available at specific cost points over time for different quantities of energy commodities.

1. **User constraints:** Linear constraints on technologies or fuels used to simulate real-world technology constraints or policy scenarios.

The TIMES-CZ model is mostly based on publicly available data but includes individual data from EU ETS reports and ADSEND questionnaire that cannot be shared, preventing open-access model sharing.

Subsections of this section describe the data sources and their usage for the first four categories. The User constraint`s sections follows.

### 4.1 Reference Energy System (RES)

The RES comprises the process-flow architecture of economic sectors and energy flows (i.e.
commodities) between technologies (i.e. processes) that consume or produce energy, energy services, emissions, and other materials. It includes the techno-economic parameters of the current technology stock. The aim is to accurately describe the base year while ensuring the model is feasible.

**Main publicly available statistical data sources:**
- Czech energy balance by Eurostat: structure and aggregate energy flows
- Energy Regulatory Office: electricity and heat statistics
- ENTSO-E: electricity statistics, hourly electricity production and load data
- Czech Statistical Office: population, building stock, households, [ENERGO surveys](https://csu.gov.cz/energo-2021)
- Ministry of Industry and Trade: Renewable energy statistics, boiler and heat pump sales, [Regional energy balances](https://mpo.gov.cz/cz/energetika/statistika/energeticke-bilance/krajske-energeticke-bilance--260319/)

**Research partner data:**
- [CDV](https://www.cdv.cz/): Driven kilometers from state technical control (per strata), emission factors calculated using COPERT methodology
- [CTU](https://www.cvut.cz/en): Energy consumption and utilization for each vehicle category (within [MOSUMO](https://czp.cuni.cz/cz/projekty/mosumo-ta-cr-theta-2021-2023) project)

**Individual facility data:**
- EU ETS Emission Reports: individual facility fuel consumption and CO2 emissions
- ADSEND questionnaire: unit-level electricity and heat production
- REZZO database: emission factors of air pollutants, fuel consumption
- Czech vehicle register: vehicle category, fuel type, first registration - worldwide, first registration in Czechia

#### 4.1.1 Regionalization

Regionalization of nationwide energy balance into 14 NUTS 3 regions is based on individual EU ETS reports, ADSEND data, Regional energy balances, and ENERGO surveys. When source data year differs from base year (2019), shares from source data are applied to base year values of nationwide energy balance. This mainly applies to Regional energy balances (2018 values) and ENERGO surveys (2015 and 2021 values). The Energo data also shows the proportion of each energy type used in households in each region, as well as its specific purpose: heating, cooling, water heating and powering other appliances.

### 4.2 Techno-economic Parameters of technology candidates for new technologies

**Main sources:**
- Majority of assumptions from TIMES PanEU (Capros et al., 2024) and JRC-EU-TIMES (Nijs & Ruiz, 2019) models
- PV and wind investment costs from [Aliance pro energetickou soběstačnost](https://www.alies.cz/) adjusted using [Module Price Index](https://www.pv-magazine.com/module-price-index/) and IEA World Energy Outlook (October 2020)
- Other renewable technologies (e.g. heat pumps, biogas, and water) from KOZE (https://komoraoze.cz/) as part of [SEEPIA](https://seepia.cz/en/) project
- Advance low-carbon technologies in steel and cement production are added based on own review of Agora, TNO and IEA conducted in 2021.
- The cost for new and used (newly registered) vehicles is based on offer prices from the largest Czech auto advertising portal [Sauto.cz](https://www.sauto.cz/), kindly provided by Seznam.cz, a.s., and predicted development of powertrain costs from the Roland Berger (2016) study further extrapolated beyond 2030. The cost projection methodology is described in [Rečka, Ščasný, Máca. (2022)](https://mpo.gov.cz/assets/cz/energetika/vyzkum-a-vyvoj-v-energetice/resene-dokoncene-projekty-a-jejich-vystupy/projekty-podporene-v-ramci-1-verejne-souteze-programu-theta/2023/2/Predikce-vyvoje-vozoveho-parku--spotreby-energii-a-emisi-z-dopravy-_vystup-V2_.pdf#page=27.31) (only in Czech).

### 4.3 Energy Service Demands

Projected energy service demands are usually scenario-specific. In most applications, industry and commercial sector demands link with E3ME macro-econometric model outputs. Transport volumes derive from Transport Policy of the Czech Republic (2021) unless stated otherwise. Residential energy service demands combine population, household, and building standard projections.

### 4.4 Energy Supply Curves

Fuel and emission allowance prices, together with projected taxes, are usually scenario-specific.

**Common external sources for fossil fuels and EUA prices:**
- European Commission recommended parameters for GHG projections
- IEA World Energy Outlook

**Electricity trade** (prices and maximum imports/exports) has same time resolution as model electricity (12 time slices, defined in section 2.2). Outcomes from PLEXOS model operated by Czech transmission system operator ČEPS, which models European electricity market based on ENTSO-E data in hourly resolution, are aggregated to TIMES-CZ time resolution and provide potentials of electricity imports and export and their prices.

**Forest biomass** is distinguished by energy utilization form. Regional availability based on forest biomass development scenarios by Cienciala and Melichar (2024). Biomass characteristics detailed in [Rečka et al. (2023)](https://www.mdpi.com/1996-1073/16/21/7380).

---

## 5. User Constraints

User constraints (UC) are linear constraints on technologies or fuels used to simulate real-world technology or behavioral constraints. Detailed documentation in the  Documentation for the TIMES Model Part II, Loulou et al. (2020). This section describes UCs common across most TIMES-CZ scenarios, focusing on left hand side (LHS) constraints.

Two main UC types:

**Absolute UC:** LHS relates to absolute value
$$\sum_{r,t,s,p,c} variable_{r,t,s,p,c} \geq/\leq/= value_{r,t,s}$$

**Relative UC:** Defines shares between variables, usually within sector and energy type
$$\sum variable_{r,t,s,j,k} \leq share_{r,t,s} \sum variable_{r,t,s,p,c}$$

Where *p* = processes, *c* = commodities, *j∈p* = selected process, *k∈c* = selected commodity.

Variables can be commodity *flow*, process *activity*, process *capacity*, process *new capacity*, or other attributes. For simplicity, unless stated otherwise, UCs are summed over regions and timeslices.

The tables with the UCs contain descriptions of the UC, correspondig times specific values and code of the UC used to in the TIMES-CZ model itself as a reference for the modellers. Where only values for some milestone years are provided,the values are interpolated between those milestone years or extrapolated till the end of the modellig horizon, i.e. 2050.

### 5.1 Power and Heat Sector Constraints


**Maximum New Capacity**

| Technology | Description | Unit | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| New Large nuclear | Maximum total new capacity | GW | - | - | 3.6 | 4.8 | 4.8 | 4.8 | AU_STD_CAP_newNUC_large |
| Small nuclear (SMR) | Maximum new capacity per period | GW | 0 | 0.4 | 0.8 | 0.8 | 1.6 | 1.6 | AU_STD_NCAP_SMR |
| Wind | Maximum new capacity per period| GW | 0.5 | 2 | 2 | 2 | 3 | 4 | AU_STD_NCAP_WIN |
| Gas | Maximum new capacity per period | GW | 5 | 5 | 5 | 5 | 5 | 5 | AU_STD_NCAP_GAS |

**Maximum Total Capacity Limits**

| Technology | Description | Unit | 2025 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |:--- |
| Compressed Air Energy Storage (CAES) | Maximum capacity | GW | 0 | 0.29 | 0.29 | AU_STD_CAP_CAES |


**Grid Integration Capacity Stages**

Grid integration stages represent increasing costs of integrating intermittent renewable power into transmission/distribution grids. Higher stages = higher costs per unit. . The capacities in the
table below relate to the virtual streams of solar and wind energies utilised at 100% within a year – i.e. 1 GW of solar grid capacity corresponds to approximately 10 GW of photovoltaic panels, and 1 GW of wind grid capacity corresponds to approximately 4 GW of wind power plants (due to the different utilisation of photovoltaic and wind power plants).

| Technology/Stage | Unit | 2025-2050 | UC Code |
|---|---|---|---|
| Solar Grid Stage 2 | GW | 1 | AU_STD_CAP_GRIDSOL2 |
| Solar Grid Stage 3 | GW | 2 | AU_STD_CAP_GRIDSOL3 |
| Solar Grid Stage 4 | GW | 4 | AU_STD_CAP_GRIDSOL4 |
| Solar Grid Stage 5 | GW | 6 | AU_STD_CAP_GRIDSOL5 |
| Wind Grid Stage 2 | GW | 3 | AU_STD_CAP_GRIDWIN2 |
| Wind Grid Stage 3 | GW | 4 | AU_STD_CAP_GRIDWIN3 |
| Wind Grid Stage 4 | GW | 5 | AU_STD_CAP_GRIDWIN4 |
| Wind Grid Stage 5 | GW | 6 | AU_STD_CAP_GRIDWIN5 |

#### 5.1.2 District Heating Constraints

**Technology Share Constraints**

| Description | Unit | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Max share of Solar heat plant at heat production | share | 0.08 | 0.1 | 0.1 | 0.1 | 0.1 | 0.1 | SU_HTH_SOL |
| Max biomass and biogas share in district heating heat plants (flow)| share | 0.4 | 0.5 | 0.63 | 0.75 | 0.88 | 1 | SU_HTH_BIOHPL |

**Public District Heating Energy Flow Constraints**

| Description | Unit | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Max heat from solar thermal systems in district heating| PJ | 0 | 0 | 0 | 0 | 12 | 12.5 | AU_HTH_SOL |
| Maximum coal consumption in district heating | PJ | 50 | 50 | 50 | 50 | 50 | 50 | AU_STD_FLO_HTH_COL |
| Maximum nuclear consumption in district heating | PJ | 2 | 10 | 15 | 20 | 20 | 20 | AU_STD_FLO_HTH_NUC |

**Maximum Total Capacity Limits**

| Technology | Description | Unit | 2025  2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |:--- |
| Large Heat pumps - district heating | Maximum capacity | GW | 40 |40 | AU_NCAP_EHE |


#### 5.1.3 Fuel Consumption Limits

| Fuel Type | Description | Unit | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Gas | Lower bound in power & heat | PJ | - | 40 | 30 | - | - | 0 | AL_ELCCOL |
| Hard coal | Maximum bound in power & heat | PJ | 163 | 144 | 143 | 136 | 133 | 130 | AU_HC |
| Starchy crops| Maximum bound in power & heat | PJ | 10 | 13 | 15 | 17 | 19 | 21 | AU_BIOSTC |
| HFO | Maximum bound in power & heat | PJ | 3 | 5 | 6 | 7 | 7 | 7 | AU_INDFHO |
| LPG | Maximum bound in power & heat | PJ | 3 | 3 | 3 | 3 | 5 | 5 | AU_INDLPG |

### 5.2 Industry Sector Constraints (IND)

#### 5.2.1 General Industrial Heat and Electricity

These constraints outline the shares of heat generated by industrial boilers and the proportion of electricity from autoproducers within the industrial sector.

| Description | 2025-2050 | UC Code |
|---|---|---|
| Max share of heat from industrial boilers | 1.0 | SU_IND_HET |
| Min share of heat from industrial boilers | 0.75 | SL_IND_HET |
| Min share of electricity produced at autoproducer CHPs | 0.25 | SU_IND_ELC |

#### 5.2.2 Iron and Steel Industry (IIS)

| Description | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
|---|---|---|---|---|---|---|---|
| Max CCS share for iron blast furnace | 0 | 0.05 | 0.2 | 0.5 | 0.6 | 0.74 | AFSU_CCS_IIS |

#### 5.2.3 Non-Ferrous Metals Industry (INF)

These constraints define the maximum share of various fuels used for high-temperature heat, machine drive, and other processes in the non-ferrous metals sub-sector (NF).

| Description | 2020 | 2050 | UC Code |
|---|---|---|---|
| Max high-temp heat from biomass | 0.5 | 0.5 | SU_INF_HTH_BIO |
| Max high-temp heat from coal | 0 | 0 | AFSU_INFHTH_INDCOA_II |
| Max NF metal machine drive from coal | 0 | 0 | AFSU_INFMCH_INDCOA_II |
| Max NF metal machine drive from coke | 0 | 0 | AFSU_INFMCH_INDCOK_II |
| Max NF metal other processes from coal | 0 | 0 | AFSU_INFOTH_INDCOA_II |
| Max NF metal other processes from coke | 0 | 0 | AFSU_INFOTH_INDCOK_II |
| Max NF metal heat processes from coal | 0.14 | 0.14 | AFSU_INFPRC_INDCOA_II |
| Max NF metal heat processes from coke | 0 | 0 | AFSU_INFPRC_INDCOK_II |
| Max NF metal steam processes from coal | 0.02 | 0.02 | AFSU_INFSTM_INDCOA_II |
| Max NF metal steam processes from coke | 0 | 0 | AFSU_INFSTM_INDCOK_II |

#### 5.2.4 Chemical Industry (ICH)

**Ammonia Production with CCS**

| Description | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
|---|---|---|---|---|---|---|
| Max CCS share in ammonia production | 0.2 | 0.35 | 0.45 | 0.5 | 0.51 | AFSU_CCS_IAM |

**Other Chemicals - Energy Sources and Technologies**

These user constraints detail the shares of different energy sources and technologies for various end-uses (e.g.high-temperature heat, machine drive, and steam generation) in the other chemicals industry.

| Description | 2025 | 2030 | 2050 | UC Code |
|---|---|---|---|---|
| Max biomass share for hightemp heat in Other Chemicals | 0.3 |  | 0.3 | SU_ICH_HTH_BIO |
| Max coal share for hightemp heat in Other Chemicals |  | 0 | 0 | AFSU_ICHHTH_INDCOA_II |
| Max heat pump share for hightemp heat in Other Chemicals | 0.05 |  | 0.4 | SU_HP_ICHHTH |
| Min electricity share for machine drive in Other Chemicals | 0.96 |  | 0.96 | SL_ICH_MCH |
| Max coal share for machine drive in Other Chemicals |  | 0 | 0 | AFSU_ICHMCH_INDCOA_II |
| Max coke share for machine drive in Other Chemicals | 0 |  | 0 | AFSU_ICHMCH_INDCOK_II |
| Max biomass share for other processes in Other Chemicals | 0.1 |  | 0.175 | SU_ICH_OTH_BIO |
| Max coal share for other processes in Other Chemicals | 0.1 |  | 0.1 | AFSU_ICHOTH_INDCOA_II |
| Max coke share for other processes in Other Chemicals | 0 |  | 0 | AFSU_ICHOTH_INDCOK_II |
| Max lignite share for other processes in Other Chemicals | 0.7 |  | 0 | AFSU_ICHOTH_INDCOL |
| Max hightemp heat for other processes in Other Chemicals | 0.9 |  | 0.4 | AFSU_ICHOTH_ICHHTH |
| Min electricity share for process heat in Other Chemicals | 0.16 |  | 0.12 | SL_ICH_PRC |
| Max biomass share in process heat in Other Chemicals | 0.175 |  | 0.175 | SU_ICH_PRC_BIO |
| Min gas share in process heat in Other Chemicals | 0.15 |  | 0.1 | SL_ICH_PRC_GAS |
| Max coal share in process heat in Other Chemicals | 0.15 |  | 0.15 | AFSU_ICHPRC_INDCOA_II |
| Max coke share in process heat in Other Chemicals | 0.04 |  | 0.04 | AFSU_ICHPRC_INDCOK_II |
| Max electricity share in process heat in Other Chemicals | 0.2 |  | 0.4 | SU_ICH_PRC_ELC |
| Max coal share in steam in Other Chemicals | 0.16 |  | 0.16 | AFSU_ICHSTM_INDCOA_II |
| Max electricity share in steam in Other Chemicals | 0.05 |  | 0.4 | AFSU_ICHSTM_INDELC |
| Max coke share in steam in Other Chemicals | 0 |  | 0 | AFSU_ICHSTM_INDCOK_II |
| Max hightemp heat from steam as part of Other Chemicals | 0.6 |  | 0.8 | AFSU_ICHSTM_ICHHTH |

#### 5.2.5 Cement Industry (ICM)

The following constraints apply to cement production, including maximum shares for public heat, CHP, coal in high-temperature heat processes, and progressive CCS share.

| Description | 2020 | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
|---|---|---|---|---|---|---|---|---|
| Max district heat share for Cement production | 0.1 |  |  |  |  |  | 0.1 | SU_PUBHEAT_ICM |
| Max CHP (on site) share for Cement | 0.15 |  |  |  |  |  | 0.15 | SU_CHP_ICM |
| Max coal share in Cement hightemp heat | 0.1 |  |  |  |  |  | 0.1 | AFSU_ICMHTH_INDCOA_II |
| Max CCS share in Cement production | 0 | 0.01 | 0.05 | 0.2 | 0.4 | 0.5 | 1.0 | AFSU_CCS_ICM |

#### 5.2.6 Lime Industry (ILM)

These user constraints for lime production specify limits on heat sources, fuel shares, and required contribution from specific kiln technologies.

| Description | 2020 | 2050 | UC Code |
|---|---|---|---|
| Max district heat share for Lime | 0.95 | 0.95 | SU_PUBHEAT_ILM |
| Max hard coal share for Lime high-temp heat | 0.1 | 0.1 | AFSU_ILMHTH_INDCOA_II |
| Min share of kilns producing high-temp heat for Lime | 0.67 | 0.67 | SL_KILN_ILM |
| Max biological/waste kilns for Cement high-temp heat | 0.05 | 0.3 | SU_BIO_WST_ICMEHTH |


#### 5.2.7 Paper Industry (IPP)

The following constraints define the minimum share of recycled paper and limits on various fuel sources for heat generation and other processes in the paper production industry.

| Description | 2025 | 2030 | 2050 | UC Code |
|---|---|---|---|---|
| Min share of recycled paper | - | 0.27 | 0.27 | AL_IPP_RYC |
| Max share of coal in high-temp heat for paper demand | 0 |  | 0 | AFSU_IPPHTH_INDCOA_II |
| Max share of biomass in high-temp heat for paper demand | 0.92 |  | 0.9 | SU_IPP_HTH_BIO |
| Max share of black liquor in high-temp heat for paper demand | 0.9 |  | 0.8 | SU_IPP_HTH_BLQ |
| Max share of high-temp heat from Heat Pumps for paper demand | 0.1 |  | 0.55 | SU_HP_IPPHTH |
| Max share of biomass in paper production processes | 0.3 |  | 0.3 | SU_IPP_PRC_BIO |
| Max share of coal in paper production processes | 0 |  | 0 | AFSU_IPPPRC_INDCOA_II |
| Max share of coke in paper production processes | 0 |  | 0 | AFSU_IPPPRC_INDCOK_II |

#### 5.2.8 Food and Tobacco Industry (IFT)

The following user constraints set maximum and minimum shares for various energy sources and technologies in heat generation and space heating.

| Description | 2020 | 2050 | UC Code |
|---|---|---|---|
| Max coal share for food & tobacco heat | 0.11 | 0.1 | AFSU_IFTHTH_INDCOA |
| Max coal share for food & tobacco sugar industry heat | 1 | 0.75 | AFSU_IFTSIH_INDCOA |
| Max share of food & tobacco heat from Heat Pumps | 0.1 | 0.55 | SU_HP_IFTHTH |
| Max share of food & tobacco sugar heat (space heat) from BHKW | 0.1 | 0.2 | SU_BHKW_IFTHSP |
| Max share of food & tobacco sugar heat – space heat – from heat exchange | 0.8 | 0.8 | SU_EX_IFTHSP |
| Max share of food & tobacco sugar heat – space heat – from air heat pump | 0.8 | 0.8 | SU_HP_AHT_IFTHSP |
| Max share of food & tobacco sugar heat – space heat – from bio boiler | 0.8 | 0.8 | SU_Bio_Boiler_IFTHSP |
| Max share of food & tobacco sugar heat – space heat – from ground heat pump | 0.8 | 0.8 | SU_HP_GEO_IFTHSP |
| Max share of food & tobacco heat from public heat | 0.3 | 0.45 | SU_PUBHEAT_IFT |

#### 5.2.9 Other Industries

The following user constraints for 'Other Industries' (IOI) detail the allowed shares of different fuels and technologies for heat, steam, and other processes, as well as phase-out constraints for existing electric cookers.

**Other Industries (IOI)**

| Description | 2020 | 2050 | UC Code |
|---|---|---|---|
| Max biomass share in other industries high-temp heat | 0.4 | 0.8 | SU_IOI_HTH_BIO |
| Max coal share in other industries high-temp heat | 0 | 0 | AFSU_IOIHTH_INDCOA_II |
| Max share from Heat Pumps in other industries processes | 0.1 | 0.4 | SU_HP_IOIHTH |
| Min electricity share in other industries other processes | 0.03 | 0.03 | SL_IOI_OTH |
| Max biomass share in other industries other processes | 0.35 | 0.8 | SU_IOI_OTH_BIO |
| Max coal share in other industries other processes | 0 | 0 | AFSU_IOIOTH_INDCOA_II |
| Max lignite share in other industries other processes | 0 | 0 | AFSU_IOIOTH_INDCOL |
| Max coke share in other industries other processes | 0 | 0 | AFSU_IOIOTH_INDCOK_II |
| Max electricity share in other industries process heat  | 0.05 | 0.4 | SU_IOI_PRC_ELC |
| Max biomass share in other industries process heat | 0.2 | 0.8 | SU_IOI_PRC_BIO |
| Max coal share in other industries process heat | 0.005 | 0.005 | AFSU_IOIPRC_INDCOA_II |
| Max coke share in other industries process heat | 0.03 | 0.03 | AFSU_IOIPRC_INDCOK_II |
| Max electricity-processes share in other industries steam | 0.05 | 0.4 | AFSU_IOISTM_INDELC |
| Max coal-processes share in other industries steam | 0 | 0 | AFSU_IOISTM_INDCOA_II |
| Max coke-processes share in other industries steam | 0 | 0 | AFSU_IOISTM_INDCOK_II |
| Max biomass-processes share in other industries steam | 0.15 | 0.8 | AFSU_IOISTM_INDBIO |
| Max share of other industries – space heat – from heat exchange | 0.1 | 0.8 | SU_EX_IOIHSP |
| Max share of other industries – space heat – from air heat pump | 0.8 | 0.8 | SU_HP_AHT_IOIHSP |
| Max share of other industries – space heat – from bio boiler | 0.8 | 0.8 | SU_Bio_Boiler_IOIHSP |
| Max share of other industries – space heat – from ground heat pump | 0.8 | 0.8 | SU_HP_GEO_IOIHSP |

**Electric Cooker Phase-out Constraints**

| Description | 2025 | 2030 | UC Code |
|---|---|---|---|
| Min share of current electric cookers on cooking (IFT sector) | 0.9 | 0.8 | SL_IFTCOOL_BY |
| Min share of current electric cookers on cooking (IOI sector) | 0.9 | 0.8 | SL_IOIMCHL_BY |

### 5.3 Residential Sector Constraints (RSD)

#### 5.3.1 General Energy Mix Constraints in Residential sector (flow)

| UC Description | 2020 | 2030 | 2035 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Maximum share of biomass energy | 0.35 | 0.4 | 0.5 |  | SU_RSD_BIO |
| Maximum share of geothermal energy | 0.001 | 0.001 | 0.001 | | SU_RSD_GEO |
| Maximum share of solar energy | 0.005 | 0.07 | | 0.2 | SU_RSD_SOL |
| Maximum share of natural gas | 0.4 | 0.47 |  | 0.6 | SU_RSD_GAS |
| Maximum share of oil | 0.01 | 0.007 |  | 0 | SU_RSD_OIL |
| Maximum share of electricity |  | 0.57 | | 1 | SU_RSD_ELC |
| Maximum share of LPG | 0.001 | 0.001 |  | 0.001 | SU_RSD_LPG |
| Minimum share of coal | 0.05 | 0.03 |  | 0 | SL_COH_RSDCOA |
| Minimum share of gas | 0.25 | 0.17 |  | 0 | SL_RSD_GAS |
| Minimum share of electricity |  | 0.20 | 0.25 |  | SL_RSD_ELC |

#### 5.3.2 Heating and Hot Water Constraints in Residential sector (flow)

| UC Description | 2020 | 2025 | 2030 | 2035 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Maximum amount of coal in residential heating [PJ] | 21.5 |  | 14.67 |  | 1 | AU_COL_RH |
| Maximum amount of coal in multi-apartments [PJ] | 1 |  |  |  | 0 | AU_COL_RHM |
| Maximum share of biomass in water heating | 0.05 |  | 0.07 |  | 0.1 | SU_RW_BIO |
| Minimum share of electricity in heating (including heat pumps) |  |  | 0.06 | | 0.1 | SL_ELC_RH |
| Minimum share of electricity boiler in water heating |  | 0.18 | 0.18 | 0.17 | 0.14 | SL_ELC_RW_RESIST |
| Minimum share of electricity in water heating (including heat pumps)|  |  | 0.25 | 0.25 | 0.25 | SL_ELC_RW |
| Minimum share of hot water on derived heat in RSD | 0.37 |  | 0.37 | 0.37 |  | SL_RW_HET |
| Minimum share of derived heat in RSD final energy consumption | 0.14 |  | 0.12 |  | 0.08 | SL_RSD_HET |
| Max amount of fireplace/biomass stove in water heating | 0 |  | 0.5 | 0.5 | 0.5 | AU_BIO101_RH |
| Minimum share of base year technologies in heating | 0.85 |  | 0.5 |  | 0 | SL_BY_RH |
| Min share of biomass in family houses heating delivery | 0.6 | 0.6 |  | 0.525 |  | SL_RHH_BIO |

**Gas Heat Pump Constraint**

| UC Description | 2021 | 2030 | 2035 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Max share of RSD ambient heat for RSD gas heat pumps | 0.1 | 0.1 | 0.1 | 0.1 | SU_RSDAHT_GAS |


#### 5.3.3 Water Heating Constraints by Dwelling Type (flow)


| UC Description | 2021 | 2050 | UC Code |
| :--- | :--- | :--- | :--- |
| Min share of electricity in water heating - Multi-family (ME) | 0.135 | 0.165 | AFSL_RWME_RSDELC |
| Min share of electricity in water heating - Detached houses (RE) | 0.18 | 0.203 | AFSL_RWRE_RSDELC |
| Min share of electricity in water heating - Semi-detached (DE) | 0.18 | 0.203 | AFSL_RWDE_RSDELC |


#### 5.3.4 New Capacity Investment Constraints in Residential heating (ncap)

| UC Description | 2025 | 2030 | 2035 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Max share of biomass in new heating capacity | 0.58 | 0.58 | 0.58 | 0.58 | 0.58 | SU_NCAP_RH_BIO |
| Max share of coal in new heating capacity | 0.25 | 0.15 | 0.1 | | 0.01 | SU_NCAP_RH_COL |
| Max share of hard coal in new heating capacity | 0.03 | 0.02 | 0.02 | 0 | 0 | SU_NCAP_RH_COH |
| Max share of electricity in new heating capacity | 0.5 | 0.7 | 0.8 | 0.9 | 0.95 | SU_NCAP_RH_ELC |


#### 5.3.5 Absolute Energy Consumption Limits in Residential sector (flow)

| UC Description | 2025 | 2030 | 2035 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Maximum level of residential gas used [PJ] | 90 | 90 | 90 | 90 | 90 | AU_RSD_GAS |
| Minimum solar energy used [PJ] | 0.65 |  |  |  | 1 | AL_RSD_SOL |
| Maximum amount of ambient heat used [PJ] | 25 | 50 | 75 | 100 | 150 | AU_RSD_AHT |

#### 5.3.6 Cooking Constraints by Fuel Type (flow)

| UC Description | 2021 | 2050 | UC Code |
| :--- | :--- | :--- | :--- |
| Min share of electricity-processes producing cooking |  0.58 | 0.58 | SL_ELC_COK |
| Max share of gas-processes producing cooking | 0.51 | 0.48 | AFSU_RCOK_RSDGAS |


#### 5.3.7 Lighting Technology Constraints in Residential sector (flow)

| UC Description | 2025 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- |
| Max share of diode lighting | | 0.25 | 0.75 | SU_RLIG_5 |
| Max share of fluorescent lighting | 0.4 | 0.5 | 0.75 | SU_RLIG_4 |


### 5.4 Tertiary Sector Constraints (UC_COM)

#### 5.4.1 Overall Energy Mix Constraints in Tertiary sector (flow)

| UC Description | 2020 | 2040 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- |
| Maximum share of biomass | 0.035 | 0.1 | 0.1 | SU_COM_BIO |
| Maximum share of biomethan (pure) | 0.005 | 0.01 | 0.02 | SU_COM_BGS |
| Maximum share of geothermal | 0.001 | 0.01 | 0.05 | SU_COM_GEO |
| Maximum share of solar | 0.001 | 0.04 | 0.05 | SU_COM_SOL |
| Maximum share of natural gas (can be blended with biomethan or hydrogen) | 0.39 | 0.56 | 0.56 | SU_COM_GAS |
| Minimum share of natural gas | 0.35 | 0 | 0 | SL_COM_GAS |
| Maximum share of oil | 0.002 | 0.1 | 0.1 | SU_COM_Oil |
| Minimum share of oil | 0.002 | 0 | 0 | SL_COM_Oil |
| Maximum share of electricity | 0.45 | 1 | 1 | SU_COM_ELC |
| Minimum share of electricity | 0.4 | 0.3 | 0.22 | SL_COM_ELC |
| Minimum share of high/low temp district heat | 0.12 | 0.1 | 0.1 | SL_COM_HET |
| Minimum share of renewables (biomass, geothermal, solar) | 0.02 | 0.03 | 0.03 | SL_COM_REN |

#### 5.4.2 Lighting Technology Constraints in Tertiary sector (flow)

The constraints outlined below set the maximum share of different lighting technologies permitted within the tertiary sector, thereby guiding the transition towards more efficient lighting.

| UC Description | 2020 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- |
| Maximum share of halogen lighting | 0.22 | | 1 | SU_CLIG_2_3 |
| Maximum share of fluorescent lighting | 0.73 | | 1 | SU_CLIG_4 |
| Maximum share of diode lighting | | 0.05 | 0.75 | SU_CLIG_5 |
| Maximum share of fluorescent public lighting | | 0.05 | 0.75 | SU_CPLI_4 |
| Maximum share of fluorescent light for commercial lighting | 0.73 | 0.73 | 0.73 | SU_COM_CLIG 

#### 5.4.3 Heating Application Constraints Tertiary sector (flow)

| UC Description | 2020 | 2040 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- |
| Maximum share of biomass for large space heating | 0.1 | 0.7 | 0.7 | SU_CHLE_BIO |
| Maximum share of biomass for small water heating | 0.1 | 0.1 | 0.1 | SU_CWSE_BIO |
| Minimum share of heat for large space heating | 0.28 | 0.2 | 0.1 | SL_CHLE_Heat |
| Minimum share of heat for small space heating | 0.28 | 0.2 | 0.2 | SL_CHSE_Heat |
| Maximum share of geothermal energy | 0.01 | 0.01 | 0.01 | SU_CH_GEO |
| Maximum share of heat for large space heating | 0.33 | 0.35 | 0.4 | SU_CHLE_Heat |
| Maximum share of heat for small space heating | 0.33 | 0.35 | 0.4 | SU_CHSE_Heat |

#### 5.4.4 Commercial Cooking Constraints Tertiary sector (flow)

| UC Description | 2020 | 2050 | UC Code |
| :--- | :--- | :--- | :--- |
| Minimum share of electricity for commercial cooking | 0.59 | 0.47 | AFSL_CCOK_COMELC |
| Minimum share of gas for commercial cooking | 0.41 | 0 | AFSL_CCOK_COMGAS |
| Minimum share of current electric cookers | 0.95 | 0.8 | SL_CCOK_ELC_BY |

#### 5.4.5 Energy Efficiency Equipment (UC_Refrigeration)

| Constraint Description | 2020 | 2050 | UC Code |
| :--- | :--- | :--- | :--- |
| Maximum share of energy-efficient commercial refrigerators | 0.1 | 1 | SU_CREF_SAVE |
| Maximum share of energy-efficient residential refrigerators | 0.1 | 1 | SU_RREF_SAVE |

### 5.5 Transport Sector Constraints (TRA)
This section details constraints for the transport sector, organized by mode and vehicle type.

#### 5.5.1 General Transport System Constraints

| UC Description | Unit | 2025 | 2027 | 2029 | 2030 | 2035 | 2040 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Maximum number of personal vehicles | thousands | 6891 | 6941 | 7041 | 7141 | 7441 | 7741 | 8341 | AU_cap_TC |
| Maximum number of LPG-fueled freight vehicles | thousands | 1.05 | | | 1.05 | | | 1.05 | AU_LPG_TFR |
| Min consumption of gasoline (excl. fuel tourism) | PJ | 40 | | | 30 | | | | AL_GSL_T |
| Max consumption of gasoline (excl. fuel tourism) | PJ | 75 | | | 75 | | | | AU_GSL_T |
| Minimum number of large vehicles | thousands | | | | | | 450 | | AL_Cap_TC_lux |
| Maximum number of hydrogen personal vehicles | thousands | 0.01 | | | 6 | 30 | 150 | 600 | AU_cap_TC_HYD |

#### 5.5.2 Personal Vehicle Market Segmentation

**Vehicle Size Class Constraints**
The model regulates market penetration by vehicle size (small, medium, large) to ensure realistic fleet composition:

$\sum pkm_{i,t} \leq share_{i,t} \sum_{j=1}^{3} pkm_{j,t}$

where i,j = 1-small, 2-medium, 3-large; t = year

| Class | UC Description | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| small | Maximum share of small vehicles | 0.33 | 0.35 | | 0.35 | | 0.35 | SU_TC_mal |
| medium | Maximum share of medium vehicles | 0.6 | 0.6 | | 0.6 | | 0.6 | SU_TC_str |
| large | Minimum share of large vehicles | 0.14 | 0.15 | 0.16 | 0.17 | 0.18 | 0.19 | SL_TC_lux |

**Electric Vehicle Size Constraints (cap)**

| Class | Description | 2025 | 2030 | 2040 | 2050 | UC Code |
|---|---|---|---|---|---|---|
| Small | Max share of small EVs | 0.38 | 0.36 | 0.36 | 0.36 | SU_TC-ELC_mal |
| Medium | Max share of medium EVs | 0.4 | 0.6 | 0.6 | 0.6 | SU_TC-ELC_str |
| Large | Min share of large EVs | 0.29 | 0.24 | 0.19 | 0.15 | SL_TC-ELC_lux |

**Fuel Mix Constraints (flow)**

| Fuel | Description | 2025 | 2030 | 2040 | 2050 | UC Code |
|---|---|---|---|---|---|---|
| Diesel | Min renewable share in diesel | 0.06 | 0.06 | 0.06 | 0.06 | SL_bio_DST |
| Gasoline | Min renewable share in gasoline | 0.04 | 0.04 | 0.04 | 0.04 | SL_bio_GSL |
| Gas/Diesel | Min gasoline vs diesel (transport) | 0.3 | 0.25 | 0.1 | 0 | SL_GSL_DST |
| Gas/Diesel | Min gasoline vs diesel (cars only) | 0.7 | 0.5 | 0.1 | 0.1 | SL_GSL_DST_CAR |

**Powertrain Type Vehicle Constraints (cap)**

| Vehicle Type | UC Description | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| hybrid | Max share of diesel hybrid cars vs gasoline hybrid cars | 0.2 | 0.3 | | 0.5 | | 0.5 | SL_Cap_Hyb_GSL |
| plug-in hybrid | Max diesel plug-in hybrid cars vs gasoline plug-in hybrid cars | 0.2 | 0.3 | | 0.5 | | 0.5 | SL_Cap_PinHyb_GSL |
| diesel/gasoline | Max diesel vs gasoline personal cars capacity | 0.75 | 0.8 | | 1.5 | | 2 | SL_CAP_GST_DTS |

#### 5.5.3 Bus Transport

**Bus Fleet Capacity Limits**

| Bus Type | UC Description | Unit | 2025 | 2027 | 2029 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| city buses | Maximum number | thousands | 12.6 | 12.7 | 12.9 | 13.0 | 15.6 | AU_cap_TBU |
| intercity coaches | Maximum number | thousands | 18.5 | 18.7 | 18.8 | 19.0 | 22.8 | AU_cap_TBI |

**Bus Technology and Fuel Mix**

| Bus Type | UC Description | 2020 | 2030 | 2040 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| intercity |Maximum share of second-hand intercity buses in the total transport performance (pkm) of intercity buses | 0.1 | 0.1 | 0.1 | 0.1 | SU_TBI_20* |
| city | Minimum share of diesel city buses on personal kilometers delivered by city buses | 0.7 | 0.3 | 0 | 0 | SL_TBU_DST |

#### 5.5.4 Freight Transport

**Freight Electric Vehicle Capacity Limits**

| Description | Unit | 2030 | 2040 | 2050 | UC Code |
|---|---|---|---|---|---|
| Max number of hybrid freight | thousands | 150 | 175 | 200 | AU_cap_TFREH |
| Max number of battery-electric freight | thousands | 90 | - | 100 | AU_cap_TFREL |

**Freight Market Share Constraints (flow)** 

| UC Description | 2020 | 2050 | UC Code |
| :--- | :--- | :--- | :--- |
| Maximum share of heavy freight transport in the total freight transport (tkm) | 0.7 | 0.75 | SU_TF_TFREH |
| Minimum share of heavy freight transport in the total freight transport (tkm)| 0.6 | 0.6 | SL_TF_TFREH |
| Minimum share of light commercial freight in the total freight transport (tkm)| 0.14 | 0.17 | SL_TF_TFRLC |
| Maximum share of light commercial freight in the total freight transport (tkm)| 0.25 | 0.25 | SU_TF_TFRLC |

#### 5.5.5 National Clean Mobility Action Plan

These constraints represent maximum and minimum deployment targets for alternative fuel vehicles according to the National Clean Mobility Action Plan (2020):

**Upper Bounds - Maximum deployment (UC_CZ_TRA_CAP_NAP_high)**

| Vehicle Type | Unit (maximum) | 2025 | 2030 | 2035 | 2040 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| LPG personal vehicles | thousand | 110 | 113 | | 113 | 113 | AU_LPG_OA_N1 |
| CNG personal vehicles | thousand | 29 | 40 | | 30 | 20 | AU_CNG_OA |
| CNG buses | thousand | 3 | 3 | | 3 | 3 | AU_CNG_BUS |
| CNG light duty vehicles | thousand | | 5 | | 4 | 4 | AU_CNG_LDV |
| CNG heavy duty vehicles | thousand | | 1.2 | | 1.5 | 2 | AU_CNG_HDV |
| LNG heavy duty vehicles | thousand | 4 | 7 | | 9 | 25 | AU_LNG_HDV |
| BEV personal vehicles | thousand | 80 | 400 | 1000 | 2000 | 7000 | AU_BEV_OA_N1 |
| BEV buses | thousand | | 1.2 | 2 | 6 | 20 | AU_BEV_BUS |
| BEV light duty vehicles | thousand | 22 | 30 | 60 | 100 | 1000 | AU_BEV_LDV |
| Personal plug-in hybrids | thousand | 64 | 187 | | 1500 | 3000 | AU_PLUG_OA |
| Plug-in hybrids with diesel | thousand | | 10 | 20 | | | AU_PLUG_OA_DSP |
| Hybrids | thousand | 365 | 500 | | 1000 | 2000 | AU_HYB_OA |
| Light-duty freight plug-in hybrids | thousand | 22 | 30 | | 45 | 60 | AU_PLUG_N1_N2 |
| Light-duty freight hybrids | thousand | 22 | 30 | | 45 | 60 | AU_HYB_N1_N2 |
| Hydrogen vehicles | thousand | 6 | 50 | 100 | 500 | 3000 | AU_HYD_OA |
| Hydrogen buses | thousand | 1 | 1 | | 2 | 10 | AU_HYD_BUS |
| Hydrogen light-duty vehicles | thousand | 0 | 6 | | 15 | 30 | AU_HYD_LDV |
| Hydrogen heavy-duty vehicles | thousand | | 0 | | 15 | 60 | AU_HYD_HDV |

**Lower Bounds - Mainimum deployment (UC_CZ_TRA_CAP_NAP_lo)**

| Vehicle Type | Unit (minimum)| 2025 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| LPG personal vehicles | thousand | | 27 | 0 | AL_LPG_OA_N1 |
| CNG personal vehicles | thousand | | 18 | 0 | AL_CNG_OA |
| CNG buses | thousand | | 2 | 0 | AL_CNG_BUS |
| CNG light-duty vehicles | thousand | | 2 | 0 | AL_CNG_LDV |
| CNG heavy-duty vehicles | thousand | | 1 | 0 | AL_CNG_HDV |
| LNG heavy-duty vehicles | thousand | | 4 | 0 | AL_LNG_HDV |
| BEV personal vehicles | thousand | 35 | 60 | 60 | AL_BEV_OA_N1 |
| BEV buses | thousand | | 0.3 | 0.3 | AL_BEV_BUS |
| BEV light-duty vehicles | thousand | | 3 | 3 | AL_BEV_N1_N2 |
| Personal plug-in hybrids | thousand | 14 | 30 | 0 | AL_PLUG_OA |
| Light-duty plug-in hybrids | thousand | | 3 | 3 | AL_PLUG_N1_N2 |
| Light-duty hybrids | thousand | | 3 | 3 | AL_HYB_N1_N2 |
| Personal hydrogen vehicles | thousand | | 5 | 5 | AL_HYD_OA |
| Hydrogen buses | thousand | | 0.1 | 0.1 | AL_HYD_BUS |
| Hydrogen light-duty vehicles | thousand | | 0.5 | 0.5 | AL_HYD_LDV |

#### 5.5.6 Clean Mobility in Public Transport  (UC_CZ_TRA_cista_mobilita)

These constraints enforce the adoption of clean mobility options in public transportation by setting minimum shares for new alternative fuel buses (electric, CNG, hydrogen).

| UC Description | 2025 | 2035 | 2045 | UC Code |
| :--- | :--- | :--- | :--- | :--- |
| Min share of alternative (electric, CNG, hydrogen) city buses on number of all new city buses (ncap) | 0.6 | 0.6 | 0.6 | SL_Ncap_TBU_alt |
| Min share of electric city buses on number ofall new city buses (ncap) | 0.3 | 0.3 | 0.3 | SL_Ncap_TBU_ELC |
| Min share of alternative intercity buses on number of all new intercity buses (ncap) | 0.2 | 0.2 | 0.2 | SL_Ncap_TBI_alt |

#### 5.5.7 Vehicle Age Structure and Market Constraints (UC_CZ_TRA_vnt)

**New vs. Second-Hand Vehicle Shares on new registrations**

These constraints regulate the proportion of new versus second-hand vehicles entering the fleet:

| Vehicle Type | UC Description | 2025 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Personal cars | Minimum share of new on newly registered personal vehicles | 0.4 | 0.4 | 0.4 | SL_Ncap_new_car |
| Personal cars | Minimum share of new diesel personal vehicles on newly regsitered diesel personal vehicles | 0.41 | 0.41 | 0.4 | SU_Ncap_DST_old_car |

**Technology-Specific Age Constraints**

| Vehicle Type | UC Description | Unit | 2025 | 2030 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Electric cars | Max number of newly registered pre-2023 electric cars  | thousand | 40 | - | AU_cap_NCAP_oldBEV_2025 |
| Electric cars | Max number of newly registered pre-2028 electric cars | thousand | - | 100 | AU_cap_NCAP_oldBEV_2030 |

**Motorbike Constraints**

| UC Description | 2020 | 2050 | UC Code |
| :--- | :--- | :--- | :--- |
| Minimum share of new on newly registered motorbikes | 0.5 | 0.5 | SL_new_mo_in_mot |

**Freight Vehicle Age Constraints on new registrations**

| Vehicle Type | UC Description | 2025 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- |
| Heavy freight | Maximum share of second-hand vehicles on newly registered vehicles | 0.2 | 0.2 | SU_Ncap_old_TFREH |
| Light commercial | Maximum share of second-hand vehicles on newly registered vehicles | 0.47 | 0.47 | SU_Ncap_old_TFREL |
| N1 light commercial | Maximum share of second-hand vehicles on newly registered vehicles | 0.36 | 0.36 | SU_Ncap_old_TFRLC |

**Bus Age Constraints**

| Bus Type | UC Description | 2025 | 2030 | 2035 | 2040 | 2045 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Inter-city buses | Maximum share of second-hand vehicles on newly registered vehicles | 0.27 | 0.27 | 0.2 | 0.15 | 0.1 | 0.05 | SU_Ncap_old_TBI |
| City buses | Maximum share of second-hand vehicles on newly registered vehicles | 0.1 | 0.1 | 0.1 | 0.1 | 0.05 | 0 | SU_Ncap_old_TBU |

#### 5.5.8 Rail Transport Constraints


| UC Description | 2025 | 2027 | 2029 | 2030 | 2050 | UC Code |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Maximum number of freight trains | 700 | 710 | 720 | 725 | 825 | AU_cap_TTF |
| Maximum number of personal trains | 1920 | 2020 | 2120 | 2220 | 2320 | AU_cap_TTP |
| Minimum number of diesel trains | 1000 | | 800 | | | AU_cap_TT |

---
##  Acknowledgment
This TIMES-CZ v03.0 Documentation was conducted within the project SS04030013: SEEPIA – Centre for socio-economic research on environmental policy impacts. The TIMES-CZ model has been developing since 2014 within the research projects listed below. The support is gratefully acknowledged. 
- TA CR TD020299: Analysis of environmental impacts of regulation and prediction of developments in the energy sector using the pan-European dynamic model TIMES
- TA CR TD03000319: System support of modeling tools for creation and analysis of energy and environmental policies
- TA CR TK01010119: RegSim – Integrated models for analysis of regulatory impacts and simulations of long-term scenarios of energy sector development
- GA CR 18-26714S: Developing Hybrid Modelling Framework for Regulatory Impact Assessment:
CGE-TIMES
- GA CR EXPRO 19-26812X: F3EM – Frontiers in Energy Efficiency Economics and Modelling
- TA CR TL02000440: LESY-ADAPT – Modeling scenarios of sustainable forest composition …, impact on the energy sector, greenhouse gas emissions and overall acceptability of these scenarios
- TA CR TITSMZP713 OZE-Tran – Optimal use of renewable energy sources in transport
- TA CR SS03010156: NOx2030 – Prediction of emission savings from road transport by 2030 achieved through the application of selected tax and fee instruments 
- TA CR SS02030031: ARAMIS – Integrated system for research, assessment and air quality control
- TA CR TC7 SS04030013: SEEPIA – Centre for socio-economic research on environmental policy impacts

---
## References

Capros, P., Paroussos, L., Fragkos, P., Tsani, S., Boitier, B., Wagner, F., Busch, S., Resch, G., Blesl, M., & Bollen, J. (2014). Description of models and scenarios used to assess European decarbonisation pathways. *Energy Strategy Reviews*, 2(3), 220–230. https://doi.org/10.1016/j.esr.2013.12.008

Cienciala, E., & Melichar, J. (2024). Forest carbon stock development following extreme drought-induced dieback of coniferous stands in Central Europe: A CBM-CFS3 model application. *Carbon Balance and Management*, 19(1), 1. https://doi.org/10.1186/s13021-023-00246-w

Loulou, R., Lehtilä, A., Kanudia, A., Remme, U., & Goldstein, G. (2020). Documentation for the TIMES Model Part II [Documentation]. Energy Technology Systems Analysis Programme. https://iea-etsap.org/docs/Documentation_for_the_TIMES_Model-PartII.pdf

Loulou, R., Remme, U., Kanudia, A., Lehtilä, A., & Goldstein, G. (2005). Documentation for the TIMES Model PART I. Energy Technology Systems Analysis Programme. http://iea-etsap.org/index.php/documentation

Ministry of Transport of the Czech Republic. (2021). Transport Policy of the Czech Republic for the period of 2021—2027, with an outlook until 2050. https://md.gov.cz/Dokumenty/Strategie/Dopravni-politika-a-MFDI/Dopravni-politika-CR-pro-obdobi-2014-2020-s-vyhled

Nijs, W., & Ruiz, P. (2019). 01_JRC-EU-TIMES Full model [Computer software]. European Commission, Joint Research Centre (JRC). http://data.europa.eu/89h/8141a398-41a8-42fa-81a4-5b825a51761b

Rečka, L., Ščasný, M., & Laxton, D. T. (2023). The Role of Biomass in Decarbonisation Efforts: Spatially Enriched Energy System Optimisation Modelling. *Energies*, 16(21), Article 21. https://doi.org/10.3390/en16217380

Rečka, L., Ščasný, M., & Máca, V. (2022). Predikce vývoje vozového parku, spotřeby energií a emisí z dopravy. https://mpo.gov.cz/assets/cz/energetika/vyzkum-a-vyvoj-v-energetice/resene-dokoncene-projekty-a-jejich-vystupy/projekty-podporene-v-ramci-1-verejne-souteze-programu-theta/2023/2/Predikce-vyvoje-vozoveho-parku--spotreby-energii-a-emisi-z-dopravy-_vystup-V2_.pdf#page=27.31

Roland Berger. (2016). Integrated Fuels and Vehicles Integrated Fuels and Vehicles Roadmap to 2030+. Roland Berger. https://www.rolandberger.com/en/Media/Integrated-Fuels-and-Vehicles-Integrated-Fuels-and-Vehicles-Roadmap-to-2030.html

Steinbach, J., & Staniaszek, D. (2015). Discount rates in energy system analysis Discussion Paper. https://www.bpie.eu/wp-content/uploads/2015/10/Discount_rates_in_energy_system-discussion_paper_2015_ISI_BPIE.pdf


Ščasný, M., Rečka, L., Máca, V., Opatrný, M., Kopečná, V., Gutzianas, I. (2025), Analýza distribuce cenového signálu skleníkových plynů. Zpráva projektu SEEPIA, Univerzita Karlova, Centrum pro otázky životního prostředí