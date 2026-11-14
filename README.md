# Global Opportunities to Fill Climate Observation Gaps with Animal-Borne Sensors

This repository contains the R code used for the analysis in the manuscript 
"Global opportunities to fill climate observation gaps with animal-borne sensors" 
by [Author list]. [Manuscript status: in preparation].

## Requirements

Analyses were performed using R-studio with **R version 4.4.1**.

### Core spatial, scientific, and taxonomy analysis
- `terra`, `sf`: Core spatial raster processing, vector manipulation, area calculations
- `argoFloats`: Identifying and processing Argo float profiles (Kelley et al., 2021)
- `move2`: Handling and standardizing animal tracking telemetry data
- `phytools`, `traitdata`, `rfishbase`, `worrms`: Accessing taxonomic databases, species 
  traits, and phylogenetic information

### Visualization & mapping
Maps were generated using the `ggplot2` and `patchwork` packages in R, with helper 
extensions including `ggnewscale`, `scales`, `rnaturalearth`/`rnaturalearthdata`, 
`rphylopic`, `ggimage`, and `tidyterra`.

### Data wrangling & infrastructure
Standard data wrangling and API-access packages (`tidyverse`, `data.table`, `httr`, 
`jsonlite`, `curl`, `future`/`furrr`, etc.) were used throughout for file handling, 
parallel processing, and API queries; see individual scripts for full package calls.

## Datasets

### Species range data

Global extent-of-occurrence polygons were obtained from:
- BirdLife International and the Handbook of the Birds of the World 
  (http://datazone.birdlife.org/species/requestdis) — requires data request/registration
- IUCN Red List of Threatened Species (https://www.iucnredlist.org/resources/spatial-data-download)
- iNaturalist (https://www.inaturalist.org/pages/developers)

### Climate observing network data
- **Oceanic (Argo)**: Global Argo Data Repository (GADR). Data was retrieved using 
  the `argoFloats` R package, encompassing 15,774 unique Argo Floats with 2,425,873 
  observations from 2010 to 2024 (Argo, 2000).
- **Atmospheric (Radiosondes)**: Integrated Global Radiosonde Archive (IGRA), NOAA NCEI 
  (Durre et al., 2016). (https://www.ncei.noaa.gov/products/weather-balloon/integrated-global-radiosonde-archive)
- **Terrestrial (GHCNd)**: Global Historical Climatology Network-Daily (GHCNd), NOAA NCEI 
  (Menne et al., 2012; Fumihiko et al., 2026). (https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily) 

## Workflow

Scripts are organized into numbered folders that should be run in order. Within each 
folder, scripts are also numbered sequentially.

1. **`1_Rasterize_shapefiles/`**
   - `1_rasterize_species_abs2026.Rmd`: Rasterizes species extent-of-occurrence 
     polygons to a 1° (5° for aerial species) geographic grid using a cell-intersection 
     rule.
   - `2_normalize_rasters_abs2026.Rmd`: Normalizes/standardizes species rasters for 
     downstream overlap analysis.

2. **`2_Argos/`**
   - `1_get_argos_abs2026.Rmd`: Retrieves and processes Argo float profiles.
   - `2_argos_species_overlap_abs2026.Rmd`: Intersects Argo coverage gaps with the 
     species occurrence rasters.

3. **`3_Radiosondes/`**
   - `1_get_radiosondes_abs2026.Rmd`: Retrieves and processes IGRA radiosonde station data.
   - `2_radiosondes_species_overlap_abs2026.Rmd`: Intersects radiosonde coverage gaps 
     with the species occurrence rasters.

4. **`4_GHCN/`**
   - `1_get_GHCN_stations_abs2026.Rmd`: Retrieves and processes GHCN-Daily station data.
   - `2_GHCN_stations_species_overlap_abs2026.Rmd`: Intersects GHCN coverage gaps with 
     the species occurrence rasters.

5. **`5_plots/`**
   - `1_abs_plots.Rmd`: Generates final figures and maps summarizing global observation 
     gaps and species overlap.
     
**Data cleaning note:** Species occurrence and range data were compiled from these 
three sources. Scientific names were standardized, duplicate/synonymous names were 
resolved, and taxonomic inconsistencies (e.g., typos, extinct-in-the-wild species, 
IUCN presence-uncertain records, empty geometries) were manually reviewed and 
corrected. The script used to merge and clean these sources is not included in this 
repository due to the iterative, manual nature of this step; the final cleaned species 
list used as input to `1_rasterize_species_abs2026.Rmd` is available upon request from the authors.

## License

Licensed under the MIT license.

## Citation

[Author list]. 2026. Global opportunities to fill climate observation gaps with 
animal-borne sensors. [Journal, in preparation].

## Acknowledgements

[Funding sources, data providers, collaborators]



- Argo (2000). Argo float data and metadata from Global Data Assembly Centre (Argo GDAC). SEANOE. https://doi.org/10.17882/42182
- Durre, I., Yin, X., Vose, R. S., Applequist, S., & Arnfield, J. (2016). Enhancing the Data Coverage in the Integrated Global Radiosonde Archive. *Journal of Atmospheric and Oceanic Technology*, 35(9), 1753-1770.
- Kelley, D., Harbin, J., & Richards, C. (2021). argoFloats: An R package for analyzing Argo data. *Frontiers in Marine Science*, 8, 635922.
- Menne, M. J., Durre, I., Vose, R. S., Gleason, B. E., & Houston, T. G. (2012). An overview of the Global Historical Climatology Network-Daily Database. *Journal of Atmospheric and Oceanic Technology*, 29(7), 897-910.
- [Add Fumihiko et al., 2026 full citation here]
