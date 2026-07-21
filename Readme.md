# Agente Autónomo Básico - n8n

Checkpoint de pre-entrega: configuración de un agente autónomo funcional (Tools Agent) en n8n, correspondiente al primer módulo del trayecto de IA Agéntica.

**Autor:** José María Müller

## Rol del agente

**Asistente de Calificación de Leads** — interactúa con potenciales clientes por chat, identifica si son un lead calificado y, en caso afirmativo, notifica automáticamente al equipo comercial.

## Stack utilizado

- **Plataforma de automatización:** [n8n](https://n8n.io/)
- **Modelo de lenguaje:** Google Gemini (Gemini 3.5 Flash), vía nodo nativo `Google Gemini Chat Model`.
  > ⚠️ La consigna sugiere preferentemente OpenAI (GPT-4o) o Anthropic (Claude 3.5 Sonnet). Se optó por Google Gemini porque su API ofrece un nivel gratuito real y permanente, sin necesidad de cargar tarjeta de crédito, a diferencia de OpenAI y Anthropic que solo otorgan un crédito de prueba único no renovable. El nodo utilizado (`@n8n/n8n-nodes-langchain.lmChatGoogleGemini`) es igualmente un nodo nativo de modelo de lenguaje dentro del ecosistema de n8n, cumpliendo el mismo rol funcional que los nodos sugeridos.
- **Herramienta (Tool) conectada al agente:** Gmail (envío de notificaciones de leads calificados).
- **Observabilidad:** Slack (log automático de cada interacción, con input, output y pasos intermedios ejecutados).

## Componentes implementados

- **Trigger:** Chat Trigger, captura el mensaje inicial del usuario.
- **AI Agent (modo Tools Agent):** conectado a Google Gemini Chat Model, con `maxIterations: 8` como guardrail de iteraciones.
- **System Prompt modular:** estructurado en Rol → Ámbito → Objetivo → Reglas y Escalamiento.
- **Tool:** nodo de Gmail conectado lateralmente al agente (no de forma secuencial), con una descripción semántica extensa que define exactamente cuándo debe activarse (lead calificado: necesidad real + presupuesto + autoridad de decisión).
- **Observabilidad:** nodo de Slack conectado a la salida del agente, que reporta el input del usuario, el output generado y la cantidad de pasos intermedios ejecutados.

## Validación

Se realizaron pruebas manuales de ejecución (`Execute Workflow`) enviando mensajes de prueba al chat, verificando que:
1. El agente responde de forma coherente ante consultas genéricas, sin activar la herramienta.
2. Ante un mensaje que cumple los criterios de lead calificado, el agente activa de forma autónoma la herramienta de Gmail y envía la notificación correspondiente.
3. Cada interacción queda registrada correctamente en el canal de Slack de observabilidad, sin errores de sintaxis en las expresiones.

## Archivo entregable

[`Checkpoint1_JoseMaria_Muller.json`](./Checkpoint1_JoseMaria_Muller.json) — workflow exportado desde n8n.