---
name: prd-test-cases
description: PRD, casos de prueba, QA manual, test-cases. Usar cuando el usuario quiera generar un documento de casos de prueba manuales desde un PRD, especialmente PRDs creados con prd.md, para que un agente autónomo de QA valide una funcionalidad e itere con un agente de desarrollo hasta dejarla 100% lista.
---

# Casos De Prueba Desde PRD

Usa este skill cuando el usuario solicite crear, derivar, revisar o guardar un documento de casos de prueba manuales a partir de un PRD.

El objetivo es producir un documento suficientemente específico para que un agente autónomo de QA pueda ejecutar validaciones manuales, registrar defectos accionables, pedir correcciones a un agente de desarrollo y repetir el ciclo hasta que la funcionalidad cumpla completamente el PRD.

Este skill está diseñado para PRDs generados mediante `prd.md`, pero también puede usarse con cualquier PRD que contenga requisitos funcionales, criterios de aceptación, sección de verificación o matriz de trazabilidad.

## Objetivo

Generar un documento Markdown con casos de prueba manuales, concretos, trazables y ejecutables desde un PRD existente.

El documento debe permitir que un agente autónomo de QA pueda:

- Validar todos los requisitos funcionales del PRD.
- Validar los requisitos no funcionales relevantes mediante observaciones manuales cuando sea posible.
- Ejecutar flujos felices, negativos, borde y regresiones.
- Detectar defectos, brechas, contradicciones y comportamientos incompletos.
- Reportar bugs con suficiente detalle para que un agente de desarrollo pueda corregirlos.
- Reintentar los casos fallidos hasta declarar la funcionalidad lista.

## Principios

- No inventes requisitos de producto.
- No generes listas genéricas de QA.
- No propongas tests unitarios, de integración ni automatización como salida principal.
- El documento debe contener solo casos de prueba manuales para QA.
- Cada requisito funcional debe tener al menos un caso de prueba manual asociado.
- Cada caso debe ser específico, ejecutable y verificable por observación.
- Cada caso debe estar trazado a IDs del PRD, criterios de aceptación o filas de la matriz de trazabilidad cuando existan.
- Si el PRD no tiene IDs, crea referencias locales estables, por ejemplo `PRD-RF-01`, `PRD-AC-03` o `PRD-RNF-02`.
- Mantén el idioma español salvo que el usuario pida explícitamente otro idioma.
- Pregunta solo cuando la información faltante cambie de forma material los casos de prueba.
- Si faltan datos de ambiente, roles, permisos, integraciones o datos de prueba, márcalos como supuestos u open questions.

## Flujo De Trabajo

### 1. Identificar El PRD

Si el usuario entrega una ruta, lee ese PRD.

Si no entrega una ruta:

- Revisa `AGENTS.md` y `CLAUDE.md` en la raíz del proyecto o rutas relevantes.
- Respeta instrucciones del proyecto sobre documentación, idioma, rutas y convenciones.
- Busca PRDs en el directorio definido por esas instrucciones.
- Si no existe instrucción específica, busca en `prds/`.

### 2. Verificar Que El PRD Sea Usable

Un PRD usable debería contener suficiente información sobre:

- Objetivo, problema o alcance.
- Requisitos funcionales.
- Criterios de aceptación.
- Flujos de usuario.
- Reglas de negocio.
- Estados, permisos o roles.
- Errores esperados o casos borde.
- Requisitos no funcionales.
- Sección de verificación o matriz de trazabilidad.

Si el PRD está incompleto, pregunta al usuario si desea continuar con supuestos explícitos o esperar un PRD completo.

### 3. Extraer Información Para QA

Antes de redactar casos, crea mentalmente un inventario con:

- Roles, permisos y tipos de usuario.
- Funcionalidades dentro de alcance.
- Comportamientos fuera de alcance.
- Requisitos funcionales.
- Criterios de aceptación.
- Reglas de negocio.
- Entidades, datos, validaciones, estados y transiciones.
- Dependencias, integraciones, APIs, notificaciones o procesos externos.
- Estados vacíos, errores y restricciones.
- Riesgos de regresión.
- Requisitos no funcionales verificables manualmente.

### 4. Hacer Preguntas Solo Si Son Necesarias

No hagas una entrevista extensa por defecto.

Pregunta únicamente si la respuesta cambia los casos de prueba. No hagas más de 5 preguntas por iteración.

Preguntas útiles:

- ¿Qué ambiente debe usar el agente QA para ejecutar estos casos?
- ¿Qué roles y permisos deben estar disponibles para la validación?
- ¿Existen datos de prueba obligatorios o cuentas específicas?
- ¿Hay navegadores, dispositivos, idiomas, monedas o zonas horarias obligatorias?
- ¿Qué dependencias externas deben estar disponibles o simuladas para QA manual?

