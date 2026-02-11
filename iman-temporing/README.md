📄 PoC: Análisis de Licitaciones y Generación de Planes (RFP Engine)
Cliente: Iman Temporing | Estado: Finalizado / Validación de Negocio

🎯 El Desafío
Iman Temporing gestiona licitaciones (RFPs) de gran volumen donde el error humano en la lectura de pliegos puede suponer la exclusión del concurso o pérdidas económicas. El objetivo es automatizar la disección de estos documentos y generar automáticamente uno de los entregables más críticos: el Plan de Formación.

🏗️ Arquitectura del Sistema (Multi-Agente Especializado)
La solución utiliza una arquitectura de Agentes de Extracción Estructurada. En lugar de un asistente genérico, se han diseñado 5 perfiles que analizan el documento en paralelo:

1. Agentes de Disección (Análisis de Pliegos)
   Analista General: Extrae la "foto fija" (Importes, fechas de presentación, solvencia técnica/económica y prórrogas).

Analista Técnico: Se centra en la ejecución (Personal a subrogar, medios materiales, número de páginas permitidas y obligaciones operativas).

Analista Administrativo: Desglosa la burocracia (Contenido exacto de los Sobres A, B y C, anexos necesarios y gestión de lotes).

Analista de Fórmulas: El "matemático" del grupo. Extrae criterios de puntuación y traduce las fórmulas de adjudicación (ej. fórmulas de baja temerosa o precio).

2. Agente de Generación (Redactor de Formaciones)
   Misión: Crear un Plan de Formación de +20 páginas basado en los requisitos del pliego.

Lógica RAG: Utiliza la herramienta contenido cursos para nutrirse de una base de conocimiento interna de IMAN y asegurar que la oferta es realista y cumple con la normativa vigente.

📚 Base de Conocimiento (RAG)
El sistema no solo lee el pliego, sino que contrasta la información con dos fuentes maestras:

Cuadro de Características del Acuerdo Marco: Para validar lotes y requisitos de solvencia pre-acordados.

Catálogo de Cursos IMAN: Para que el "Redactor de Formaciones" proponga cursos reales con objetivos y duraciones precisas.
