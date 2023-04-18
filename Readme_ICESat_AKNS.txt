Melanie J Frost
4/21/23
ICESat-2 ATL13 AK North Slope Bathymetry

ICESAT-2 Data

1a. ICESat-2_Granules.ipynb (ICESat-2_Granules.ipynb)
- Identify ICESat Granules for area
 -- Read in Caleb Spradlin's CMR process
 -- Specify dates and bounding box for AKNS
 -- Find Collection ID for ATL13 v005 from earthdata
 -- Run search to create lists
 -- Create test list

1b. ICESat-2_Granules (ICESat-2_Granules.ipynb)
- Extract contents of HDF files to CSV file
 -- Specify directory where data is located
 -- Create empty dataframe
 -- Create function to identify subdirectory by month/date/year
 -- Loop over granule list and beam list
   --- Make filepaths for variables
   --- Extract variable and save each to a list
   --- Put lists together to make dataframe
   --- Append new dataframe to bottom of overall df
   --- Change date to datetime
   --- See how big file is
 -- Write to CSV

2. ICESat Data Filter (Data_filter.ipynb)
- Filter lakes dataset by criteria
 -- Lakes only
 -- Size > 0.10 km2
 -- Not cloudy
 -- Not Noise
 -- Not very shallow
 -- Not extreme wind
 -- Not icy or snowy (though, most points IDed as "snow free land"
 -- No extreme waves
 -- Subsurface anomaly flag pretty vague, so discarding
 -- Depth most be <40m for ICESat-2 to detect
 -- Between 1-July and 15-Sep every year
 -- >= 90% of the quality flags are qual0 (nominal)
 -- remove "transistion" for orientation
 -- change not applicaple (extremely high) values to np.nan
- Data calculation
 -- calculate sseg distance (was not able to get this to work before project's end)
 -- Identify Strong and Short beams

3. Clipping ICESat Data from CSV (Geo_filter.ipynb)
- Use shapefile of AKNS to clip the csv and convert to points
 -- Convert csv to points shapefile
 -- Check CRSs and convert AKNS to WGS84
 -- Plot data
 -- Clip points file to AKNS shape and save as .shp and as .csv

4. ICESat-Data Exploration (EDA.ipynb)
- Begin exploring what the whole dataset looks like
 -- data:
   ---cycle
   ---id
   ---size
   ---type
   ---cloud_flag
   ---bkgrd_flag
   ---shallow_flag	
   ---wind_flag
   ---rgt
   ---snow_ice_flag
   ---wave_flag
   ---depth
   ---anomalies
   ---beam
   ---beam strength
   ---start_date
   ---qual0
   ---qual1
   ---qual2
   ---qual3
   ---orientation

5. Compare different data types
- Compare Depth vs no Depth
- Compare depth along all other variables
 
LAKES
1. North Slope Lakes (completed on QGIS)
- Lakes within Area of Interest
 -- Download HydroLAKES (underpins ICESat-2 Data mask) here: https://www.hydrosheds.org/products/hydrolakes
 -- Clip to AK North Slope Shapefile: https://catalog.northslopescience.org/sv/dataset/514
 -- Calculate area of lakes
 -- Restrict to lakes with an area of >= .45 km 
2. Lake Stability (GSW_filter.ipynb)
- Get information about Lake size stability from GSW and GLAD data
 -- Download Lake Occurance/Recurrance from Global Surface Water (GSW) (Pekel, et al): 150W-170W, 70N-80N
   --- https://global-surface-water.appspot.com/download
 -- Download Water classification and Water % maps from Global Land Analysis & Discovery (GLAD) 
    (Pickens, et al): 150W-170W, 70N-80N
   --- https://www.glad.umd.edu/dataset/global-surface-water-dynamics
      ---- Classification = "Class99_21.tif"
	  ---- Percent maps = "August_percent_99_21.tif"
 -- Clip to AK North Slope Boundary and mosaic together each of 4 sets (GSW Recurrance, GSW Occurance,
    GLAD classification, GLAD percent)
3. Calculate statistics for Lake Stability
- GLAD
 -- Use ArcGIS to Join GLAD percent Raster to table, calculating 10,25, 75, and 90 %
 -- Join table to Hydrolakes Large database
 -- Export features where 75% of GLAD data is greater than or equal to 90%
 -- Add 75m buffer around lakes
- GSW
 -- Use ArcGIS to Join GSW Reoccurance percent Raster to table, calculating 10,25, 75, and 90 %
 -- Join table to Hydrolakes Large database
 -- Export features where 90% of DSW data is greater than or equal to 90%
- Alernate (if no spatial analysis license):
 -- Use QGIS to export raster values to ICESat points. Keep points > 85%
4. ABoVE
 - Download ABoVE Surface water extent, 2011, h = 0 & 1, v = 0: https://daac.ornl.gov/cgi-bin/dsviewer.pl?ds_id=1324
 - Use ArcGIS to Clip to AKNS, Mosiac files
 - Raster to Polygon, simplify
 - Clip ICESat 2 data to ABoVE lake boundaries
5. Analyze results
 - Expect shallow lakes
 - Expect gradual changes in depth
 - Expect near shore shallower than middle of lake

  
