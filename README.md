# Hedging-against-Trump

Analyzes how ASEAN states “hedged” between the U.S. and China during the Trump presidency (2017–2020). Tests whether Trump’s policies shifted trade dependencies, military spending, and diplomatic alignment in Southeast Asia.


## Motivation

Donald Trump’s unorthodox foreign policy—withdrawals from multilateral agreements and abrupt rhetoric—prompted allies to question U.S. commitment to the liberal order. Meanwhile, China’s global influence continous to rise. Southeast Asian (ASEAN) countries often pursue **“hedging”** strategies to balance relations with both powers.  

This project examines whether Trump’s term materially:

1. **(H1)** Strengthened China’s economic leverage over ASEAN economies (relative to the U.S.).  
2. **(H2)** Prompted ASEAN states with territorial disputes against China to boost military spending.  
3. **(H3)** Caused those same dispute-affected states to align more closely with the U.S. in UN votes.  


## Data

### 1. IMF Direction of Trade Statistics  
**Source:**  
International Monetary Fund (IMF). (2024). Direction of trade statistics. Retrieved 16.7.2024, from http://www2.imfstatistics.org/DOT/

**Description:**  
Monthly bilateral trade flows (imports + exports) between each ASEAN country and all partners.

**Preprocessing:**  
- Filtered to ASEAN members (Brunei, Cambodia, Indonesia, Laos, Malaysia, Myanmar, Philippines, Singapore, Thailand, Vietnam).  
- Computed `trade_dependency_CHN` = (trade with China) / (total trade).  
- Computed `trade_dependency_USA` = (trade with U.S.) / (total trade).  
- Pivoted data from wide to long format: one row per country × month.

### 2. World Bank Governance & Development Indicators  
**Sources:**  
- World Bank. (2023). Worldwide Governance Indicators. Data Bank. Retrieved 19.7.2024, from https://databank.worldbank.org/source/worldwide-governance- indicators/Series/PV.EST#
- World Bank. (2024). World Development Indicators. Data Bank. Retrieved 18.7.2024, from https://databank.worldbank.org/reports.aspx?source=2&series=NY.GDP.MKTP.CD&country=#

**Variables:**  
- `GDP` = real GDP of country i in year t.  
- `Stab` = political stability score (–2.5 to +2.5).

### 3. FDI Inflows  
**Sources:**  
- CEIC Data. (n.d.). China Outward Direct Investment. CEIC Data. Retrieved 16.7.2024, from https://www.ceicdata.com/en/china/outward-direct-investment-by-country  
- OECD. (2022). International direct investment database. OECD Data Explorer. Retrieved 18.7.2024, from https://data- explorer.oecd.org/vis?df[ds]=DisseminateFinalDMZ&df[id]=DSD_FDI%2540DF_FDI_CTR Y_IND_SUMM&df[ag]=OECD.DAF.INV&dq=USA...DO......._T.A.&pd=2005%2C2022&t o[TIME_PERIOD]=false&vw=tb


**Variables:**  
- `FDI_CHN` = annual USD inflows from China.  
- `FDI_USA` = annual USD inflows from the United States.

### 4. Freedom House Freedom in the World  
**Source:**  
Freedom House. (2024). Freedom House: Freedom in the World. Retrieved 25.7.2024, from https://prosperitydata360.worldbank.org/en/dataset/FH+FIW

**Variable:**  
- `Freedom` = political rights & civil liberties (1 = Not Free, 2 = Partly Free, 3 = Free).

### 5. SIPRI Military Expenditure  
**Source:**  
SIPRI Military Expenditure Database. (2024). Stockholm International Peace Research Institute. Retrieved 19.7.2024, from https://doi.org/10.55163/CQGC9685

**Variable:**  
- `Mil_GDP` = military spending as % of GDP.  
- *Note:* Laos and Vietnam have missing values for some years; treated as NA.

### 6. UN General Assembly Ideal Point Distances  
**Source:**  
Bailey, M. A., Strezhnev, A., & Voeten, E. (2017). Estimating dynamic state preferences from United Nations voting data. Journal of Conflict Resolution, 61(2), 430–456. http://www.jstor.org/stable/26363889

**Variable:**  
- `IdealPointDist` = absolute distance between U.S. and country i ideal points (lower = closer alignment).

### Final Merged Dataset  
- **Period:** 2011–2022  
- **Unit:** Country × Month (for trade) or Country × Year (for all others)  
- **Main panel:** 10 ASEAN countries × 144 months (2011–2022) for Trade; 10 × 12 years for Models 3–4.  
- **Key covariates:** GDP, Stability, Freedom, FDI from China/U.S., Military GDP, IdealPointDist.


