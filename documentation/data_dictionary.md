# Diccionario de Datos

## 1. Objetivo del documento

Este documento describe la estructura del conjunto de datos *Brazilian E-Commerce Public Dataset by Olist*, identificando las tablas, columnas, tipos de datos esperados, descripciones y relaciones entre ellas.

Su propósito es facilitar la comprensión de los datos antes de realizar los procesos de limpieza, transformación, integración y análisis en Power BI.

## 2. Descripción general del conjunto de datos

El proyecto utiliza información histórica de una plataforma brasileña de comercio electrónico llamada Olist.

El conjunto de datos contiene información relacionada con:

- Clientes.
- Pedidos.
- Productos.
- Vendedores.
- Pagos.
- Reseñas de clientes.
- Ubicaciones geográficas.
- Categorías de productos.

Los archivos originales se encuentran en formato CSV y se utilizarán como fuente de información para el proyecto.

## 3. Diccionario de tablas

### 3.1. customers

**Descripción:** Contiene información de los clientes que realizaron pedidos en la plataforma.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| customer_id | Texto | Identificador del cliente asociado a un pedido. |
| customer_unique_id | Texto | Identificador único que permite reconocer al mismo cliente en diferentes pedidos. |
| customer_zip_code_prefix | Texto | Prefijo del código postal del cliente. |
| customer_city | Texto | Ciudad de residencia del cliente. |
| customer_state | Texto | Estado brasileño de residencia del cliente. |

**Granularidad:** Un registro por `customer_id`.

**Clave principal:** `customer_id`.

**Consideración:** Para analizar clientes recurrentes se utilizará `customer_unique_id`, ya que un mismo cliente puede aparecer con diferentes valores de `customer_id`.

### 3.2. orders

**Descripción:** Contiene información general de los pedidos realizados en la plataforma, incluyendo su estado y las fechas relacionadas con su procesamiento y entrega.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| order_id | Texto | Identificador único del pedido. |
| customer_id | Texto | Identificador del cliente que realizó el pedido. |
| order_status | Texto | Estado actual o registrado del pedido. |
| order_purchase_timestamp | Fecha y hora | Fecha y hora en que se realizó el pedido. |
| order_approved_at | Fecha y hora | Fecha y hora de aprobación del pedido. |
| order_delivered_carrier_date | Fecha y hora | Fecha en que el pedido fue entregado al transportista. |
| order_delivered_customer_date | Fecha y hora | Fecha en que el pedido fue entregado al cliente. |
| order_estimated_delivery_date | Fecha y hora | Fecha estimada de entrega del pedido. |

**Granularidad:** Un registro por pedido.

**Clave principal:** `order_id`.

**Consideraciones:**
- Esta tabla será fundamental para analizar la evolución temporal de los pedidos.
- `order_status` permitirá identificar pedidos entregados, cancelados y otros estados.
- Las fechas permitirán analizar tiempos de entrega y cumplimiento de fechas estimadas.
- Se deberá definir qué estados de pedido se incluirán en los indicadores comerciales.

### 3.3. order_items

**Descripción:** Contiene el detalle de los productos incluidos en cada pedido, así como información de precios, vendedores y costos de envío.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| order_id | Texto | Identificador del pedido al que pertenece el producto. |
| order_item_id | Número entero | Número de posición del producto dentro del pedido. |
| product_id | Texto | Identificador del producto. |
| seller_id | Texto | Identificador del vendedor que ofrece el producto. |
| shipping_limit_date | Fecha y hora | Fecha límite para que el vendedor entregue el producto al transportista. |
| price | Número decimal | Precio del producto dentro del pedido. |
| freight_value | Número decimal | Valor del envío asociado al producto. |

**Granularidad:** Un registro por producto o artículo dentro de un pedido.

**Clave compuesta:** `order_id` + `order_item_id`.

**Consideraciones:**
- Un pedido puede contener varios productos.
- La columna `price` será la base para calcular los ingresos por productos.
- `freight_value` se analizará por separado para distinguir los ingresos por productos del valor del envío.
- No se deberá contar cada fila como un pedido. Para calcular pedidos se utilizará el conteo distinto de `order_id`.

### 3.4. products

