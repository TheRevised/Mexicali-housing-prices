# Análisis de valores atípicos en precios de viviendas

Análisis exploratorio de un conjunto de datos inmobiliarios de Mexicali. El proyecto identifica valores atípicos, los visualiza y propone transformaciones para preparar los datos para análisis estadístico o modelos predictivos.

## Objetivo

Responder de forma reproducible las siguientes preguntas:

1. ¿Existen datos atípicos? ¿En qué variables?
2. ¿Qué decisión tomar con esos registros?
3. ¿Qué transformación conviene aplicar?

## Resultados principales

El dataset contiene **940 viviendas y 7 variables**, sin valores faltantes.

| Variable | Resultado con IQR |
|---|---:|
| `precio(mxn_peso)` | 52 valores atípicos (5.53%), superiores a $9,756,000 |
| `area(m2)` | 42 valores atípicos (4.47%), superiores a 334 m² |
| `baños` | 34 valores atípicos (3.62%), correspondientes a viviendas con 4 baños |
| `habitaciones` | Variable discreta; el IQR no es concluyente porque Q1 = Q3 = 3 |

Los valores atípicos de precio y área no se eliminan automáticamente: pueden representar viviendas reales de mayor tamaño o de lujo. El notebook conserva todos los registros y crea la variable `precio_atipico` para analizarlos por separado.

## Visualizaciones

El notebook genera:

- Boxplot del precio con cuartiles, límite superior del IQR y atípicos.
- Strip plot donde cada punto representa una vivienda.
- Histogramas del precio en escala normal y logarítmica.
- Gráfica de dispersión entre precio y área.
- Tabla con las viviendas de precio más alto.

Los precios se muestran en millones de pesos en las gráficas para mejorar su legibilidad.

## Transformaciones

Se conservan las variables originales y se crean:

```python
df["precio_atipico"] = ...
df["precio_log"] = np.log1p(df["precio(mxn_peso)"])
df["area_log"] = np.log1p(df["area(m2)"])
```

La transformación logarítmica reduce la asimetría y la influencia de los precios más altos. No sustituye la variable original.

## Estructura del proyecto

```text
.
├── data/
│   └── houses.csv
├── main.ipynb
├── requirements.txt
├── .gitignore
└── README.md
```

## Instalación y ejecución

Se recomienda Python 3.10 o superior.

### Windows PowerShell

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter notebook main.ipynb
```

También se puede abrir el proyecto en VS Code y seleccionar el intérprete de `.venv` como kernel del notebook.

## Método estadístico

Para cada variable numérica se calcula:

```text
IQR = Q3 - Q1
Límite inferior = Q1 - 1.5 × IQR
Límite superior = Q3 + 1.5 × IQR
```

Los registros que quedan fuera de esos límites se marcan como posibles atípicos. La decisión final debe considerar el contexto del negocio: un precio alto no es necesariamente un error.

## Nota sobre las variables

- `precio(mxn_peso)` y `area(m2)` se analizan como variables numéricas continuas.
- `habitaciones` y `baños` son variables discretas.
- `codigo_postal` se interpreta como categoría geográfica, no como una magnitud numérica.
- `tipo` y `localidad` son variables categóricas.

## Reproducibilidad

El notebook fija la semilla utilizada para el desplazamiento visual de los puntos (`seed = 42`) y calcula todos los límites directamente a partir de `data/houses.csv`. Esto permite volver a ejecutar el análisis y obtener resultados consistentes.
# Mexicali-housing-prices
