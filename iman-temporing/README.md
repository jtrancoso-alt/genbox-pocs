# 📄 PoC: Análisis de Licitaciones y Generación de Planes (RFP Engine)

**Cliente:** Iman Temporing | **Estado:** `Finalizado` / `Validación de Negocio`

---

## 🎯 El Desafío

Iman Temporing gestiona licitaciones (**RFPs**) de gran volumen donde el error humano en la lectura de pliegos puede suponer la exclusión del concurso o pérdidas económicas significativas.

El objetivo de esta PoC fue validar el uso de **IA Generativa** para:

- **Automatizar la disección** de documentos complejos.
- **Detectar riesgos** tempranos.
- **Generar entregables críticos**, específicamente el **Plan de Formación**, de forma coherente y alineada con la normativa.

---

## 🏗️ Arquitectura del Sistema (Multi-Agente)

En lugar de un asistente genérico, se diseñó una arquitectura de **Agentes de Extracción Estructurada** que analizan el documento en paralelo para garantizar la máxima precisión.

### 1. Fase de Disección (Análisis de Pliegos)

- **Analista General:** Extrae la "foto fija" del proyecto (Importes, fechas de presentación, solvencia técnica/económica y prórrogas).
- **Analista Técnico:** Se centra en la ejecución operativa (Personal a subrogar, medios materiales, límites de páginas y obligaciones).
- **Analista Administrativo:** Desglosa la "burocracia" (Contenido exacto de los Sobres A, B y C, anexos requeridos y gestión de lotes).
- **Analista de Fórmulas:** El perfil matemático. Identifica criterios de puntuación y traduce las fórmulas de adjudicación (bajas temerarias, cálculos de precio, etc.).

### 2. Fase de Generación (Redactor de Formaciones)

- **Misión:** Creación de un **Plan de Formación de +20 páginas** basado estrictamente en los requisitos del pliego.
- **Lógica RAG:** Se integra con la base de conocimiento de IMAN para asegurar que la oferta es realista y cumple con los estándares internos de la compañía.

---

## 📚 Base de Conocimiento (RAG)

Para evitar alucinaciones y asegurar la calidad técnica, el sistema contrasta el pliego con dos fuentes maestras:

> [!IMPORTANT]
> **Fuentes de Verificación:**
>
> 1. **Cuadro de Características del Acuerdo Marco:** Utilizado para validar lotes y requisitos de solvencia pre-acordados.
> 2. **Catálogo de Cursos IMAN:** Permite al "Redactor de Formaciones" proponer cursos reales con objetivos, contenidos y duraciones precisas.

---

## 🛠️ Stack Tecnológico

- **Orquestador:** Genbox AI.
- **Modelos:** Claude 3.5 Sonnet (Recomendado para análisis legal/técnico).
- **Capacidades:** Procesamiento de PDF, RAG y extracción de entidades.
