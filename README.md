# Cenozoic Landscape Evolution of Peninsular India

I use this project to model the long-term topographic evolution of Peninsular India during the Cenozoic (66 to 0 Ma) using the [Landlab](https://landlab.readthedocs.io/) framework.

The workflow combines time-varying uplift and precipitation forcing with detachment/transport-limited fluvial incision and hillslope diffusion to simulate elevation and sediment deposition through geologic time.

## Project Highlights

- Study region: Peninsular India
- Simulation window: 66 Ma to present (0 Ma)
- Model grid: `120 x 80` nodes (`dx = dy = 50,000 m`)
- Core Landlab components:
  - `FlowAccumulator` (D8 flow routing + depression handling)
  - `ErosionDeposition` (stream-power based incision/deposition)
  - `LinearDiffuser` (hillslope diffusion)
- Time-varying inputs:
  - Uplift fields by interval (`66-56`, `56-34`, `34-23`, `23-11`, `11-6`, `6-2`, `2-0 Ma`)
  - Precipitation maps for major paleoclimate intervals
- Key outputs:
  - Evolving topography (`topographic__elevation`)
  - Cumulative sediment thickness (`sediment_deposition`)
  - Cross-section stratigraphy through time


## Modeling Approach

The notebook follows this sequence:

1. Load parameters from `model_params_upd.yaml`.
2. Initialize a `RasterModelGrid` and assign 66 Ma elevation as the starting topography.
3. Set fixed-gradient boundary conditions on all edges.
4. Add precipitation-driven runoff (`runoff = precipitation * runoff_coef`).
5. Run sequential time loops for each geologic interval:
   - `66-56 Ma` (10 Myr)
   - `56-34 Ma` (22 Myr)
   - `34-23 Ma` (11 Myr)
   - `23-11 Ma` (12 Myr)
   - `11-6 Ma` (5 Myr)
   - `6-2 Ma` (4 Myr)
   - `2-0 Ma` (2 Myr)
6. In each step:
   - Update hillslopes (`LinearDiffuser`)
   - Route flow (`FlowAccumulator`)
   - Apply fluvial erosion/deposition (`ErosionDeposition`)
   - Accumulate net sediment thickness
   - Apply uplift increment for that interval
7. Visualize topography, precipitation, sediment deposition, and cross-section stratigraphy.



