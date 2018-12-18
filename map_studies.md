## Mapping studies
```
#map studies
library(rworldmap)
library(ggplot2)
library(readr)
#read data
meta_latlong <- read_delim("C:/Users/patty/Dropbox/R/host_meta/meta_latlong.txt", "\t", escape_double = FALSE, trim_ws = TRUE)

#make columns of lat/long numeric
meta_latlong$latitude_deg<-as.numeric(meta_latlong$latitude_deg)
meta_latlong$longitude_deg<-as.numeric(meta_latlong$longitude_deg)
str(meta_latlong)
# Get world map from rworldmap
worldmap <- map_data("world")
str(worldmap)


# Plot entire world map with studies 
ggplot(worldmap, aes(x = long, y = lat, group = group)) +
  geom_polygon(colour = "grey", fill = "grey") +
  theme_bw() +
  geom_point(data=meta_latlong, aes(x=longitude_deg, y=latitude_deg, fill=microbial_habitat_type, colour=microbial_habitat_type, group=microbial_habitat_type), size = 3)+
  ylab("Latitude")+
  xlab("Longitude")+
  coord_cartesian


microbial_habitat_type


ggplot(data = worldmap, aes(x = long, y = lat, group = group)) +
  geom_polygon(colour = "grey", fill = "grey") +
  theme_bw() +
  geom_point(data = acc_map, aes(x = long, y = lat, fill=Cat, colour = Cat, group=Cat), size = 3)+
  ylab("Latitude")+
  xlab("Longitude")+
  scale_colour_manual(values=pal)+
  scale_fill_manual(values=pal)
  ```
