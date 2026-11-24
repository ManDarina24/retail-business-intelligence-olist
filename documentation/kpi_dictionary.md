# Diccionario de Indicadores (KPIs)
## Retail Business Performance & Intelligence | Olist

## 1. Objetivo del documento

Este documento define los indicadores clave de desempeño (KPIs) que se utilizarán para analizar el comportamiento comercial de Olist.

Para cada indicador se establece su objetivo, fórmula de cálculo, fuente de información, interpretación y consideraciones de negocio.

Los indicadores permitirán evaluar la evolución de los ingresos, el comportamiento de los pedidos, el desempeño de los productos, la recurrencia de los clientes y el cumplimiento de las entregas.

## 2. Reglas generales de cálculo

Antes de definir los indicadores, se establecen las siguientes reglas:

- **Ingresos por productos:** se calcularán utilizando `price` de la tabla `order_items`. No se incluirá `freight_value` inicialmente.
- **Pedidos:** se utilizará el conteo distinto de `order_id`, evitando contar varias veces un pedido que contiene diferentes productos.
- **Clientes:** se utilizará `customer_unique_id` para identificar clientes únicos entre diferentes pedidos.
- **Fecha de análisis comercial:** se utilizará `order_purchase_timestamp` para determinar el periodo en que se realizó cada pedido.
- **Estados de pedidos:** se revisará `order_status` antes de establecer los filtros definitivos de los indicadores.
- **Granularidad:** se considerará la estructura de cada tabla para evitar duplicar importes al combinar información.

Las reglas podrán ajustarse después de revisar la calidad y distribución de los datos originales.

## 3. KPIs principales

Estos indicadores estarán en el centro del dashboard ejecutivo y permitirán evaluar el desempeño comercial general.

### KPI 1. Ingresos por productos

**Objetivo:** Medir el valor total generado por la venta de productos durante un periodo determinado.

**Fórmula:**

Ingresos por productos = Suma de `price`.

**Fuente:**
- Tabla: `order_items`.
- Columna: `price`.

**Unidad de medida:** Reales brasileños (R$).

**Interpretación:** Permite conocer cuánto representan las ventas de productos y observar su evolución a lo largo del tiempo.

**Consideraciones:**
- No incluye el valor del envío.
- Se deberá definir qué estados de pedidos se incluirán.
- No representa utilidad ni ganancia neta.

### KPI 2. Total de pedidos

**Objetivo:** Medir la cantidad de pedidos realizados en la plataforma.

**Fórmula:**

Total de pedidos = Conteo distinto de `order_id`.

**Fuente:**
- Tabla: `orders`.
- Columna: `order_id`.

**Unidad de medida:** Pedidos.

**Interpretación:** Permite evaluar el volumen de actividad comercial y su evolución durante el periodo analizado.

**Consideraciones:**
- No se debe contar el número de filas de `order_items`.
- Se deberá documentar el tratamiento de pedidos cancelados y otros estados.

### KPI 3. Ticket promedio

**Objetivo:** Conocer el ingreso promedio por pedido.

**Fórmula:**

Ticket promedio = Ingresos por productos / Total de pedidos.

**Fuente:**
- Tabla: `order_items`.
- Columna: `price`.
- Identificador: `order_id`.

**Unidad de medida:** Reales brasileños por pedido (R$/pedido).

**Interpretación:** Permite identificar si el valor promedio de los pedidos aumenta o disminuye con el tiempo.

**Consideraciones:**
- El denominador debe corresponder a los pedidos incluidos en los ingresos.
- Se calculará considerando los pedidos que tengan artículos asociados.
- No incluye el costo del envío.

### KPI 4. Clientes únicos

**Objetivo:** Identificar cuántos clientes diferentes realizaron compras durante el periodo analizado.

**Fórmula:**

Clientes únicos = Conteo distinto de `customer_unique_id`.

**Fuente:**
- Tabla: `customers`.
- Columna: `customer_unique_id`.
- Relación: `customers` con `orders` mediante `customer_id`.

**Unidad de medida:** Clientes.

**Interpretación:** Permite conocer el alcance de la actividad comercial en términos de clientes diferentes.

**Consideraciones:**
- No se debe utilizar `customer_id` para identificar clientes recurrentes.
- El cálculo debe considerar únicamente clientes con pedidos incluidos en el análisis.

## 4. KPIs de evolución comercial