Si el usuario pide avanzar sin responder, continúa con supuestos explícitos.

### 5. Generar El Documento

Genera un documento Markdown siguiendo la estructura obligatoria de este skill.

### 6. Guardar El Documento

Antes de escribir el archivo, propone la ruta exacta y pide confirmación, salvo que el usuario haya indicado explícitamente una ruta.

Reglas de destino:

- Si `AGENTS.md`, `CLAUDE.md` u otra documentación del proyecto define una ruta para casos de prueba, úsala.
- Si no existe convención, usa `test-cases/`.
- Usa el formato `YYYY-MM-DD-casos-prueba-<nombre-prd>.md`.
- El nombre debe ir en minúsculas y separado por guiones.

## Estructura Obligatoria Del Documento

### 1. Metadatos

Incluye:

- Título del documento.
- Ruta del PRD fuente.
- Fecha o versión del PRD si existe.
- Fecha de generación.
- Nombre de la funcionalidad.
- Agente QA responsable si se conoce.
- Ambiente objetivo si se conoce.
- Rama, ticket, épica o referencia relacionada si existe.

### 2. Alcance De QA

Incluye:

- Comportamientos que deben validarse.
- Comportamientos fuera de alcance.
- Dependencias y precondiciones generales.
- Supuestos que afectan la ejecución.
- Preguntas abiertas que impiden validar al 100%.

### 3. Estrategia De QA Manual

Describe cómo debe ejecutar la validación un agente autónomo de QA.

Incluye:

- Priorización de casos críticos primero.
- Validación de flujos felices principales.
- Validación de errores y escenarios negativos.
- Validación de permisos y roles.
- Validación de estados vacíos, límites y transiciones.
- Validación de regresión cuando el PRD mencione comportamiento existente.
- Validación manual de requisitos no funcionales observables.

No incluyas estrategia de tests unitarios, integración o E2E automatizados. Si el PRD menciona automatización, úsala solo como contexto, pero el documento generado debe enfocarse en ejecución manual.

### 4. Datos De Prueba

Define datos concretos siempre que el PRD lo permita:

- Usuarios y roles.
- Permisos.
- Cuentas de prueba.
- Registros existentes.
- Valores válidos.
- Valores inválidos.
- Valores límite.
- Estados iniciales.
- Respuestas esperadas de dependencias externas.
- Fechas, horas, zonas horarias, idiomas, monedas o locales.

Si no se conocen valores exactos, usa placeholders explícitos como `<usuario-admin-valido>` y márcalos como supuestos.

### 5. Matriz De Cobertura

Incluye una matriz que cubra todos los requisitos funcionales.

Formato obligatorio:

| Ref PRD | Requisito | Criterio de aceptación | Casos de prueba | Prioridad | Estado |
| ------- | --------- | ---------------------- | --------------- | --------- | ------ |

El estado inicial debe ser `Pendiente`, salvo que el usuario entregue resultados de ejecución.

Ningún requisito funcional puede quedar sin casos de prueba asociados. Si algo no puede probarse manualmente, indícalo explícitamente y explica qué información falta.

### 6. Casos De Prueba Manuales

Cada caso debe usar este formato:

```markdown
#### TC-001: <comportamiento específico a validar>

- Referencias PRD: <RF/AC/RNF o sección>
- Prioridad: Crítica | Alta | Media | Baja
- Tipo: Flujo feliz | Negativo | Borde | Permisos | Regresión | No funcional | Exploratorio guiado
- Objetivo:
- Precondiciones:
- Datos de prueba:
- Pasos:
  1. <acción exacta>
  2. <acción exacta>
- Resultado esperado:
- Evidencia a capturar:
- Señales de falla:
- Notas para el agente QA:
```

Reglas para escribir casos:

- Cada caso debe validar un comportamiento claro.
- Los pasos deben ser suficientemente precisos para que un agente los ejecute sin reinterpretar el PRD.
- El resultado esperado debe ser observable.
- Incluye casos negativos para entradas inválidas, acciones prohibidas, datos faltantes, estados inválidos, duplicados, dependencias caídas y permisos insuficientes cuando aplique.
- Incluye casos borde para mínimos, máximos, vacíos, límites, concurrencia visible, reintentos, estados obsoletos y transiciones cuando aplique.
- Incluye regresiones si el PRD menciona comportamiento existente que no debe romperse.
- Marca como `Crítica` cualquier prueba cuya falla bloquee el objetivo principal, corrompa datos, rompa permisos, afecte seguridad, impida operar la funcionalidad o bloquee release.

