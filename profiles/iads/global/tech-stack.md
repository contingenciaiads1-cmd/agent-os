# Tech Stack — Ecosistema IADS

## Repo: crmIads — CRM operacional (músculo WhatsApp + MCP)

**Backend:**
- PHP 8.3 + CodeIgniter 4 (Stackposts base, rebrand IADS)
- Node 20 + Baileys 6.7 (motor WhatsApp Web por QR, `wa_server/waziper.js`)

**Base de datos:**
- MySQL — usuario `sp_user`, DB `sp_db`
- Todas las tablas tienen prefijo `sp_` (ej: `sp_meta_messages`, `sp_livechat_*`)
- Redis + Bull queues para jobs async

**Canal WhatsApp:**
- Meta Cloud API directo (responder = GRATIS, canal principal)
- QR Baileys via bridge (canal de respaldo, riesgo ban asumido)

**Infraestructura:**
- VPS: `161.132.41.141`
- Dominios: `crm.iads.solutions` (panel PHP) / `api.iads.solutions` (Node + MCP)
- OS: Ubuntu + Nginx + PHP-FPM 8.3 + pm2 (Node)
- Backups: Backblaze B2

**MCP:**
- 37 tools vivos en `api.iads.solutions/mcp` con bearer tokens
- HTTP transport, autenticación por token por equipo

---

## Repo: agent-os-IADS — producto nuevo (spine de agentes IA)

**Frontend:**
- React 19 + TypeScript
- Vite (build tooling)
- Tailwind CSS

**Backend / DB:**
- Supabase (Postgres + Auth + Storage)
- RLS multi-tenant activo — aislamiento a nivel DB por `team_id`

**IA:**
- Google Gemini (`gemini-2.5-flash-lite` — modelo correcto con free tier)
- Clave Gemini debe vivir SERVER-SIDE (no en browser — deuda P0)

**Integraciones planeadas:**
- WhatsApp: Meta Cloud API directo
- E-commerce: Shopify / Tiendanube / WooCommerce (a definir según pilotos)
- Notion: sync KB (bloqueado hasta acceso del jefe)
