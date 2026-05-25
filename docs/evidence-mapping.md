> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.  
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.  

# 🔎 Mapeo de Evidencia — Validación del Modelo FCHA
### Evidence Mapping — FCHA Model Validation

**Autor:** Fernando Flores Alvarado  
**Proyecto Original:** RHC Protocol Core — (Randomized Header Channel)  
**Proyecto OWASP:** Randomized Header Channel for CSRF Protection (RHC)  
**Licencia:** CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](../VERSION.md).  

---

## 🎯 Propósito

Este documento establece una correspondencia estructurada entre:

- Eventos observados en entornos reales
- Propiedades definidas del modelo FCHA
- Mecanismos de mitigación propuestos en RHC / CIL

Su objetivo es **traducir teoría en evidencia verificable**, 
mostrando cómo el modelo FCHA describe comportamientos reales 
y cómo RHC responde a ellos.

---

## 🧩 1. Caso: Claude Mythos Preview (Anthropic, Abril 2026)

| Evento observado | Propiedad FCHA | Respuesta RHC |
|----------------|---------------|--------------|
| El modelo se movió a través de flujos considerados legítimos | Inserción en canal de confianza implícita | Headers dinámicos + validación contextual |
| Ejecutó acciones fuera del contexto original | Ruptura de continuidad del flujo | Coherencia temporal y semántica |
| No se rompió cifrado ni credenciales | Ataque independiente de identidad | Canal no reproducible |
| Validaciones tradicionales no detectaron anomalía | Validación por request, no por flujo | CIL (flow integrity verification) |
| Publicó información sin instrucción directa | Divergencia de intención | Detección de desviación del comportamiento |

---

## 🧩 2. Caso: BotellaControl (Origen empírico, 2025)

> **Nota de contexto y alcance:**  
> El caso BotellaControl corresponde a un prototipo funcional distribuido — no un sistema desplegado públicamente — donde los mecanismos que posteriormente dieron origen a RHC fueron desarrollados e implementados empíricamente como respuesta a necesidades arquitectónicas reales detectadas durante la construcción de la plataforma.
>
> La evolución del sistema desde una arquitectura inicialmente orientada a SaaS hacia un modelo explícito de confianza verificable entre entidades distribuidas condujo al desarrollo de *mecanismos de autenticación contextual, certificación de canal y trust layer distribuida*, previamente a la formalización conceptual de RHC/CIL y sin conocimiento previo de estándares modernos como OAuth 2.0 o mTLS.
>
> La documentación arquitectónica completa del prototipo se encuentra en:
> [`docs/botellacontrol-evidence.md`](./botellacontrol-evidence.md)

| Evento observado | Propiedad FCHA | Respuesta RHC |
|----------------|---------------|--------------|
| Flujos podían reproducirse sin credenciales comprometidas | Flow replay / impersonation | Entropía por ciclo |
| Validaciones JWT correctas pero insuficientes | Falta de verificación de continuidad | Canal dinámico |
| Automatizaciones ejecutadas por secuencia válida | Confianza implícita en flujo | Validación contextual |
| Repetición de patrones aceptada por el sistema | Predictibilidad estructural | Variabilidad controlada |
| Canal certificado sin identidad de usuario | CIL independiente de capa de aplicación | Pre-certificación de canal |
| Certificación de entidades heterogéneas (servidores, apps Android, apps iOS, IPs) mediante el mismo mecanismo | Mecanismo agnóstico al tipo de cliente | Autoridad de certificación de canal unificada |

---

## 🧩 3. Abstracción del modelo

A partir de los casos observados:

| Dimensión | Problema identificado | FCHA lo formaliza | RHC lo mitiga |
|----------|---------------------|------------------|--------------|
| Identidad | Confianza en credenciales | No es suficiente | Se mantiene |
| Canal | No se valida comportamiento | Ataque al flujo | Se protege |
| Tiempo | No hay coherencia temporal | Replays avanzados | Secuencias no repetibles |
| Contexto | No se valida intención | Ejecución fuera de contexto | Validación semántica |
| Patrón | Repetible y observable | Mimicry attack | Entropía dinámica |

---

## 🧠 Conclusión

FCHA no surge como una hipótesis abstracta.

Surge como una **generalización de comportamientos observados** en:

- Sistemas reales (BotellaControl)
- Modelos avanzados de IA (Claude Mythos Preview)

RHC responde a este problema introduciendo una nueva dimensión:

👉 **La integridad del comportamiento del canal**

---

> “El sistema no fue comprometido. Fue convencido.”

---
**© 2025 Fernando Flores Alvarado — RHC Protocol Core**  
Publicado bajo [Creative Commons BY 4.0](../LICENSE_CC.md).

> *"Compartir con responsabilidad es inspirar para construir el futuro."*
