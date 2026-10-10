# coderhouse-ai-automation
# Agente de Triaje de Solicitudes de Compra

Proyecto integrador del trayecto AI Automation (Coderhouse). Un agente en n8n que recibe pedidos de compra internos escritos en lenguaje libre, los estructura, decide si están completos y los registra para su gestión por el área de Abastecimiento de una operadora de petróleo y gas.

El proyecto crece módulo a módulo: cada carpeta contiene la versión correspondiente del workflow.

## M1 · Agente base

**Archivo:** `M1/checkpoint1_mariano_musacchio.json`

**Flujo:** Chat Trigger → AI Agent (Tools Agent) → Gmail (log de observabilidad)

| Componente | Configuración |
|---|---|
| Modelo | Anthropic Chat Model · Claude Sonnet 4.6 · temperatura 0.2 |
| Guardrail | Máximo 6 iteraciones |
| System Prompt | Modular: Rol → Ámbito → Objetivo → Reglas y Escalamiento |
| Tool | Google Sheets (Append Row), conectada lateralmente, con descripción de casos de uso y de exclusión |
| Observabilidad | Intermediate steps enviados por Gmail en cada ejecución |

**Comportamiento validado:**
- Pedido completo → registra la solicitud en la planilla.
- Pedido urgente sin justificación → no registra y pide el dato faltante.
- Consulta fuera de ámbito (proveedores, precios) → no registra y deriva.

**Para importarlo:** n8n → Import from File, y asignar credenciales propias de Anthropic, Google Sheets y Gmail. La planilla destino necesita estas columnas: `Fecha_Registro, Session_ID, Area_Solicitante, Tipo, Descripcion, Cantidad, Unidad, Destino, Prioridad, Justificacion, Estado, Observaciones_Agente`.
