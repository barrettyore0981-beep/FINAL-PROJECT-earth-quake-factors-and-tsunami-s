# FINAL-PROJECT-earth-quake-factors-and-tsunami's

## Dataset Description

### Overview
This dataset contains **782 earthquake records**. The Global Earthquake-Tsunami Risk Assessment Dataset is a comprehensive, machine learning-ready dataset containing seismic characteristics and tsunami potential indicators for 782 significant earthquakes recorded globally from 2001 to 2022. This dataset is specifically designed for tsunami risk prediction, earthquake analysis, and seismic hazard assessment applications.

### Feature Descriptions

| Feature     | Type     | Description                                  | Range/Values                 | Tsunami Relevance                         |
|------------|----------|----------------------------------------------|-----------------------------|-------------------------------------------|
| magnitude  | Float    | Earthquake magnitude (Richter scale)          | 6.5 – 9.1                   | **High** – primary tsunami predictor      |
| cdi        | Integer  | Community Decimal Intensity (felt intensity)  | 0 – 9                       | Medium – population impact measure        |
| mmi        | Integer  | Modified Mercalli Intensity (instrumental)    | 1 – 9                       | Medium – structural damage indicator      |
| sig        | Integer  | Event significance score                      | 650 – 2910                  | **High** – overall hazard assessment      |
| nst        | Integer  | Number of seismic monitoring stations         | 0 – 934                     | Low – data quality indicator              |
| dmin       | Float    | Distance to nearest seismic station (degrees) | 0.0 – 17.7                  | Low – location precision                  |
| gap        | Float    | Azimuthal gap between stations (degrees)      | 0.0 – 239.0                 | Low – location reliability                |
| depth      | Float    | Earthquake focal depth (km)                   | 2.7 – 670.8                 | **High** – shallow = higher tsunami risk  |
| latitude   | Float    | Epicenter latitude (WGS84)                    | −61.85° to 71.63°           | **High** – ocean proximity indicator      |
| longitude  | Float    | Epicenter longitude (WGS84)                   | −179.97° to 179.66°         | **High** – ocean proximity indicator      |
| Year       | Integer  | Year of occurrence                           | 2001 – 2022                 | Medium – temporal patterns                |
| Month      | Integer  | Month of occurrence                          | 1 – 12                      | Low – seasonal analysis                   |
| tsunami    | Binary   | **Tsunami potential (TARGET)**               | 0, 1                        | **TARGET VARIABLE**                       |

### Key Dataset Characteristics
- **Total Records**: 782 earthquakes
- **Temporal Coverage**: 21 years (2001-2022)
- **Magnitude Threshold**: All earthquakes ≥6.5 magnitude
- **Geographic Scope**: Global coverage
- **Tsunami Events**: 304 (38.9%)
- **Non-Tsunami Events**: 478 (61.1%)
- **Data Quality**: Pre-cleaned with verified seismic parameters

## Data Preparation and Methodology

### Initial Dataset Condition
The dataset was obtained in a pre-cleaned state, containing earthquake records with basic seismic parameters. However, significant manipulation and feature engineering were required to enable meaningful analysis of tsunami generation patterns.

### Feature Engineering

#### Ocean Distance Calculation
A critical variable for tsunami analysis—distance from the ocean—was not present in the original dataset. This feature was calculated using the latitude and longitude coordinates of each earthquake epicenter. The `ocean_distance_km` column was created by computing the shortest distance from each earthquake location to the nearest ocean coastline, providing a quantitative measure of how proximity to water bodies influences tsunami generation potential.

#### Binning Strategy for Categorical Analysis
To better understand the relationship between continuous variables and tsunami occurrence, three key variables were transformed into categorical bins:

**Magnitude Bins:**
- 6.5-7.0
- 7.0-7.5
- 7.5-8.0
- 8.0-8.5
- 8.5-9.1

