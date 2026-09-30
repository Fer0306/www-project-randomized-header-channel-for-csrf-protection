> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# Alineación de Licencias — RHC Protocol Core

**Autor:** Fernando Flores Alvarado  
**Proyecto Original:** RHC Protocol Core — (Randomized Header Channel)  
**Proyecto OWASP:** Randomized Header Channel for CSRF Protection (RHC)  
**Licencia:** Apache 2.0 (código) + CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](./VERSION.md).

---

## Propósito

Este documento explica el modelo de licenciamiento del repositorio **RHC Protocol Core**:
por qué se adoptó un esquema dual, cómo se aplica a cada tipo de contenido, y qué significa
para organizaciones, investigadores y desarrolladores que deseen adoptarlo o citarlo.

---

## 1. Justificación del esquema dual

El protocolo RHC combina un **componente técnico** (implementación) y un **componente conceptual** (investigación).
Por tanto, se adopta una estructura dual que protege ambos aspectos de forma independiente:

| Tipo de contenido | Licencia aplicada | Propósito |
|---|---|---|
| Código, scripts y PoC | Apache 2.0 | Permitir libre uso, distribución y modificación, manteniendo atribución |
| Documentación, diagramas y textos | CC BY 4.0 | Permitir divulgación y adaptación con reconocimiento autoral |

Esta dualidad garantiza compatibilidad con OWASP, repositorios académicos (arXiv, IEEE)
y publicaciones abiertas (Medium, Dev.to), respetando los derechos intelectuales del autor original.

---

## 2. Compatibilidad entre licencias

Ambas licencias son abiertas y compatibles en su propósito:

- La **Apache 2.0** es permisiva, reconocida por la Free Software Foundation y OSI.
- La **CC BY 4.0** permite reutilización siempre que se dé crédito apropiado.
- Ninguna impone restricciones de copyleft o distribución cerrada.

En conjunto, permiten que:

- El código sea reutilizado o integrado en otros proyectos.
- La documentación sea citada o traducida libremente, manteniendo la autoría.
- Los usuarios comprendan qué partes son reutilizables y bajo qué términos.

Este modelo no introduce restricciones adicionales más allá de las definidas en las licencias originales (Apache 2.0 y CC BY 4.0).

---

## 3. Estructura y ubicación de los archivos de licencia

| Archivo | Contenido | Aplicación |
|---|---|---|
| `/LICENSE` y `/LICENSE.md` | Texto completo de Apache License 2.0 | Código, PoC y scripts |
| `/LICENSE_CC` (inglés) y `/LICENSE_CC.md` (español) | Resumen de CC BY 4.0 con enlace al texto legal oficial | Documentación y textos |
| `/NOTICE` y `/NOTICE.md` | Atribución oficial y resumen de derechos del autor | Todo el repositorio |
| `/VERSION` y `/VERSION.md` | Número de versión técnica, codename y estado del proyecto | Identificación de versión y coherencia documental |

Las versiones `.md` son las que se enlazan desde el README y la documentación; las versiones sin extensión se conservan por compatibilidad con las convenciones habituales de licencias y atribución. Cada par debe mantenerse con el mismo contenido de fondo.

---

## 4. Filosofía del modelo de licenciamiento

Este modelo refleja un principio central del protocolo RHC:

> Las implementaciones de seguridad deben permanecer adaptables y confidenciales,
> mientras que el conocimiento y la innovación deben permanecer abiertos y compartidos.

El código es abierto para que cualquier organización pueda implementarlo libremente.
La documentación es abierta para que el origen intelectual del protocolo permanezca
rastreable y preservado, independientemente de cómo evolucione cada implementación.

Esto significa que dos organizaciones pueden implementar RHC de formas completamente
distintas — con sus propias estructuras de headers, sus propios modelos de entropía,
su propia lógica interna — sin que ninguna de las dos tenga obligación de revelar
su implementación, y sin que el autor necesite acceder a sus sistemas.

El conocimiento es compartido. Las implementaciones son privadas. La autoría permanece atribuible y rastreable a lo largo del tiempo.

---

## 5. Implementación sin exposición — para organizaciones

Las organizaciones pueden adoptar RHC sin necesidad de:

- Compartir su arquitectura interna.
- Exponer configuraciones de seguridad.
- Proporcionar acceso a sistemas propietarios.
- Divulgar adaptaciones o extensiones del modelo.

La licencia Apache 2.0 permite integración en sistemas privados o sensibles
sin ninguna obligación de divulgación. Esto hace que RHC sea compatible con:

- Sistemas financieros y bancarios
- Plataformas gubernamentales
- Infraestructuras de salud
- Aplicaciones empresariales de alto impacto

Cada organización puede seleccionar el nivel de implementación adecuado (Básico a Adaptativo),
definir sus propias estructuras de headers, personalizar sus estrategias de entropía
y evolucionar el modelo internamente con el tiempo.

Este modelo no introduce restricciones adicionales más allá de las definidas en las licencias originales (Apache 2.0 y CC BY 4.0).

> RHC no es una implementación fija — es un modelo de seguridad que evoluciona dentro de cada sistema.

---

## 6. Modelo de atribución recomendada

Cuando se utiliza, cita o adapta contenido de este repositorio:

> **RHC Protocol Core** — Autor: Fernando Flores Alvarado (2025).
> Basado en el protocolo *Randomized Header Channel (RHC)*.
> Código bajo Apache 2.0. Documentación bajo CC BY 4.0.
> [https://github.com/Fer0306/RHC_Protocol_Core](https://github.com/Fer0306/RHC_Protocol_Core)

---

## 7. Relación con OWASP y coherencia de licencias

El proyecto oficial de OWASP utiliza:

- Apache 2.0 para el código del sitio y demostraciones.
- CC BY-SA 4.0 para el contenido alojado en el portal OWASP.

El **RHC Protocol Core** mantiene:

- Apache 2.0 para compatibilidad técnica y futura integración.
- CC BY 4.0 (sin Share-Alike) para conservar independencia académica y editorial.

De esta forma se logra un equilibrio entre apertura comunitaria y preservación de autoría original.

---

## 8. Referencias y recursos legales

- [Apache License 2.0 — Texto oficial](https://www.apache.org/licenses/LICENSE-2.0)
- [Creative Commons Attribution 4.0 — Texto oficial](https://creativecommons.org/licenses/by/4.0/)
- [OWASP Licensing Policy](https://owasp.org/about/licensing/)
- [Free Software Foundation License List](https://www.gnu.org/licenses/license-list.html)
- [Open Source Initiative Approved Licenses](https://opensource.org/licenses/)

---

## 📜 Licencia

- **Código fuente y scripts:** [Apache License 2.0](./LICENSE.md)
- **Documentación y diagramas:** [Creative Commons BY 4.0](./LICENSE_CC.md)

> © 2025 Fernando Flores Alvarado — RHC Protocol Core  
> *“Compartir con responsabilidad es inspirar para construir el futuro.”*
