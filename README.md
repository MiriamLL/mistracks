
<!-- README.md is generated from README.Rmd. Please edit that file -->

# sula <img src="man/figures/logo.png" align="right" width = "120px"/>

[![DOI](https://zenodo.org/badge/354821022.svg)](https://zenodo.org/badge/latestdoi/354821022)

This packages contains: <br>  
- **Tracking data** from Masked boobies *(Sula dactylatra)* collected at
Easter Island.  
- **Functions** to clean your data and calculate the foraging trip
parameters of the individuals.

**[Enlace en
español](https://www.miriam-lerma.com/posts/2021-05-14-sula/).**

## Installation

``` r
# install.packages("devtools")
devtools::install_github("MiriamLL/sula")
```

``` r
library(sula)
```

# Funciones

## ajustar\_hora 🕐

This function allows you to correct the time of the GPS according to
your GMT. You need to provide your data frame, the name of you columns
containing time and date, the format of those columns and the time
difference between the data and your GMT. It returns hora\_corregida
with the corrected time.

``` r
GPS_gmt<-ajustar_hora(GPS_data = GPS_raw,
                      dia_col = 'DateGMT',
                      hora_col = 'TimeGMT',
                      formato="%d/%m/%Y %H:%M:%S",
                      dif_hor = 5)
```

## Un individuo

### recortar\_periodo

This function allows you to cut specific periods in your data. Inicio
means start, Final means end.

``` r
GPS_recortado<-recortar_periodo(GPS_data=GPS_01,
                                inicio='02/11/2017 18:10:00',
                                final='05/11/2017 14:10:00',
                                dia_col='DateGMT',
                                hora_col='TimeGMT',
                                formato="%d/%m/%Y %H:%M:%S")
#> El track original contenia 1038 filas y el track editado contiene 986 filas
```

#### localizar\_nido 🐣

This functions identifies the location of the nest assuming that the
first column is the nest location.

``` r
nest_loc<-localizar_nido(GPS_data = GPS_01,
                          lat_col="Latitude",
                          lon_col="Longitude")
```

#### identificar\_viajes 🛩️

This function adds a column with the distance from the nest, and a
column to let you know if the location corresponds to a foraging trip or
not. You need to give a buffer in **distancia\_km**.

``` r
GPS_trip<-identificar_viajes(GPS_data=GPS_01,
                        nest_loc=nest_loc,
                        distancia_km=1)
```

#### contar\_viajes 🧮

This function gives a number to each trip and eliminates the locations
that do not correspond to trips.

``` r
GPS_edited<-contar_viajes(GPS_data=GPS_trip)
```

#### dist\_colonia 📏

This function calculates the distance from the colony of each location.
Returns a new column called ‘maxdist\_km’.

``` r
GPS_dist<-dist_colonia(GPS_data = GPS_edited, nest_loc=nest_loc)
```

#### dist\_puntos 📐

This function calculates the distance between each location. Returns a
new column called ‘pointsdist\_km’.

``` r
GPS_dist<-dist_puntos(GPS_data = GPS_edited)
```

#### calcular\_duracion ⏳

This function calculates the duration of each trip. Returns a new data
frame with four columns: trip\_id, trip\_start, trip\_end y duration.

``` r
duracion<-calcular_duracion(GPS_data = GPS_edited,
                              col_diahora = "tStamp",
                              formato = "%Y-%m-%d %H:%M:%S",
                              unidades="hours")
```

#### calcular\_totaldist 📐

This function calculates the total distance travelled by each trip.

``` r
totaldist_km<-calcular_totaldist(GPS_edited = GPS_edited)
```

#### calcular\_maxdist 📏

This function calculates the maximum distance of each foraging trip.

``` r
maxdist_km<-calcular_maxdist(GPS_data = GPS_edited, nest_loc=nest_loc)
```

#### calcular\_tripparams 📐⏳📏

Calculates trip duration, total distance from the colony and maximum
distance and returns a data frame with the information.

``` r
trip_params<-calcular_tripparams(GPS_data = GPS_edited,
                              col_diahora = "tStamp",
                              formato = "%Y-%m-%d %H:%M:%S",
                              nest_loc=nest_loc)
```

#### recortar\_por\_ID ✂️✂️✂️

This function allows you to cut specific periods in your data using a
data frame with field information (Notas).

``` r
GPS_recortados<-recortar_por_ID(GPS_data=GPS_raw,
                                Notas=Notas,
                                formato="%d/%m/%Y %H:%M:%S")
```

#### localizar\_nidos 🐣🐣🐣

This functions identifies the location of the nest from several
individuals assuming that the first column is the nest location.

``` r
Nidos_df<-localizar_nidos(GPS_data=GPS_raw,
                         lon_col="Longitude",
                         lat_col="Latitude",
                         ID_col="IDs")
```

#### preparar\_varios 🔌🔌🔌

This functions adds the columns: maximum distance from the colony per
location, distance between locations, trip number

``` r
GPS_preparado<-preparar_varios(GPS_data=GPS_raw,ID_col="IDs",
                               lon_col="Longitude",lat_col="Latitude",
                               distancia_km=1,sistema_ref="+init=epsg:4326")
```

#### tripparams\_varios 📐📐📐

This function calculates trip parameters for several individuals.

``` r
trip_params<-tripparams_varios(GPS_data=GPS_preparado,
                               col_ID = "IDs",
                               col_tripnum="trip_number",
                               ol_diahora="hora_corregida")
```

# Citation

The data comes from this publications. 🔓  
- Lerma M, Dehnhard N, Luna-Jorquera G, Voigt CC, Garthe S (2020)
Breeding stage, not sex, affects foraging characteristics in masked
boobies at Rapa Nui. Behavioral ecology and sociobiology 74: 149.

-   Lerma M (2021) Package sula. Zenodo.
    <http://doi.org/10.5281/zenodo.4682898>

[![DOI](https://zenodo.org/badge/354821022.svg)](https://zenodo.org/badge/latestdoi/354821022)
