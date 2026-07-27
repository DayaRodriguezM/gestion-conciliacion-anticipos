# Gestión y conciliación de solicitudes de anticipo en SAP

## Descripción

Proyecto de análisis de datos aplicado a un proceso real de remuneraciones, desarrollado para automatizar validaciones, consolidar solicitudes de anticipo, conciliar su procesamiento en SAP y analizar las personas y montos considerados mes a mes durante 2026.

La solución permite seguir el proceso completo:

**Solicitudes recibidas → validación de reglas de negocio → conciliación con SAP → resultado de la solicitud → personas procesadas → monto mensual pagado.**

El dashboard fue desarrollado inicialmente en Power BI para uso empresarial y posteriormente replicado en Tableau para disponer de una versión pública e interactiva.

---

## Objetivo

Analizar y conciliar las solicitudes de anticipo registradas durante 2026, comparándolas con las vigencias existentes en SAP para determinar cuáles fueron procesadas, cuáles no generaron modificaciones y qué situaciones explican las diferencias entre una solicitud recibida y el resultado final del proceso.

El análisis busca responder preguntas como:

- ¿Cuántas solicitudes se reciben cada mes?
- ¿Qué proporción se registra en SAP?
- ¿Cuáles quedan fuera del corte?
- ¿Qué solicitudes corresponden a duplicados, reemplazos o anulaciones?
- ¿Cuántas personas reciben anticipo en cada periodo?
- ¿Qué monto se procesa mensualmente?
- ¿Qué vigencias comienzan en el mes y cuáles continúan desde periodos anteriores?

---

## Problema de negocio

El proceso de anticipos integra información proveniente de distintas fuentes y requiere aplicar varias reglas antes de procesar la nómina.

Una solicitud no siempre genera un nuevo registro en el sistema de remuneraciones, ya que puede:

- corresponder a un anticipo ya vigente;
- ser recibida después de la fecha de corte;
- reemplazar una solicitud anterior;
- solicitar una modificación de monto;
- corresponder a una anulación;
- generar un registro a partir del mes siguiente;
- no cumplir las condiciones necesarias para ser procesada.

La revisión manual de estos escenarios dificulta la trazabilidad, aumenta el tiempo de validación y puede generar diferencias entre las solicitudes, las vigencias registradas y el pago mensual.

---

## Solución desarrollada

Se construyó una solución analítica que integra información de:

- solicitudes realizadas mediante intranet;
- registros y vigencias de anticipos en el sistema de remuneraciones;
- estado contractual de los trabajadores;
- fechas de alta, baja e inicio de vigencia;
- licencias y condiciones de elegibilidad;
- montos solicitados y registrados;
- empresas y área de nómina.

La automatización inicial fue desarrollada en Python para consolidar las fuentes, aplicar validaciones y preparar la información para el análisis.

Posteriormente, los resultados fueron modelados y visualizados en Power BI y replicados en Tableau.

---

## Validaciones del proceso

Antes de procesar un anticipo en nómina se aplican distintas reglas de negocio.

Entre las principales validaciones se encuentran:

- trabajador activo al momento del proceso;
- antigüedad laboral requerida;
- ausencia de licencias que impidan el pago;
- monto dentro de los límites permitidos;
- cumplimiento de la fecha de corte;
- ausencia de solicitudes duplicadas;
- identificación de solicitudes reemplazadas;
- validación de registros y montos existentes en SAP;
- vigencia contractual durante el periodo correspondiente.


## Reglas de negocio principales

### Fecha de corte de solicitudes

- Las solicitudes recibidas dentro del corte pueden procesarse en el mismo mes.
- Las solicitudes mensuales recibidas después del corte no generan registro.
- Las solicitudes indefinidas recibidas después del corte pueden registrarse a partir del mes siguiente.

### Duplicidad y reemplazo

- Si una persona ya posee una vigencia indefinida por el mismo monto, una nueva solicitud puede clasificarse como duplicada.
- Cuando existen varias solicitudes en un mismo mes, se identifica si una fue reemplazada por otra posterior.
- Una solicitud no siempre implica una modificación en el sistema de remuneraciones si ya existe un registro vigente compatible.

