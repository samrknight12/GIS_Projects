
# Chapman Lake, BC, Canada - Watershed Delineation

The process and final map of a watershed delineation for a watershed around Chapman Creek Located on the Sunshine Coast, BC, Canada.



## [Final Map PDF](https://github.com/samrknight12/GIS_Projects/blob/master/Chapman%20Lake%20Watershed%20Delineation/ChapmanLake_WaterShed.pdf)
## Acknowledgements
Raw DEM data was aquired from the [USGS Earth Explorer](https://earthexplorer.usgs.gov/s). 











## Process
- Import the raw DEM .tif file aquired from the USGS Earth Explorer.
![Raw DEM](https://github.com/samrknight12/GIS_Projects/blob/master/Chapman%20Lake%20Watershed%20Delineation/Screenshots/DEM_Raw.PNG) 
- Fix the gaps that are present in the raw DEM 
    - Mask Tool: Creates a raster object by specifying a range of valid pixels. In this case I used 0 as the lower limit and the max value of the raw DEM 2634.
    - Elevation Void Fill: First a method where averaging the neighouring 8 pixel values is applied. Then plane fitting is attempted. If the error from plane fitting is too large, and inverse distance weighted algorithm is applied.
![Filled DEM](https://github.com/samrknight12/GIS_Projects/blob/master/Chapman%20Lake%20Watershed%20Delineation/Screenshots/Filled_DEM.PNG)
- Now that the gaps are filled, minor imperfections where there is a sudden downwards gap nned to be filled.
    - Fill Tool: Fixes the sinks in the raster.
- Find the flow direction.
    - Flow Direction Tool (D8): The flow direction tool determines the direction of flow from each cell in a raster. The selected D8 method is determined by the direction of steepest descent in the cell.
![Flow Direction](https://github.com/samrknight12/GIS_Projects/blob/master/Chapman%20Lake%20Watershed%20Delineation/Screenshots/Direction_DEM.PNG)
- Determine the Flow Accumulation.
    - Flow Accumulation Tool: Determines the accumlation by calculating the number of cells that flow into the output raster cell based on the flow direction raster.
    - In the symbology of the output raster, a data exclusion of 0-300 was applied to visualize the major streams in the area of interest. 
![Flow Accumulation](https://github.com/samrknight12/GIS_Projects/blob/master/Chapman%20Lake%20Watershed%20Delineation/Screenshots/Flow_Accumlation_view.PNG)
- The Flow Accumulation raster was then reclassified.
    - Reclassify Tool: I created two manual breaks 0-300 (The same threshold that was chosen in the previous step) and > 300. The values for the two classes were set to 0 and 1 respectively.
-The reclassified raster was then turned into a polyline.
    - Raster to Polyline Tool: The field was set to VALUE of each class and the background set to Zero so that a polyline was only made of each stream.
- A pour point was chosen and made into a feature class. The pour point represents the location on the flow accumulation raster that the system should end at. In this project it was at the exit of the lake where the water continues downstream.
![Pour Point](https://github.com/samrknight12/GIS_Projects/blob/master/Chapman%20Lake%20Watershed%20Delineation/Screenshots/PourPoint.PNG)
- The Watershed is created.
    - Watershed Tool: Determines the contributing area of cells in a raster based on the D8 direction raster and the pour point.
    - The output raster was then converted from raster to polygon using the Raster To Polygon tool.
