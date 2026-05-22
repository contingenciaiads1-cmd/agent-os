# Misión del Producto — IADS

## Qué construimos

Una **fábrica de negocios verticales operados con agentes IA**, vendida como servicio **DFY (done-for-you) + reventa** en LATAM.

**No es un CRM.** No es una plataforma SaaS de autoservicio.

Se vende **resultado gestionado high-ticket**: el cliente no configura nada — nosotros configuramos, operamos y el cliente ve resultados.

## Para quién

- **Cliente final:** negocio eCommerce en LATAM (Shopify, Tiendanube, WooCommerce) que quiere automatizar atención, recupero de carritos, y seguimiento de clientes vía WhatsApp.
- **Revendedor:** agencia o consultor que vende el servicio bajo su marca (white-label).

## Cómo lo resolvemos

Modelo mental: **Workspace / Brain / Flow**

- **Workspace** = panel limpio que ve el cliente (crmPrueba, con su marca). Ve dashboard, pipeline, bandeja de mensajes, y kill switch.
- **Brain** = Supabase: grafo de cliente + base de conocimiento + RLS multi-tenant. Cada cliente tiene su propio espacio aislado.
- **Flow** = motor que ejecuta secuencias en el tiempo (recupero carrito, reintentos, paso-a-humano, alertas). Es el SPINE — la columna vertebral que aún no existe y es el grueso del trabajo.

## Lo que nos diferencia

- **DFY**: el cliente NO configura. Nosotros onboarding, setup, y operación.
- **Multi-canal**: WhatsApp Meta Cloud API (canal principal, gratis responder) + QR Baileys (respaldo).
- **MCP vivo**: 37 tools del CRM expuestos como API — son las "manos" del agente IA.
- **Aislamiento real**: RLS en Supabase, no solo filtros de aplicación.

## Lo que NO construimos (aún)

- Panel de autoservicio para que el cliente configure su propio agente.
- Integración con IG/Telegram (P6, futuro).
- Conexión directa a Notion/Drive real del cliente (v2).
