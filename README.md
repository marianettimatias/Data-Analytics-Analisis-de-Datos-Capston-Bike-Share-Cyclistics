# Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics
**Resolución de un caso práctico del curso de Análisis de Datos de Google brindado por Coursera.** <br>

El objetivo del proyecto es aplicar los conceptos y las herramientas obtenidas en el curso. Para el mismo se usa una empresa ficticia de alquiler de bicicletas compartidas y realizar el análisis de los datos obtenidos del uso de las mismas por los distintos usuarios.
Para el mismo se uso como herramienta R y RStudio.

### Escenario
Eres un analista de datos júnior que trabaja en el equipo de análisis de marketing de Cyclistic. Cyclistic es una empresa de bicicletas  compartidas  de  Chicago, cuentar con una flota de 5824 bicicletas geolocalizadas y conectadas a una red de 692 estaciones en Chicago. Las bicicletas se pueden desbloquear en una estación y devolver en cualquier otra del sistema en cualquier momento.
Los clientes que compran pases para un solo viaje o para un día completo se denominan pasajeros ocasionales. Los clientes que compran membresías anuales son miembros de Cyclistic.
El director de marketing cree que el éxito futuro de la empresa depende de maximizar el número de suscripciones anuales. Por lo tanto, tu equipo quiere comprender cómo los usuarios ocasionales y los usuarios anuales usan las bicicletas de Cyclistic de forma diferente.A partir de esta información, tu equipo diseñará una nueva estrategia de marketing para convertir a los usuarios ocasionales en usuarios anuales. Pero primero, los ejecutivos de Cyclistic deben aprobar tus recomendaciones, por lo que deben estar respaldadas por información convincente y visualizaciones de datos profesionales.

### Steakholders
* Lily Moreno: Directora de marketing y su gerente. Moreno es responsable del desarrollo de campañas e iniciativas para promover el programa de bicicletas compartidas.
* Equipo de análisis de marketing cíclico: un equipo de analistas de datos que son responsables de recopilar, analizar e informar datos que ayudan a guiar la estrategia de marketing cíclico.
* Equipo ejecutivo de Cyclistic: El equipo ejecutivo notoriamente orientado a los detalles decidirá si se debe aprobar el programa de marketing recomendado.

### Tarea empresarial

Moreno se ha marcado un objetivo claro: diseñar estrategias de marketing dirigidas a convertir a los ciclistas ocasionales en socios anuales. Para ello, el equipo necesita comprender mejor cómo se diferencian los socios anuales y los ciclistas ocasionales, por qué estos últimos adquieren una membresía y cómo los medios digitales podrían influir en sus estrategias de marketing.

**Para la resolución del mismo nos basamos en las 6 etapas del Análisis de Datos:**
## Etapas
* [Preguntar](#preguntar)
- Preparar.
- Procesar.
- Analizar.
- Visualizar.
- Actuar.

### Preguntar

**Preguntas para analizar** <br>

¿Cuál es el porcentaje actual de ciclistas ocasionales y miembros anuales en el total de usuarios de las bicicletas compartidas?<br>

¿Cuáles son las diferencias clave en el uso de las bicicletas Cyclistic entre los ciclistas ocasionales y los miembros anuales? Respecto a:<br>
* El tiempo de uso de la bicicleta.<br>
* Los días que más usan las bicicletas y el tipo de bicicletas.<br>
* El horario durante el día en el que recogen la bicicleta para hacer uso del servicio.<br>

¿En qué se diferencian los socios anuales y los ciclistas ocasionales con respecto al uso de las bicicletas de Cyclistic?.<br>

### Preparar

Los datos han sido puestos a disposición por Motivate International Inc. 
Se seleccionaron los datos correspondientes a los 12 meses del año 2022.

```{r Importo los datos}

#Importo los datos

viajes_202201 <- read_csv("202201-divvy-tripdata.csv")
viajes_202202 <- read_csv("202202-divvy-tripdata.csv")
viajes_202203 <- read_csv("202203-divvy-tripdata.csv")
viajes_202204 <- read_csv("202204-divvy-tripdata.csv")
viajes_202205 <- read_csv("202205-divvy-tripdata.csv")
viajes_202206 <- read_csv("202206-divvy-tripdata.csv")
viajes_202207 <- read_csv("202207-divvy-tripdata.csv")
viajes_202208 <- read_csv("202208-divvy-tripdata.csv")
viajes_202209 <- read_csv("202209-divvy-tripdata.csv")
viajes_202210 <- read_csv("202210-divvy-tripdata.csv")
viajes_202211 <- read_csv("202211-divvy-tripdata.csv")
viajes_202212 <- read_csv("202212-divvy-tripdata.csv")

```
Con los datos ya importados en mi entorno creo una lista de los dataset para facilitar la verificación de los mismos.

```{r Creo lista}
lista_viajes_bici <- list(viajes_202201,
                          viajes_202202,
                          viajes_202203,
                          viajes_202204,
                          viajes_202205,
                          viajes_202206,
                          viajes_202207,
                          viajes_202208,
                          viajes_202209,
                          viajes_202210,
                          viajes_202211,
                          viajes_202212)
```

Una vez creada la lista verifico la estructura de los datos para asegurarme que estén completos, sean relevantes para el análisis y libres de incongruencias.

```{r Verifico estructura de los datos}
for (df in lista_viajes_bici) {
 print(str(df))
}
```
Lo cual arroja un resultado como el siguiente para cada dataset <br>

![estructura dataset](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/8d5f5960dceb16c217bdb0a2c993913ce60f4290/Imagenes/str.png)

Una vez verificado que todos los dataset tienen la misma estructura y formato de datos los uno a todos en un solo dataset.

```{r Uno los dataset}
Viajes_bici_completo <- bind_rows(lista_viajes_bici)
```
Elimino las columnas que poseen los datos de localización de inicio y fin de cada viaje.
```{r Elimino columnas de localización}
Viajes_bici <- Viajes_bici_completo %>%
   select(-c(start_lat,start_lng,end_lat,end_lng))
```
Verifico si hay columnas que no contienen algún dato o datos na

```{r columnas na}
names(Viajes_bici)[colSums(is.na(Viajes_bici))>0]
```
Lo cual me arroja el siguiente resultado, que significa que las columnas que no contienen datos osea con datos na son "start_station_name" "start_station_id"   "end_station_name"   "end_station_id" . <br>

![columnas con na](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/31f6f7e1da22ccee78bf2f4158e0003718afaaab/Imagenes/columnas%20con%20na.png)

