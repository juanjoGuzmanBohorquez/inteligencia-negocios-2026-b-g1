# Modelo dimensional — Sistema de Control de Asistencia Attendance

## 1. Definición del grano de la tabla de hechos

## Tabla de hechos: Fact_Asistencia

### Grano definido

Una fila de la tabla de hechos representa:

> Un registro de asistencia de un estudiante en una clase específica durante una sesión determinada.

Por lo tanto:

```text
1 estudiante + 1 clase + 1 fecha/sesión + 1 registro de asistencia = 1 fila en Fact_Asistencia
```

---

## Justificación del grano

Se selecciona este nivel de detalle porque permite conservar información individual de cada estudiante y realizar análisis más completos.

Este grano permite responder preguntas como:

- ¿Qué estudiantes tienen mayor porcentaje de asistencia?
- ¿Qué cursos presentan más ausencias?
- ¿Qué estudiantes llegan tarde con mayor frecuencia?
- ¿Cómo cambia la asistencia según el periodo académico?

Mantener el detalle por estudiante evita perder información histórica y permite generar diferentes indicadores de negocio o académicos.

---

# 2. Medidas de la tabla de hechos

Las medidas representan valores numéricos que pueden ser analizados mediante operaciones como suma, promedio o porcentaje.

## Medidas principales

| Medida | Tipo | Descripción |
|---|---|---|
| cantidad_asistencia | Entero | Representa un registro de asistencia generado |
| minutos_retraso | Entero | Cantidad de minutos después de la hora establecida |
| asistio | Entero | Indicador donde 1 representa asistencia y 0 representa ausencia |

---

## Indicadores que se pueden obtener

### Porcentaje de asistencia

```text
(Total asistencias / Total registros) * 100
```

Permite conocer el nivel de cumplimiento de asistencia de estudiantes, cursos o programas.

---

### Promedio de retraso

```text
Total minutos_retraso / Cantidad de registros
```

Permite analizar el comportamiento de llegada tarde.

---

# 3. Modelo dimensional

El modelo está compuesto por una tabla de hechos y tres dimensiones principales.

---

# Dimensión Estudiante

Tabla:

```text
Dim_Estudiante
```

Campos:

| Campo | Descripción |
|---|---|
| id_estudiante | Identificador único del estudiante |
| nombre | Nombre del estudiante |
| carrera | Programa académico |
| semestre | Semestre actual |

---

# Dimensión Curso

Tabla:

```text
Dim_Curso
```

Campos:

| Campo | Descripción |
|---|---|
| id_curso | Identificador del curso |
| nombre_curso | Nombre de la asignatura |
| profesor | Docente encargado |
| programa | Programa académico |

---

# Dimensión Tiempo

Tabla:

```text
Dim_Tiempo
```

Campos:

| Campo | Descripción |
|---|---|
| id_fecha | Identificador de fecha |
| fecha | Fecha del registro |
| mes | Mes del registro |
| semestre | Periodo académico |
| año | Año académico |

---

# 4. Tabla de hechos

## Fact_Asistencia

La tabla almacena cada registro individual de asistencia.

| Campo | Descripción |
|---|---|
| id_asistencia | Identificador del registro |
| id_estudiante | Relación con Dim_Estudiante |
| id_curso | Relación con Dim_Curso |
| id_fecha | Relación con Dim_Tiempo |
| cantidad_asistencia | Cantidad registrada |
| minutos_retraso | Minutos de retraso |
| asistio | Estado de asistencia |

---

# 5. Datos de prueba

Los datos de prueba deben cumplir con el grano definido:

> Cada fila representa la asistencia de un estudiante en una clase y fecha específica.

Ejemplo:

| id_asistencia | id_estudiante | id_curso | id_fecha | cantidad_asistencia | minutos_retraso | asistio |
|---|---|---|---|---|---|---|
| 1 | 101 | 10 | 20260901 | 1 | 0 | 1 |
| 2 | 102 | 10 | 20260901 | 1 | 5 | 1 |
| 3 | 103 | 10 | 20260901 | 1 | 0 | 1 |
| 4 | 104 | 11 | 20260902 | 1 | 10 | 1 |
| 5 | 105 | 11 | 20260902 | 0 | 0 | 0 |
| 6 | 101 | 12 | 20260903 | 1 | 3 | 1 |
| 7 | 102 | 12 | 20260903 | 1 | 0 | 1 |
| 8 | 103 | 12 | 20260903 | 0 | 0 | 0 |
| 9 | 104 | 10 | 20260904 | 1 | 2 | 1 |
| 10 | 105 | 10 | 20260904 | 1 | 0 | 1 |
| 11 | 101 | 11 | 20260905 | 1 | 8 | 1 |
| 12 | 102 | 11 | 20260905 | 0 | 0 | 0 |

---

# 6. Reflexión sobre un grano más grueso

Un grano más grueso sería:

> Una fila por curso y día con el total de estudiantes asistentes.

Ejemplo:

| Curso | Fecha | Total asistentes |
|---|---|---|
| Programación Móvil | 01/09/2026 | 35 |

---

## Análisis que se perdería

Al utilizar un grano más general se perdería información importante como:

- Qué estudiante faltó.
- Quién llegó tarde.
- Historial individual de asistencia.
- Comparación entre estudiantes.
- Seguimiento académico personalizado.

Aunque este modelo ocuparía menos espacio y sería más rápido para consultas generales, tendría menor capacidad de análisis detallado.

---

# Conclusión

El grano seleccionado para `Fact_Asistencia` permite almacenar el mayor nivel de detalle del sistema de control de asistencia.

Cada fila representa una asistencia individual y permite analizar la información desde diferentes dimensiones como estudiante, curso y tiempo.

Este diseño facilita la creación de indicadores y reportes en herramientas de Inteligencia de Negocios como Power BI.
