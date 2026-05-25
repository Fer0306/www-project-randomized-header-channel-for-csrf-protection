> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.  
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# Prototipo de implementación real — BotellaControl

**Autor:** Fernando Flores Alvarado  
**Proyecto Original:** RHC Protocol Core — (Randomized Header Channel)  
**Proyecto OWASP:** Randomized Header Channel for CSRF Protection (RHC)  
**Licencia:** Apache 2.0 (código) + CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](../VERSION.md).

---

> **Nota de precisión sobre la propuesta original:**  
> En los documentos de propuesta inicial enviados a OWASP (junio–julio 2025), se hace referencia a una *"real-world SaaS platform"*. Esta nota complementa y precisa ese contexto: BotellaControl es un **prototipo funcional local** — no un sistema desplegado públicamente — concebido inicialmente con arquitectura orientada a SaaS para una plataforma de inventario visual con IA aplicada al sector de hostelería.  
>  
> Durante la evolución de la plataforma surgió la necesidad de establecer mecanismos de autenticación y confianza verificable entre nodos distribuidos, lo que condujo al desarrollo empírico de una arquitectura tipo *Server-to-Server Trust Layer*.  
>  
> Su valor como evidencia radica en la validación funcional del mecanismo y en la evolución arquitectónica que dio origen posteriormente al modelo conceptual RHC/CIL, no en su escala de producción.

---

## Contexto

Previo a la especificación formal de RHC, el autor desarrolló un prototipo local denominado **BotellaControl** — un sistema distribuido para el monitoreo de contenido en envases translúcidos — en el cual los principios fundamentales de la pre-certificación de canal fueron implementados de forma independiente y validados en un entorno funcional controlado.

Este prototipo constituye evidencia empírica de que el mecanismo RHC fue derivado a partir de una necesidad arquitectónica real, antes de su conceptualización formal como proyecto OWASP.

---

## Evolución del proyecto

BotellaControl no nació inicialmente como una plataforma distribuida orientada a integridad de canal.

El proyecto comenzó originalmente como una herramienta experimental para medición visual de contenido en envases translúcidos. Durante su evolución, las necesidades arquitectónicas del sistema condujeron progresivamente hacia un modelo explícito de confianza verificable entre nodos distribuidos.

### Historia del proyecto

| Período | Fase |
|---------|------|
| 2009–2010 | Diseño original de la herramienta de medición |
| 2010–2014 | Desarrollo fase 1 — funcional, sin detección de botellas (tecnología ML no disponible)<br><blockquote>La herramienta de medición alcanzó una implementación funcional temprana, aunque limitada por las capacidades tecnológicas disponibles en ese momento, particularmente en visión por computadora y modelos de detección.</blockquote> |
| Oct 2023 – Nov 2024 | Se retoma el proyecto y se hace reconstrucción total utilizando tecnologías modernas de ML/TensorFlow — se define el nombre "BotellaControl" |
| Dic 2024 | Decisión de desarrollar plataforma propia — ninguna plataforma existente alcanzaba el nivel de seguridad requerido |
| **Ene–May 2025** | **Desarrollo de la plataforma — aquí nace el mecanismo S2S y RHC** |
| Jun–Jul 2025 | Preparación y envío de la propuesta formal a OWASP |

Durante el desarrollo de la plataforma web (enero–mayo 2025), surgió un problema arquitectónico específico:

> ¿Cómo verificar de forma confiable la identidad e integridad de nodos distribuidos antes de permitir comunicación sensible entre ellos?

La necesidad de resolver este problema condujo al desarrollo empírico de un mecanismo propio de autenticación y certificación entre servidores (*Server-to-Server Trust Layer*), dentro del cual posteriormente emergerían los principios fundamentales de RHC.

La conceptualización formal del protocolo como proyecto independiente ocurriría posteriormente, durante la preparación de la propuesta enviada a OWASP en 2025.

---

## Arquitectura relevante

El prototipo implementa una arquitectura centrada en un **servidor principal (Servidor A)** que actúa como autoridad de certificación de canal para múltiples tipos de entidades heterogéneas dentro de un ecosistema distribuido de comunicación confiable.

### Tipos de entidades soportadas