Estos indicadores permitirán identificar tendencias, cambios y variaciones en los ingresos a través del tiempo.

### KPI 5. Crecimiento mensual de ingresos (MoM)

**Objetivo:** Medir el cambio porcentual de los ingresos respecto al mes anterior.

**Fórmula:**

Crecimiento mensual (%) =

((Ingresos del mes actual - Ingresos del mes anterior) / Ingresos del mes anterior) × 100.

**Fuente:**
- Tabla: `order_items`.
- Columna: `price`.
- Fecha relacionada: `orders.order_purchase_timestamp`.

**Unidad de medida:** Porcentaje (%).

**Interpretación:**
- Resultado positivo: los ingresos aumentaron.
- Resultado negativo: los ingresos disminuyeron.
- Resultado igual a cero: los ingresos se mantuvieron.

**Consideraciones:**
- Se necesita una tabla calendario para realizar comparaciones temporales.
- Si el mes anterior no tiene ingresos o su valor es cero, el indicador deberá mostrarse sin resultado.
- La comparación debe utilizar periodos consecutivos.

### KPI 6. Crecimiento anual de ingresos (YoY)

**Objetivo:** Comparar los ingresos de un periodo con los del mismo periodo del año anterior.

**Fórmula:**

Crecimiento anual (%) =

((Ingresos del periodo actual - Ingresos del mismo periodo del año anterior) / Ingresos del mismo periodo del año anterior) × 100.

**Fuente:**
- Tabla: `order_items`.
- Columna: `price`.
- Fecha relacionada: `orders.order_purchase_timestamp`.

**Unidad de medida:** Porcentaje (%).

**Interpretación:** Permite identificar si el desempeño comercial mejora o disminuye respecto al año anterior.

**Consideraciones:**
- La comparación dependerá de la disponibilidad de datos históricos.
- Se deberán comparar periodos equivalentes.
- Los resultados de los primeros periodos sin información comparable deberán mostrarse como no disponibles.

## 5. KPIs de productos y categorías

Estos indicadores permitirán identificar qué productos y categorías tienen mayor participación en los ingresos.

### KPI 7. Ingresos por categoría

**Objetivo:** Identificar las categorías de productos que generan mayores ingresos.

**Fórmula:**

Ingresos por categoría = Suma de `price` agrupada por categoría.

**Fuente:**
- Tabla: `order_items`.
- Tabla: `products`.
- Columna: `price`.
- Columna de clasificación: `product_category_name`.

**Unidad de medida:** Reales brasileños (R$).

**Interpretación:** Permite comparar el desempeño comercial de las diferentes categorías.

**Consideraciones:**
- Se deberá relacionar `order_items` con `products` mediante `product_id`.
- Se podrá utilizar la tabla de traducción de categorías para facilitar la presentación.
- Se deberán identificar productos sin categoría asignada.

### KPI 8. Participación de ingresos por categoría

**Objetivo:** Medir qué proporción de los ingresos totales representa cada categoría.

**Fórmula:**

Participación por categoría (%) =

(Ingresos de la categoría / Ingresos totales) × 100.

**Fuente:**
- Tabla: `order_items`.
- Tabla: `products`.
- Columna: `price`.

**Unidad de medida:** Porcentaje (%).

**Interpretación:** Permite conocer qué categorías concentran una mayor proporción de los ingresos comerciales.

**Consideraciones:**
- El total utilizado como denominador debe respetar los mismos filtros de periodo y estados de pedidos, excepto el filtro de categoría.
- La suma de las participaciones deberá ser aproximadamente 100%, considerando las categorías incluidas y los posibles registros sin clasificación.

## 6. KPIs de comportamiento de clientes

Estos indicadores permitirán conocer la recurrencia de compra y la relación de los clientes con la plataforma.

### KPI 9. Clientes recurrentes

**Objetivo:** Identificar la cantidad de clientes que realizaron más de un pedido durante el periodo analizado.

**Regla de clasificación:**

Un cliente se considera recurrente cuando tiene más de un `order_id` asociado a su `customer_unique_id`.

**Fórmula:**

Clientes recurrentes = Conteo de clientes únicos cuyo número de pedidos es mayor que uno.

**Fuente:**
- Tabla: `customers`.
- Tabla: `orders`.
- Columnas: `customer_unique_id`, `customer_id` y `order_id`.