**Descripción:** Contiene información descriptiva y características físicas de los productos comercializados.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| product_id | Texto | Identificador único del producto. |
| product_category_name | Texto | Nombre de la categoría del producto en portugués. |
| product_name_lenght | Número entero | Longitud del nombre del producto. |
| product_description_lenght | Número entero | Longitud de la descripción del producto. |
| product_photos_qty | Número entero | Cantidad de fotografías asociadas al producto. |
| product_weight_g | Número decimal | Peso del producto en gramos. |
| product_length_cm | Número decimal | Longitud del producto en centímetros. |
| product_height_cm | Número decimal | Altura del producto en centímetros. |
| product_width_cm | Número decimal | Ancho del producto en centímetros. |

**Granularidad:** Un registro por producto.

**Clave principal:** `product_id`.

**Consideraciones:**
- `product_category_name` permitirá analizar los ingresos por categoría.
- Algunas categorías pueden tener valores nulos.
- El nombre `product_name_lenght` contiene una falta ortográfica en el archivo original. Se conservará el nombre original durante la identificación de campos y podrá renombrarse posteriormente en Power Query.

### 3.5. sellers

**Descripción:** Contiene información geográfica de los vendedores registrados en la plataforma.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| seller_id | Texto | Identificador único del vendedor. |
| seller_zip_code_prefix | Texto | Prefijo del código postal del vendedor. |
| seller_city | Texto | Ciudad donde se encuentra el vendedor. |
| seller_state | Texto | Estado brasileño donde se encuentra el vendedor. |

**Granularidad:** Un registro por vendedor.

**Clave principal:** `seller_id`.

**Consideraciones:**
- Permitirá analizar la contribución comercial de los vendedores.
- Los campos geográficos permitirán comparar el desempeño por estado y ciudad.

### 3.6. order_payments

**Descripción:** Contiene información de los pagos realizados para cada pedido, incluyendo el método de pago, número de cuotas y valor pagado.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| order_id | Texto | Identificador del pedido asociado al pago. |
| payment_sequential | Número entero | Número de secuencia del pago dentro del pedido. |
| payment_type | Texto | Método de pago utilizado. |
| payment_installments | Número entero | Cantidad de cuotas del pago. |
| payment_value | Número decimal | Valor correspondiente al registro de pago. |

**Granularidad:** Un registro por operación o registro de pago dentro de un pedido.

**Clave compuesta:** `order_id` + `payment_sequential`.

**Consideraciones:**
- Un pedido puede tener varios registros de pago.
- Permitirá analizar métodos de pago y valores pagados.
- No se deberá unir directamente esta tabla con `order_items` sin controlar la granularidad, ya que podría duplicar los importes de los productos.

### 3.7. order_reviews

**Descripción:** Contiene las reseñas y calificaciones proporcionadas por los clientes después de sus pedidos.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| review_id | Texto | Identificador de la reseña. |
| order_id | Texto | Identificador del pedido relacionado con la reseña. |
| review_score | Número entero | Calificación otorgada por el cliente. |
| review_comment_title | Texto | Título del comentario del cliente. |
| review_comment_message | Texto | Contenido del comentario del cliente. |
| review_creation_date | Fecha y hora | Fecha de creación de la reseña. |
| review_answer_timestamp | Fecha y hora | Fecha y hora de respuesta a la reseña. |

**Granularidad:** Un registro por reseña.

**Clave principal propuesta:** `review_id`.

**Consideraciones:**
- `review_score` permitirá analizar la satisfacción de los clientes.
- Los comentarios pueden contener valores nulos.
- Se deberá verificar si existen varios registros de reseñas asociados a un mismo pedido antes de establecer las relaciones del modelo.

### 3.8. geolocation

**Descripción:** Contiene información geográfica relacionada con los prefijos de códigos postales de Brasil.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| geolocation_zip_code_prefix | Texto | Prefijo del código postal. |
| geolocation_lat | Número decimal | Latitud de la ubicación geográfica. |
| geolocation_lng | Número decimal | Longitud de la ubicación geográfica. |
| geolocation_city | Texto | Ciudad asociada a la ubicación. |
| geolocation_state | Texto | Estado brasileño asociado a la ubicación. |

