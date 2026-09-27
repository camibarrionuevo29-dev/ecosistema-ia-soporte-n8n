# Ecosistema IA de soporte e-commerce (n8n + Notion + Gemini + Slack + Gmail)

Entrega final del curso **Arquitecto de Flujos IA** — Camila Barrionuevo.

Flujo que atiende tickets de soporte de punta a punta:

1. **Gmail** (etiqueta *Soporte*) dispara el flujo → filtro anti-loop (noreply, fuera de oficina, vacíos).
2. **Notion**: busca al cliente (si no existe lo crea) → tabla *Clientes* relacionada con *Tickets*.
3. **Gemini 3.6 Flash** clasifica el mail (sentimiento / intención / prioridad, JSON, max tokens limitados).
4. **RAG**: lee la *Base de Conocimiento* verificada de Notion y Gemini redacta el borrador citando la fuente.
5. Se crea el ticket en Notion (`Procesado_IA`) y se pide **aprobación humana en Slack** (botones Aprobar / Rechazar).
6. Si se aprueba: `Aprobado_Humano` → respuesta por **Gmail en el mismo hilo** (Thread ID) → `Enviado`. Si se rechaza: `Rechazado` + aviso para revisión manual.
7. **Errores**: si falla la IA se registra en Notion (`Error`) + alerta en Slack; además hay un Error Trigger global.

## Archivos

| Archivo | Qué es |
|---|---|
| `Documentacion tecnica - Ecosistema IA.pdf` | Diagrama de arquitectura, estructuras de datos (Notion + JSON), matriz de costos, seguridad y resiliencia |
| `Logica del flujo - n8n.json` | Workflow de n8n (importar en n8n) |
| `capturas/` | Evidencias (canvas, ejecuciones, Slack, Notion, Gmail) |

## Evidencias

1. [Flujo en n8n](capturas/01%20Flujo%20n8n.png)
2. [Ejecución: entrada de tickets](capturas/02%20Ejecucion%20entrada%20de%20tickets.png)
3. [Ejecución: IA con Gemini + pedido de aprobación](capturas/03%20Ejecucion%20IA%20con%20Gemini.png)
4. [Aprobación en Slack](capturas/04%20Slack%20aprobacion.png)
5. [Ejecución: respuesta aprobada](capturas/05%20Ejecucion%20respuesta%20aprobada.png)
6. [Respuesta en Gmail en el mismo hilo](capturas/06%20Respuesta%20en%20Gmail%20mismo%20hilo.png)
7. [Ticket en Notion](capturas/07%20Ticket%20en%20Notion.png)
8. [Tabla de tickets en Notion](capturas/08%20Tabla%20de%20tickets%20Notion.png)

![Flujo en n8n](capturas/01%20Flujo%20n8n.png)

## Pruebas realizadas (n8n Cloud)

- Ejecución completa real: Gmail → Notion → Gemini → Slack → aprobación → respuesta en el mismo hilo (sin errores).
- Camino infeliz: payload inválido, ticket inválido y doble clic sobre un ticket ya enviado → descartados, sin doble envío.

## Links

- Dashboard de control (Notion, vista pública): _completar_
- Base de datos en modo lectura (Notion): _completar_

> Las credenciales/API keys no están en el repo (n8n las guarda aparte).
