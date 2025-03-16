# [MUSICA](https://wiki.ucar.edu/display/MUSICA/MUSICA+Home)

- [official description](https://www2.acom.ucar.edu/sections/multi-scale-infrastructure-chemistry-modeling-musica)
- [github](https://github.com/NCAR/MUSICA)
- https://docs.google.com/document/d/e/2PACX-1vRe4Aky5YgrlskcXGSGcETySvNkCsVRAbO7hK5TT_InTfK_81VxRax358mgaiTQtXGproEn-7epChlN/pub#h.l5ctsbmr79jr

- 接收MUSICA 相关邮件: 向[drews@ucar.edu](mailto:drews@ucar.edu)写邮件申请，[**musica-info**](https://groups.google.com/a/ucar.edu/d/forum/musica-info)



## Basic knowledge

- **MUSICAv0** is an initial configuration based on the CESM Community Atmosphere Model with chemistry using the Spectral Element with **Regional Refinement (RR)** dynamical core.
- **MusicBox** is a box model using a model independent chemistry module.
  - For technical support: music-box-support@ucar.edu
  - For information exchange: music-box-info@ucar.edu 
- **MELODIES** is a modular framework to compare model results with observations.
- MUSICA is part of **SIMA** (System for Integrated Modeling of the Atmosphere).

- **MUSICAv1**: MPAS-hexagonal mesh, finer scale to 3km



## compset

- FCnudged
  - **MOZART-TS1.1**: The default CAM-chem MOZART-TS1 chemistry has been updated in CESM2 to include NOx-dependence in the VBS-SOA scheme, as described in [Jo et al. (2020)](https://doi.org/10.5194/acp-2020-543).
- FCts2nudged
  - **MOZART-TS2**: Improved isoprene and terpene oxidation (Rebecca Schwantes, NCAR) [Schwantes et al. (2020)](https://doi.org/10.5194/acp-20-3739-2020).



## Tutorial

- [Nanjing tutorial](./Nanjing_tutorial)
  - The tutorial materials are available at: https://wiki.ucar.edu/display/MUSICA/Nanjing+Tutorial

- [musica-tutorial github repo 2021](https://github.com/NCAR/musica-tutorial)
- [musica-tutorial github repo 2024 for Nanjing](https://github.com/jzhan166/MUSICAv0_Nanjing_tutorial_2024?tab=readme-ov-file)



## [MusicBox](https://musicbox.acom.ucar.edu/)



## Input

[Anthropogenic_emissions](./Anthropogenic_Emissions.xlsx)

- [download](https://docs.google.com/spreadsheets/d/1WHtJtli9F2vMBwPvyLhkPN95PlQGq5pK9QgGIyJZ7dU/edit?gid=0#gid=0)

[NCAR HPC file directory](./HPC_input_files_location.xlsx)

- [Download](https://docs.google.com/spreadsheets/d/e/2PACX-1vQqNablLIRxQhTtoEoEoTvwEQ8NjRPLL207ktC3l8F6miK9tojQ-oanEYhgYUIRmQ/pubhtml?)

[UK National Atmospheric Emissions Inventory (NAEI)](https://naei.energysecurity.gov.uk/)
