# Roadmap

Plan por fases para llevar Compras del Hogar de MVP a plataforma completa.

## Estado actual (2026-10-12)

- PWA Familia: single-file funcionando con Firebase
- Auth anonima sin aislamiento (todas las familias ven la misma lista)
- Sin comercios ni choferes integrados
- Sin pagos
- Documentacion completa en docs/
- Costo: $0/mes

## Fase 0 - Validacion manual (1-2 semanas)

**Objetivo:** validar el flujo end-to-end antes de construir todo.

- [ ] Perfecto el flujo actual con tu propia familia (2-3 celulares).
- [ ] Acuerdo con 1 comercio amigo (abarrotes o restaurante).
- [ ] 1 chofer de confianza (de tu red escolar).
- [ ] Pedido manual: familia -> WhatsApp comercio -> chofer -> entrega.
- [ ] Cobro con Stripe Payment Link (sin Connect todavia).
- [ ] Documentar aprendizajes en docs/piloto-fase-0.md.

**Entregable:** 10 pedidos reales hechos a mano. Se aprende que
funciona y que no. Se ajusta antes de construir software.

**Costo:** $0 (todo manual).

## Fase 1 - Documentacion (completada)

- [x] Modelo de negocio (docs/07)
- [x] Arquitectura tecnica (docs/08)
- [x] Roadmap (este documento)
- [x] Fiscal y legal (docs/10)
- [x] Indice (docs/00)
- [x] README actualizado

## Fase 2 - Multi-hogar (1-2 semanas)

**Objetivo:** cada familia tiene su espacio aislado.

- [ ] Coleccion `hogares` en Firestore.
- [ ] Codigo de invitacion de 6 caracteres (unico).
- [ ] Migrar `items` a tener `hogarId`.
- [ ] Migrar `catalog` a tener `hogarId`.
- [ ] Pantalla "Crear hogar" en la PWA.
- [ ] Pantalla "Unirse con codigo".
- [ ] Reglas Firestore por hogar.
- [ ] Selector de hogar si el usuario tiene varios.
- [ ] Migracion de los 53 productos actuales.

**Entregable:** cada familia tiene su lista privada.

**Metrica de exito:** 3 familias activas (la tuya + 2 invitadas).

## Fase 3 - Piloto con restaurantes (2-3 semanas)

**Objetivo:** validar el vertical mas rentable.

- [ ] Onboarding manual de 1-3 restaurantes amigos.
- [ ] Onboarding manual de 2-3 choferes.
- [ ] Stripe Connect Express para restaurantes (test mode).
- [ ] Stripe Connect Express para choferes (test mode).
- [ ] Pago split automatico (application_fee).
- [ ] Pedidos con alimentos preparados (protocolo sanitario).
- [ ] Prueba con hieleras + geles refrigerantes.

**Entregable:** 100 pedidos reales con Stripe Connect en produccion.

**Metrica de exito:** 30 pedidos/semana en restaurantes.

## Fase 4 - PWA Comercio (3-4 semanas)

**Objetivo:** los comercios gestionan su operacion sin WhatsApp.

- [ ] PWA Comercio (Vue 3 + Vite).
- [ ] Login con email (Firebase Auth real).
- [ ] Alta de comercio (vertical, categorias, horario).
- [ ] Subir catalogo (productos + precios + fotos).
- [ ] Ver pedidos en tiempo real.
- [ ] Generar ticket con folio.
- [ ] Marcar "listo para recoger".
- [ ] Panel de ventas del mes.

**Entregable:** comercios operan sin asistencia manual.

**Metrica de exito:** 5 comercios activos, 200 pedidos/mes.

## Fase 5 - PWA Chofer (3-4 semanas)

**Objetivo:** asignacion automatica y trazabilidad de entregas.

- [ ] PWA Chofer (Vue 3 + Vite).
- [ ] Login con OTP (telefono + PIN).
- [ ] Ver pedidos asignados.
- [ ] Aceptar/rechazar pedido.
- [ ] Escanear ticket del comercio al recoger.
- [ ] Marcar entregado con firma digital.
- [ ] Historial de entregas + pagos.
- [ ] Cloud Function `asignarChofer` (por zona + disponibilidad).
- [ ] Notificaciones PUSH (FCM).

**Entregable:** pedidos llegan a la familia sin intervencion manual.

**Metrica de exito:** 10 choferes activos, 500 pedidos/mes.

## Fase 6 - Admin y monitoreo (1-2 semanas)

**Objetivo:** AutoElekIA gestiona todo desde un panel.

- [ ] Panel Admin (Vue 3 + Vite).
- [ ] Ver hogares activos.
- [ ] Ver comercios y sus ventas.
- [ ] Ver choferes y sus entregas.
- [ ] Reportes de GMV y comisiones.
- [ ] Firebase App Check activado.
- [ ] Sentry para errores.
- [ ] UptimeRobot.
- [ ] Backups automaticos de Firestore.

**Entregable:** operacion autocontenida.

## Fase 7 - Escala (2-6 meses)

**Objetivo:** 500+ familias y 30+ comercios.

- [ ] CDN para imagenes de productos.
- [ ] Busqueda avanzada (Algolia).
- [ ] Multi-ciudad (Puebla, CDMX, Guadalajara).
- [ ] API publica para integraciones.
- [ ] Marketplace de ofertas.
- [ ] App nativa iOS/Android (si PWA no alcanza).
- [ ] Programa de referidos.
- [ ] Expansion vertical (farmacia, floreria, mascotas).

## Riesgos identificados

| Riesgo | Probabilidad | Mitigacion |
|--------|--------------|------------|
| Falta de masa critica | Alta | Enfocarse en 1 colonia primero |
| Comercios no quieren Stripe | Media | Alternativa: SPEI directo o efectivo |
| Choferes no se activan | Media | Pago fijo por entrega ($30) |
| Firebase se vuelve caro | Media | Migrar a Cloud Run o backend propio |
| Confusion fiscal | Baja | Documentar con contador |
| Competencia de Rappi/Uber | Baja | Foco en comisiones bajas (5-8%) |
| Productos perecederos | Baja | Protocolo sanitario documentado |

## Metricas de exito por fase

| Fase | Metrica | Meta |
|------|---------|------|
| Fase 0 | Pedidos manuales | 10 |
| Fase 2 | Familias activas | 3 |
| Fase 3 | Pedidos/semana (restaurantes) | 30 |
| Fase 4 | Comercios activos | 5 |
| Fase 5 | Choferes activos | 10 |
| Fase 6 | Ingreso AutoElekIA | $5,000 MXN/mes |
| Fase 7 | Familias / Comercios | 500+ / 30+ |

## Proximos 3 pasos concretos

1. **Fase 0:** Acuerdo con 1 comercio amigo + 1 chofer de confianza.
2. **Fase 0:** Hacer 10 pedidos manuales esta semana.
3. **Fase 2:** Documentar aprendizajes, arrancar multi-hogar.

## Tecnologias a evaluar

- Stripe Connect vs Conekta vs Openpay (para comercios sin RFC).
- Firebase FCM vs WhatsApp Business API (notificaciones).
- Canvas nativo vs libreria (firma digital).
- Google Maps vs Leaflet (mapas de entregas).
