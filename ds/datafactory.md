En **Azure Data Factory**, tanto los **pipelines** como los **data flows** son componentes esenciales para orquestar y transformar datos. Aquí están las diferencias clave entre ellos:

1. **Pipelines**:
    
    - Los **pipelines** son orquestadores de procesos.
    - **No transforman datos directamente**.
    - Gestionan una serie de una o más **actividades**, como **Copiar Datos** o **Ejecutar Procedimiento Almacenado**.
    - Los pipelines son como flujos de trabajo que coordinan las tareas de transferencia y procesamiento de datos.
    - [Pueden ejecutarse sin un **data flow**, pero un data flow no puede ejecutarse sin un pipeline](https://stackoverflow.com/questions/62015027/difference-between-dataflow-and-pipelines)
2. **Data Flows**:
    
    - Los **data flows** son para la **transformación de datos**.
    - Se basan en **Apache Spark** y se ejecutan en un entorno Spark.
    - Realizan transformaciones a nivel de **filas y columnas**, como análisis de valores, cálculos, agregaciones, renombrado o eliminación de columnas, e incluso adición o eliminación de filas.
    - Los data flows permiten a los ingenieros de datos desarrollar lógica de transformación gráfica sin escribir código.
    - [Son más flexibles y potentes que las actividades de copia en los pipelines](https://stackoverflow.com/questions/62015027/difference-between-dataflow-and-pipelines)

[En resumen, los **pipelines** son como directores de orquesta que coordinan las tareas, mientras que los **data flows** son los artistas que transforman los datos en el escenario](https://stackoverflow.com/questions/62015027/difference-between-dataflow-and-pipelines)