- **Servidor A (principal):** API con base de datos. Actúa como autoridad de certificación del canal de comunicaciones y proveedor de recursos. Centraliza el registro y validación de todas las entidades autorizadas.
- **Servidores web secundarios (B, C...):** Aplicaciones web en dominios distintos. Se autentican ante el Servidor A antes de construir y entregar cualquier página al navegador.
- **Apps nativas Android:** Aplicaciones móviles Android. Su identidad consiste en un identificador lógico persistente (`Dominio_Id`) embebido en compilación y verificable por el servidor principal.
- **Apps nativas iOS:** Aplicaciones móviles iOS. Su identidad consiste en un identificador lógico persistente (`Dominio_Id`) embebido en compilación y verificable por el servidor principal.
- **Entornos locales de desarrollo autorizados:** Instancias locales utilizadas durante el desarrollo del prototipo en la computadora del autor, registradas explícitamente mediante dirección IP para permitir pruebas controladas de comunicación certificada fuera del entorno principal.

El servidor principal mantiene un **registro centralizado unificado** (`$AC_Allowed_Origins_Servers`) que consolida todas las entidades autorizadas independientemente de su tipo, demostrando que el mecanismo de certificación es agnóstico a la naturaleza de la entidad participante (el cliente).

---

### Roles operativos del Servidor A

El Servidor A posee una arquitectura de múltiples roles operativos:

- Como autoridad de certificación de canal, valida entidades autorizadas y emite credenciales de comunicación.
- Como proveedor de recursos, centraliza el acceso a servicios y datos protegidos.
- Como servidor de aplicación web, puede actuar también como entidad participante del flujo certificado cuando aloja directamente interfaces web.

Esto implica que el mismo nodo físico puede operar bajo diferentes contextos funcionales dentro de la arquitectura, manteniendo separación lógica entre:

- autoridad de certificación
- proveedor de recursos
- entidad participante de aplicación web

Incluso cuando el Servidor A aloja directamente una aplicación web, el mecanismo conserva el mismo modelo conceptual de certificación contextual del canal antes de entregar páginas o recursos sensibles al navegador.

---

## Modelo federado de certificación de entidades (Federated Entity Certification Model)

El servidor principal actúa como una **autoridad de certificación de canal** para cualquier tipo de entidad que requiera comunicación autorizada — no exclusivamente servidores.  

El mecanismo de certificación es unificado, aunque la forma en que cada entidad genera o mantiene su identidad varía según su naturaleza operacional.  

### Diagrama conceptual simplificado

```text
                  ┌───────────────────────────┐
                  │     Servidor Principal    │
                  |  Channel Trust Authority  |
                  └─────────────┬─────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
                ▼               ▼               ▼

          ┌────────────┐  ┌───────────┐  ┌────────────┐
          │  Web Apps  │  │  Android  │  │  iOS Apps  │
          │  Servers   │  │  Clients  │  │  Clients   │
          └─────┬──────┘  └─────┬─────┘  └──────┬─────┘
                │               │               │
                └───────────────┼───────────────┘
                                ▼

                     ┌─────────────────────┐
                     |  Federated Entity   |
                     |   Trust Registry    |
                     │ ------------------- │
                     │   • Domains         │
                     │   • IUS             │
                     │   • Domain_Id       │
                     │   • Local Dev IPs   │
                     └─────────────────────┘
```

La solicitud de autenticación **transporta dos componentes diferenciados**:  

### Encabezados HTTP personalizados  

Identifican el contexto y tipo de solicitud:  

```http
X-MOBILEAPP: Web
X-PLATFORM-REQUEST: ASWeb
Content-Type: application/json; charset=UTF-8
```

### Cuerpo POST en JSON  

Transporta la identidad de la entidad participante (viaja cifrado por POST):

```json
{
  "SOY": "www.botellacontrol.test",
  "IUS": "<identificador_unico_de_Servidor>"
}
```

### Identidad por tipo de entidad  

