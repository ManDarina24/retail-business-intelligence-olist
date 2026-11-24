# Requisitos del Negocio
## Inteligencia de Negocios para el Desempeño Comercial de Olist

### 1. Contexto del negocio

Olist es una plataforma brasileña de comercio electrónico que conecta diferentes vendedores con consumidores a través de un marketplace.

Para este proyecto se plantea un caso de negocio hipotético en el que el área comercial necesita consolidar y analizar información histórica de ventas, productos, vendedores y clientes para comprender el desempeño del negocio e identificar oportunidades de crecimiento.

La información disponible se encuentra distribuida en diferentes archivos CSV, por lo que es necesario integrarla y transformarla para obtener una visión integral del comportamiento comercial.

### 2. Problema de negocio

La distribución de la información en diferentes fuentes dificulta analizar de manera conjunta el desempeño de las ventas y comprender los factores que influyen en los resultados comerciales.

Se requiere desarrollar una solución de Inteligencia de Negocios (BI) que permita evaluar la evolución de los ingresos, identificar las principales fuentes de generación de ventas, analizar el comportamiento de los clientes y reconocer oportunidades comerciales a partir de los datos históricos.

### 3. Objetivo del proyecto

Desarrollar una solución de Inteligencia de Negocios que permita analizar el desempeño comercial de Olist mediante la integración y transformación de datos, la construcción de indicadores clave de desempeño (KPI), el análisis histórico de ventas y la elaboración de un pronóstico de ingresos.

El resultado será un dashboard interactivo que facilite la interpretación de los resultados comerciales y proporcione información útil para respaldar la toma de decisiones.

### 4. Pregunta principal de negocio

**¿Cómo han evolucionado las ventas de Olist a lo largo del tiempo, qué factores influyen en su desempeño comercial y qué oportunidades pueden identificarse para impulsar su crecimiento futuro?**

### 5. Preguntas de negocio secundarias

#### 5.1. Desempeño de ventas

- ¿Cómo han evolucionado los ingresos y el volumen de pedidos a lo largo del tiempo?
- ¿Cómo se comparan los ingresos con los meses anteriores y con los mismos periodos de años anteriores?
- ¿Existen tendencias o patrones estacionales en las ventas?
- ¿Cómo ha cambiado el valor promedio de compra por pedido?

#### 5.2. Desempeño de productos y categorías

- ¿Qué categorías de productos generan mayores ingresos?
- ¿Qué categorías tienen mayor participación en las ventas totales?
- ¿Cómo ha cambiado el desempeño de las categorías a lo largo del tiempo?
- ¿Qué categorías muestran crecimiento o disminución de ingresos?

#### 5.3. Desempeño geográfico y de vendedores

- ¿Qué estados de Brasil generan mayores ingresos por ventas?
- ¿Qué vendedores contribuyen más a los ingresos y al volumen de pedidos?
- ¿Existen diferencias en el desempeño comercial entre regiones y vendedores?

#### 5.4. Comportamiento de los clientes

- ¿Cuántos clientes realizan compras recurrentes?
- ¿Qué proporción de clientes corresponde a compradores nuevos y recurrentes?
- ¿Cómo varía el comportamiento de compra de los clientes a lo largo del tiempo?

#### 5.5. Pronóstico de ventas

- ¿Qué tendencias pueden identificarse en los ingresos mensuales históricos?
- ¿Qué niveles de ingresos podrían esperarse durante los siguientes tres meses del periodo analizado?
- ¿Qué tan preciso es el modelo al estimar los ingresos futuros?

### 6. Alcance del proyecto

El proyecto contempla las siguientes actividades:

- Exploración y evaluación de los conjuntos de datos CSV disponibles.
- Limpieza, transformación e integración de datos mediante Power Query.
- Diseño del modelo de datos y sus relaciones en Power BI.
- Desarrollo de indicadores comerciales y medidas analíticas utilizando DAX.
- Análisis de ventas, productos, categorías, vendedores, distribución geográfica y comportamiento de clientes.
- Análisis de tendencias históricas y comparación entre periodos.
- Desarrollo de un modelo de pronóstico de ingresos utilizando Python.
- Construcción de un dashboard interactivo en Power BI.
- Interpretación de resultados y elaboración de recomendaciones de negocio.

### 7. Entregables esperados

1. Documentación de los requisitos del negocio y los indicadores clave.
2. Conjuntos de datos preparados e integrados.
3. Modelo de datos en Power BI.
4. Dashboard de desempeño comercial.
5. Modelo de pronóstico en Python y resultados de su evaluación.
6. Hallazgos y recomendaciones de negocio.
7. Documentación y archivos del proyecto publicados en GitHub.

### 8. Herramientas y tecnologías

- **Power BI:** modelado de datos y desarrollo de dashboards interactivos.
- **Power Query:** limpieza, transformación e integración de datos.
- **DAX:** cálculo de indicadores y medidas analíticas.
- **Python:** análisis estadístico y pronóstico de ingresos.
- **Git y GitHub:** control de versiones y documentación del proyecto.

### 9. Fuente de datos

**Conjunto de datos:** Brazilian E-Commerce Public Dataset by Olist.

**Fuente:** Kaggle  
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

El conjunto de datos contiene información histórica del comercio electrónico, incluyendo pedidos, productos, vendedores, clientes, pagos y reseñas.

### 10. Supuestos y limitaciones

- El proyecto representa un caso de negocio hipotético basado en información histórica disponible públicamente.
- El conjunto de datos abarca aproximadamente el periodo 2016–2018; por lo tanto, el ejercicio de pronóstico será retrospectivo y no representará las ventas actuales de Olist.
- Los ingresos se calcularán inicialmente utilizando el precio de los productos (`price`), excluyendo los costos de envío.
- No será posible calcular la rentabilidad real, ya que el conjunto de datos no proporciona toda la información de costos necesaria.
- La recurrencia de clientes se medirá utilizando el identificador único del cliente (`customer_unique_id`).
- Los resultados del pronóstico se considerarán estimaciones y no valores futuros garantizados.

### 11. Criterios de éxito

El proyecto se considerará exitoso cuando:

- Los principales conjuntos de datos estén integrados y validados.
- Los indicadores comerciales cuenten con definiciones y reglas de cálculo documentadas.
- El dashboard permita responder las preguntas de negocio establecidas.
- Las comparaciones históricas se calculen de manera consistente.
- El modelo de pronóstico sea evaluado mediante métricas de error apropiadas.
- El análisis genere hallazgos relevantes y recomendaciones respaldadas por los datos.
