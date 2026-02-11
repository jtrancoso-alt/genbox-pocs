# Sol Naciente: Tasación Inteligente (Pipeline Multi-Agente)

Sistema automatizado de valoración de vehículos de ocasión mediante visión y análisis documental.

### 🤖 Pipeline de Agentes
El flujo se orquesta mediante un **Jupyter Notebook** que conecta 4 asistentes especializados:

1. **Analizador de Fotos (Claude 3.5 Sonnet):** Detecta daños exteriores y puntúa su gravedad.
2. **Analizador de Ficha (Nova Pro 1.0):** Realiza el OCR técnico y mapea campos industriales.
3. **Sintetizador de Estado (Claude 3.5 Sonnet):** Une visión y datos, infiriendo información faltante (KM, extras).
4. **Tasador de Mercado (Claude 3.5 Sonnet):** Cruza el informe con tablas de precios históricas y da un rango de valor.

### 🚀 Camino a Producción
Este proyecto está en fase de despliegue. La lógica de orquestación pasará de un Notebook a un servicio de micro-agentes coordinados por una Lambda central.