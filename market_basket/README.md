# Market Basket Analysis: asociaciones entre características de viviendas

Esta actividad aplica el algoritmo **Apriori** al dataset `data/houses.csv` para encontrar asociaciones entre características de las viviendas.

## Idea del análisis

Una vivienda se interpreta como una transacción y sus características categorizadas como ítems. Por ejemplo:

```text
{precio_alto, area_grande, habitaciones_4_o_mas, banos_3_o_mas}
```

Como precio y área son variables numéricas continuas, primero se discretizan en categorías interpretables:

- Precio: `precio_bajo`, `precio_medio`, `precio_alto`.
- Área: `area_pequena`, `area_mediana`, `area_grande`.
- Habitaciones: `habitaciones_1_2`, `habitaciones_3`, `habitaciones_4_o_mas`.
- Baños: `banos_1`, `banos_2`, `banos_3_o_mas`.

Los umbrales de precio y área se definen con cuantiles para evitar que los valores extremos dominen la categorización.

## Métricas

Para cada regla `X -> Y` se calculan:

- **Soporte:** frecuencia de `X` y `Y` juntos en las 940 viviendas.
- **Confianza:** proporción de viviendas con `X` que también tienen `Y`.
- **Lift:** comparación de la confianza observada con la frecuencia independiente de `Y`.

Un `lift > 1` indica una asociación positiva. La confianza puede interpretarse como probabilidad condicional dentro de esta muestra, no como causalidad.

## Ejecución

Desde la raíz del proyecto:

```powershell
python -m pip install -r requirements.txt
jupyter notebook market_basket/market_basket_apriori.ipynb
```

El notebook imprime las mejores reglas, filtra reglas relacionadas con precio y área y genera gráficas de soporte-confianza y lift.

También genera un mapa de calor final donde cada fila suma 100% y muestra qué proporción de cada categoría de precio corresponde a áreas pequeñas, medianas o grandes. La celda `Precio alto / Área grande` permite interpretar visualmente la confianza de la regla `precio_alto -> area_grande`.

## Interpretación responsable

Este dataset es inmobiliario y no contiene transacciones de compra reales. Por eso el análisis no afirma que un cliente compre una vivienda por comprar otra característica. Las reglas describen **co-ocurrencias entre atributos de viviendas**. La asociación entre precio alto y área grande puede ser consistente con los datos, pero no demuestra que una variable cause la otra.