### Conciliación

La conciliación compara:

- monto solicitado;
- monto registrado;
- fecha esperada de procesamiento;
- fecha de inicio de la vigencia;
- estado del corte;
- resultado final de la solicitud.

---

## Herramientas utilizadas

- **Python:** automatización, consolidación de fuentes y validación de reglas de negocio.
- **Power Query:** limpieza y transformación de datos.
- **Power BI:** modelado, medidas DAX y desarrollo de la solución empresarial.
- **Tableau:** replicación y publicación del dashboard interactivo.
- **Excel:** conciliaciones y validaciones complementarias.
- **SAP ERP:** fuente de registros y vigencias de anticipos.

---

## Estructura del dashboard

El dashboard está organizado en tres vistas.

### 1. Overview

Resume el volumen de solicitudes, su evolución mensual, los tipos de anticipo y el resultado final del proceso de conciliación.

### 2. Detalle de solicitudes

Permite revisar cada ticket y comparar la solicitud con el registro encontrado en SAP, incluyendo fechas, montos y resultado de conciliación.

### 3. Pagos mensuales

Muestra las personas y montos procesados por mes, el promedio por persona, las vigencias iniciadas durante el periodo y aquellas que continúan desde meses anteriores.

---

## Principales resultados

Durante el periodo analizado se conciliaron **4.607 solicitudes de anticipo**.

- El **68,5 %** fue registrado en SAP durante el mes correspondiente.
- Al incluir los registros programados para el mes siguiente, el **72,3 %** de las solicitudes generó un procesamiento en SAP.
- El **8,0 %** quedó fuera de corte sin registro.
- El **5,0 %** correspondió a anulaciones.
- El **1,8 %** no presentó registro en SAP.
- El resto correspondió principalmente a solicitudes duplicadas, reemplazadas, registros vigentes con montos distintos o casos que no requerían una modificación.

El análisis permitió comprobar que el volumen de solicitudes no representa directamente la cantidad de personas procesadas, ya que una misma persona puede ingresar varias solicitudes durante el año.

## Interpretación de los indicadores

El análisis combina distintos niveles de granularidad:

- **Solicitud:** cada ticket ingresado.
- **Persona:** trabajador único.
- **Persona-mes:** persona contabilizada una vez en cada mes procesado.
- **Vigencia:** registro de anticipo activo en SAP.

Una persona puede generar varias solicitudes durante el año, pero se contabiliza una sola vez dentro de cada mes en que recibe el anticipo.

En la vista de pagos mensuales:

- con un mes seleccionado, los indicadores muestran las vigencias iniciadas durante ese mes y las que continúan desde meses anteriores;
- sin filtro mensual, muestran personas únicas acumuladas según el año de inicio de la vigencia.

---

## Privacidad de los datos

La información fue anonimizada antes de su publicación.

Los identificadores personales fueron reemplazados por códigos secuenciales.

---

## Dashboard

### Overview

![Overview](images/tableau-overview.png)

### Detalle de solicitudes

![Detalle de solicitudes](images/tableau-detalle.png)

### Pagos mensuales

![Pagos mensuales](images/tableau-pagos-mensuales.png)

## Dashboard interactivo

[Explorar el dashboard en Tableau Public](COLOCAR_ENLACE_TABLEAU)

---

## Aprendizajes

Este proyecto permitió fortalecer conocimientos en:

- automatización de procesos con Python;
- aplicación de reglas de negocio;
- integración de fuentes con distintas granularidades;
- conciliación entre solicitudes, registros y pagos;
- calidad y trazabilidad de datos;
- modelado de datos;
- desarrollo de medidas DAX;
- creación de campos calculados en Tableau;
- diseño de dashboards ejecutivos y operativos;
- validación de resultados contra fuentes oficiales;
- anonimización y protección de información.

---

## Autora

**Dayana Rodríguez**

Data Analyst | Business Intelligence | SQL | Python | Power BI | Tableau | Data Quality
