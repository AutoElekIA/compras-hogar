# Modelo de Negocio

## Vision general

Compras del Hogar es una plataforma que conecta a cuatro actores:
**familias**, **comercios**, **choferes** y **AutoElekIA** como
facilitador tecnologico. El objetivo es que las familias compren a
comercios locales, con entrega a domicilio realizada por choferes
que ya operan rutas de transporte escolar.

## Los cuatro actores

### 1. Familias (usuarios gratuitos)

- Arman su lista de compras en la app.
- Ven ofertas de comercios cercanos.
- Pagan al comercio via Stripe Connect.
- Reciben su pedido en casa.
- **No pagan por usar la plataforma.**

### 2. Comercios (clientes que pagan)

- Publican su catalogo y ofertas.
- Reciben pedidos de familias reales.
- Preparan el pedido y emiten ticket.
- Reciben el pago directo de la familia via Stripe Connect.
- **Pagan a AutoElekIA una comision por servicio.**

### 3. Choferes (aliados operativos)

- Ya trabajan en rutas de transporte escolar.
- Usan tiempo muerto entre rutas para entregas.
- Recogen el pedido en el comercio.
- Entregan a la familia.
- Confirman entrega en la app.
- **Reciben un pago por entrega (definido por AutoElekIA o el comercio).**

### 4. AutoElekIA (plataforma)

- Conecta familias con comercios.
- Asigna choferes a entregas.
- Genera y valida tickets de entrega.
- Cobra comision al comercio via Stripe Connect (application fee).
- **Nunca toca el dinero de la compra.**

## Flujo operativo completo

    1. FAMILIA crea pedido
       App envia lista + direccion al comercio

    2. COMERCIO acepta
       Prepara productos + genera ticket con folio
       Notifica "listo para entrega"

    3. AUTOLEKIA asigna chofer
       Chofer mas cercano / disponible
       Notifica al chofer: recoger pedido #X en comercio Y

    4. CHOFER recoge
       Escanea ticket del comercio
       App registra "recogido" con timestamp

    5. CHOFER entrega
       Llega con la familia
       Familia ya pago (via Stripe)
       Chofer marca "entregado" en app
       Familia firma digitalmente el ticket

    6. AUTOLEKIA cierra el ciclo
       Ticket firmado queda en el sistema
       Se registra comision para facturar al comercio
       Datos disponibles para analytics

## Flujo del dinero (Stripe Connect)

    FAMILIA
      │ paga $500 (total del ticket)
      ▼
    STRIPE
      │
      ├─ $450 al COMERCIO (connected account)
      │
      └─ $50 a AUTOLEKIA (application fee)
         │
         └─ menos fee de Stripe (~3.6% + $3 MXN)

**Puntos clave:**
- AutoElekIA NUNCA recibe el dinero de la compra.
- El comercio es merchant of record (factura al cliente).
- AutoElekIA factura solo su comision como servicio B2B.
- Stripe maneja la liquidacion automatica a ambas partes.

## Modelo de ingresos para AutoElekIA

**Fuente principal: application fee por transaccion**

- Porcentaje del ticket (ej. 10%)
- Se cobra automaticamente via Stripe Connect
- Sin facturacion manual por cada pedido
- Stripe reporta mensualmente el total de fees

**Fuentes secundarias (futuro):**

- Suscripcion mensual al comercio (destacar productos)
- Pago por visibilidad premium (aparecer primero en busquedas)
- Comision por entrega al chofer (opcional)

## Modelo de ingresos para choferes

- **Opcion A:** Pago fijo por entrega ($30-$50 MXN)
- **Opcion B:** Porcentaje del ticket (5-10%)
- **Opcion C:** Propinas de la familia
- **Opcion D:** Combinacion de A + C

**Decision pendiente** (definir con los choferes antes de lanzar).

## Ventajas competitivas

| Actor | Beneficio |
|-------|-----------|
| **Familias** | Compran mas barato (ven ofertas), sin ir al super, con entrega |
| **Comercios** | Reciben demanda real (no publicidad ciega), con conversion alta |
| **Choferes** | Ingreso extra usando tiempo muerto entre rutas escolares |
| **AutoElekIA** | Ingreso recurrente por comision, sin riesgo crediticio |

## Reglas de negocio clave

1. **La familia paga antes de que el comercio prepare el pedido.**
   Cero riesgo para el comercio.

2. **El chofer no maneja efectivo.** Todo el pago es digital via Stripe.

3. **El ticket es el documento central.** Une a los 4 actores y permite
   trazabilidad completa.

4. **AutoElekIA no toca el dinero de la compra.** Solo cobra su fee.

5. **Los choferes no son empleados de AutoElekIA.** Son aliados
   independientes que cobran por entrega.

## Metricas de exito

| Metrica | Meta mes 6 | Meta mes 12 |
|---------|-----------|-------------|
| Familias activas | 50 | 500 |
| Comercios pagando | 5 | 30 |
| Choferes activos | 10 | 50 |
| Pedidos/mes | 500 | 5,000 |
| GMV (volumen transaccionado) | $250,000 MXN | $2,500,000 MXN |
| Ingreso AutoElekIA (10% fee) | $25,000 MXN | $250,000 MXN |

## Estado actual

- **Fase 0:** app de lista familiar funciona (single-file PWA).
- **Fase 1:** documentacion del modelo (este documento).
- **Fase 2:** multi-hogar + Stripe Connect (siguiente).
- **Fase 3:** portales de comercio y chofer.
