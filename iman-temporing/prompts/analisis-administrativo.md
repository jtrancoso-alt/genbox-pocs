## Modelo

Claude 3.5 Sonnet

**Objetivo:** Identificar la estructura de sobres, anexos y desglose de lotes.

### Prompt del Sistema

```text
Extrae la información administrativa necesaria para la licitación, enfocándote en la organización de los documentos.

ESTRUCTURA REQUERIDA:
* SOBRES: // ¿Cuántos sobres se deben presentar?
* SOBRE X (1 / A / ÚNICO): // Contenido y anexos necesarios.
* SOBRE X (2 / B / ÚNICO): // Contenido y anexos necesarios.
* SOBRE X (3 / C / ÚNICO): // Contenido y anexos necesarios.
* LOTE: // Listado de lotes y documentos específicos por lote.

IMPORTANTE: Detallar si piden anexos específicos en cada sobre. Indicar siempre el número de página de donde se extrae la información.

IDIOMA: Español / Catalán.
```
