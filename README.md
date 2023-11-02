# J2DProject: Análisis de la relación entre los Precios de Alquiler y la Seguridad Vial en Barcelona (2017)
                                                                                                          Jhonatan Barcos Gambaro

## 1. Introducción
En primer lugar, describamos los datasets a unificar escogidos y razonamos el por qué de dicha decisión:
Como dicta el enunciado del proyecto, nuestro dataset base será "Alquiler mensual medio (€/mes) y por superficie (€/ m2) de la ciudad de Barcelona - 2017_Lloguer_preu_trim.csv - Open Data Barcelona" al cual hemos denominado 'lloguer_dt'. Por otra parte, como dataset complementario a unificar con el anterior hemos escogido ": Accidents segons causa conductor gestionats per la Guàrdia Urbana a la ciutat de Barcelona - 2017_ACCIDENTS_CAUSA_CONDUCTOR_GU_BCN_.csv - Open Data Barcelona" al cual hemos denominado 'accidents_dt'.

La razón de elegir este dataset frente a ": Población expuesta a los niveles de ruido del Mapa Estratégico de Ruido de la ciudad de Barcelona - 2017_Poblacio_exposada_barris_Mapa_Estrategic_Soroll_BCN_LONG.csv - Open Data Barcelona ", al que denominaremos ‘soroll_dt’, es que a mi parecer, la relación entre soroll_dt era más obvia que la de accidents_dt frente a nuestro dataset base. Sin embargo, tras el análisis correspondiente de lloguer_dt y accidents_dt he comprendido que las hipótesis iniciales resultaron ser similares para ambas posibilidades.

Estos dos datasets: lloguer_dt y accidents_dt han sido unificados en un nuevo dataset: merged_dt.  Este análisis tiene como objetivo proporcionar información valiosa que podría respaldar la toma de decisiones en áreas relacionadas con el mercado inmobiliario y la seguridad vial en la ciudad. Definamos a continuación cada una de las variables que lo componen:
Any: Año de registro de los datos.
Trimestre: Trimestre del año en que se recopilaron los datos.
Codi_Districte: Identificador numérico del distrito de Barcelona.
Nom_Districte: Nombre del distrito.
Codi_Barri: Identificador numérico del barrio de Barcelona.
Nom_Barri: Nombre del barrio.
Descripcio_causa_conductor: Descripción de la causa del accidente de tráfico.
Accidents_causa_trim: Número de accidentes de tráfico causados en un trimestre por una causa específica.
Accidents_trim: Número total de accidentes de tráfico en un trimestre específico.
Preu_mes: Precio medio del alquiler mensual en euros.
Preu_m2: Precio medio del alquiler por metro cuadrado en euros.

El objetivo de análisis de "merged_dt" se basa en identificar patrones y relaciones entre los accidentes de tráfico, los precios de alquiler y otros factores relevantes. Algunos de los objetivos clave incluyen:
Evaluar si existen correlaciones entre los precios de alquiler y la seguridad vial en distintos distritos y barrios.
Identificar las causas más frecuentes de accidentes de tráfico y su distribución a lo largo del tiempo.
Explorar patrones estacionales en accidentes de tráfico y en los precios de alquiler.

# 2. Depuración de datos
En nuestro caso hemos dividido la depuración de datos en dos procesos diferenciados: (2.1) Depuración previa a la unificación, en el cual hemos tratado los datasets por separado y hemos reducido las variables necesarias  y (2.2) Depuración posterior a la unificación, donde hemos seleccionado las variables relevantes del dataset unificado para un fiable tratamiento de datos.

## 2.1. Depuración previa a la unificación
### 2.1.1. Lloguer_dt
En primer lugar, hemos comprobado que existen datos nulos en el dataset. Por lo tanto, para un correcto tratamiento de los mismos hemos optado por descartar las filas que tengan algún dato desconocido. 

Seguidamente, hemos dividido la variable 'Preu' en dos: 'Preu_mes' y 'Preu_trim' a través de la elaboración de una función auxiliar dividir_precio(df). Seguidamente hemos eliminado la variable 'Lloguer_mitja' y hemos eliminado las filas repetidas. Así hemos conseguido reducir el dataset a la mitad sin perder información. Posteriormente, casteamos los valores de estas nuevas columnas a floats para poder trabajar con estos datos más adelante.

### 2.1.2. Accidents_dt
Al igual que con el dataset anterior, comprobamos si existen datos nulos en el dataset y en caso afirmativo, descartamos dichas filas.

Por otra parte, hemos creado una variable 'Trimestre' a través de una función auxiliar mesToTrimestre(mes) la cual detecta según en qué mes nos encontramos 'Mes_any' el trimestre que le corresponde y lo almacena en dicha nueva variable.

Hemos observado que el nombre de dos variables:'Descripcio_causa_conductor' y 'Descripcio_turn' estaban intercambiadas por lo que renombramos dichas variables para un correcto tratamiento de la información posterior.

Agrupamos en una nueva variable 'Accidents_causa_trim' el número de accidentes por trimestre y por causa con objetivo de visualizar de forma más sencilla los accidentes más comunes según el distrito y barrio así como reducir nuestro dataset sin perder información. 

Por último decidimos prescindir de la mayoría de variables del dataset que no resultan relevantes para la unificación con lloguer_dt para evitar el exceso de ruido en nuestros datos.

## 2.2. Depuración posterior a la unificación
Hemos unificado nuestros datasets anteriores en merged_dt a través de las variables comunes 'Any','Trimestre', 'Nom_Districte', 'Nom_Barri' y hemos reorganizado el orden de todas las variables del dataset.

