# 🔧 PoC: Análisis de Licitaciones para Würth

**Cliente:** Würth España | **Estado:** `Finalizado`

---

## 🎯 El Desafío

Würth participa en licitaciones de obra pública donde necesita evaluar rápidamente si su catálogo de productos industriales cubre los requisitos del pliego, si las condiciones comerciales son asumibles y si los precios son competitivos.

El objetivo de esta PoC fue validar el uso de **IA Generativa** para:

- **Extraer automáticamente** los materiales y precios de un pliego técnico (PDF).
- **Cruzar semánticamente** cada material contra el catálogo Würth (62 productos, 7 familias).
- **Analizar la viabilidad comercial**: plazos, pagos, solvencia, penalizaciones.
- **Comparar precios** del pliego vs catálogo Würth con cálculo de márgenes.
- **Generar un scorecard** de viabilidad (0-100) con recomendación Go/No-Go.
- **Producir un informe HTML** profesional listo para el equipo comercial.

---

## 🏗️ Arquitectura del Sistema (Pipeline Multi-Agente)

Se diseñó un pipeline secuencial de **5 agentes especializados** que procesan el pliego de principio a fin sin intervención humana.

```
PDF Pliego
  ├── Pág. administrativas (1-12) → [wurth-analyzer] → Viabilidad + semáforos
  └── Pág. técnicas (21-28)       → [wurth-extractor] → Materiales + precios
                                                              ↓
                                    Catálogo Würth (JSON) → [wurth-matcher] → Coincidencias semánticas
                                                              ↓
                                                    Competitividad de precios
                                                              ↓
                                                        [wurth-scorer] → Scorecard 0-100 + Go/No-Go
                                                              ↓
                                                        [wurth-reporter] → Informe narrativo
                                                              ↓
                                                        Generación HTML → Informe visual
```

### Agentes y roles

| Agente              | Modelo            | Rol                                                                                                                                                                                  |
| ------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **wurth-analyzer**  | Claude 3.5 Haiku  | Extrae ficha técnica (organismo, presupuesto, plazos) y condiciones comerciales con semáforos de riesgo (🟢🟡🔴) para plazos, pagos, solvencia y penalizaciones.                     |
| **wurth-extractor** | Claude 3.5 Haiku  | Identifica cada producto individual del pliego con descripción, cantidad, unidad y precio unitario. Extrae ~48 materiales del pliego de prueba.                                      |
| **wurth-matcher**   | Claude 3.5 Sonnet | Cruza cada material contra el catálogo Würth por similitud semántica. Devuelve top 3 coincidencias con índice de confianza (0-100%) y clasifica por familia.                         |
| **wurth-scorer**    | Claude 3.5 Sonnet | Calcula scorecard de viabilidad (0-100 pts) en 5 dimensiones: Afinidad Catálogo (/40), Plazos (/20), Pagos (/20), Solvencia (/10), Servicios (/10). Integra precios. Emite Go/No-Go. |
| **wurth-reporter**  | Claude 3.5 Sonnet | Genera informe narrativo con resumen ejecutivo, análisis por familias, recomendaciones y veredicto.                                                                                  |

### Optimización de modelos

Se evaluó **Haiku vs Sonnet** en el matcher:

- **Haiku**: Rápido y barato, pero infla coincidencias artificiales (confianza media 0.84, fuerza matches incorrectos).
- **Sonnet**: Más selectivo y preciso (confianza media 0.66), descarta matches dudosos.

Criterio aplicado: **Haiku para extracción** (tareas repetitivas), **Sonnet para razonamiento** (matching, scoring, redacción).

---

## 📊 Scorecard de Viabilidad

El scorer evalúa 5 dimensiones y emite una recomendación:

| Dimensión             | Máx pts | Qué evalúa                                            |
| --------------------- | ------- | ----------------------------------------------------- |
| Afinidad de Catálogo  | 40      | Cobertura de requisitos + competitividad de precios   |
| Plazos de Entrega     | 20      | ¿Plazos realistas? (>5 días = bien, <3 días = riesgo) |
| Condiciones de Pago   | 20      | ¿Pago ≤60 días? ¿Avales razonables?                   |
| Solvencia Técnica     | 10      | ¿Cumplimos sin socios?                                |
| Potencial de Servicio | 10      | ¿Oportunidad de ORSY, renting, servicios?             |

| Puntuación | Semáforo    | Recomendación   |
| ---------- | ----------- | --------------- |
| >80        | 🟢 Verde    | Go              |
| 50-80      | 🟡 Amarillo | Go con reservas |
| <50        | 🔴 Rojo     | No-Go           |

---

## 💰 Comparativa de Precios

El sistema extrae precios unitarios del pliego y los compara con los precios de referencia del catálogo Würth:

- Calcula margen por producto: `(precio_pliego - precio_wurth) / precio_pliego × 100`
- Resume: productos comparables, margen medio, cuántos Würth es más barato/caro.
- El scorer usa estos datos para ajustar la puntuación de Afinidad de Catálogo (±5 pts).

Ejemplo de resultado: margen medio +9%, Würth más barato en 10 de 13 productos comparables.

---

## 🧪 Resultados de las Pruebas

Pliego de prueba: **AGUASVIRA** (suministro de material de fontanería, herramientas y EPIS, 99.758,55 € sin IVA).

| Ejecución                  | Scorecard | Coincidencias | Margen medio | Würth más barato |
| -------------------------- | --------- | ------------- | ------------ | ---------------- |
| Haiku matcher              | 70/100 🟡 | 20/20         | -69.7%       | 11/20            |
| Sonnet matcher             | 62/100 🟡 | 14/20         | -41.5%       | 8/14             |
| Sonnet (precios ajustados) | 76/100 🟡 | 13/20         | +9.0%        | 10/13            |

---

## 🔮 Evolución a Producción

Con catálogo real (~10.000 productos) y pliegos de 300+ requisitos:

| Mejora                     | Qué hace                              | Impacto              |
| -------------------------- | ------------------------------------- | -------------------- |
| Clasificación por familia  | Filtrar catálogo antes del matching   | 7x menos tokens      |
| Embeddings + vector search | Pre-filtrar a top 30 candidatos (RAG) | 50x menos tokens     |
| Batching                   | Agrupar 5-10 requisitos por llamada   | 5-10x menos llamadas |
| Paralelización             | Llamadas concurrentes al matcher      | 5-10x más rápido     |

Resultado estimado: 300 requisitos × 10.000 productos en ~2 minutos.

---

## � Informe de Ejemplo

👉 [Ver informe HTML renderizado](https://htmlpreview.github.io/?https://github.com/jtrancoso-alt/genbox-pocs/blob/main/wurth-licitaciones/assets/informe-ejemplo.html)

---

## �🛠️ Stack Tecnológico

- **Orquestador:** Pipeline Python con 5 agentes Genbox.
- **Modelos:** Claude 3.5 Haiku + Claude 3.5 Sonnet (AWS Bedrock vía Genbox).
- **Extracción PDF:** pdfplumber.
- **Salida:** Informe HTML visual + JSON estructurado.
- **Código fuente:** [wurth-genbox-poc](https://github.com/jtrancoso-alt/wurth-genbox-poc) (repo privado).
