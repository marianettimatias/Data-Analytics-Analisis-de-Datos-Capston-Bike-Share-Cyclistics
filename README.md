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

Tres preguntas guiarán el futuro programa de marketing:

1- ¿En qué forma diferente utilizan las bicicletas ciclistas los socios anuales y los ciclistas ocasionales?<br>
2- ¿Por qué los ciclistas ocasionales comprarían membresías anuales de Cyclistic?<br>
3- ¿Cómo puede Cyclistic utilizar los medios digitales para influir en los ciclistas ocasionales para que se conviertan en miembros?<br>

**Moreno me ha asignado la primera pregunta a responder: ¿En qué forma diferente utilizan las bicicletas Cyclistic los miembros del programa y los ciclistas casuales?**<br>

Para responder esto responderemos las siguientes preguntas:<br>

1- ¿Cuál es el porcentaje actual de ciclistas ocasionales y miembros anuales en el total de usuarios de las bicicletas compartidas?<br>

2- ¿Cuáles son las diferencias clave en el uso de las bicicletas Cyclistic entre los ciclistas ocasionales y los miembros anuales? Respecto a:<br>
* El tiempo de uso de la bicicleta.<br>
* Los días que más usan las bicicletas y el tipo de bicicletas.<br>
* El horario durante el día en el que recogen la bicicleta para hacer uso del servicio.<br>

### Preparar

Los datos han sido puestos a disposición por Motivate International Inc. 
Se seleccionaron los datos correspondientes a los 12 meses del año 2022.

*Importo los datos*
```{r Importo los datos}

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
*Con los datos ya importados en mi entorno creo una lista de los dataset para facilitar la verificación de los mismos.*

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

### Conclusión luego de preparar los datos:

Cada dataset contiene 13 columnas las cuales son:
* ride_id:  id del viaje. Formato: col_character().
* rideable_type: Tipo de bicicleta que se uso en ese viaje. Formato: col_character().
* started_at: Fecha y hora a la que se inicio el viaje. Formato: col_datetime(format = "").
* ended_at: Fecha y hora a la que finalizó el viaje. Formato: col_datetime(format = "").
* start_station_name: nombre de la estación en la que se inicio el viaje. Formato: col_character().
* start_station_id: id de la estación en la que se inicio el viaje. Formato: col_character().
* end_station_name: Nombre de la estación en la que finalizó el viaje. Formato: col_character().
* end_station_id: id de la estación en la que finalizó el viaje. Formato: col_character().
* start_lat: Localización de latitud en la que se inició el viaje. Formato: col_double().
* start_lng: Localización de longitud en la que se inició el viaje. Formato: col_double(). col_double().
* end_lat: Localización de latitud en la que finalizó el viaje. Formato: col_double().
* end_lng: Localización de longitud en la que finalizó el viaje. Formato: col_double(). col_double().
* member_casual: Tipo de usuario. Formato: col_character().

Por lo tanto, podemos concluir que:

Los datos son confiables, ya que se obtuvieron directamente de la compañía que administra las bicicletas de alquiler, lo que minimiza el riesgo de manipulaciones o errores de terceros.
Los datos contienen toda la información necesaria para responder a las preguntas empresariales, como la identificación de cada viaje, así como la fecha y hora de inicio y fin de cada uno, y el tipo de miembro que realizó cada viaje.
Los datos eran actuales y corresponden al período estudiado.

## Procesar

En esta etapa se llevará a cabo la limpieza de los datos, que consistirá en seleccionar las columnas que contienen la información necesaria para realizar el análisis, verificar la ausencia de datos relevantes, eliminar duplicados y confirmar que los datos tengan el formato adecuado.

Una vez verificado que todos los conjuntos de datos (datasets) tienen la misma estructura y formato, los combinaré en un solo dataset.

*Combino los datos.*

```{r Uno los dataset}
Viajes_bici_completo <- bind_rows(lista_viajes_bici)
```
Obtengo un solo dataset con 5.667.717 registros.

*Elimino las columnas que poseen los datos de localización de inicio y fin de cada viaje ya que no son relevantes para el análisis.*

```{r Elimino columnas de localización}
Viajes_bici <- Viajes_bici_completo %>%
   select(-c(start_lat,start_lng,end_lat,end_lng))