## Methods
I estimate a mix of OLS and Difference-in-Differences (DiD) models to test H1–H3 over 2011–2022. All regressions include country and time indicators where appropriate.

### Models 1 & 2: OLS Trade Dependency

**Model 1:** Trade dependency Chinaim = α + β1Trump + β2GDPit + β3Stabit + β4Freedomit + β5US FDIit + β6Countryt β6Date + εit

**Model2:** Trade dependency USim = α + β1Trump + β2GDPit + β3Stabit + β4Freedomit + β5China FDIit + β6Countryt β6Date + εit

Where:
- Trade dependency Chinaim/Trade dependency USim is the estimated trade dependency on China/the US for country i in month m, respectively.
- α is the intercept.
- Trump is a dummy variable for years after Donald Trumps entered office, beginning in 2017. Serves as the main indpeendent variable.
- GDPit is GDP of a country i in year t.
- Stabit is the political stability country i in year t, ranging from -2.5 to 2.5.
- Freedomit is the political freedom country i in year t, rated on a three point scale.
- US FDIit is the total flow of FDI into country i in year t originating from the US.
- China FDIit is the total flow of FDI into country i in year t originating from China.
- Country is indicator for each ASEAN country at time t.
- Date is an indicator for time, measured monthly.
- β1,2,..,n are the coefficients for the various independent variables.
- εit is the idiosyncratic error term.

### Model 3: DiD on Military Spending

Model 3: Miltary Expendetureit = α + β1Disputit + β2Stabit + β3Freedomit + β4Countryit + δt + εit

Where:
- Miltary Expendetureit is the estimated percent of military expenditure as share of a GDP for country i in year t.
- Disputeit is the treatment variable indicating if a country i has an ongoing territorial dispute with China in year 2018 and onward (i.e., lagged by one year after Trump's
  first year in office).
- δtm is the year-specific fixed effect.

### Model 4: DiD on UN Voting Alignment

Model 4: IdealPointDistanceit = α + β1Disputeit + β2GDPit + β3Stabit + β4Freedomit + β5Countryt + δt + εit

Where:
IdealPointDistance is the estimated UN voting similarity in the UN between the US and country i in year t

## Results

#### H1: Trade Dependency on Great Powers
Model 1 (China):
- Trump coefficient: +0.002 (p = 0.643) → positive sign but not significant
- Effect size: ~0.2 pp change in trade dependence over Trump’s term—negligible

Model 2 (United States):
- Trump coefficient: –0.002 (p = 0.252) → negative sign but not significant
- Political stability: β = –0.014 (p < 0.05) → more stable countries show slightly lower US trade dependence
- Interpretation: No evidence

> **Takeaway:** No evidence that Trump’s term shifted ASEAN trade dependence toward China or away from the U.S.

#### H2: Military Expenditure (% of GDP)
Model 3 (DiD on “Dispute”):
- Dispute coefficient: –0.058 (p = 0.557) → negative but not significant
- Controls (stability, freedom): all statistically insignificant
- Visual inspection (see Figure 1): Treated and control countries exhibit parallel military‐spending trends before/after 2018.
- Interpretation: No detectable effect of Trump’s term on defense budgets, even among ASEAN states disputing territory with China.

> **Takeaway:** Dispute-affected states did not significantly change military spending post-2018.

#### H3: UN Voting Alignment
Model 4 (DiD on “Dispute”):
- Dispute coefficient: –0.031 (p = 0.598) → negative (i.e. closer alignment) but not significant
- Political freedom: β = –0.099 (p < 0.10) → more democratic countries vote more closely with the US
- Time trends: No consistent post-2018 alignment boost among “threatened” states; a mild convergence across all ASEAN members emerges only from 2021 onward.
- Interpretation: No robust evidence that Trump’s presidency spurred closer diplomatic alignment among dispute-affected ASEAN countries.

> **Takeaway:** No robust DiD effect of Trump on voting alignment among dispute-affected states.


#### Overall Takeaways
All three hypotheses are **falsified**. Trump’s presidency did **not** produce significant hedging shifts in trade dependency, military budgets, or UN voting among ASEAN states. Null results may reflect limited sample size (n = 10) and proxy limitations. However, political stability and freedom remain significant predictors in their respective models.

## Usage

### 1. **Clone the repo**  
   ```bash
   git clone https://github.com/YourUsername/Hedging-against-Trump.git
   cd Hedging-against-Trump
   ```
   
### 3. Install R Packages
In R
```r
install.packages(c(
  "tidyverse",
  "haven",
  "stargazer"
))
```

### 4. Render the R Markdown 
In R
```r
rmarkdown::render("Hedging-against-Trump.Rmd")
```

