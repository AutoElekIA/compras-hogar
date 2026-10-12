# Roadmap

Plan por fases para llevar Compras del Hogar de MVP a plataforma completa.

## Estado actual (2026-10-12)

- PWA Familia: single-file funcionando con Firebase
- Auth anonima sin aislamiento (todas las familias ven la misma lista)
- Sin comercios ni choferes integrados
- Sin pagos
- Costo: $0/mes

## Fase 1 - Documentacion (esta semana)

- [x] Modelo de negocio documentado
- [x] Arquitectura tecnica documentada
- [x] Roadmap (este documento)
- [ ] Documento fiscal-legal
- [ ] README mejorado
- [ ] Indice de docs

## Fase 2 - Multi-hogar (1-2 semanas)

**Objetivo:** que cada familia tenga su propio espacio aislado.

- [ ] Coleccion `hogares` en Firestore
- [ ] Codigo de invitacion de 6 caracteres (unico)
- [ ] Migrar `items` a tener `hogarId`
- [ ] Migrar `catalog` a tener `hogarId`
- [ ] Pantalla de "Crear hogar" en la PWA
- [ ] Pantalla de "Unirse con codigo"
- [ ] Reglas Firestore por hogar
- [ ] Selector de hogar si el usuario tiene varios
- [ ] Migracion de datos existentes (los 53 productos actuales)

**Entregable:** cada familia tiene su lista privada. Sin esto, no se
puede escalar a mas de 1 familia.

## Fase 3 - Comercios y ofertas (2-4 semanas)

**Objetivo:** los comercios pueden publicar catalogo y recibir pedidos.

- [ ] Coleccion `comercios`
- [ ] Coleccion `ofertas`
- [ ] PWA Comercio (Vue 3)
  - Login con email (Firebase Auth real, no anonima)
  - Alta de comercio (nombre, direccion, categorias)
  - Subir catalogo
  - Ver pedidos en tiempo real
  - Generar ticket
- [ ] Matching de ofertas en PWA Familia
  - Al agregar producto, sugerir comercio con mejor precio
  - Al armar pedido, agrupar por comercio
- [ ] Onboarding Stripe Connect Express para comercios

**Entregable:** familias ven ofertas, comercios reciben pedidos.

## Fase 4 - Choferes y entregas (2-4 semanas)

**Objetivo:** las entregas las hacen choferes de la red escolar.

- [ ] Coleccion `choferes`
- [ ] PWA Chofer (Vue 3)
  - Login con OTP (telefono + PIN, mismo patron que transporte escolar)
  - Ver pedidos asignados
  - Escanear ticket del comercio al recoger
  - Marcar entregado con firma digital
  - Historial de entregas + pagos
- [ ] Cloud Function `asignarChofer` (por zona + disponibilidad)
- [ ] Notificaciones PUSH (FCM)
- [ ] Firma digital (canvas en app)
- [ ] Generar PDF del ticket firmado

**Entregable:** pedidos llegan a la familia.

## Fase 5 - Pagos con Stripe Connect (1-2 semanas)

**Objetivo:** cobrar al comercio sin manejar el dinero.

- [ ] Configurar Stripe Connect Platform
- [ ] Onboarding de comercios (Express)
- [ ] Onboarding de choferes (Express, para pagos por entrega)
- [ ] Cloud Function `crearPaymentIntent` con `application_fee_amount`
- [ ] Webhook `webhookStripe` para confirmar pagos
- [ ] Flujo completo: familia paga -> split automatico
- [ ] Panel de comercio: ver pagos recibidos
- [ ] Facturacion de comisiones AutoElekIA -> comercio

**Entregable:** ingresos recurrentes para AutoElekIA.

## Fase 6 - Admin y monitoreo (1-2 semanas)

**Objetivo:** AutoElekIA puede gestionar todo desde un panel.

- [ ] Panel Admin (Vue 3)
  - Ver hogares activos
  - Ver comercios y sus ventas
  - Ver choferes y sus entregas
  - Reportes de GMV y comisiones
- [ ] Firebase App Check activado
- [ ] Sentry para errores
- [ ] UptimeRobot
- [ ] Backups automaticos de Firestore

## Fase 7 - Escala (2-6 meses)

**Objetivo:** 500+ familias y 30+ comercios.

- [ ] CDN para imagenes de productos
- [ ] Busqueda avanzada (Algolia)
- [ ] Multi-ciudad (Puebla, CDMX, Guadalajara)
- [ ] API publica para integraciones
- [ ] Marketplace de ofertas
- [ ] App nativa iOS/Android (si PWA no alcanza)

## Riesgos identificados

| Riesgo | Probabilidad | Mitigacion |
|--------|--------------|------------|
| Falta de masa critica de familias | Alta | Enfocarse en 1 ciudad primero |
| Comercios no quieren pagar comision | Media | Empezar con fee bajo (3%) |
| Stripe Connect rechaza cuentas | Baja | Documentacion clara, soporte |
| Choferes no se activan | Media | Incentivo: pago fijo por entrega |
| Firebase se vuelve caro | Media | Migrar a Cloud Run o backend propio |
| Confusion fiscal | Baja | Documentar con contador |

## Proximos 3 pasos concretos

1. Completar documentacion (docs 09 y 10)
2. Crear coleccion `hogares` + reglas Firestore (Fase 2)
3. Modificar PWA Familia para multi-hogar

## Metricas de exito por fase

| Fase | Metrica | Meta |
|------|---------|------|
| Fase 2 | Familias activas | 5 |
| Fase 3 | Comercios registrados | 3 |
| Fase 4 | Entregas completadas | 50 |
| Fase 5 | GMV mensual | $50,000 MXN |
| Fase 6 | Ingreso AutoElekIA | $2,500 MXN/mes |
| Fase 7 | Familias | 500+, Comercios 30+ |