| Tipo de entidad | Mecanismo de identidad | Naturaleza |
|---|---|---|
| Servidor web | `IUS` — `md5(IP + hostname + OS + PHP_VERSION)` | Dinámica — huella determinista del entorno físico |
| App Android | `Dominio_Id` persistente | Fija — embebida en compilación |
| App iOS | `Dominio_Id` persistente | Fija — embebida en compilación |
| Entorno local de desarrollo | Dirección IP registrada explícitamente | Temporal — entorno controlado para pruebas |

Esta variedad de mecanismos de identidad evidencia que el servidor principal no asume **cómo** se genera la identidad de una entidad — únicamente exige que **exista** una identidad verificable y que el canal sea certificado antes de establecer comunicación sensible.  

El `IUS` (Identificador Único de Servidor) es una **huella determinista del entorno del servidor**, generada como:

```text
md5(IP + hostname + OS + PHP_VERSION)
```

Para el mismo servidor produce consistentemente el mismo identificador. No es aleatorio por sesión, sino único por entorno físico y configuración operacional.  

La identidad no viaja en encabezados HTTP sino en el cuerpo POST de la solicitud, donde la información es transportada fuera de los encabezados HTTP visibles del canal.
---

## Modelo de confianza entre nodos

La arquitectura evolucionó progresivamente desde una separación tradicional entre front-end y API hacia un modelo explícito de confianza verificable entre nodos.

En lugar de asumir que cualquier servidor perteneciente al mismo ecosistema era automáticamente confiable, el sistema exige que cada nodo demuestre su identidad antes de establecer comunicación sensible con el servidor principal.

Este enfoque introdujo una capa de validación previa al acceso a recursos, separada de la autenticación de usuarios y enfocada exclusivamente en la integridad del canal de comunicación entre componentes distribuidos.

El mecanismo fue desarrollado de forma empírica e independiente, como respuesta directa a necesidades arquitectónicas reales detectadas durante el desarrollo de la plataforma, sin conocimiento previo de estándares modernos como OAuth 2.0 Client Credentials Flow, mTLS o arquitecturas formales de trust layers distribuidas.

Conceptualmente, esta arquitectura converge con modelos modernos de:

- Federated Server Authentication
- Server-to-Server Trust Layer
- Mutual Authentication
- Dynamic Service Identity
- Federated Trust Architecture
- Entity-Based Trust Validation

aunque fue desarrollada de forma independiente como respuesta a necesidades arquitectónicas prácticas detectadas durante el desarrollo de la plataforma.

---

## Tres niveles de confianza — decisión arquitectónica clave

El sistema distingue tres escenarios con respuestas diferenciadas:

| Escenario | Función | Respuesta del sistema |
|-----------|---------|----------------------|
| Webapp en el **mismo servidor** que el backend | `WA_EsElMismoServidor() = TRUE` | Autenticación local directa, sin HTTP |
| Webapp en **servidor autorizado externo** (B o C) | `WA_EsElMismoServidor() = FALSE` | Autenticación vía cURL al Servidor A |
| Webapp en **servidor no autorizado** | Verificación secundaria `WA_EsElMismoServidorReal() = TRUE` para entornos de desarrollo autorizados | `WA_RastrearDatosServidor()` + redirección a página de aplicación no autorizada |

El tercer nivel — rastreo activo de servidores no autorizados y de entornos de desarrollo no autorizados — va más allá de rechazar la solicitud: el sistema identifica y registra el intento de acceso desde un nodo desconocido.

---

## Respuesta del Servidor A

Tras validar la identidad del nodo, el Servidor A emite:

- Token de acceso JWT
- Token de refresco JWT
- Encabezado RHC (seleccionado aleatoriamente del pool definido)

---

## El concepto de "página certificada"

Los tokens emitidos son **embebidos en la página HTML antes de ser entregada al navegador**. Esto significa que cuando el navegador recibe la página, ya porta las credenciales necesarias para hacer peticiones seguras al Servidor A — sin requerir un viaje adicional para obtener tokens.

Este mecanismo — denominado aquí **página certificada** — garantiza que el canal de comunicación lleve credenciales de integridad desde el momento en que se establece, no a partir de la primera solicitud del usuario.

```text
Flujo estándar (OAuth / SPA):
  Servidor → [página sin tokens] → Navegador → [pide tokens] → API → [recibe tokens] → peticiones

Flujo BotellaControl (página certificada):
  Servidor B → [autentica con A] → [recibe JWT + RHC] → [construye página + embebe tokens] → Navegador → [peticiones ya certificadas]
```

