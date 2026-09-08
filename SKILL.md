---
name: desiluminador
description: Busca información en Jira y en repos y la cuenta como historias. Modos: desiluminar, iluminar.
---

# Desiluminador

Una pregunta entra con sus fuentes. Se buscan las luces, se cruzan, y se cuentan como voces. Si se pide, las voces se iluminan.

## Vocabulario

- **Luces**: las informaciones. Viven en Jira, en repos, en Confluence, en internet.
- **Desiluminar**: buscar las luces y cruzarlas.
- **Voces**: el reporte, en forma de historias. Claro y conciso.
- **Iluminar**: guardar las voces en la memoria. La memoria es Confluence.

## Cómo se usa

- `desiluminar <pregunta>` — la pregunta trae las fuentes en el texto: claves o links de Jira, rutas de repos, y una línea de qué es el sistema. Ejemplo: `tenemos ~/backend/api, ~/backend/core y esta app; mirá PROJ-123; la app es un punto de cobro en efectivo`.
- `iluminar` — guarda las últimas voces de la conversación en Confluence.

Las repreguntas siguen en la misma conversación, con todas las luces en mano. No se vuelve a desiluminar salvo que la repregunta lo necesite.

## Piezas

| Pieza | Archivo |
|---|---|
| El método | `SKILL.md` |
| El explorador de repos | `explorador.md` |
| Encargo al explorador | `plantillas/encargo.md` |
| Plantilla de las voces | `plantillas/voces.md` |

## Límites

1. **No se escribe nada** fuera de la página de Confluence pedida. Ningún repo se modifica, ni siquiera un `git stash`.
2. **Las luces de la gente las lee el desiluminador, completas.** No se delegan a un explorador ni se resumen antes de leerlas: la luz clave suele ser una frase de un comentario.
3. **Los exploradores no escriben ni publican.** Devuelven lo que encontraron.
4. **Las voces no llevan archivos, líneas, funciones ni hashes.** Eso se queda en la conversación.
5. **Cada afirmación de las voces tiene respaldo en una luz.** Lo que no se pudo verificar se dice como inferido.

## Desiluminar

### 1. Encuadre

De la pregunta sacar: qué se quiere saber, fuentes, repos con su rol (app, backend intermedio, core, portal), y el sistema en una línea. Si falta la línea del sistema, deducirla del README de los repos y decirlo como supuesto. Identificar **quién opera**: las voces se cuentan desde esa persona.

### 2. Luces de la gente

1. Leer el ticket dado con todos sus campos y comentarios. Si es un epic, buscar sus hijos con JQL: `parent = KEY OR "Epic Link" = KEY`. Siempre buscar además los enlazados: `issue in linkedIssues(KEY)`. Si hay tickets hermanos del mismo sistema en otro cliente o instancia, traerlos por término en el título: cuentan el mismo flujo desde otro ángulo.
2. Campos a pedir: summary, description, status, issuetype, priority, assignee, reporter, created, updated, resolution, comment, attachment, parent, issuelinks. Hasta 100 por búsqueda.
3. Si el resultado es grande, volcarlo a un archivo temporal de trabajo con este formato por ticket: clave, tipo, estado, prioridad, creado, actualizado, asignado, reporter, links, nombres de adjuntos, descripción completa, comentarios en orden cronológico con fecha y autor. **Leerlo completo**, en trozos si hace falta.
4. Anotar: cronología, personas y su rol, qué se probó y qué pasó, qué quedó abierto, y las **frases textuales** que van a tener que conciliarse con el código: códigos de error, etiquetas de estado, nombres de campos, valores raros.

Si la pregunta apunta a páginas de Confluence o a internet, mismo tratamiento: se leen completas.

### 3. Luces del código

1. Lanzar **un explorador por repo, en paralelo**, siguiendo `explorador.md`, con `plantillas/encargo.md` completado: sistema en una línea, pregunta, ruta y rol del repo, otros repos, tickets en una línea cada uno, los términos textuales, y las preguntas específicas que ese repo puede responder.
   Si el asistente no tiene subagentes, hace él mismo de explorador: sigue `explorador.md` para cada repo, uno por vez, y deja el reporte en la conversación antes de pasar al siguiente.
2. Mientras corren, hacer los primeros greps con los términos textuales en todos los repos. Son búsquedas de un hecho, no exploración: no duplicar el trabajo de los exploradores.
3. Al llegar cada reporte, leerlo entero. No predecir ni resumir un reporte que todavía no llegó.

### 4. Cruce

1. Por cada afirmación que vaya a conectar un ticket con el código, verificar con `grep` o `sed -n`. Se verifica lo que se va a decir, no todo lo que dijeron los exploradores.
2. Clasificar cada luz: **verificada** (leída en el código), **inferida** (deducida de datos o de comportamiento), **no verificable** (vive en una librería o sistema fuera de los repos; decir cuál).
3. Conciliar contradicciones de forma explícita: un ticket dice que se corrigió y el código dice otra cosa; una release note promete un cambio que el diff no contiene; QA ve un valor que el código explica.
4. Si hay un diff sin commitear, decir qué cambia y qué no, respecto de la pregunta.

### 5. Voces

Completar `plantillas/voces.md` con sus reglas de escritura. Entregarlas en la conversación y terminar con una sola línea ofreciendo iluminar.

## Iluminar

1. Encontrar la carpeta de la herramienta en Confluence: la página titulada **Desiluminador**. Si no existe, preguntar en qué espacio crearla y crearla con una frase: "Voces del desiluminador, una página por pregunta."
2. **Una página por búsqueda.** Título: `YYYY-MM-DD · <pregunta en pocas palabras>`, máximo 80 caracteres. Nunca se actualiza una página existente salvo que se pida con el link.
3. Body: las voces sin repetir el título. Empieza con la línea de fecha y fuentes, después las secciones de `plantillas/voces.md`.
4. Antes de publicar, revisar: sin rutas ni nombres de archivo, sin `archivo:línea`, sin nombres de función, sin hashes. Si aparece alguno, reescribir esa frase en lenguaje de negocio.
5. Crear la página como hija de Desiluminador y reportar el link.
