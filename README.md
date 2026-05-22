<img width="1200" height="675" alt="Agent OS" src="https://github.com/user-attachments/assets/97ad4491-d199-4b9b-9482-ae710291dfb4" />

## Agents that build the way you would

[Agent OS](https://buildermethods.com/agent-os) — fork adaptado para el ecosistema **IADS** (contingenciaiads1-cmd).

Funciona junto a Claude Code, Cursor y otras herramientas de IA. Cualquier lenguaje, cualquier framework.

---

## Qué es Agent OS (en una línea)

Un **sistema de memoria institucional para tu IA**. En vez de que Claude empiece desde cero en cada sesión y tenga que re-descubrir cómo está hecho tu código, Agent OS le inyecta el contexto exacto que necesita antes de que escriba una sola línea.

---

## La analogía que importa

Imaginá que contratás a un programador nuevo. Tiene dos opciones:

- **Sin Agent OS**: llega el primer día, no sabe nada, tiene que leer todo el código, pregunta cosas básicas, y aun así comete errores de estilo o rompe convenciones.
- **Con Agent OS**: llega con un manual ya armado que le dice *exactamente* cómo trabaja tu equipo — qué patrones se usan, por qué se hacen así, cuáles son las reglas no escritas.

Agent OS es ese manual, pero generado automáticamente desde tu propio código.

---

## Capacidades principales

| Comando | Qué hace |
|---|---|
| `/discover-standards` | Lee tu código y extrae los patrones reales que ya existen. Pregunta "¿por qué hacen esto así?" y arma un estándar escrito. |
| `/index-standards` | Mantiene un `index.yml` con todos los estándares catalogados para búsqueda rápida. |
| `/inject-standards` | Antes de codear, detecta qué tarea vas a hacer e inyecta automáticamente los estándares relevantes al contexto. |
| `/shape-spec` | Antes de escribir código, arma una especificación estructurada con plan, referencias de código similar, y estándares aplicables. |
| `/plan-product` | Genera 3 docs base: misión del producto, roadmap, y tech stack — que se usan como contexto en toda sesión. |

---

## Flujo real de una sesión

```
1. /plan-product       → Claude conoce el producto
2. /inject-standards   → Claude conoce las reglas de tu codebase
3. /shape-spec         → Claude arma el plan antes de codear
4. Codea con contexto completo → menos errores, menos re-trabajo
```

---

## Por qué vive DENTRO del repo (recomendación)

Agent OS se instala como una carpeta `/agent-os` dentro de cada repositorio donde trabaja la IA. La razón es simple: **Claude lee archivos**.

Claude Code funciona leyendo los archivos de tu proyecto para entender el contexto. No tiene memoria mágica entre sesiones — literalmente abre archivos y los lee. Entonces Agent OS no es un programa que "corre" en ningún lado: es una carpeta con archivos de texto que Claude lee antes de codear.

```
tu-repo/
└── agent-os/          ← Claude lee esto antes de trabajar
    ├── product/       ← "qué es este producto"
    ├── standards/     ← "cuáles son las reglas"
    └── specs/         ← "qué se está construyendo ahora"
```

Si esa carpeta estuviera en otro lado (por ejemplo en el escritorio), Claude no la vería.

**El 90% de los equipos elige este enfoque:** la carpeta `agent-os/` vive dentro del repo y se commitea junto con el código. Así cualquier IA que trabaje en ese repo hereda el contexto automáticamente — sin configuración extra, sin pasos adicionales.

Las dos alternativas si no querés "ensuciar" el repo:
1. **`.gitignore` la carpeta** — queda en tu máquina local pero nunca sube a GitHub.
2. **Repo separado para estándares** — más complejo de mantener, no recomendado para equipos pequeños.

---

## Cómo se usa en el ecosistema IADS

El ecosistema IADS tiene **dos repos** con necesidades distintas:

### `crmIads` — CRM operacional (PHP/CI4 + Node/Baileys)

**El problema:** cada sesión de IA tiene que re-descubrir trampas conocidas del repo:
- El timeout de axios en `waziper.js` debe ser 60s (Gemini tarda 13-17s)
- Las rutas NO pueden usar closures (rompen el Auth filter de CI4)
- El prefijo de todas las tablas es `sp_`
- El webhook de Meta usa el token global de `sp_options`, no el por-conexión
- `git stash pop` puede dejar conflict markers sin avisar — siempre `php -l` después
- Y ~10 trampas más documentadas en CLAUDE.md

**Solución:** correr `/discover-standards` una vez, capturar esas trampas como estándares formales en `crmIads/agent-os/standards/`, y nunca más perder tiempo en el mismo error.

**Stack del repo:**
- Backend: PHP 8.3 + CodeIgniter 4
- WhatsApp Engine: Node 20 + Baileys 6.7 (wa_server)
- DB: MySQL (`sp_*` prefix en todas las tablas)
- Cache: Redis + Bull queues
- VPS: `161.132.41.141` / `crm.iads.solutions` / `api.iads.solutions`

### `crmPrueba` — Producto nuevo (React/Supabase)

**El problema:** el spine de agentes IA (F0–F7) está por construirse. Cada sesión de IA puede armar el código de una manera distinta — componentes con naming diferente, patrones de Supabase inconsistentes, lógica de RLS replicada de formas distintas.

**Solución:** antes de cada feature del spine, `/shape-spec` arma un plan que referencia código existente similar + estándares del repo. El resultado es código consistente entre sesiones, incluso si son semanas de diferencia.

**Stack del repo:**
- Frontend: React 19 + TypeScript + Vite
- Backend/DB: Supabase (Postgres + Auth + RLS multi-tenant)
- IA: Google Gemini (gemini-2.5-flash-lite)
- Styling: Tailwind CSS

---

## Estructura de carpetas que crea en cada repo

```
repo/
└── agent-os/
    ├── product/
    │   ├── mission.md          ← qué construís y para quién
    │   ├── roadmap.md          ← fases del producto (F0-F7 para IADS)
    │   └── tech-stack.md       ← stack tecnológico del repo
    ├── standards/
    │   ├── index.yml           ← catálogo de todos los estándares
    │   ├── php/                ← estándares CI4, filtros, rutas
    │   ├── node/               ← estándares wa_server, Baileys, timeouts
    │   ├── database/           ← convenciones sp_* tables, migraciones
    │   └── security/           ← CSRF, auth, secrets
    └── specs/
        └── 2026-05-22-feature-spine/
            ├── plan.md
            ├── shape.md
            └── standards.md
```

---

## Norte del producto IADS (contexto para la IA)

**No es un CRM.** Es una **fábrica de negocios verticales operados con agentes IA**, vendida **DFY (done-for-you) + reventa** en LATAM. Se vende **resultado gestionado high-ticket**, no licencia de software.

Modelo mental: **Workspace / Brain / Flow**
- **Workspace** = panel limpio que ve el cliente (crmPrueba, su marca)
- **Brain** = Supabase: grafo de cliente + conocimiento + RLS multi-tenant
- **Flow** = motor que ejecuta secuencias en el tiempo (recupero carrito, paso-a-humano) — el SPINE

Fases del roadmap:
- **P0** — Fix Gemini server-side (clave no expuesta en browser)
- **P1** — Knowledge base por cliente
- **P2** — SPINE/Flow (agente que ejecuta secuencias)
- **P2g** — Gobernanza multi-tenant
- **P3** — Pack e-commerce (Shopify/Tiendanube/WooCommerce)
- **VENTA** — Primeros 2-5 pilotos DFY

---

## Instalación

Ver documentación oficial → [buildermethods.com/agent-os](https://buildermethods.com/agent-os)

Este fork incluye el perfil `iads` pre-configurado con el stack y contexto del ecosistema IADS. Ver `profiles/iads/`.

---

## Créditos

Herramienta original creada por [Brian Casel @ Builder Methods](https://buildermethods.com).
Fork mantenido por el equipo IADS — [contingenciaiads1-cmd](https://github.com/contingenciaiads1-cmd).