### 7. Flujos Críticos De Release

Lista solo los flujos que deben pasar antes de liberar.

Para cada flujo incluye:

- ID del flujo.
- Referencias PRD cubiertas.
- Rol del usuario.
- Resumen del camino feliz.
- Casos de prueba incluidos.
- Validaciones obligatorias.
- Escenario negativo o de recuperación principal.
- Requisitos de limpieza de datos.

### 8. Escenarios Negativos Y Borde

Agrupa escenarios por riesgo:

- Validaciones y entradas malformadas.
- Autenticación, autorización y permisos.
- Estados vacíos, parciales, duplicados u obsoletos.
- Transiciones de estado inválidas.
- Errores de dependencias externas.
- Timeouts, reintentos o pérdida de conectividad visibles para el usuario.
- Concurrencia observable por QA manual.
- Navegadores, dispositivos, idioma, zona horaria o accesibilidad cuando aplique.

Cada escenario debe referenciar uno o más IDs de casos de prueba.

### 9. Validación Manual De Requisitos No Funcionales

Cubre solo requisitos no funcionales presentes en el PRD o verificables manualmente sin inventar umbrales.

Áreas posibles:

- Rendimiento percibido.
- Seguridad observable.
- Privacidad y exposición de datos.
- Accesibilidad básica.
- Observabilidad visible para QA, como logs, trazas o mensajes de auditoría si el proyecto los expone.
- Compatibilidad de navegador o dispositivo.
- Usabilidad.

No inventes thresholds. Si el PRD no define métricas, escribe `umbral no definido en el PRD` y deja una pregunta abierta.

### 10. Protocolo Para Agente Autónomo De QA

Incluye este ciclo de ejecución:

1. Leer el PRD y este documento completo.
2. Preparar ambiente, usuarios, permisos y datos de prueba.
3. Ejecutar primero casos `Crítica`, luego `Alta`, `Media` y `Baja`.
4. Registrar evidencia para cada caso crítico y cada falla.
5. Para cada caso fallido, crear un reporte de bug con ID del caso, referencias PRD, pasos, resultado esperado, resultado actual, evidencia, severidad y riesgo.
6. Enviar el bug al agente de desarrollo.
7. Esperar corrección o aclaración.
8. Reejecutar el caso fallido y los casos de regresión relacionados.
9. Repetir hasta que todos los casos críticos y altos pasen o exista una excepción aprobada explícitamente.
10. Generar un resumen final de QA con aprobación, rechazo o aprobación con riesgos.

### 11. Plantilla De Reporte De Bug

Incluye esta plantilla:

```markdown
### BUG-<número>: <título corto>

- Caso relacionado: TC-<número>
- Referencias PRD:
- Severidad: Bloqueante | Crítica | Mayor | Menor | Trivial
- Ambiente:
- Precondiciones:
- Pasos para reproducir:
- Resultado esperado:
- Resultado actual:
- Evidencia:
- Área sospechosa:
- Riesgo de regresión:
- Notas de retest:
```

### 12. Criterios De Salida

Define criterios explícitos para declarar la funcionalidad lista:

- Todos los requisitos funcionales tienen al menos un caso de prueba manual asociado.
- Todos los casos `Crítica` pasan.
- Todos los casos `Alta` pasan o tienen una excepción aprobada explícitamente.
- No quedan bugs bloqueantes o críticos abiertos.
- Los flujos críticos de release pasan.
- Los requisitos no funcionales manualmente verificables pasan o tienen riesgo aceptado.
- Las preguntas abiertas no bloquean los criterios de aceptación del PRD.
- El resumen final de QA fue generado.

## Criterios De Calidad Del Documento

Antes de finalizar, verifica:

- Todos los requisitos funcionales del PRD están cubiertos.
- Todos los criterios de aceptación tienen al menos un caso o una razón documentada para no poder validarse manualmente.
- Los casos no son duplicados.
- Los casos no son vagos.
- Los pasos son ejecutables por un agente autónomo.
- Los resultados esperados son observables.
- Los casos negativos y borde cubren los riesgos principales.
- Las precondiciones y datos de prueba son claros.
- Las preguntas abiertas y supuestos están explícitos.

## Respuesta Final

Después de crear o actualizar el documento de casos de prueba, responde con:

- Ruta del documento generado.
- Cantidad de casos de prueba creados.
- Cantidad de requisitos del PRD cubiertos.
- Supuestos o preguntas abiertas.
- Recordatorio de reiniciar opencode solo si este skill acaba de instalarse o modificarse.
