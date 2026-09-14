# README - Consumo Abusivo de SPA en Jovenes de Bogota

# Consumo abusivo de SPA en jóvenes de Bogotá

## Un análisis sociodemográfico desde los datos de VESPA (2019 – 2026)

Proyecto de la asignatura Data Experience. Analiza los casos de consumo abusivo de sustancias psicoactivas (SPA) en adolescentes y jóvenes reportados al sistema de vigilancia epidemiológica VESPA de Bogotá, para identificar patrones sociodemográficos y construir un modelo predictivo de riesgo por localidad.

## Integrantes

- Sara Gabriela Cajamarca Lozano
- Santiago Herrera León
- Raúl Felipe Ropero Barbosa
- Dumar Alexis Ruiz Bernal
- Valeria Vélez Molina

## Pregunta problema

¿Qué características sociodemográficas y de contexto (lugar habitual de consumo, nivel educativo, aseguramiento en salud) predominan en los casos de consumo abusivo de sustancias psicoactivas en adolescentes y jóvenes de Bogotá, reportados al sistema VESPA entre 2019 y 2026?

**Preguntas de apoyo:**

1. ¿Cómo se distribuyen los casos según la localidad de residencia?
2. ¿Cómo ha evolucionado el número de casos a lo largo de los años del periodo?
3. ¿Qué diferencias hay entre hombres y mujeres jóvenes en el número de casos?
4. ¿En qué lugares habituales de consumo se concentran más los casos?
5. ¿Qué relación existe entre el nivel educativo y el tipo de aseguramiento en salud?

**Pregunta del modelo predictivo:** a partir del perfil sociodemográfico de un caso (sexo, nivel educativo, tipo de aseguramiento, lugar habitual de consumo y año), ¿es posible predecir si proviene de una localidad de alto riesgo (Kennedy, Bosa, Suba, Ciudad Bolívar o Engativá)?

## Fuente de datos

- **Origen:** Secretaría Distrital de Salud de Bogotá, subsistema VESPA (Vigilancia Epidemiológica del Consumo de Sustancias Psicoactivas).
- **Portal:** Datos Abiertos Bogotá / Observatorio SaluData.
- **Archivo:** osb_saludmental_-consumoabusivo_spageneral.csv
- **Tamaño original:** 101.500 filas × 21 columnas.
- **Unidad de análisis:** una fila no es una persona; es una combinación de categorías (año, sexo, localidad, etc.) y la columna CASOS indica cuántos casos reales corresponde a esa combinación.

## Estructura del notebook

| # | Sección | Contenido |
|---|---|---|
| 1 | Búsqueda y obtención de datos | Carga del CSV crudo desde el portal de Datos Abiertos |
| 2 | Comprensión del conjunto de datos | Tamaño, tipos de variables, nulos, duplicados, categorías |
| 3 | Limpieza y preparación | Unificación de "sin dato", filtrado a adolescentes/jóvenes 2019-2026, exportación de spa_jovenes_limpio.csv |
| 4 | Análisis exploratorio (EDA) | Respuesta a las 5 preguntas de apoyo con gráficos (barras, líneas, mapa de calor) |
| 5 | Análisis estadístico | Estadística descriptiva de CASOS, prueba de normalidad (Shapiro-Wilk), expansión del dataset, pruebas chi-cuadrado + V de Cramér |
| 6 | Selección del modelo | Definición del problema de clasificación binaria (LOCALIDAD_RIESGO) y justificación de los 3 modelos comparados |
| 7 | Aplicación del modelo | Entrenamiento de Regresión Logística, Árbol de Decisión y Random Forest (80/20 estratificado + validación cruzada 5-fold) |
| 8 | Evaluación e interpretación | Matrices de confusión, curvas ROC, boxplot de estabilidad, importancia de variables |
| Extra | Pronóstico de series de tiempo | Modelo Prophet para proyectar casos mensuales a 12 meses |

## Principales hallazgos

