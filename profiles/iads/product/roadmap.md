# Roadmap — IADS

## Objetivo inmediato: Vertical Mínima Vendible (VMV) eCommerce

Primeros 2-5 pilotos DFY con negocios eCommerce en LATAM.
**Regla:** si algo no ayuda a la primera venta, es v2.

---

## Fases

### P0 — Fix Gemini server-side (BLOQUEANTE)
- Clave Gemini expuesta en browser (agent-os-IADS) — mover a server-side
- Bloqueado por billing Google Cloud (lo activa el jefe)
- **Sin esto:** el agente IA no puede operar de forma segura ni escalable

### P1 — Conocimiento por cliente
- Base de conocimiento privada por agente (`sp_ai_knowledge_base` por `instance_id`)
- Sync inicial: seed manual de Q&A + eventual Notion/Drive
- **Resultado:** el agente responde con contexto real del negocio del cliente

### P2 — SPINE / Flow (el grueso del trabajo)
- Motor que ejecuta secuencias en el tiempo
- Casos de uso MVP: recupero de carrito, reintentos de contacto, paso-a-humano
- Tablas: `sp_flows`, `sp_flow_steps`, `sp_flow_executions`
- **Resultado:** el agente hace cosas en el tiempo, no solo responde mensajes

### P2g — Gobernanza multi-tenant
- Test adversarial: verificar que un tenant no puede ver datos de otro
- Auditoría de RLS en Supabase
- **Resultado:** sistema listo para clientes reales sin riesgo de fuga

### P3 — Pack eCommerce
- Conector nativo con Shopify / Tiendanube / WooCommerce
- Templates de flujos pre-armados para eCommerce
- **Resultado:** onboarding de 1 día, no 1 semana

### VENTA — Primeros pilotos
- 2-5 clientes DFY activos
- Proceso documentado y repetible
- **Resultado:** validación comercial en LATAM

---

## Decisiones cerradas (no reabrir)

| Tema | Decisión |
|---|---|
| Canal WhatsApp principal | Meta Cloud API directo (responder = GRATIS) |
| Canal barato | QR Baileys via bridge (riesgo ban asumido) |
| Conocimiento por cliente | Espacio propio por tenant (como Notion interno) |
| Conectar Notion/Drive real del cliente | v2, NO entra en VMV |
| CRM visible del cliente | agent-os-IADS Workspace scoped (dashboard + pipeline + bandeja + kill switch) |
| IG/Telegram | P6, futuro |

---

## Tiempos estimados

En **sesiones de IA** (no días-programador):
- P0: 1 sesión (desbloqueado por billing del jefe)
- P1: 2-3 sesiones
- P2 SPINE: 4-6 sesiones (el más complejo)
- P2g: 1-2 sesiones
- P3: 3-4 sesiones
