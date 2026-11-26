# Informe de calidad de datos - Olist

## 1. Objetivo

Evaluar la calidad de los datos originales del conjunto de datos de Olist, identificando valores nulos y posibles inconsistencias que puedan afectar el análisis comercial.

## 2. Análisis de valores nulos

Durante la exploración inicial se identificaron valores nulos en tres tablas:

### Orders
Se encontraron valores nulos en las fechas de aprobación y entrega de pedidos. Estos registros se conservarán inicialmente, ya que la ausencia de fechas puede estar relacionada con el estado del pedido.

### Order Reviews
Se identificó una proporción elevada de reseñas sin título o comentario. Estos valores no representan necesariamente errores, debido a que los clientes pueden calificar su experiencia sin escribir una opinión.

### Products
Se encontraron registros sin categoría y algunos productos con información física incompleta. Se investigará la ausencia de categorías, ya que esta información es relevante para el análisis del desempeño comercial por producto.

## 3. Decisiones iniciales

- No eliminar registros únicamente por contener valores nulos.
- Conservar las fechas de entrega faltantes hasta revisar los estados de los pedidos.
- Mantener los comentarios de reseñas vacíos como información no proporcionada.
- Investigar los productos sin categoría antes de realizar el análisis por categorías.
- Evaluar la relevancia de los datos físicos de productos según su utilidad para el proyecto.

## 4. Conclusión preliminar

Los valores nulos identificados no representan necesariamente errores en los datos. Su tratamiento dependerá de su contexto y de su impacto en los indicadores comerciales y análisis posteriores.

## 5. Análisis de registros duplicados

Se realizó una revisión de duplicados exactos en las nueve tablas del conjunto de datos.

### Resultados

La mayoría de las tablas no presentó registros completamente duplicados. Sin embargo, la tabla `olist_geolocation_dataset` presentó los siguientes resultados:

- Registros originales: 1,000,163.
- Registros duplicados exactos: 261,831.
- Porcentaje de duplicados: 26.18%.
- Registros restantes después de eliminar duplicados exactos: 738,332.

### Análisis adicional

Se investigaron los códigos postales con mayor número de registros y se identificó que algunos contienen múltiples coordenadas geográficas y, en ciertos casos, más de una ciudad asociada.

Esto indica que la repetición de códigos postales no debe interpretarse automáticamente como un error, ya que pueden representar diferentes ubicaciones dentro de una misma zona.

### Decisión inicial

- Conservar los archivos originales sin modificaciones.
- Considerar la eliminación de duplicados exactos durante la preparación de datos.
- No eliminar registros únicamente por compartir un código postal.
- Evaluar la granularidad de la tabla antes de establecer relaciones con clientes y vendedores.

### Análisis de identificadores en Order Reviews

Durante la validación de claves se identificaron 814 registros que comparten un valor de `review_id`.

Al investigar estos casos, se encontraron 789 identificadores distintos repetidos, cada uno asociado a dos pedidos diferentes.

En los ejemplos revisados, los registros mantienen la misma calificación, comentario y fechas, aunque corresponden a distintos pedidos.

**Hallazgo:** `review_id` no garantiza unicidad dentro de la tabla.

**Decisión inicial:** no eliminar estos registros, ya que corresponden a pedidos diferentes. Se evaluará la combinación `review_id` + `order_id` como posible clave compuesta.

**Clave candidata:** combinación de `review_id` + `order_id`.

**Observación de calidad:** `review_id` presenta identificadores repetidos asociados a distintos pedidos. La combinación de ambas columnas no presenta duplicados en la revisión realizada, por lo que se considera una clave compuesta candidata.