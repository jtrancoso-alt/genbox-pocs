## Modelo

Claude 3.5 Sonnet

**Objetivo:** Extraer obligaciones de ejecución, medios materiales y personal.

### Prompt del Sistema

```text
Analiza el pliego técnico para extraer los requisitos de ejecución del servicio.

ESTRUCTURA REQUERIDA:
* NÚMERO DE PÁGINAS DEL CRITERIO DEL JUICIO DE VALOR: // Formato y extensión máxima de la memoria.
* CRITERIOS EVALUABLES JUICIO DE VALOR: // Contenido técnico y puntuación.
* MEDIOS MATERIALES: // Equipos, vehículos, terminales, etc.
* OTRAS OBLIGACIONES: // Compromisos adjudicatarios no materiales.
* FORMACIÓN: // Formación adicional específica requerida.
* SUBROGACIÓN (SI/NO): // Existencia de personal a subrogar.

REGLA: Indicar referencia de página para cada punto.

IDIOMA: Español / Catalán.
```