These equal-width bins allow for comparison of tsunami rates across energy levels while accounting for the logarithmic nature of the magnitude scale.

**Distance Bins:**
Created using quantile-based binning to ensure roughly equal sample sizes:
- Very Close
- Close
- Medium
- Far
- Very Far

This approach addresses the highly skewed distribution of ocean distances (min: 2.6 km, mean: 294.8 km, max: 2508.4 km).

**Depth Bins:**
Created using quantile-based binning:
- Very Shallow
- Shallow
- Medium
- Deep
- Very Deep

This binning strategy facilitates analysis of how depth influences tsunami generation while maintaining adequate sample sizes in each category for statistical comparison.

The binning approach enabled stratified analysis to control for confounding variables and examine the independent effects of each factor on tsunami occurrence.

### Data Type Optimization

#### Boolean Conversion
After initial correlation analysis revealed no strong linear relationships with the numeric tsunami variable, the `tsunami` column was converted from a binary numeric variable (0/1) to a boolean type (False/True). This conversion improved:
- Human readability in data exploration
- Clarity in filtering operations
- Semantic accuracy (tsunami occurrence is inherently a categorical, not quantitative, variable)

### Filtered Dataset Creation

#### Tsunami-Only Subset
A filtered version of the dataset containing only tsunami-generating earthquakes (tsunami=True, n=304) was created to facilitate targeted analysis. This subset enabled:

**Variable Grouping Analysis:**
- Examination of which combinations of magnitude, depth, and distance are present when tsunamis do occur
- Identification of typical conditions for successful tsunami generation
- Calculation of central tendencies (median magnitudes) within each categorical bin for tsunami events

**Factor Presence Investigation:**
- Determination of whether certain conditions are universally present in tsunami events
- Analysis of the distribution of tsunami-generating earthquakes across binned categories
- Assessment of whether tsunamis cluster in predictable parameter ranges

**Simplified Comparative Analysis:**
By isolating tsunami events, comparisons could be made more efficiently between different groupings (e.g., shallow vs. deep tsunami earthquakes, near vs. far tsunami earthquakes) without the need for repeated filtering operations during exploratory analysis.

This methodological approach—combining feature engineering, categorical binning, data type optimization, and strategic subsetting—provided the analytical framework necessary to uncover the complex, multivariate relationships between earthquake characteristics and tsunami generation.
# Earthquake and Tsunami Analysis: Key Findings

## Dataset Overview
- **Total earthquakes analyzed**: 782
- **Tsunami-generating earthquakes**: 304 (38.9%)
- **Non-tsunami earthquakes**: 478 (61.1%)
- **Magnitude range**: 6.5 - 9.1 (mean: 6.94)
- **Depth range**: 2.7 - 670.8 km (mean: 75.9 km)
- **Distance from ocean range**: 2.6 - 2508.4 km (mean: 294.8 km)
- **Time period**: 2001 - 2022

## Dataset Limitations
- **Magnitude threshold**: Only earthquakes ≥6.5 magnitude included
- **Missing critical variable**: Fault mechanism/type data not available


## Primary Findings

### 1. No Single Variable Shows Direct Correlation
Correlation analysis reveals extremely weak relationships between individual variables and tsunami occurrence:
- **Magnitude vs Tsunami**: r = -0.005 (essentially zero)
- **Depth vs Tsunami**: r = 0.057 (very weak)
- **Distance vs Tsunami**: r = 0.058 (very weak)

No individual variable demonstrates a simple, direct correlation with tsunami occurrence. Instead, tsunami generation requires a **combination of conditions** working together, with an additional unmeasured factor playing a critical role.

### 2. Necessary but Not Sufficient Conditions

While individual correlations are weak, tsunamis consistently occur under specific conditions. However, **the majority of earthquakes meeting all favorable criteria do not generate tsunamis**.