**Unidad de medida:** Clientes.

**Interpretación:** Permite conocer cuántos clientes regresaron a realizar otra compra.

**Consideraciones:**
- Se deberá contar el número distinto de pedidos por cliente único.
- La clasificación se realizará dentro del periodo seleccionado.
- Se utilizarán los estados de pedidos definidos para el análisis comercial.

### KPI 10. Tasa de clientes recurrentes

**Objetivo:** Medir qué proporción de los clientes realizó más de un pedido.

**Fórmula:**

Tasa de clientes recurrentes (%) =

(Clientes recurrentes / Clientes únicos) × 100.

**Fuente:**
- Tabla: `customers`.
- Tabla: `orders`.

**Unidad de medida:** Porcentaje (%).

**Interpretación:** Permite evaluar el nivel de recurrencia de compra de los clientes.

**Consideraciones:**
- El numerador y el denominador deben utilizar los mismos filtros de periodo y estados de pedidos.
- Un cliente con dos o más pedidos dentro del periodo seleccionado se considera recurrente.

## 7. KPIs logísticos

Estos indicadores permitirán complementar el análisis comercial con información sobre el cumplimiento de las entregas.

### KPI 11. Tasa de pedidos entregados

**Objetivo:** Medir qué proporción de los pedidos considerados fue entregada al cliente.

**Fórmula:**

Tasa de pedidos entregados (%) =

(Pedidos entregados / Total de pedidos) × 100.

**Fuente:**
- Tabla: `orders`.
- Columnas: `order_id`, `order_status`.

**Unidad de medida:** Porcentaje (%).

**Interpretación:** Permite conocer el nivel de cumplimiento de los pedidos en términos de entrega.

**Consideraciones:**
- Se identificarán los pedidos con estado `delivered`.
- Se deberá definir el conjunto de estados incluido en el denominador.
- Se debe distinguir entre pedidos entregados y pedidos que aún están en proceso.

### KPI 12. Tasa de entregas tardías

**Objetivo:** Medir qué proporción de los pedidos entregados llegó después de la fecha estimada.

**Regla de clasificación:**

Un pedido se considera tardío cuando:

`order_delivered_customer_date > order_estimated_delivery_date`

**Fórmula:**

Tasa de entregas tardías (%) =

(Pedidos entregados tarde / Pedidos entregados con fechas válidas) × 100.

**Fuente:**
- Tabla: `orders`.
- Columnas:
  - `order_id`
  - `order_status`
  - `order_delivered_customer_date`
  - `order_estimated_delivery_date`

**Unidad de medida:** Porcentaje (%).

**Interpretación:** Permite evaluar el cumplimiento de las fechas estimadas de entrega.

**Consideraciones:**
- Solo se considerarán pedidos entregados.
- Se excluirán del cálculo los pedidos que no tengan fechas válidas para comparar.
- Se considerará tardío un pedido cuya fecha real de entrega sea posterior a la fecha estimada.

## 8. Resumen de KPIs

| KPI | Categoría | Unidad |
|---|---|---|
| Ingresos por productos | Comercial | R$ |
| Total de pedidos | Comercial | Pedidos |
| Ticket promedio | Comercial | R$/pedido |
| Clientes únicos | Clientes | Clientes |
| Crecimiento mensual de ingresos | Evolución comercial | % |
| Crecimiento anual de ingresos | Evolución comercial | % |
| Ingresos por categoría | Productos | R$ |
| Participación de ingresos por categoría | Productos | % |
| Clientes recurrentes | Clientes | Clientes |
| Tasa de clientes recurrentes | Clientes | % |
| Tasa de pedidos entregados | Logística | % |
| Tasa de entregas tardías | Logística | % |

## 9. Consideraciones finales

Los indicadores definidos en este documento representan la propuesta inicial para el análisis comercial de Olist.

Antes de implementarlos en Power BI será necesario:

1. Revisar la calidad de los datos originales.
2. Validar las relaciones entre las tablas.
3. Definir los estados de pedidos que se incluirán en cada indicador.
4. Construir una tabla calendario.
5. Implementar las medidas utilizando DAX.
6. Validar los resultados mediante cálculos de control.

Los indicadores podrán ajustarse cuando se identifiquen particularidades en los datos originales.

---

**Fuente de información:** Brazilian E-Commerce Public Dataset by Olist, disponible en Kaggle.

https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce