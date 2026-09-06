# Análisis OLTP, OLAP y Data Warehouse

## Objetivos

* Clasificar sistemas reales como **OLTP** u **OLAP**.
* Formular una pregunta analítica que requiera un **Data Warehouse**.
* Comprender el flujo **OLTP → ETL → Data Warehouse → Reporte**.

---

## 1. Clasificación de sistemas

### Sistema 1: Sistema de matrícula universitaria — OLTP

El sistema de matrícula universitaria se clasifica como **OLTP (Online Transaction Processing)** porque registra y procesa las operaciones diarias de la universidad, como la inscripción de estudiantes, matrícula de asignaturas, actualización de información y registro de pagos.

Este sistema necesita procesar las transacciones de forma rápida y mantener los datos actualizados y consistentes. Su objetivo principal es gestionar las operaciones del día a día y no realizar análisis históricos complejos.

### Sistema 2: Sistema de análisis de asistencia estudiantil — OLAP

El sistema de análisis de asistencia estudiantil se relaciona con un entorno **OLAP (Online Analytical Processing)** porque permite analizar información histórica y realizar consultas agregadas.

Por ejemplo, permite conocer el porcentaje de asistencia por estudiante, asignatura, semestre o periodo académico. Su objetivo es facilitar el análisis de los datos y apoyar la toma de decisiones de profesores y directivos.

> **Nota:** El dashboard puede considerarse una herramienta de reporte que consume información de un entorno OLAP o Data Warehouse.

---

## 2. Pregunta de análisis

Una pregunta analítica para este caso sería:

> **¿Cuál ha sido el porcentaje promedio de asistencia de los estudiantes por asignatura durante los últimos tres semestres y cuáles asignaturas presentan mayores niveles de inasistencia?**

Esta pregunta requiere trabajar con información histórica y realizar cálculos agrupados por asignatura y periodo. Por esta razón, no sería conveniente ejecutarla directamente sobre la base de datos operativa.

---

## 3. ¿Por qué utilizar un Data Warehouse?

Llevar los datos a un **Data Warehouse** permite almacenar y organizar información histórica específicamente para realizar análisis.

Esto presenta varias ventajas:

* **Rendimiento:** las consultas analíticas no afectan directamente al sistema operativo.
* **Información histórica:** permite comparar diferentes semestres y periodos académicos.
* **Estructura:** los datos pueden organizarse de manera adecuada para realizar análisis y generar indicadores.
* **Integración:** permite combinar información proveniente de diferentes fuentes.
* **Toma de decisiones:** facilita la creación de reportes y dashboards para identificar tendencias y problemas.

En este caso, el Data Warehouse permitiría analizar la asistencia de los estudiantes durante varios semestres y detectar asignaturas o estudiantes con altos niveles de inasistencia.

---

## 4. Flujo OLTP → ETL → Data Warehouse → Reporte

El proceso para este caso sería:

```text
┌─────────────────────────────┐
│            OLTP             │
│                             │
│ Sistema de asistencia       │
│ y matrícula universitaria   │
│                             │
│ Registra las operaciones    │
│ del día a día               │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│             ETL             │
│                             │
│ Extraer                     │
│ Transformar                 │
│ Cargar                      │
│                             │
│ Limpia y prepara los datos  │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│       DATA WAREHOUSE        │
│                             │
│ Almacena información        │
│ histórica de asistencia     │
│                             │
│ Datos organizados para      │
│ realizar análisis           │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│           REPORTE           │
│                             │
│          Power BI           │
│                             │
│ Gráficos, KPI, tendencias   │
│ y análisis de asistencia    │
└─────────────────────────────┘
```

---

## 5. Resumen

| Elemento           | Descripción                                                             |
| ------------------ | ----------------------------------------------------------------------- |
| **OLTP**           | Sistema de matrícula y asistencia que registra las operaciones diarias. |
| **ETL**            | Proceso que extrae, transforma y carga los datos.                       |
| **Data Warehouse** | Almacena los datos históricos organizados para análisis.                |
| **OLAP**           | Permite realizar análisis históricos y consultas agregadas.             |
| **Reporte**        | Presenta los resultados mediante dashboards e indicadores en Power BI.  |

### Conclusión

El uso de un **Data Warehouse** permite separar las operaciones diarias de los procesos de análisis. De esta manera, la universidad puede conservar información histórica y analizarla de forma más eficiente para identificar tendencias de asistencia y apoyar la toma de decisiones académicas.