#### Distribution by Magnitude:
| Magnitude Range | Total Quakes | Tsunamis | Tsunami Rate |
|-----------------|--------------|----------|--------------|
| 6.5-7.0         | 548          | 215      | 39.2%        |
| 7.0-7.5         | 144          | 56       | 38.9%        |
| 7.5-8.0         | 67           | 25       | 37.3%        |
| 8.0-8.5         | 18           | 8        | 44.4%        |
| 8.5-9.1         | 5            | 0        | 0.0%         |

**Key observation**: Tsunami rates remain relatively constant (~37-44%) across magnitude ranges from 6.5-8.5, showing no clear increasing trend with magnitude. The 8.5-9.1 category shows 0% due to small sample size (only 5 earthquakes).

#### Distribution by Distance from Ocean:
| Distance Category | Total Quakes | Tsunamis | Tsunami Rate |
|-------------------|--------------|----------|--------------|
| Very Close        | 157          | 64       | 40.8%        |
| Close             | 156          | 51       | 32.7%        |
| Medium            | 156          | 53       | 33.9%        |
| Far               | 156          | 66       | 42.3%        |
| Very Far          | 157          | 70       | 44.6%        |

**Key observation**: A U-shaped pattern emerges—high tsunami rates at both very close (40.8%) and very far (44.6%) distances, with lower rates in the middle range (~33%). This counterintuitive pattern reveals important selection effects discussed below.

#### Distribution by Depth:
| Depth Category | Total Quakes | Tsunamis | Tsunami Rate |
|----------------|--------------|----------|--------------|
| Very Shallow   | 170          | 69       | 40.6%        |
| Shallow        | 145          | 58       | 40.0%        |
| Medium         | 166          | 43       | 25.9%        |
| Deep           | 145          | 66       | 45.5%        |
| Very Deep      | 156          | 68       | 43.6%        |

**Key observation**: Medium-depth earthquakes show the lowest tsunami rate (25.9%), while shallow and deep categories show similar rates (40-45%). This unexpected pattern also reflects selection effects in the data.

### 3. Magnitude Findings

**Across Distance Categories (Tsunami earthquakes only):**
Tsunami-generating earthquakes exhibit remarkably consistent median magnitudes across all distance categories:
- Very Close: 6.88
- Close: 6.96
- Medium: 6.96
- Far: 6.93
- Very Far: 6.96

This narrow range (6.88-6.96) demonstrates that **distance from ocean does not require substantially different magnitudes for tsunami generation**. All values cluster just below magnitude 7.0, approaching the commonly cited threshold from NOAA for tsunami warnings.

**Across Depth Categories (Tsunami earthquakes only):**
A modest but clear depth-magnitude relationship exists:
- Very Shallow: 6.81
- Shallow: 6.81
- Medium: 6.98
- Deep: 7.02
- Very Deep: 7.07

**Magnitude increase of 0.26 units from shallowest to deepest** (representing approximately 1.8× more seismic energy). This reflects the physical reality that deeper earthquakes must release more energy to achieve sufficient seafloor displacement for tsunami generation.

### 4. The Importance of All Four Variables

While no single variable shows direct correlation, the analysis reveals that **magnitude, depth, distance from ocean, and fault type all matter**:

#### Magnitude
Provides the energy necessary for significant seafloor displacement. The Richter/Moment Magnitude scale is logarithmic—each whole number increase represents 10× more ground motion and ~31.6× more energy released. Despite this, tsunami rates remain relatively stable across magnitude ranges, indicating magnitude alone is insufficient for prediction.

#### Depth
measured in km below surface
Influences the efficiency of energy transfer to the ocean surface. The depth-magnitude relationship among tsunami earthquakes (0.26 magnitude increase from shallow to deep) confirms that deeper earthquakes require more energy to generate tsunamis.

