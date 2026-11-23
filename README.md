# telecom-analysis
## Objetivo del proyecto
Evaluar el comportamiento de los clientes de la empresa de telecomunicaciones ConnectaTel en Latinoamérica con información registrada hasta el año 2024. El propósito es explorar, limpiar y analizar los datos para construir un perfil estadístico de los clientes, detectar comportamientos atípicos y crear segmentos (por uso y por edad) que permitan identificar patrones de consumo, diseñar estrategias de retención y sugerir mejoras en los planes ofrecidos.

## Datasets utilizados
/datasets/plans.csv (plans / plans_original): información de los planes actuales (Basico y Premium), incluyendo precio mensual (usd_monthly_pay), minutos incluidos (minutes_included), mensajes incluidos (messages_included), GB incluidos (gb_per_month) y costos por excedente (usd_per_gb, usd_per_message, usd_per_minute). Tiene 2 filas y 8 columnas.  

/datasets/users_latam.csv (users / users_original): información de los 4,000 clientes en 8 columnas (user_id, first_name, last_name, age, city, reg_date, plan, churn_date).  

/datasets/usage.csv (usage / usage_original): detalle del uso real de llamadas (call) y mensajes de texto (text) con 40,000 registros y 6 columnas (id, user_id, type, date, duration, length)

## Etapas del análisis
1. Carga y exploración: importación de librerías (pandas, seaborn, matplotlib.pyplot), carga de los CSV, revisión preliminar con .head() y revisión de estructura/tipos de datos con .info().
   
3.  Identificación de problemas de calidad de datos: cálculo de cantidad y proporción de valores nulos; detección de valores inválidos y sentinels en variables numéricas (.describe()) y categóricas (.unique()); conversión de fechas (pd.to_datetime) y detección de fechas fuera de rango o nulas.
   
4. Limpieza básica de datos: reemplazo del sentinel -999 en age por la mediana de edades válidas; sustitución de "?" por pd.NA en city; marcado como pd.NaT de fechas en reg_date posteriores al 2024-12-31; y verificación de que los nulos en duration y length son MAR (dependientes de type siendo 'text' o 'call'), manteniéndolos como nulos.
   
5. Summary statistics de uso por usuario: creación de columnas auxiliares para llamadas y mensajes, agrupación por user_id (usage_agg) con cant_mensajes, cant_llamadas y cant_minutos_llamada, unión con users (user_profile) y obtención de resúmenes estadísticos numéricos y distribución porcentual de plan.
   
6. Visualización de distribuciones y outliers: generación de histogramas (sns.histplot) para age, cant_mensajes, cant_llamadas y cant_minutos_llamada, identificación visual de valores atípicos mediante boxplots (sns.boxplot), cálculo de límites inferiores y superiores con el método IQR y decisión de conservar los outliers al representar consumo real.
   
7. Segmentación de clientes: creación de grupo_uso (Bajo uso, Uso medio, Alto uso) según cantidades de llamadas y mensajes, creación de grupo_edad (Joven, Adulto, Adulto mayor) según age, y visualización de ambos segmentos.   

## Cómo ejecutar el notebook
Entorno: abre el notebook en un entorno de Python compatible, como Jupyter Notebook o Google Colab

Dependencias: asegúrate de tener instaladas las librerías pandas, seaborn y matplotlib

Archivos de entrada: comprueba que los archivos /datasets/plans.csv, /datasets/users_latam.csv y /datasets/usage.csv existan en las rutas esperadas

Orden de ejecución: corre las celdas de manera secuencial (execution_count 1 a 56), ya que cada bloque depende de objetos y transformaciones generadas previamente.

## Guía de reproducción
-Bloque de importación y carga: importa pandas, seaborn y matplotlib.pyplot, y lee los tres CSV para crear los DataFrames plans, users y usage.

-Bloque de exploración y calidad: inspecciona .shape, .info(), conteos y proporciones de nulos (.isna().sum()), resúmenes con .describe(), valores únicos categóricos y los años presentes en reg_date y date.

-Bloque de limpieza: sustituye -999 en age por la mediana (calculada sin incluir -999), cambia "?" en city por pd.NA, asigna pd.NaT a fechas de reg_date mayores a 2024-12-31 y revisa cómo se asocian los nulos de duration y length según type.

-Bloque de agregación y unión: agrupa por user_id para obtener el total de mensajes (type == 'text'), el total de llamadas (type == 'call') y la suma de duration; renombra las columnas a cant_mensajes, cant_llamadas y cant_minutos_llamada, y une con users por user_id para formar user_profile.

-Bloque de distribuciones y outliers: construye los histogramas y boxplots de las variables numéricas clave, calcula los límites con IQR ($Q1 - 1.5 \times IQR$ y $Q3 + 1.5 \times IQR$) para las columnas de uso y mantiene los valores extremos.   

-Bloque de segmentación y gráficos: aplica clasificar_uso y clasificar_edad mediante .apply(..., axis=1) para crear grupo_uso y grupo_edad, y genera las gráficas de distribución correspondientes
