# Skills, Commands And Subagents

Este repositorio contiene recursos reutilizables para trabajar con agentes de IA en flujos de producto, arquitectura y QA.

## Contenido

- `commands/prd.md`: comando para crear PRDs de forma iterativa.
- `commands/adr.md`: comando para crear ADRs de forma iterativa.
- `skills/prd-test-cases/SKILL.md`: skill para generar casos de prueba manuales desde un PRD.
- `subagents/qa.md`: subagente QA para ejecutar casos de prueba manuales y reportar aprobación o rechazo.

## Instalación En OpenCode

Copia los archivos a tu configuración local de OpenCode:

```bash
mkdir -p ~/.config/opencode/commands
mkdir -p ~/.config/opencode/skills/prd-test-cases
mkdir -p ~/.config/opencode/agent

cp commands/prd.md ~/.config/opencode/commands/prd.md
cp commands/adr.md ~/.config/opencode/commands/adr.md
cp skills/prd-test-cases/SKILL.md ~/.config/opencode/skills/prd-test-cases/SKILL.md
cp subagents/qa.md ~/.config/opencode/agent/qa.md
```

Reinicia OpenCode después de instalar o modificar comandos, skills o subagentes para que los cambios se carguen correctamente.

## Uso En OpenCode

### Crear Un PRD

En una sesión de OpenCode dentro del proyecto donde quieras documentar la funcionalidad, ejecuta:

```text
/prd
```

El comando iniciará una entrevista guiada. Primero revisará instrucciones del proyecto como `AGENTS.md` o `CLAUDE.md`, luego preguntará qué quieres documentar y finalmente propondrá guardar el PRD en `prds/` o en la ruta definida por el proyecto.

### Crear Un ADR

Ejecuta:

```text
/adr
```

El comando guiará la creación de un Architecture Decision Record, revisará ADRs existentes si corresponde y guardará el resultado en `adr/`, `adrs/` o en la ruta indicada por las instrucciones del proyecto.

### Generar Casos De Prueba Desde Un PRD

Pide explícitamente a OpenCode que use el skill:

```text
Usa el skill prd-test-cases para generar casos de prueba manuales desde prds/2026-07-29-mi-feature.md
```

El skill generará un documento Markdown con matriz de cobertura, casos manuales, flujos críticos, escenarios negativos, plantilla de bugs y criterios de salida. Si no indicas una ruta de destino, propondrá una ruta en `test-cases/` antes de escribir el archivo.

### Ejecutar QA Con El Subagente

Después de tener un PRD y un documento de casos de prueba, puedes pedir:

```text
Usa el subagente qa para ejecutar los casos de prueba de test-cases/2026-07-29-casos-prueba-mi-feature.md contra la app local
```

Entrega también, cuando aplique:

- Ruta del PRD.
- Ruta o URL de la aplicación.
- Comandos para levantar el proyecto.
- Ambiente objetivo.
- Usuarios, credenciales o datos de prueba.

El subagente QA no modifica código de producto. Ejecuta validaciones observables con comandos del proyecto, Playwright si está disponible o `curl` para APIs, y responde con `Aprobado` o `Rechazado`.

## Uso En Claude Code

Claude Code puede reutilizar estos archivos como comandos y skills del mismo modo conceptual.

### Opción 1: Usarlos Como Comandos Del Proyecto

Copia los comandos al directorio de comandos de Claude Code del proyecto:

```bash
mkdir -p .claude/commands
cp commands/prd.md .claude/commands/prd.md
cp commands/adr.md .claude/commands/adr.md
```

Luego ejecútalos desde Claude Code:

```text
/prd
/adr
```

### Opción 2: Instalar El Skill De Casos De Prueba

Copia el skill a tu directorio local de skills de Claude:

```bash
mkdir -p ~/.claude/skills/prd-test-cases
cp skills/prd-test-cases/SKILL.md ~/.claude/skills/prd-test-cases/SKILL.md
```

Luego pide a Claude Code:

```text
Usa el skill prd-test-cases para crear casos de prueba manuales desde prds/<archivo>.md
```

### Opción 3: Usar El Subagente QA Como Referencia

Si tu configuración de Claude Code soporta subagentes, copia `subagents/qa.md` a la ubicación de agentes del proyecto o de usuario que uses en tu instalación. Si no tienes subagentes configurados, puedes pegar el contenido de `subagents/qa.md` como instrucciones para una sesión dedicada de QA.

## Uso En Codex

Codex no usa necesariamente el mismo sistema de comandos, skills o subagentes que OpenCode. La forma más portable es reutilizar estos archivos como prompts de sistema o instrucciones de tarea.

### Crear PRDs O ADRs

Entrega el contenido de `commands/prd.md` o `commands/adr.md` como instrucciones iniciales y pide que siga el flujo:

```text
Sigue las instrucciones de commands/prd.md para crear un PRD para esta funcionalidad.
```

```text
Sigue las instrucciones de commands/adr.md para documentar esta decisión arquitectónica.
```

### Generar Casos De Prueba

Entrega `skills/prd-test-cases/SKILL.md` como instrucciones y referencia el PRD:

```text
Sigue las instrucciones de skills/prd-test-cases/SKILL.md y genera casos de prueba manuales para prds/<archivo>.md.
```

### Ejecutar QA

Entrega `subagents/qa.md` como rol/instrucciones para la sesión de QA:

```text
Actúa siguiendo subagents/qa.md. Ejecuta los casos de prueba de test-cases/<archivo>.md contra <URL o comando local> y reporta Aprobado o Rechazado.
```

Incluye siempre ambiente, comandos de ejecución, credenciales de prueba y datos necesarios. Si falta información, el agente debe marcar casos como bloqueados en lugar de inventar resultados.

## Flujo Recomendado

1. Crear PRD con `/prd`.
2. Generar casos de prueba manuales desde ese PRD con `prd-test-cases`.
3. Crear un plan de implementación en uno o varios archivos, según la estructura del proyecto. Por ejemplo, en un monorepo o workspace puedes separar planes para frontend, backend, microservicios, infraestructura o migraciones.
4. Entregar el PRD, el plan y los casos de prueba a [Ralphy](https://github.com/michaelshimeles/ralphy) para que ejecute la implementación completa.
5. Pedir a Ralphy que use los casos de prueba con el subagente `qa`, corrija defectos y repita el ciclo hasta completar la funcionalidad.

Prompt sugerido para Ralphy:

```text
Implementa la funcionalidad descrita en el PRD y sigue el plan de implementación adjunto.

Contexto:
- PRD: <ruta-del-prd>
- Plan o planes de implementación: <rutas-de-los-planes>
- Casos de prueba manuales: <ruta-de-casos-de-prueba>

Instrucciones:
1. Lee completamente el PRD, el plan y los casos de prueba antes de modificar código.
2. Implementa la funcionalidad siguiendo el plan.
3. Si el proyecto está dividido en frontend, backend, microservicios o paquetes, ejecuta cada parte del plan en el componente correspondiente.
4. Usa el subagente qa para ejecutar los casos de prueba manuales definidos en <ruta-de-casos-de-prueba>.
5. Si QA devuelve Rechazado, corrige los defectos reportados y vuelve a ejecutar los casos fallidos y las regresiones relacionadas.
6. Repite el ciclo implementación -> QA -> corrección hasta que QA devuelva Aprobado.
7. Al finalizar, entrega un resumen con archivos modificados, pruebas ejecutadas, resultado QA y cualquier riesgo pendiente.
```