According to NOAA, earthquakes must occur less than 100 kilometers (62 miles) below Earth's surface because earthquakes deeper than this are unlikely to displace the ocean floor. Research from the University of Hawaii's School of Ocean and Earth Science and Technology (SOEST) provides the physical explanation: in subduction zones, the upper plate is thinner and less rigid than the underthrusting plate near the trench, and a concentrated near-trench or shallow rupture produces relatively weak ground shaking as recorded by seismometers, but the displaced water in the overlying deep ocean has enhanced energy and produces shorter tsunami waves that amplify at a high rate as they move toward the shore.

The key finding from SOEST research is that for a given earthquake magnitude, if the rupture extends to shallow depth in the less rigid part of the plate, the resulting tsunami is larger than if the rupture is deeper. This explains why shallow earthquakes more efficiently generate tsunamis:
- Less energy dissipation through overlying rock
- More direct seafloor displacement
- Shallow crust properties allow greater vertical movement and more efficient water displacement

However, the unexpected similarity in tsunami rates between shallow (40%) and deep (43-45%) categories in this dataset suggests that when deep earthquakes DO generate tsunamis, they tend to be the stronger ones (median magnitude 7.07 vs 6.81 for shallow) that successfully overcome the depth disadvantage through sheer energy.

#### Distance from Ocean
measured in km
The weak correlation (r = 0.058) and U-shaped distribution pattern reveal complex dynamics. Distance affects whether displaced water reaches inhabited coastlines and whether tsunamis are detected/reported, but shows minimal effect on the magnitude required for generation itself. The consistent magnitudes (6.88-6.96) across all distance bins confirm this.

#### Fault Type (Unmeasured—The Critical Missing Variable)
NOAA explains that an earthquake must be big enough and close enough to the ocean floor to cause the vertical movement of the ocean floor that typically sets a tsunami in motion. The critical insight is that earthquake fault mechanism determines the **direction of movement**:

- **Thrust/reverse faults** (vertical/upward-downward movement): Displace water vertically → tsunami possible
- **Strike-slip faults** (horizontal movement): Minimal water displacement → tsunami unlikely
- **Normal faults** (vertical/downward movement): Can displace water → sometimes generate tsunamis

Since magnitude measures only the intensity of ground shaking (total energy released), not the directionality of movement, two earthquakes of identical magnitude, depth, and location can have completely different tsunami outcomes based solely on fault mechanism. According to supplementary research from NOAA sources, the vertical displacement characteristic of thrust faults in subduction zones is what enables efficient water displacement and tsunami generation, while horizontal strike-slip movement—regardless of magnitude—does not produce the vertical seafloor motion necessary for significant tsunami generation.

### 5. Why Most "Perfect Condition" Earthquakes Don't Generate Tsunamis

Even within favorable condition categories, **60-67% of earthquakes do not generate tsunamis**:
- Very Close + Very Shallow earthquakes: ~60% non-tsunami
- High magnitude (7.0-8.5): ~56-63% non-tsunami

The discriminating factor appears to be **fault mechanism**—whether the earthquake produces vertical seafloor displacement. This explains why:
- Simple magnitude and location thresholds are insufficient for tsunami prediction
- Modern tsunami warning systems require rapid seismic analysis to determine fault type
- Ocean buoy networks detect actual water displacement rather than relying solely on earthquake characteristics

## Confounding and Selection Effects

The analysis revealed important confounding patterns that initially appear counterintuitive:

**U-Shaped Distance Pattern:** High tsunami rates at both very close (40.8%) AND very far (44.6%) distances, with lower rates at medium distances (33%). This is a **selection effect**—distant earthquakes that successfully generate detectable tsunamis are disproportionately large magnitude events. They don't generate tsunamis more easily at distance; rather, only the most powerful distant earthquakes overcome the disadvantage to produce observable tsunamis that get recorded in the dataset.

**Similar Depth Pattern:** Shallow (40%) and deep (43-45%) earthquakes show similar tsunami rates, while medium-depth shows the lowest (26%). Again, this reflects selection: deep earthquakes in this dataset that generate tsunamis are likely the stronger ones (median magnitude 7.07 vs 6.81 for shallow), having overcome the depth disadvantage through sheer energy—consistent with the SOEST findings that deeper ruptures require more energy to produce equivalent tsunamis.

