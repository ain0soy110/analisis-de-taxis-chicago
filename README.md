Análisis del Impacto de Factores Externos en el Servicio de Transporte de Pasajeros en Chicago

Este proyecto de análisis predictivo y estadístico examina el comportamiento de los viajes en taxi en la ciudad de Chicago utilizando conjuntos de datos consolidados mediante consultas SQL. El objetivo principal es identificar patrones de demanda en los principales distritos comerciales, evaluar la competitividad y cuota de mercado de las empresas de transporte, y validar estadísticamente el impacto de las condiciones climáticas adversas en la duración de los traslados hacia nodos de conectividad clave.

El desarrollo de este proyecto simula el flujo de trabajo de un analista de datos en un entorno de producción, abarcando desde la auditoría de calidad de la información hasta la ejecución de pruebas de hipótesis para la toma de decisiones estratégicas de negocio.

Estructura de Dependencias y Entorno de Ejecución
Para garantizar la reproducibilidad del análisis, se definen las siguientes librerías core dentro del ecosistema de Python:

Pandas y NumPy: Utilizados para la reestructuración de dataframes, optimización de tipos de datos y agregaciones vectoriales.

Matplotlib y Seaborn: Implementados para el diseño de la arquitectura de visualización de distribuciones y detección visual de valores atípicos.

SciPy (Módulo Stats): Utilizado para el cálculo probabilístico y contraste de hipótesis inferenciales.

Estructura de los Conjuntos de Datos

El pipeline procesa tres conjuntos de datos específicos extraídos de la base de datos relacional de la compañía de movilidad:

project_sql_result_01.csv (Rendimiento por operador):

company_name: Identificador textual de la empresa de taxis.

trips_amount: Volumen total de viajes completados el 15 y 16 de noviembre de 2016.

project_sql_result_04.csv (Distribución geográfica de destinos):

dropoff_location_name: Nombre del barrio de finalización del trayecto.

average_trips: Promedio de viajes diarios que finalizaron en dicha localización durante el mes de noviembre de 2016.

project_sql_result_07.csv (Registros de trayectos específicos los días sábados):

start_ts: Marca de tiempo de inicio del viaje.

weather_conditions: Clasificación de la condición climática del intervalo (Good o Bad).

duration_seconds: Duración total del traslado en segundos (Variable objetivo).

Procesamiento de Datos y Rigor Metodológico

Se realizó un control de calidad inicial mediante métodos de inspección estructural (.info(), .describe()). Si bien no se detectaron registros nulos o faltantes que comprometieran la representatividad muestral, se ejecutaron las siguientes correcciones de tipos de datos para asegurar el rendimiento informático y la precisión analítica:

1. Optimización y Conversión de Variables Temporales

La variable start_ts se importó originalmente como una cadena de texto (tipo object). Se transformó al tipo nativo de fecha y hora para permitir filtros temporales cronológicos eficientes:

weather_trips_df['start_ts'] = pd.to_datetime(weather_trips_df['start_ts'])

2. Conversión a Tipos de Datos Discretos

Las variables duration_seconds y average_trips presentaban formatos de punto flotante de precisión doble (float64). Debido a la naturaleza intrínseca de los datos (segundos transcurridos y conteo promedio de trayectos), se reformatearon a enteros de 64 bits (int64), optimizando el uso de memoria en producción.

Análisis Exploratorio de Datos (EDA)

Concentración del Mercado por Operador
El análisis descriptivo del volumen de trayectos revela una asimetría pronunciada en la cuota de mercado. Un grupo selecto de empresas consolidadas absorbe la mayor parte de la demanda operativa en Chicago, mientras que un número extenso de competidores menores opera en la periferia transaccional.

Identificación de Centros Logísticos y Destinos Críticos

Al clasificar y ordenar de manera descendente los barrios de finalización de viajes, se determinó que los diez distritos con mayor afluencia representan los principales centros financieros, comerciales y turísticos de la ciudad. Las zonas de Loop, River North y Streeterville encabezan de forma contundente la recepción de pasajeros, reflejando la necesidad de concentrar la oferta vehicular en estos cuadrantes geográficos específicos durante las horas de alta demanda.

Análisis Estadístico Inferencial (Prueba de Hipótesis)

Un componente crítico del proyecto consistió en evaluar si los factores exógenos climáticos alteran de manera estadísticamente significativa la eficiencia del servicio en rutas clave.

Planteamiento de Hipótesis

Se estructuró un contraste de hipótesis para evaluar la duración promedio de los trayectos desde el distrito financiero (Loop) hasta el Aeropuerto Internacional O'Hare en días sábados, bajo dos escenarios climáticos:

Hipótesis Nula ($H_0$): La duración promedio de los viajes desde el Loop hasta el Aeropuerto Internacional O'Hare es idéntica en días con condiciones climáticas buenas y en días con condiciones climáticas malas.Hipótesis Alternativa ($H_1$): La duración promedio de los viajes desde el Loop hasta el Aeropuerto Internacional O'Hare difiere significativamente cuando cambian las condiciones climáticas.

Criterio y Ejecución de la PruebaSe aplicó una prueba t de Student para dos muestras independientes. Se estableció un nivel de significancia estadística estándar de 0.05 

Tras el contraste, el valor $p$ obtenido resultó significativamente inferior al umbral alfa establecido lo que obligó a rechazar la hipótesis nula en favor de la hipótesis alternativa.

Conclusiones de Negocio y Operaciones
Validación del Impacto Climático: Existe evidencia estadística sólida para afirmar que las malas condiciones climáticas (como lluvias o tormentas los días sábados) incrementan de forma significativa el tiempo de traslado hacia el aeropuerto O'Hare. Esto permite a la dirección de operaciones fundamentar modelos de tarificación dinámica por congestión y recalibrar los tiempos estimados de arribo (ETA).

Planificación de la Oferta Vehicular: La identificación de los diez barrios principales de destino proporciona una base cuantitativa para la distribución estratégica de la flota. Concentrar la disponibilidad de unidades en puntos neurálgicos como el Loop minimiza los tiempos de espera y optimiza la tasa de conversión de la plataforma frente a los competidores tradicionales.

Instrucciones para la Reproducción del Proyecto
Asegurar que los datos de origen se encuentren localizados en una carpeta interna denominada datasets/ dentro del directorio raíz del espacio de trabajo.

Instalar las librerías obligatorias mediante la terminal de comandos:

Bash
pip install -r requirements.txt
Iniciar la interfaz de desarrollo y ejecutar secuencialmente todas las celdas del documento:

Bash
jupyter notebook taxis-chicago.ipynb