```
*Verifico si hay columnas que no contienen datos o que contienen valores nulos (NA).*

```{r Columnas na}
names(Viajes_bici)[colSums(is.na(Viajes_bici))>0]
```
Lo cual me arroja el siguiente resultado:

![columnas con na](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/31f6f7e1da22ccee78bf2f4158e0003718afaaab/Imagenes/columnas%20con%20na.png)

Que significa que las columnas que no contienen datos osea con datos na son "start_station_name" "start_station_id"   "end_station_name"   "end_station_id" . <br>
Debido a que la falta de estos datos no influye en mi análisis, no haré nada con estos.

*Elimino las filas duplicadas*

```{r Elimino las filas}
Viajes_bici <- Viajes_bici %>%
  distinct()
```
Se mantiene la misma cantidad de filas por lo que se conluye que no habían filas duplicadas.

*Agrego una columna en la que calculo la duración de los viajes en minutos.*

```{r Calculo la duración de los viajes}
Viajes_bici <- Viajes_bici %>%
  mutate(duracion_viajes = difftime(ended_at, started_at, units = "mins"))
```
La columna creada tiene formato de tiempo, la convierto en formato numérico para el análisis.

```{r Convierto a formato numérico}
Viajes_bici <- Viajes_bici %>%
  mutate(duracion_viajes = as.numeric(duracion_viajes))
```

*Verifico si existen viajes con duración 0 o negativa.*

```{r Busco viajes con duración 0 o negativa}
any(Viajes_bici$duracion_viajes <= 0, na.rm = TRUE)
```
Por consola me arroja TRUE lo que significa que hay valores 0 o negativos.

*Elimino las filas con duración 0 o negativa.*

```{r Elimino las filas con duración 0 o negativa}
Viajes_bici <- Viajes_bici %>%
  filter(duracion_viajes >0)
```
Luego de esto obtengo un dataset con 5.667.186 registros.

Con esto conlcuimos la etapa de Preparar los datos y el dataset está listo para pasar a la etapa de Análisis.

## Análisis.

Con los datos ya limpios pasamos a la etapa de análisis donde usaremos los mismos para responder las preguntas empresariales.

*¿Cuál es el porcentaje actual de ciclistas ocasionales y miembros anuales en el total de usuarios de las bicicletas compartidas?*

```{r Calculo del pocentaje de cada tipo de ciclista que uso el servicio}
porcentaje_tipo_usuarios <- Viajes_bici %>%
  group_by(member_casual) %>%
  summarize(cant_usuarios = n(), porcentaje = n()/ sum(nrow(Viajes_bici))*100, .groups = "drop")
```
Lo cual nos arroja el siguiente resultado:

![Porcentaje tipo de usuario](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/04aec4508b073b7f4ef7e981a31682746ec7b984/Imagenes/Porcentaje%20tipo%20de%20usuario.png)

El porcentaje de ciclistas miembros que usan el servicio es de 59.03% y el de ciclistas casuales es de 40.97%.

*¿Cuáles son las diferencias clave en el uso de las bicicletas Cyclistic entre los ciclistas ocasionales y los miembros anuales? Respecto a:<br>*
* El tiempo de uso de la bicicleta.<br>

Calculamos el tiempo promedio, mínimo y máximo de uso por tipo de usuario en minutos.

```{r Tiempo de uso}
tiemp_uso_por_tipo_de_usuario <- Viajes_bici %>%
  group_by(member_casual)%>%
  summarize(prom_uso = mean(duracion_viajes), min_uso = min(duracion_viajes), max_uso = max(duracion_viajes), mediana_uso = median(duracion_viajes))
