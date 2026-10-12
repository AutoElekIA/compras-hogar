# Indice de Documentacion — Compras del Hogar

Plataforma de compras colaborativas del hogar con entrega a domicilio
realizada por choferes de la red AutoElekIA.

## Documentos

### Negocio
- [07-modelo-negocio.md](07-modelo-negocio.md) — Los 4 actores, flujos, Stripe Connect
- [10-fiscal-legal.md](10-fiscal-legal.md) — Regimen fiscal, obligaciones, contratos

### Tecnico
- [08-arquitectura.md](08-arquitectura.md) — Stack, modelo de datos, integraciones
- [09-roadmap.md](09-roadmap.md) — Plan por fases (7 fases)

## Vision rapida

- **Familias:** usan gratis, arman lista, piden, reciben en casa
- **Comercios:** publican catalogo, reciben pedidos, pagan comision (5%)
- **Choferes:** entregan en tiempo muerto, ganan por entrega
- **AutoElekIA:** cobra comision via Stripe Connect, no toca el dinero

## Estado actual

- PWA Familia: funcionando (single-file PWA)
- Backend: Firebase (Firestore + Auth anonima)
- Multi-hogar: pendiente
- Comercios, choferes, Stripe: pendientes

Ver [09-roadmap.md](09-roadmap.md) para el plan completo.

## URLs

- PWA Familia: https://autoelekia.github.io/compras-hogar
- Repo: https://github.com/AutoElekIA/compras-hogar
