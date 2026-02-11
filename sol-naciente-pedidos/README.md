# Sol Naciente: Seguimiento de Pedidos

Sistema de atención al cliente en lenguaje natural para consultar el estado de pedidos de vehículos.

### 🏗️ Arquitectura de Modelos
Se utiliza un enfoque de **Model Sharding** para optimizar costes y latencia:
* **Amazon Nova Pro 1.0:** Capa conversacional, validación de DNI/CIF y formateo de respuesta humana.
* **Amazon Nova Micro 1.0:** Capa de ejecución. Extrae los parámetros y llama a la API técnica.

### ⚙️ Lógica de Integración
* **Herramienta:** `track-car-order`
* **Parámetros:** `cif`, `concesionario`.
* **Backend:** Una Lambda centraliza la consulta SQL en cascada (Concesionario -> Importador -> OMS), evitando que el LLM tenga que conocer la estructura de la DB.

### 💡 Punto Clave
La IA traduce términos técnicos como `LineOffDate` a "Fecha de Salida" y calcula automáticamente la entrega estimada (+2 meses) si el coche está en fabricación.