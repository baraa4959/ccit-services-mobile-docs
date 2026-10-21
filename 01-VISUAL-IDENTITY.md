# 01 — CCIT Mobile Visual Identity

| Item | Guidance |
|---|---|
| Display name | **خدمات كلية الحاسبات وتقنية المعلومات** |
| Institution | **جامعة بيشة** (University of Bisha) |
| Device/app-store display name | **CCIT Services** |
| iOS Bundle ID | `com.ccitstudents.services` |
| Android application ID / package name | `com.ccitstudents.services` |
| Institution/legal context | Formal Arabic name above and University of Bisha remain institutionally governed; app label is not a change to legal ownership. |
| Primary logo / app mark | [`assets/ccit-mark.svg`](assets/ccit-mark.svg); preserve its aspect ratio, geometry, and original colors; use a properly approved platform icon export. |
| Institutional art | Existing [`assets/university-logo.png`](assets/university-logo.png) is a report asset; do not assume permission to substitute or redraw the app mark. |

## Palette (light UI)

| Role | Hex |
|---|---|
| Primary action blue | `#1769AA` |
| Primary hover/pressed | `#115B98` |
| Deep navy | `#0B315F` |
| Background | `#F4F7FA` |
| Surface / card | `#FFFFFF` |
| Primary text | `#172434` |
| Muted text | `#667588` |
| Border | `#DDE5ED` |
| Restrained accent, not an action color | `#B98A3D` |
| Destructive | `#B44545` |

Use accessible semantic success/warning/error variants and test contrast. Dark mode should use a dedicated palette, not reuse light foreground values unchanged.

## UI language

- Arabic-first **RTL**; use Latin `0–9` digits for counters, dates and identifiers. Use logical padding/alignment to permit English/LTR later.
- Use readable Arabic typography such as the website's **IBM Plex Sans Arabic** family **only if appropriately licensed and separately sourced by the mobile team**. Do not extract or redistribute font binaries from the backend repository. DG Heaven assets in the website support certificate/report rendering; they do not establish a new mobile text style.
- Restrained academic/institutional style: flat solid surfaces, modest corner radii, consistent 8-point spacing, light borders and minimal shadows. No gradients, glassmorphism, invented KPI tiles or ornamental screens.
- Neutral full-width tabs, clear selected state, consistent disabled/loading/empty/error states. Use native platform accessibility and safe-area conventions.
- Face ID / device biometric prompt is a **native device security control**, not a place to invent a CCIT login form.

This standalone guide and the two supplied image assets can be used as the mobile visual reference. The project owner must confirm institutional approval of final store artwork before release.
