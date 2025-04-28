# Tracking Microplastics Across Our Oceans: An Animated Visualization using NASA CYGNSS Data

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Last Commit](https://img.shields.io/github/last-commit/Benersem/microplastics-ocean-tracking)](https://github.com/Benersem/microplastics-ocean-tracking/commits/main)
![![newplot (3)](https://github.com/user-attachments/assets/ddc93c89-6a38-4ed2-a4d8-6a99e1030a43)
]

## Project Overview

Our story begins on the shore, where plastic waste transforms into insidious microplastics – particles less than 5 millimeters in size. These tiny fragments embark on a global journey, carried by rain, rivers, and wind into our oceans. An estimated 4.8 to 12.7 million metric tons of plastic enter the ocean each year, accumulating in massive garbage patches and even entering the food chain.

Tracking these microplastics is vital because, unlike larger debris, they are largely invisible to the naked eye, yet they are pervasive. Traditional tracking methods are expensive, slow, and provide sparse data. This project leverages a unique approach: utilizing publicly available data from NASA's Cyclone Global Navigation Satellite System (CYGNSS) mission. Originally designed to monitor hurricanes, CYGNSS data reveals that areas with higher microplastic concentrations exhibit a smoother ocean surface than expected, even with high winds.

This project visualizes the daily microplastic concentrations across the globe throughout the year 2019, using latitude-longitude bins to provide a clear and dynamic picture of their distribution. The resulting animated heatmap reveals critical insights into where and when these microplastics accumulate, offering valuable information for targeted cleanup efforts and policy interventions.

**Data Source:** [NASA CYGNSS Level 3 Microplastic Concentration Retrievals Version 3.2](https://podaac.jpl.nasa.gov/cloud-datasets?search=microplastic%20concentration)

**Citations:**

> CYGNSS. CYGNSS Level 3 Microplastic Concentration Retrievals Version 3.2. Ver. 3.2. PO.DAAC, CA, USA. accessed [YYYY-MM-DD] at 10.5067/CYGNS-L3P32
>
> Journal Reference: Evans M. C., and Ruf C. S. (2021) Toward the Detection and Imaging of Ocean Microplastics with a Spaceborne Radar IEEE Transactions on Geoscience and Remote Sensing 10.1109/TGRS.2021.3081691

**Data Description:**

The CYGNSS L3 Ocean Microplastic Concentration V3.2 dataset provides daily netCDF files containing gridded maps of microplastic number density (#/km²). This data is indirectly estimated through an empirical relationship between ocean surface roughness and wind speed. The dataset offers near-gap-free Earth coverage with a daily temporal and 0.25-degree latitude/longitude spatial grid, and a 30-day, 1-degree feature resolution. It's important to exercise caution in regions where non-correlative factors can affect ocean surface roughness, such as the Intertropical Convergence Zone, biogenic surfactants, and oil spills. Version 3.2 utilizes updated Level 1 scattering cross-section data and processing algorithm parameterizations.

## Technical Overview

This project involved the following key steps and tools:

* **Data Source:** NASA CYGNSS Level 3 Microplastic Concentration Retrievals Version 3.2 ([https://podaac.jpl.nasa.gov/cloud-datasets?search=microplastic%20concentration](https://podaac.jpl.nasa.gov/cloud-datasets?search=microplastic%20concentration)).
* **Data Access:** A custom Python function was developed and implemented via a Jupyter Notebook (`auto_download.ipynb`) to download the daily netCDF4 files using the openDAP API. Direct S3 access was initially attempted but proved challenging within the project's time constraints.
* **Data Processing:**
    * **Geospatial Handling:** The satellite data, provided in netCDF4 format with a 0-360 degree longitude convention, was processed to align with the -180 to 180 degree convention used in standard shapefiles. This involved creating a custom function to adjust longitude values.
    * **Ocean Labeling:** Shapefiles were used to label different ocean regions. Geospatial joins were performed using libraries like `geopandas` to associate microplastic concentration data with specific ocean areas.
    * **Spatial Binning:** To manage computational resources effectively, the microplastic concentration data was aggregated into larger grid cells (2.5 x 2.5 degrees latitude and longitude). The mean microplastic concentration was calculated within each bin.
    * **Libraries Used:** `numpy`, `pandas`, `xarray`, `shapely.geometry`, `geopandas`.
* **Data Visualization:**
    * **Tool:** Plotly was chosen as the visualization library due to its ability to handle large geospatial datasets and create interactive and animated visualizations, overcoming limitations encountered with Looker Studio and Tableau.
    * **Animated Heatmap:** An animated heatmap was generated to display the daily microplastic concentration across the oceans for the entire year of 2019. The animation frames are driven by the date, allowing for the observation of temporal changes and the identification of hotspots.
    * **Color Styling:** Different color scales were explored to effectively represent the microplastic concentration levels.
      ![newplot (1)](https://github.com/user-attachments/assets/a290a398-0b1f-4bcc-9280-7dc5cea169a9)

* **Programming Language:** Python, BigQuery (for potential future analysis or data storage).


## Project Structure

## Project Structure

```
├── data/                                           
│   ├── shapefiles/                                 # Contains the shapefiles for ocean boundaries
├── notebooks/                                      # Jupyter notebooks documenting the analysis and visualization steps
│   ├── data_exploration.ipynb
│   ├── automated_download.ipynb                    # Function to download data using OpenDAP
│   ├── data_process_function_daily.ipynb           # Scripts for geospatial handling, binning, and ocean labeling
|   ├── microplastics_dashboard.ipynb               # Notebook for generating the Plotly animated map
│   ├── microplastics_dashboard_viirs_style.ipynb
|   └── earthdata.env.example              
├── visualizations/                                  # Output visualizations (HTML interactive plot, project presentation)
│   ├── microplastic_presentation.pdf
|   ├── microplastic_distribution_2019.png
│   └── microplastic_distribution_2019_viirs_style.png
├── README.md
└── LICENSE
```



## Challenges and Learnings

Throughout this project, I encountered and overcame several challenges:

* **Data Access via S3:** Initially, accessing the data directly via S3 proved challenging. This was resolved by implementing a Python-based download function utilizing the openDAP API, demonstrating adaptability in accessing remote data.
* **Geospatial Data Alignment:** Handling the discrepancy in longitude conventions (0-360 degrees in the NASA data vs. -180 to 180 degrees in shapefiles) required the development of a specific function to ensure accurate geospatial joins and ocean labeling.
* **Computational Constraints:** Processing the large volume of daily satellite data locally posed computational limitations. Implementing spatial binning (2.5 x 2.5 degree grids) was crucial for reducing computational load while still capturing meaningful spatial patterns.
* **Visualization of Large Geospatial-Temporal Data:** Standard business intelligence tools like Looker Studio and Tableau struggled to effectively visualize the large, time-series geospatial data. This led to the adoption of Plotly, which proved to be a more suitable library for creating the desired animated heatmap.
* **Understanding Satellite Data:** Gaining a foundational understanding of the CYGNSS mission and the interpretation of sea surface roughness data in relation to microplastic concentration required careful review of the mission documentation and relevant research papers.

Key learnings from this project include:

* **Proficiency in Geospatial Data Handling:** Developed skills in working with netCDF4 files, shapefiles, performing geospatial joins using `geopandas`, and handling coordinate system transformations.
* **Remote Data Access Techniques:** Gained experience in programmatically downloading data from remote APIs like openDAP.
* **Advanced Data Visualization with Plotly:** Mastered the creation of animated heatmaps for visualizing temporal and spatial data, including customization of color scales and animation frames.
* **Computational Optimization:** Learned the importance of data aggregation techniques like spatial binning for managing computational resources when working with large datasets.
* **Interdisciplinary Approach:** Understood the intersection of remote sensing, environmental science, and data analytics in addressing complex global issues like microplastic pollution.

## Tools Used

* **Programming Language:** Python
* **Core Libraries:** `numpy`, `pandas`, `xarray`, `shapely.geometry`, `geopandas`, `plotly`.
* **Other Tools:** Jupyter Notebooks, Git, GitHub, BigQuery (for potential future use).

## Potential Next Steps

* **Incorporate Additional Data:** Explore integrating data from ocean current models (e.g., HYCOM), wind data, and potentially even citizen science microplastic sampling data to create a more comprehensive understanding.
* **Expand Temporal Scope:** Analyze CYGNSS data from multiple years to identify long-term trends in microplastic distribution and assess the impact of environmental events.
* **Develop Interactive Visualizations:** Enhance the Plotly visualization with interactive elements, allowing users to zoom, pan, and inspect specific regions or time periods.
* **Quantitative Analysis and Modeling:** Move beyond visualization to perform statistical analysis, such as identifying correlations between microplastic hotspots and oceanographic features, or developing predictive models.
* **Deployment:** Explore deploying the interactive visualization as a web application using platforms like Streamlit or Heroku to make it accessible to a wider audience.
* **Investigate the Impact of Data Resolution:** Analyze the effect of different binning strategies on the visualization and the insights derived.

## Acknowledgements

* Thank you to NASA and the CYGNSS Science Team at the University of Michigan for providing this valuable public dataset.
* Special thanks to the developers of the Python libraries used in this project for their powerful and user-friendly tools.
* I would like to sincerely thank WBS Coding School for providing me with the foundational knowledge and skills in data analytics that made this project possible. I am also grateful for their support, which led to this presentation being selected for the community demo day.

## Contact

* [Semira Bener]
* [www.linkedin.com/in/semira-bener]