```
Obtenemos los siguientes resultados:

![Tiempos de uso por tipo de usuario](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/e7e7c6dd8a31b963c92bbc61dab296eb6bea3059/Imagenes/Tiempo%20de%20uso%20por%20tipo%20de%20usuario.png)

A partir de los resultados obtenidos podemos observar que los usuarios casuales tienen un promedio de uso mayor a los miembros del programa.
Se observa que para los usuarios casuales el valors máximo es extremadamente altos, lo que puede deberse a un caso atípico. De igual manera los valores mínimos para ambos miembros son muy bajos por lo que puede deberse a situaciones accidentales.
<br>
Sería importante realizar un análisis de los valores atípicos para evaluar la influencia de los valores extremos en las conclusiones.
De igual manera se obtiene una mediana de 13 minutos para los usuarios casuales y una mediana de 8,83 minutos para los usuarios miembros, lo que se sigue observando la diferencia de uso entre los dos grupos.
Por lo que podemos decir que los usuarios miembros del programa hacen uso de las bicicletas para recorridos más cortos.

*Días y horario en que hacen uso del servicio<br>

Creo una nueva columna para el día de uso

```{r Creo una nueva columna para el día de uso}
Viajes_bici <- mutate(Viajes_bici, dia_uso = (weekdays(started_at)))
```
Ordeno los días

```{r Ordeno los días}
Viajes_bici$dia_uso <-factor(Viajes_bici$dia_uso, levels = c("domingo", "lunes", "martes", "miércoles", "jueves","viernes", "sábado"))
```
Calculo el promedio de uso por día de la semana por tipo de usuario

```{r Promedio de uso por día de la semana por tipo de usuario}
prom_dia_tipo_de_usuario <- Viajes_bici %>%
  group_by(member_casual, dia_uso) %>%
  summarize(prom_uso_min = mean(duracion_viajes))
```

![Tiempo de uso por día](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/5d3fbc31c0fc81596dfba56fefe65fc0614190d7/Imagenes/Tiempo%20de%20uso%20por%20d%C3%ADa.png)

Acá podemos observar que tanto los miembros del programa como los usuarios casuales tienen un promedio de uso mayor de viernes a lunes.
También haciendo foco en los miembros del programa podemos ver que durante los días de semana el promedio de uso se mantiene estable, lo que se podría decir es que hacen uso de las bicicletas para tareas de rutina.

*Franja horaria del día en que hacen uso del servicio<br>

Primero creo una función para determinar la franja horaria en que hicieron uso.

```{r Función franja horaria}
horario <- function(hora){
  case_when(
    hora >= 0 & hora < 6 ~ "madrugada",
    hora >= 6 & hora < 12 ~ "mañana",
    hora >= 12 & hora < 18 ~ "tarde",
    TRUE ~ "noche"
  )
}
```
Creo la columna en la cual guardaré la hora del día en que hicieron uso del servicio para luego al pasarla por la función obtendre la franja horaria.

```{r Hora de uso}
Viajes_bici <- Viajes_bici %>%
  mutate(hora = hour(started_at))
```

Creo la columna en la que guardaré la etiqueta de la franja horaria.

```{r Columna franja horaria}
Viajes_bici <- Viajes_bici %>%
  mutate(franja_horaria = horario(hora))
```
Ordeno los días

```{r Ordeno los días}
Viajes_bici$franja_horaria <- factor(Viajes_bici$franja_horaria, levels = c("madrugada", "mañana", "tarde", "noche"))
```
Calculo el promedio de uso por franja horaria y por tipo de usuario

```{r Promedio de uso por franja horaria y por tipo de usuario}
prom_uso_franja_horaria <- Viajes_bici %>%
  group_by(member_casual, franja_horaria) %>%
  summarize(promedio = mean(duracion_viajes), .groups = "drop")
```
Obtengo los siguientes resultados:

![Promedio de uso por franja horaria](https://github.com/marianettimatias/Data-Analytics-Analisis-de-Datos-Capston-Bike-Share-Cyclistics/blob/de5f98d1bab625d2e34e44a10c70653f031c4143/Imagenes/prom%20uso%20franja%20horaria.png)


