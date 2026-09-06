# Modelo Estrella — Control de Asistencia Estudiantil

## Objetivos

* Identificar la **tabla de hechos** y sus medidas.
* Diseñar **dimensiones** con sus respectivos atributos.
* Justificar las decisiones tomadas en el modelado.
* Crear un modelo estrella orientado al análisis de la asistencia estudiantil.

---

## 1. Proceso de negocio

El proceso de negocio seleccionado es el **control de asistencia estudiantil** de una universidad.

El objetivo del modelo es almacenar y analizar los registros de asistencia de los estudiantes a sus diferentes asignaturas y clases. Esto permitirá generar indicadores y reportes para identificar niveles de asistencia, inasistencias y posibles riesgos académicos.

---

## 2. Tabla de hechos

### Fact_Asistencia

La tabla de hechos representa el **evento de asistencia de un estudiante a una clase específica**.

Cada registro representa una asistencia o inasistencia de un estudiante en una fecha determinada, para una asignatura y un profesor.

### Medidas

Las principales medidas son:

| Medida           | Descripción                                                                  |
| ---------------- | ---------------------------------------------------------------------------- |
| `cantidad_clase` | Representa una clase registrada. Su valor normalmente es 1.                  |
| `asistio`        | Indica si el estudiante asistió. Puede tomar 1 si asistió y 0 si no asistió. |
| `inasistencia`   | Indica si el estudiante faltó. Puede tomar 1 si faltó y 0 si asistió.        |

Estas medidas permiten realizar operaciones como **SUM**, por ejemplo, sumar las asistencias o las inasistencias y calcular porcentajes.

---

## 3. Dimensiones

### Dimensión Tiempo

**Tabla:** `Dim_Tiempo`

Permite analizar la asistencia según diferentes periodos de tiempo.

**Atributos:**

* `id_tiempo` — Clave primaria.
* `fecha`
* `dia`
* `mes`
* `nombre_mes`
* `trimestre`
* `semestre`
* `año`

**Justificación:**
La dimensión Tiempo permite comparar la asistencia entre días, meses, semestres y años. Es fundamental para realizar análisis históricos.

---

### Dimensión Estudiante

**Tabla:** `Dim_Estudiante`

Contiene la información descriptiva de los estudiantes.

**Atributos:**

* `id_estudiante` — Clave primaria.
* `nombre`
* `apellido`
* `programa`
* `semestre`
* `jornada`

**Justificación:**
Permite analizar la asistencia individual y comparar el comportamiento entre diferentes estudiantes, programas y semestres académicos.

---

### Dimensión Asignatura

**Tabla:** `Dim_Asignatura`

Contiene la información relacionada con las asignaturas.

**Atributos:**

* `id_asignatura` — Clave primaria.
* `nombre_asignatura`
* `codigo_asignatura`
* `programa`
* `semestre`
* `creditos`

**Justificación:**
Permite identificar qué asignaturas presentan mayores niveles de asistencia o inasistencia y realizar comparaciones entre ellas.

---

### Dimensión Profesor

**Tabla:** `Dim_Profesor`

Contiene la información de los profesores encargados de las asignaturas.

**Atributos:**

* `id_profesor` — Clave primaria.
* `nombre`
* `apellido`
* `departamento`
* `tipo_contrato`

**Justificación:**
Permite analizar los registros de asistencia según el profesor y relacionarlos con las asignaturas que tiene a su cargo.

---

## 4. Diagrama del modelo estrella

```text
                         ┌─────────────────────────┐
                         │       DIM_TIEMPO        │
                         ├─────────────────────────┤
                         │ PK id_tiempo            │
                         │ fecha                   │
                         │ dia                     │
                         │ mes                     │
                         │ nombre_mes              │
                         │ trimestre               │
                         │ semestre                │
                         │ año                     │
                         └────────────┬────────────┘
                                      │
                                      │
                                      ▼
┌─────────────────────────┐     ┌─────────────────────────┐     ┌─────────────────────────┐
│    DIM_ESTUDIANTE       │     │     FACT_ASISTENCIA     │     │     DIM_ASIGNATURA      │
├─────────────────────────┤     ├─────────────────────────┤     ├─────────────────────────┤
│ PK id_estudiante        │────▶│ PK id_asistencia        │◀────│ PK id_asignatura        │
│ nombre                  │     │ FK id_tiempo            │     │ nombre_asignatura       │
│ apellido                │     │ FK id_estudiante        │     │ codigo_asignatura       │
│ programa                │     │ FK id_asignatura        │     │ programa                │
│ semestre                │     │ FK id_profesor          │     │ semestre                │
│ jornada                 │     │                         │     │ creditos                │
└─────────────────────────┘     │ cantidad_clase          │     └─────────────────────────┘
                                │ asistio                 │
                                │ inasistencia            │
                                └────────────┬────────────┘
                                             │
                                             │
                                             ▼
                                ┌─────────────────────────┐
                                │      DIM_PROFESOR        │
                                ├─────────────────────────┤
                                │ PK id_profesor           │
                                │ nombre                   │
                                │ apellido                 │
                                │ departamento             │
                                │ tipo_contrato             │
                                └─────────────────────────┘
```

### Estructura simplificada

```text
                 DIM_TIEMPO
                      │
                      │
DIM_ESTUDIANTE ── FACT_ASISTENCIA ── DIM_ASIGNATURA
                      │
                      │
                 DIM_PROFESOR
```

La **Fact_Asistencia** se encuentra en el centro porque contiene los eventos y medidas que serán analizados, mientras que las dimensiones proporcionan el contexto necesario para realizar diferentes consultas.

---

## 5. Preguntas de negocio

### Pregunta 1

**¿Qué estudiantes presentan el menor porcentaje de asistencia durante un semestre?**

El modelo puede responder esta pregunta utilizando la dimensión **Estudiante** y la dimensión **Tiempo**.

Se pueden sumar las medidas `asistio` e `inasistencia` y calcular el porcentaje:

```text
Porcentaje de asistencia =
(SUM(asistio) / SUM(cantidad_clase)) × 100
```

Después se pueden ordenar los estudiantes de menor a mayor porcentaje de asistencia para identificar aquellos que necesitan seguimiento.

---

### Pregunta 2

**¿Qué asignaturas presentan los mayores niveles de inasistencia durante los últimos tres semestres?**

Para responder esta pregunta se utilizan las dimensiones **Asignatura** y **Tiempo**, junto con la medida `inasistencia`.

Se pueden sumar las inasistencias y agrupar los resultados por asignatura y semestre. De esta manera, se pueden identificar las asignaturas que presentan mayores niveles de inasistencia y observar si el comportamiento se mantiene o cambia con el tiempo.

---

## 6. Justificación del modelo

Se eligió un **modelo estrella** porque facilita el análisis de los datos de asistencia. La tabla de hechos concentra los eventos y las medidas, mientras que las dimensiones permiten analizar esos datos desde diferentes perspectivas, como estudiante, asignatura, profesor y tiempo.

Este modelo también facilita la creación de dashboards en herramientas de inteligencia de negocios como **Power BI**, permitiendo visualizar KPI, porcentajes de asistencia, tendencias e indicadores de inasistencia.

---

## Conclusión

El modelo estrella propuesto permite transformar los registros de asistencia en información útil para la universidad. A partir de la tabla de hechos y sus dimensiones se pueden realizar análisis históricos, identificar estudiantes con baja asistencia y detectar asignaturas con altos niveles de inasistencia. Esto facilita la toma de decisiones y permite generar alertas tempranas para realizar un seguimiento académico.
