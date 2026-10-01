# Encargo: explorar {{repo_nombre}}

## El sistema
{{sistema_en_una_linea}}

## La pregunta que hay que poder responder
{{pregunta}}

## El repo
- Ruta: `{{repo_ruta}}`
- Rol en el sistema: {{rol}} (app móvil, backend intermedio, core, portal, librería)
- Rama actual y si hay cambios sin commitear: revisalo y reportalo.
- Otros repos que participan: {{otros_repos}}. No los explores. Sí reportá cada punto donde este repo habla con ellos: endpoint, método, campos que manda y qué hace con la respuesta.

## Lo que dijo la gente (orienta la búsqueda, no la prueba)
{{tickets_una_linea_cada_uno}}

Textos exactos de los tickets que hay que buscar en el código:
{{textos_exactos}}

## Qué necesito que reportes

1. **Mapa**: módulos, rutas, servicios y tablas que tocan la pregunta. Una línea por pieza, con su ruta.
2. **Cada flujo relevante, paso a paso**, con `archivo:línea` en cada paso: pantalla o ruta, función, endpoint (método, path, payload con nombres de campos), estado que se escribe y con qué valor.
3. **Estados y transiciones**: qué estados existen, dónde se definen, quién los escribe, qué guarda cada transición.
4. **Integraciones**: hacia qué sistema, con qué campos, y qué pasa con la respuesta o el callback.
5. **Historia reciente**: `git log` de los últimos meses sobre las rutas relevantes, y los cambios sin commitear si los hay. Qué cambió y por qué, según el mensaje del commit.
6. **Hallazgos**: lo que parece un bug, lo que contradice un ticket, lo que se dice corregido y no está. Cada uno con evidencia, marcado **leído** o **deducido**.
7. **Lo que no pudiste responder** y por qué: código en una librería externa, configuración en base de datos, un servicio que no está en el repo.

Preguntas específicas para este repo:
{{preguntas_especificas}}

Reglas: solo lectura, nada inventado, `archivo:línea` en cada afirmación, separar lo leído de lo deducido, y decir qué parte del repo no llegaste a leer.