**Stable Magnitude Rates:** The relatively constant ~38-44% tsunami rate across magnitude categories (6.5-8.5) is the strongest evidence that **magnitude alone cannot predict tsunami occurrence**. If magnitude were the primary factor, we would expect a clear increasing trend.


## Conclusions

This analysis of 782 major earthquakes (magnitude ≥6.5) demonstrates that tsunami generation is a **multivariate, physics-dependent phenomenon** that cannot be predicted from magnitude and location alone. 

### The Four Critical Variables:

1. **Magnitude** (energy available) - Shows near-zero correlation (r = -0.005) but remains necessary; most tsunami earthquakes cluster around magnitude 6.8-7.0

2. **Depth** (efficiency of energy transfer to water displacement) - Weak correlation (r = 0.057) but shows clear physical relationship; deeper tsunamis require ~0.26 higher magnitude. Research from SOEST demonstrates that shallow ruptures produce enhanced tsunami energy due to the less rigid properties of shallow plate materials and more efficient water displacement.

3. **Distance from ocean** (propagation and detection) - Weak correlation (r = 0.058); does not substantially affect magnitude requirements for generation

4. **Fault mechanism** (direction of displacement—vertical vs horizontal) - **CRITICAL but unmeasured in this dataset**; NOAA research confirms that vertical seafloor movement from thrust/reverse faults is what enables tsunami generation, while horizontal movement from strike-slip faults does not produce significant water displacement regardless of magnitude

### Key Insights:

**Only 38.9% of major earthquakes (≥6.5) generate tsunamis**, even though all possess sufficient energy. Within favorable condition categories, this rate holds relatively constant, indicating that the measured variables (magnitude, depth, distance) establish necessary preconditions but do not determine tsunami occurrence.

**Magnitude measures "how much the earth shakes," not "how much water moves."** Vertical seafloor displacement—determined by fault type—is what actually generates tsunamis. This explains why:
- Two magnitude 7.0 earthquakes at the same depth and location can have opposite outcomes
- Tsunami warning systems require sophisticated rapid seismic analysis of fault mechanisms
- Simple threshold-based alerts (e.g., "magnitude >7.0 + coastal") generate excessive false positives

The absence of fault type data represents a fundamental limitation of this dataset. However, the consistent finding that ~60% of earthquakes with favorable magnitude, depth, and location characteristics fail to generate tsunamis provides strong indirect evidence that **fault mechanism is the primary discriminating factor** between tsunami and non-tsunami earthquakes.

Research from NOAA and the University of Hawaii (SOEST) confirms the physical mechanisms underlying these patterns: vertical fault movement (thrust/reverse) is necessary for water displacement, and shallow ruptures transfer energy to water more efficiently than deep ruptures due to differences in crustal rigidity and the mechanics of wave generation. Understanding the complete physics of earthquake rupture—including the direction and magnitude of seafloor displacement—is essential for accurate tsunami prediction and hazard assessment.

## Sources
- NOAA - Tsunami Generation from Earthquakes: https://www.noaa.gov/jetstream/tsunamis/tsunami-generation-earthquakes
- NOAA - Science Behind Tsunamis: https://www.noaa.gov/explainers/science-behind-tsunamis
- University of Hawaii, School of Ocean and Earth Science and Technology (SOEST) - Earthquake Depth Impacts Potential Tsunami Threat: https://www.soest.hawaii.edu/soestwp/announce/news/earthquake-depth-impacts-potential-tsunami-threat/
- Data Set https://www.kaggle.com/datasets/ahmeduzaki/global-earthquake-tsunami-risk-assessment-dataset

#note:
do to time contraints AI helped alot with writing this report all findings are human ai also helped with some of the proggramming 
