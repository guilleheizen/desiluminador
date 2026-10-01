---
name: desiluminador
description: Responde preguntas sobre un sistema cruzando lo que dice Jira con lo que hace el código de varios repos, y lo cuenta como un relato que cualquiera puede seguir. Se activa con "desiluminar" o "desiluminá", "investigá PROJ-123", "explicame este epic", y con "guardalo en Confluence" para publicar el relato. También cuando piden entender o explicar un epic, un ticket, un flujo o un incidente que cruza varios repos.
---

# Desiluminador

Responde una pregunta sobre un sistema. Lee los tickets de Jira, lee el código de los repos, verifica dónde coinciden y dónde no, y lo cuenta como un relato. Si se pide, guarda el relato en Confluence.

## Palabras que usa

- **Desiluminar**: buscar la información en tickets y repos, y cruzarla.
- **Relato**: el resultado. Explica la operación de punta a punta y cada ticket como una historia, sin detalles de código.
- **Guardar en Confluence**: publicar el relato como página nueva.
- **Revelio**: el agente que lee un repo, sin modificar nada, y devuelve lo que encontró.

## Cómo hablarle al usuario

- Lenguaje simple y directo. La menor cantidad de palabras posible sin perder la idea.
- Todo bloque de código va con una descripción de qué hace.
- Ante una duda que no se resuelve leyendo, preguntar. No asumir.
- Un problema se explica así:
  - **Problema:** qué ve el usuario. Ej: el pago queda pendiente aunque se cobró.
  - **Porque:** la causa en el código. En la conversación sí van nombres de archivos y funciones.
  - **Solución:** simple y adaptada al código existente, explicando cómo funciona. Sin escribir código.

## Qué necesita

Para **desiluminar**:

- **Una pregunta.** Qué se quiere entender.
- **Al menos un ticket o página.** Una clave o link de Jira, o una página de Confluence.
- **Los repos.** Las rutas, y el rol de cada uno si no es obvio por el nombre. Si la conversación transcurre dentro de un repo, ese cuenta como uno.
- **El sistema en una línea.** Qué es y quién lo usa. Si falta, proponer una leyendo el README de los repos y pedir que se confirme.

Todo lo que falte se pide en un solo mensaje, antes de empezar. Ejemplo de pedido completo: `tenemos ~/backend/api, ~/backend/core y esta app; mirá PROJ-123; la app es un punto de cobro en efectivo`.

Para **guardar en Confluence** no hace falta nada: se usa el último relato de la conversación. Si todavía no hay relato, decirlo.

Las preguntas siguientes se responden en la misma conversación, con la información que ya se juntó. No se vuelve a desiluminar salvo que la pregunta nueva lo necesite.

## Piezas

| Pieza | Archivo |
|---|---|
| El método | `SKILL.md` |
| El agente que lee cada repo | `revelio.md` |
| El pedido a Revelio | `plantillas/encargo.md` |
| La plantilla del relato | `plantillas/relato.md` |

## Límites

1. **No se escribe nada** fuera de la página de Confluence pedida. Ningún repo se modifica, ni siquiera con un `git stash`.
2. **Los tickets y páginas los lee el desiluminador, completos.** No se delegan a Revelio ni se resumen antes de leerlos: el dato clave suele ser una frase de un comentario.
3. **Revelio no escribe ni publica.** Devuelve lo que encontró.
4. **El relato no lleva archivos, líneas, funciones ni hashes.** Eso queda en la conversación.
5. **Cada afirmación del relato tiene respaldo** en un ticket, una página o el código. Lo que no se pudo verificar se dice como inferido.

## Desiluminar

### 1. Encuadre

De la pregunta sacar: qué se quiere saber, tickets o páginas, repos con su rol (app, backend intermedio, core, portal) y el sistema en una línea. Identificar **quién usa el sistema**: el relato se cuenta desde esa persona.

### 2. Tickets y páginas

1. Leer el ticket dado con todos sus campos y comentarios. Si es un epic, buscar sus hijos con JQL: `parent = KEY OR "Epic Link" = KEY`. Siempre buscar además los enlazados: `issue in linkedIssues(KEY)`. Si hay tickets del mismo flujo en otro cliente o instancia, traerlos buscando por palabras del título: cuentan lo mismo desde otro ángulo.
2. Campos a pedir: summary, description, status, issuetype, priority, assignee, reporter, created, updated, resolution, comment, attachment, parent, issuelinks. Hasta 100 por búsqueda.
3. Si el resultado es grande, volcarlo a un archivo temporal de trabajo, fuera de los repos, con este formato por ticket: clave, tipo, estado, prioridad, creado, actualizado, asignado, reporter, links, nombres de adjuntos, descripción completa, y comentarios en orden cronológico con fecha y autor. **Leerlo completo**, en partes si hace falta.
4. Anotar: cronología, personas y su rol, qué se probó y qué pasó, qué quedó abierto, y los **textos exactos** que hay que buscar en el código: códigos de error, nombres de estados, nombres de campos, valores raros.

Las páginas de Confluence o de internet que nombre la pregunta se leen igual: completas.

### 3. Código

1. Lanzar **un Revelio por repo, en paralelo**, siguiendo `revelio.md`, con `plantillas/encargo.md` completado: el sistema en una línea, la pregunta, ruta y rol del repo, los otros repos, los tickets en una línea cada uno, los textos exactos y las preguntas específicas que ese repo puede responder.
   Si el asistente no puede lanzar subagentes, hace él mismo de Revelio: sigue `revelio.md` para cada repo, uno por vez, y deja el reporte en la conversación antes de pasar al siguiente.
2. Mientras corren, buscar con `grep` los textos exactos en todos los repos. Solo para ubicar un dato puntual: no repetir el trabajo de Revelio.
3. Leer cada reporte entero cuando llega. No adelantar ni resumir un reporte que todavía no llegó.

### 4. Verificación

1. Verificar con `grep` o `sed -n` cada afirmación que vaya a unir un ticket con el código. Se verifica lo que se va a decir, no todo lo que reportó Revelio.
2. Marcar cada dato como:
   - **Verificado**: se leyó en el código.
   - **Inferido**: se deduce de datos o de comportamiento.
   - **No verificable**: vive en una librería o en un sistema fuera de los repos. Decir cuál.
3. Explicar cada contradicción. Ejemplos: un ticket dice que algo se corrigió y el código no lo tiene; una release note promete un cambio que no está; QA ve un valor que el código explica.
4. Si hay cambios sin commitear, decir qué cambian y qué no, respecto de la pregunta.

### 5. Relato

Completar `plantillas/relato.md` siguiendo sus reglas de escritura. Entregarlo en la conversación y terminar con una sola línea que ofrezca guardarlo en Confluence.

## Guardar en Confluence

1. Buscar en Confluence la página titulada **Desiluminador**: es la carpeta de todos los relatos. Si no existe, preguntar en qué espacio crearla y crearla con una frase: "Relatos del desiluminador, una página por pregunta."
2. **Una página por pregunta.** Título: `YYYY-MM-DD · <pregunta en pocas palabras>`, máximo 80 caracteres. Nunca se actualiza una página existente, salvo que se pida con el link.
3. Contenido: el relato sin repetir el título. Empieza con la línea de fecha y fuentes; después, las secciones de `plantillas/relato.md`.
4. Antes de publicar, revisar que no haya rutas, nombres de archivo, `archivo:línea`, nombres de función ni hashes. Si aparece alguno, reescribir esa frase en lenguaje de negocio.
5. Crear la página como hija de **Desiluminador** y pasar el link.
