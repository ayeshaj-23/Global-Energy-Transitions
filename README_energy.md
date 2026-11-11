# Global Energy Transitions: Pathways to a Sustainable Future

A data analysis of how countries differ in their progress towards clean energy, using World Bank and UN indicators for 2000 to 2020. The project looks at electricity access, emissions, renewable energy use and income, and asks whether all countries are really on the same path to clean energy.

It was built for the Data Science for Social Good course at Habib University, and linked to SDG 7 (Affordable and Clean Energy) and SDG 13 (Climate Action).

## The question

What distinct energy transition pathways exist globally, and how do socioeconomic factors influence renewable energy adoption and energy access across country clusters from 2000 to 2020?

## What we found

Countries fall into four groups, and they are not walking the same road.

| Group | Countries | What it looks like |
|---|---|---|
| Low-Access Developing (Energy Poverty) | 49 | About half the population has electricity, and a high renewable share that is mostly traditional biomass such as firewood |
| High-Income High-Emission | 29 | Near-universal electricity, very high emissions, low renewable share |
| Middle-Income Emerging | 96 | Nearly everyone has electricity, with a mix of fossil fuels and modern renewables |
| China | 1 | Treated as an outlier because no other country matches its scale |

**The biomass illusion.** In the Low-Access group, the renewable share looks high on paper (about 64 per cent on average), but much of it is firewood and other traditional biomass burned for survival. It is not a clean energy transition. Global reporting that counts all renewables together hides this, so our policy brief recommends reporting modern renewables separately.

**The trade-off.** Between 2000 and 2019, most countries that expanded electricity access also saw their emissions per person rise. Of 151 countries with valid data, 94 raised both access and emissions, 8 raised access while cutting emissions, and 3 lost access while emissions rose.

## About the data

- Global Data on Sustainable Energy (2000 to 2020), 3,649 rows and 21 columns, covering 176 countries
- Indicators include access to electricity and clean cooking fuels, renewable energy share, electricity by source, CO2 emissions, GDP, energy intensity and country location

## What I did

1. **Explored and cleaned the data.** I checked missing values and filled gaps using median imputation (under 5 per cent missing) or KNN imputation (5 to 20 per cent). Columns with more than 20 per cent missing were left as they were.
2. **Fixed a data quality problem.** The source file holds a placeholder CO2 value for 2020, which produced impossible per capita emissions. I filtered it out and used 2019 as the most recent reliable year for modelling.
3. **Analysed relationships and trends** using correlations, income group comparisons, growth rates and a quadrant analysis of electricity access against emissions.
4. **Clustered countries with K-means** on a 2019 snapshot of 175 countries, using 14 energy, socioeconomic, emissions and location features, standardised first.
5. **Validated the clusters with a Random Forest** and used it to see which features separate the groups best.
6. **Tested fairness and interpretability** with a chi-square test between income category and cluster, and SHAP values.

## Results

- Elbow and silhouette analysis did not point to a single clear number of clusters. Both 3 and 4 clusters produced a one-country cluster (China), so I kept 4 because the other three groups were more balanced and easier to interpret.
- A 5-fold cross-validated Random Forest recovered the cluster labels with 94.3 per cent accuracy (94.0 per cent weighted F1). Accuracy was 98.0, 96.6 and 92.7 per cent for the three multi-country clusters.
- The features that separated the clusters most were CO2 emissions per capita (29.1 per cent), GDP per capita (24.3 per cent) and electricity access (20.9 per cent).
- Income category and cluster were strongly linked (chi-square p < 0.001).

## Limitations

- The clusters come from a single snapshot (2019). The 2000 to 2020 data is used for trends, not for the clustering itself.
- Cluster structure is moderate (silhouette about 0.24), and China forms a cluster of its own.
- The Random Forest uses the same features that created the clusters, so its high accuracy shows the groups are separable, not that they are the only valid grouping.
- Results show associations between income and energy pathways, not proof of cause.
- Some indicators had high missingness (renewable capacity per capita, for example) and were imputed.
```
## Author
Ayesha Jawed, Habib University
