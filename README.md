# Compras del Hogar

PWA colaborativa para organizar las compras de una familia, con
entrega a domicilio realizada por choferes de la red AutoElekIA.

**Estado:** MVP funcional. En proceso de conversion a plataforma
de marketplace con comercios locales.

## Que hace

- **Familias** arman su lista de compras en tiempo real. Todos los
  miembros ven que falta y que ya se compro.
- **Comercios** (proximamente) publican su catalogo. Las familias
  compran a traves de la plataforma.
- **Choferes** de la red AutoElekIA entregan el pedido en casa.
- **AutoElekIA** conecta los 3 actores y cobra una comision al
  comercio via Stripe Connect.

## Modelo de negocio

    FAMILIA paga $500
        |
        v
    STRIPE divide:
        |
        +-- $475 al COMERCIO
        |
        +-- $25 a AUTOLEKIA (comision)

AutoElekIA nunca toca el dinero de la compra. Solo cobra su comision
por servicio a traves de Stripe Connect (application_fee_amount).

## Stack tecnologico

- **PWA Familia:** HTML/CSS/JS vanilla (single-file)
- **Backend:** Firebase (Firestore + Auth anonima)
- **Pagos:** Stripe Connect (Express accounts)
- **Hosting:** GitHub Pages (por ahora)
- **Notificaciones:** Firebase Cloud Messaging (FCM)

## Estructura

    .
    index.html              PWA Familia (todo el codigo)
    manifest.json           Config PWA
    service-worker.js       Cache offline
    icon-192.png
    icon-512.png
    docs/                   Documentacion completa
    README.md               Este archivo

## Documentacion

Ver docs/00-indice.md para el catalogo completo.

- Modelo de negocio: docs/07-modelo-negocio.md
- Arquitectura tecnica: docs/08-arquitectura.md
- Roadmap: docs/09-roadmap.md
- Fiscal y legal: docs/10-fiscal-legal.md

## Instalacion (Firebase)

1. Crear proyecto en https://console.firebase.google.com
2. Habilitar Firestore (modo produccion)
3. Habilitar Authentication (proveedor Anonimo)
4. Configurar reglas Firestore (ver docs/08-arquitectura.md)
5. Pegar el firebaseConfig en index.html
6. Deploy a GitHub Pages o Cloudflare Pages

## Uso

1. Abrir la URL en el celular
2. Aceptar instalar la PWA (aparece banner)
3. Agregar productos a la lista
4. Los demas miembros del hogar ven los cambios en tiempo real

## Roadmap

Ver docs/09-roadmap.md.

Proximas fases:
- Fase 2: Multi-hogar con codigo de invitacion
- Fase 3: Portales de comercios
- Fase 4: PWA Chofer con asignacion de entregas
- Fase 5: Stripe Connect con pagos automaticos

## Autor

Equipo AutoElekIA (c) 2026