## 3. Resultados
A continuación hemos realizado una serie de análisis de las variables más relevantes del dataset total así como exploramos la correlación existente entre éstas. Por último seleccionaremos una muestra de dicho conjunto de datos y la analizaremos a fondo con tal de poder sacar las conclusiones necesarias para nuestro proyecto.

### 3.1. Análisis del dataset unificado
Para llevar a cabo nuestro objetivo hemos realizado histogramas de las variables numéricas, distintos box plots de las más relevantes y además hemos visualizado la matriz de correlación para ver cómo éstas se relacionaban entre sí.

### 3.2. Análisis de los datos de una muestra
Dada una muestra “sample_dt” escogida de forma aleatoria de nuestro dataset unificado, hemos realizado distintos estudios como distintos plots y éstas han sido las diferentes observaciones, las cuales nos serán de gran utilidad para llegar a determinadas conclusiones.

A través del análisis de la matriz de correlación observamos lo siguiente:
Los precios/mes y precios/m2 están altamente correlacionados positivamente entre sí por lo que si una de éstas variables aumenta, la otra también tenderá a ello.
Los precios/mes y accidentes/trimestre están correlacionados positivamente de manera significativa. Éste hecho será clave para nuestro estudio.

### 3.2.1. Scatter plot: Precio de Alquiler Mensual vs Precio por Metro Cuadrado
* Sarrià-Sant Gervasi, seguido de Les Corts, Eixample y Sant Martí son los distritos con mayor precio de alquiler mensual y por metro cuadrado.
* Nou Barris y Sant Andreu son los distritos con menor precio de renta mensual y por metro cuadrado.
*  El precio de alquiler mensual y el precio por metro cuadrado está correlacionado positivamente, es decir, por norma general si el precio de alquiler mensual aumenta, el precio por metro cuadrado tenderá a aumentar.


### 3.2.2. Scatter plot: Precio de Alquiler Mensual vs Accidentes de Tráfico
* Eixample es el distrito con mayor nº de accidentes el cual tiene un precio de renta mensual por encima de la media.
* Sarrià-Sant Gervasi junto a Les Corts son los distritos con mayor nº de accidentes los cuales además son los segundos distritos con mayor precio de renta mensual.
* Nou Barris es el distrito con menos nº de accidentes el cual además tiene el menor precio medio de renta mensual.
* Distritos con un mayor precio de renta mensual tienden a un gran nº de accidentes por barrio.
* Distritos con menor precio de renta mensual tienden a un menor nº de accidentes por barrio pero con una mayor densidad.

### 3.2.3. Bar plot: Causas de Accidentes de Tráfico
* Las causas principales son: falta de atención, giro indebido o desobedecer semáforo.
* Las causas menos comunes son: avería mecánica, envadir acera contraria o no ceder la calle.

### 3.2.4. Descomposición en componentes principales (PCA)
A través del análisis de la visualización de la dispersión del PCA podemos confirmar los resultados hallados anteriormente: como por ejemplo, las variables Preu_mes y Preu_m2  cuyas varianzsa se distribuyen de manera similar. Esto tiene sentido pues como hemos visto anteriormente, éstas se encuentran positivamente correlacionadas. Así como la varianza de las variables Preu_mes y Accidents_trim las cuales se encuentran correlacionadas significativamente pero no en exceso. Hecho clave para determinar las siguientes conclusiones.

## 4. Conclusiones
### 4.1. Relación entre el precio de alquiler medio mensual y por metro cuadrado
La relación entre el precio de alquiler mensual y el precio por metro cuadrado suele estar positivamente correlacionado. Sin embargo, existen distintas áreas de Barcelona, como por ejemplo Trinitat Vella, en las cuales el precio por metro cuadrado es muy elevado mientras que el precio de alquiler mensual se mantiene en la media. Ésto quiere decir que las viviendas, a pesar de que su renta se mantenga en la media, son viviendas mucho más pequeñas respecto a las de otras áreas costando el mismo precio mensual.

### 4.2. Relación entre Precios de Alquiler y Accidentes de Tráfico:
Se encontró que por norma general, cuando el precio de alquiler era superior a la media, el nº de accidentes de tráfico se disparaba. Sobretodo en el distrito de Eixample. Ésto es debido a una serie de razones:
Densidad de Tráfico: En áreas de Barcelona con un alto costo de alquiler, las cuales generalmente suelen ser más céntricas, es posible que también haya una mayor densidad de tráfico debido a la proximidad a lugares de trabajo, áreas comerciales y otros destinos. Una mayor densidad de tráfico podría estar asociada con un mayor número de accidentes de tráfico.
Factores Económicos: Los precios de alquiler más altos podrían estar relacionados con áreas económicamente prósperas, donde las personas tienden a tener automóviles y conducir con más frecuencia así como que en dichas áreas se establezca una mejor comunicación con las distintas carreteras de la ciudad. Esto podría aumentar la probabilidad de accidentes de tráfico.

Por otra parte, los distritos con precios de alquiler muy inferiores como por ejemplo Nou Barris, el nº de accidentes disminuye drásticamente lo cual puede ser debido a:
Infraestructura vial: En áreas con precios de alquiler más bajos, tratándose por lo general de zonas más alejadas del centro, es posible que la infraestructura vial sea menor por lo que disminuiría la densidad de tráfico en esas zonas. 

### 4.3. Causas de accidentes de tráfico
Las causas más comunes de accidentes de tráfico en Barcelona incluyen "Desatención", "No respetar la señalización" y "Giro indebido", siendo la causa más frecuente la "Desatención", una preocupación importante a tener en cuenta para asegurar la seguridad vial.