---

## Relevancia para la Communication Integrity Layer (CIL)

Esta implementación opera **completamente a nivel de canal** — de forma independiente a la autenticación de usuarios, la cual no fue implementada en el prototipo.

Esta decisión de diseño demuestra empíricamente que RHC pertenece a una capa de seguridad distinta:

| Capa | Mecanismo | ¿Requiere identidad de usuario? |
|---|---|---|
| Capa de Red | TLS / mTLS | No |
| **Communication Integrity Layer** | **RHC — BotellaControl** | **No** |
| Capa de Aplicación | Sesiones, CSRF tokens tradicionales | Sí |

El canal queda certificado **sin ningún contexto de identidad de usuario**. Esta es la característica fundamental de los mecanismos a nivel CIL: aseguran el canal antes de que la capa de aplicación intervenga.

> Esta arquitectura evidencia que RHC no opera como una mejora de mecanismos tradicionales de capa de aplicación, sino como una capa transversal independiente de integridad del canal de comunicación entre componentes distribuidos, ubicada entre la capa de transporte y los mecanismos tradicionales de autenticación y seguridad de aplicación.

---

## Las tres entradas de la API del Servidor A

La API del nodo principal implementa responsabilidades claramente separadas:

1. **Autenticación de nodos secundarios** — Valida `IUS` + dominio mediante encabezados específicos. Emite JWT + Refresh JWT + RHC.
2. **Refresco de tokens** — Valida el token de refresco enviado por el nodo secundario. Emite nuevos JWT + Refresh JWT + RHC.
3. **Acceso a recursos** — Valida RHC (primera capa) + JWT + CSRF token. Solo si todo es válido, permite acceso a los recursos del Servidor A.

Esta separación de responsabilidades anticipa el patrón de *separation of concerns* que posteriormente formalizarían arquitecturas modernas de autenticación distribuida.

---

## Declaración de alcance

BotellaControl es un **prototipo funcional local**, no un sistema desplegado públicamente.

El proyecto fue concebido con intención de despliegue SaaS y evolución comercial futura, aunque dicha etapa no fue completada al momento de la propuesta enviada a OWASP.

Su valor como evidencia no radica en la escala de producción, sino en demostrar que:

- El mecanismo RHC fue **derivado de forma independiente** a partir de una necesidad práctica concreta.
- Fue implementado y validado funcionalmente antes de su conceptualización formal.
- La arquitectura evolucionó desde un modelo SaaS tradicional hacia un esquema explícito de confianza verificable entre nodos.
- La solución resultante converge conceptualmente con patrones modernos de autenticación distribuida y trust layers utilizados en la industria.

---

## Conclusión

La existencia de BotellaControl como implementación independiente previa respalda la afirmación de que RHC aborda un **problema de seguridad real y no trivial** — uno que surgió orgánicamente durante el desarrollo de una arquitectura distribuida y que fue resuelto mediante razonamiento arquitectónico práctico.

La evolución del sistema desde una arquitectura inicialmente orientada a SaaS hacia un modelo explícito de confianza entre nodos constituye el origen empírico del concepto posteriormente formalizado como RHC/CIL.

Esta convergencia conceptual con patrones modernos de autenticación distribuida — alcanzada de forma independiente y sin conocimiento previo de estándares como OAuth 2.0 o mTLS durante el desarrollo inicial del mecanismo — refuerza la validez de RHC como mecanismo de seguridad formalizado y su pertenencia a la **Communication Integrity Layer** como categoría diferenciada de protección de canal.

---

*Autor: Fernando Flores Alvarado — OWASP Project Leader, RHC*  
*Fecha de este anexo: 2025*

---

## 📜 Licencia

- **Código fuente y scripts:** [Apache License 2.0](../LICENSE.md)
- **Documentación y diagramas:** [Creative Commons BY 4.0](../LICENSE_CC.md)

> © 2025 Fernando Flores Alvarado — RHC Protocol Core  
> *"Compartir con responsabilidad es inspirar para construir el futuro."*