- **Localidad:** los casos se concentran en Kennedy, Bosa, Suba, Ciudad Bolívar y Engativá.
- **Evolución temporal:** el número de casos varía notablemente año a año en el periodo 2019-2026.
- **Sexo:** existe una diferencia marcada en el número de casos reportados entre hombres y mujeres.
- **Lugar de consumo:** la vivienda y la vía pública concentran la mayoría de los casos.
- **Nivel educativo / aseguramiento:** hay asociación estadísticamente significativa entre ambas variables (chi-cuadrado, p ≈ 0), aunque de magnitud débil (V de Cramér entre 0.03 y 0.12).
- **Modelo predictivo:** los tres modelos (Regresión Logística, Árbol de Decisión, Random Forest) alcanzan entre 56% y 58% de accuracy, apenas por encima de la línea base (52.6%). Random Forest generaliza mejor, pero el margen es modesto: el perfil sociodemográfico por sí solo predice con fuerza limitada la localidad de riesgo, porque esta depende más de factores geográficos y socioeconómicos estructurales.
- **Variables más relevantes (Random Forest):** año de notificación, tipo de aseguramiento "Vinculado" y varios lugares habituales de consumo — con la salvedad de que Random Forest tiende a sobrestimar la importancia de variables numéricas con muchos valores distintos como ANO.

## Estructura del repositorio

```
.
├── CODIGO_COMPLETO_CONSUMO_ABUSIVO_SPA.ipynb        # Notebook principal con todo el análisis
├── osb_saludmental_-consumoabusivo_spageneral.csv   # Dataset crudo (fuente: SaluData)
├── spa_jovenes_limpio.csv                           # Dataset limpio, generado al ejecutar el notebook
├── requirements.txt                                 # Dependencias de Python
└── README.md
```

## Requisitos

Todas las dependencias están listadas en requirements.txt:

- pandas
- numpy
- matplotlib
- seaborn
- scipy
- scikit-learn
- prophet

**Instalación:**

```
pip install -r requirements.txt
```

## Cómo clonar y ejecutar

**Opción 1 — Google Colab (recomendado, sin instalar nada localmente):**

1. Hacer clic en el botón "Open in Colab" al inicio de este README, o subir manualmente el notebook a Google Colab.
2. Subir el archivo osb_saludmental_-consumoabusivo_spageneral.csv al entorno de ejecución (mismo directorio raíz que el notebook).
3. Ejecutar las celdas en orden, de arriba hacia abajo.

**Opción 2 — Local (Jupyter):**

```
git clone https://github.com/USUARIO/NOMBRE-DEL-REPO.git
cd NOMBRE-DEL-REPO
pip install -r requirements.txt
jupyter notebook CODIGO_COMPLETO_CONSUMO_ABUSIVO_SPA.ipynb
```

Al final de la sección 3 se genera spa_jovenes_limpio.csv, el dataset ya filtrado y limpio que alimenta el resto del análisis.

## Archivos generados

spa_jovenes_limpio.csv — dataset filtrado a adolescentes y jóvenes (2019-2026), con valores nulos disfrazados unificados.

## Limitaciones

- La variable CASOS está agregada por combinación de categorías, no por persona; se expandió el dataset repitiendo filas según CASOS para el análisis de asociación e hipótesis, lo que es una aproximación y no un registro individual real.
- El poder predictivo del modelo de clasificación es limitado (56-58% accuracy), por lo que sus resultados deben leerse como una señal exploratoria y no como una herramienta de decisión operativa.

## Contexto académico

Trabajo desarrollado para la asignatura Data Experience, siguiendo el flujo de un proyecto de ciencia de datos: obtención de datos, comprensión, limpieza, EDA, análisis estadístico, selección y evaluación de modelos.

## Licencia

Este proyecto se comparte con fines exclusivamente académicos como parte de la asignatura Data Experience. Los datos originales pertenecen a la Secretaría Distrital de Salud de Bogotá (Observatorio SaluData) y se usan bajo los términos de la licencia de Datos Abiertos Bogotá. El código del análisis no cuenta con una licencia de código abierto formal; si se desea reutilizar, por favor citar a los autores.