**Granularidad:** Un registro por ubicación geográfica registrada para un prefijo postal.

**Clave:** No se asume una clave única en esta tabla.

**Consideraciones:**
- Un mismo prefijo postal puede tener múltiples registros geográficos.
- Será necesario revisar duplicados y consistencia de ubicaciones antes de utilizar esta tabla.
- Su uso será opcional y dependerá de las necesidades del análisis geográfico.

### 3.9. product_category_name_translation

**Descripción:** Contiene la equivalencia en inglés de los nombres de las categorías de productos originalmente registrados en portugués.

| Columna | Tipo de dato esperado | Descripción |
|---|---|---|
| product_category_name | Texto | Nombre original de la categoría en portugués. |
| product_category_name_english | Texto | Traducción al inglés de la categoría. |

**Granularidad:** Un registro por categoría traducida.

**Clave principal:** `product_category_name`.

**Consideraciones:**
- Permitirá relacionar las categorías originales de `products` con sus traducciones.
- Facilitará la presentación de categorías en los análisis y visualizaciones.
- Se deberá verificar si existen categorías sin traducción.

## 4. Relaciones entre tablas

Las relaciones principales identificadas para el modelo de datos son:

| Tabla de origen | Columna | Tabla relacionada | Columna |
|---|---|---|---|
| orders | customer_id | customers | customer_id |
| order_items | order_id | orders | order_id |
| order_items | product_id | products | product_id |
| order_items | seller_id | sellers | seller_id |
| order_payments | order_id | orders | order_id |
| order_reviews | order_id | orders | order_id |
| products | product_category_name | product_category_name_translation | product_category_name |

La tabla `geolocation` podrá relacionarse con `customers` o `sellers` mediante los prefijos de código postal, pero primero será necesario evaluar su granularidad y posibles duplicados.

## 5. Consideraciones para el análisis comercial

Para mantener la consistencia de los indicadores y evitar errores de cálculo, se establecen las siguientes consideraciones iniciales:

1. **Ingresos por productos:** Se calcularán utilizando la columna `price` de `order_items`, sin incluir inicialmente el costo de envío.
2. **Número de pedidos:** Se calculará mediante el conteo distinto de `order_id`.
3. **Ticket promedio:** Se calculará dividiendo los ingresos por productos entre el número de pedidos considerados.
4. **Clientes recurrentes:** Se identificarán mediante `customer_unique_id`, considerando los pedidos realizados por cada cliente único.
5. **Análisis temporal:** Se utilizará principalmente `order_purchase_timestamp` para analizar la evolución de las ventas.
6. **Pagos:** Se analizarán de manera independiente para evitar duplicaciones al relacionarlos con los artículos de los pedidos.
7. **Estados de pedidos:** Se revisará la distribución de `order_status` para definir qué pedidos formarán parte de los indicadores comerciales.
8. **Rentabilidad:** No se calculará directamente, ya que el conjunto de datos no proporciona información completa sobre costos operativos y costos de adquisición.

## 6. Validaciones pendientes

Las siguientes validaciones se realizarán durante la etapa de exploración y calidad de datos:

- Verificar que los nombres de las columnas coincidan con los archivos CSV originales.
- Identificar tipos de datos incorrectos.
- Revisar valores nulos y registros duplicados.
- Validar la unicidad de las claves principales y compuestas.
- Identificar registros sin correspondencia entre tablas relacionadas.
- Revisar valores negativos, atípicos o inconsistentes.
- Verificar la cobertura de fechas y estados de pedidos.
- Evaluar la granularidad de las tablas antes de realizar las integraciones.

Los resultados de estas validaciones se documentarán en `data_quality_report.md`.

## 7. Fuente de información

**Nombre:** Brazilian E-Commerce Public Dataset by Olist.

**Fuente:** Kaggle.

**Enlace:** https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

**Formato original:** CSV.

**Periodo histórico aproximado:** 2016 a 2018.

---

**Nota:** Este diccionario representa la estructura esperada del conjunto de datos. Las descripciones, tipos de datos y relaciones deberán contrastarse con los archivos originales durante la exploración. Cualquier ajuste realizado durante la limpieza deberá documentarse.