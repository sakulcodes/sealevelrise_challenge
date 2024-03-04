
#load required libraries:
library(ncdf4) ; library(raster) ; library(ggplot2)

#load the dataset: (just a single observation - time stamp 1)
data <- nc_open("/Users/sakul/Desktop/SLR-Challenge/1993/01/dt_global_twosat_phy_l4_19930101_vDT2021.nc")
print(data) #prints the summary
names(data$var) #prints the variable names:

#extracting the variables:
latitude <- ncvar_get(data, "lat_bnds") ;  longitude <- ncvar_get(data, "lon_bnds") ; 
sla_data <- ncvar_get(data, "sla") ; error_in_sla <- ncvar_get(data, "err_sla")

#geostrophic velocity anomalies(departure from the mean)
#east-west direction:
ugosa_var <- ncvar_get(data, "ugosa") ; error_in_ugosa <- ncvar_get(data, "err_ugosa")
#north-south direction:
vgosa_var <- ncvar_get(data, "vgosa") ; error_in_vgosa <- ncvar_get(data, "err_vgosa")

#Absolute geostrophic velocity (without anamolies)
ugos_var <- ncvar_get(data, "ugos")  ; vgos_var <- ncvar_get(data, "vgos") 

#coordinate reference system
crs_var <- ncvar_get(data, "crs") #no meaningful information

#---Absolute Dynamic Topography (adt) =  Sea Level Anomaly (sla) + Mean Dynamic Topography (mdt) - units (meter)
adt_var <- ncvar_get(data, "adt") ; dim(adt_var)

#tpa_corrections: corrections applied to the sea surface height (SSH) data to account for instrumental drift observed during the TOPEX-A mission
tpa_var <- ncvar_get(data,"tpa_correction") #just a singel variable

#flag_ice: presence or absence of sea ice in the corresponding grid cells of the dataset. 
flag_ice_var <- ncvar_get(data, "flag_ice")


##-------------------landscape view:--------------------------------------


library(ggplot2) ; library(sf) ; library(rnaturalearth); library(osmdata); library(tmap)
world <- ne_countries(scale = "medium", returnclass = "sf")
# Define bounding box around Gulf of Mexico and US East Coast
bbox <- st_bbox(c(xmin = -100, xmax = -60, ymin = 15, ymax = 45), crs = st_crs(world))
# Filter the world data to the bounding box
area_of_interest <- st_crop(world, bbox)
ggplot() +
  geom_sf(data = area_of_interest) +
  theme_minimal() +
  ggtitle("Landscape Near Gulf of Mexico and US East Coast")


bbox <- c(-81, 25,  # Min longitude (west), Min latitude (south)
          -66, 47)

query <- opq(bbox = bbox) %>%
  add_osm_feature(key = "residential") # Example to get roads, adjust as needed
data <- osmdata_sf(query)

ggplot() +
  geom_sf(data = data$osm_lines) + # Plot roads
  geom_sf(data = data$osm_polygons, fill = "green", alpha = 0.5) + # Plot areas like parks, water, etc.
  theme_minimal()

tm_shape(data$osm_lines) +
  tm_lines() +
  tm_shape(data$osm_polygons) +
  tm_fill(col = "green", alpha = 0.5) +
  tm_borders()

