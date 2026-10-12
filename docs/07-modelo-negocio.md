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

**Verticales soportadas:**

- **Abarrotes / super / tiendas de barrio:** margen bajo (10-15%),
  volumen alto. Comision sugerida: 5%.
- **Restaurantes y comida preparada:** margen alto (50-70%),
  recurrencia diaria. Comision sugerida: 8%.
- **Panaderia, tortilleria, carniceria:** margen bajo, ticket bajo.
  Comision sugerida: 5%.
- **Farmacias:** margen medio, urgencia alta. Comision sugerida: 6%.
- **Servicios (taller, cerrajeria, etc.):** ticket alto. Comision
  sugerida: 10%.

**Lo que hacen:**
- Publican su catalogo y ofertas.
- Reciben pedidos de familias reales.
- Preparan el pedido y emiten ticket.
- Reciben el pago directo de la familia via Stripe Connect.
- **Pagan a AutoElekIA una comision por servicio.**

**Ventaja para el comercio:**
- Rappi / Uber Eats cobran 25-35% al comercio.
- Compras del Hogar cobra 5-10% segun vertical.
- Un restaurante que hoy paga 30% a Uber Eats y migra a nosotros
  multiplica su margen por 3x-6x.

**Sobre los restaurantes especificamente:**
- Ya tienen infraestructura de delivery (empaques, protocolo).
- Ya aceptan pedidos a domicilio (usan Rappi/Uber Eats).
- Solo necesitan un repartidor mas barato.
- Los choferes escolares (ociosos entre rutas) son el repartidor
  perfecto: conocen la zona, tienen vehiculo, ya estan en movimiento.

**Vertical de mayor potencial inicial:** restaurantes y comida
preparada. La recurrencia es casi diaria, el margen es alto, y la
conversacion de venta es simple: "pagas 30% hoy, con nosotros 8%".

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




## Seguridad como diferenciador

Las plataformas actuales (Rappi, Uber Eats, Cornershop) usan
motocicletas como vehiculo principal. En Mexico, eso significa:

- Motos destartaladas, con anos de uso, sin mantenimiento regular.
- Sin documentacion en regla (placas, licencia, poliza).
- Sin seguro del repartidor.
- Repartidor anonimo, rotativo, sin relacion con la familia.
- Cobertura de carga limitada (una mochila termica).

**Nuestra propuesta:**

| Aspecto | Competencia | Compras del Hogar |
|---------|-------------|-------------------|
| Vehiculo | Moto destartalada | Camioneta escolar |
| Documentacion | Nula o irregular | Federal B, poliza, verificacion |
| Seguro | Del repartidor | De la empresa |
| Chofer | Anonimo, rotativo | Conocido (chofer escolar) |
| Capacidad | Mochila termica | Hieleras grandes |
| Productos fragiles | No soporta | Si soporta |
| Relacion con familia | Cero | Ya la conocen |

**Mensaje de venta:**

> "Tu super viaja seguro, no en una moto destartalada."

**Ventaja emocional (México):**

Las familias ya conocen al chofer escolar. Le confian a sus hijos
todos los dias. Ahora tambien le confian su despensa. **Eso no tiene
precio en el mercado mexicano.**

**Ventaja operativa:**

- Las camionetas escolares ya tienen capacidad de 15-35 pasajeros.
- Con los asientos ocupados por hieleras, caben 20-30 pedidos
  pequenos o 5-10 pedidos grandes en una sola ruta.
- Costo por entrega marginal: casi cero (el vehiculo ya esta en
  circulacion).


## Protocolo sanitario

**Los choferes estan capacitados tecnicamente para el manejo higienico
de alimentos, cumpliendo con las normas sanitarias mexicanas
(NOM-251-SSA1 y aplicables).**

### Cadena de frio completa

- **Equipo:** hieleras rigidas con geles refrigerantes, capacidad de
  mantener temperatura por 4-6 horas.
- **Pre-enfriamiento:** los geles se congelan la noche anterior.
- **Monitoreo:** temperatura objetivo por tipo de producto.
- **Registro:** incidencias registradas en la app (foto + timestamp).

### Prevencion de contaminacion cruzada

Los operadores aplican el protocolo de separacion por tipo de producto:

- **Carnes rojas crudas:** empaque primario + bolsa sellada + hielera
  dedicada.
- **Pescados y mariscos:** empaque primario + hielera dedicada
  (nunca comparte con carnes rojas).
- **Lacteos:** hielera separada o division rigida dentro de la
  misma hielera.
- **Alimentos preparados / cocidos:** empaque del restaurante +
  bolsa externa sellada.
- **Frutas y verduras:** bolsa transpirable, hielera aparte.
- **No perecederos:** sin restriccion de temperatura.

### Capacitacion tecnica de operadores

Los choferes cuentan con:

- Curso de manejo higienico de alimentos.
- Protocolo escrito de cadena de frio y contaminacion cruzada.
- Identificacion de temperaturas criticas por producto.
- Procedimiento de actuacion ante incidencias (derrame, rotura de
  empaque, exceso de tiempo en transito).
- Constancia de capacitacion vigente.

### Por que esto es un diferenciador

| Aspecto | Competencia (moto) | Compras del Hogar |
|---------|-------------------|-------------------|
| Carnes rojas | Sin control | Hielera dedicada + empaque |
| Pescados y mariscos | Casi nunca | Hielera dedicada exclusiva |
| Lacteos | Sin control | Cadena de frio continua |
| Contaminacion cruzada | Sin protocolo | Protocolo escrito aplicado |
| Capacitacion | Ninguna | Curso formal documentado |
| Trazabilidad | Cero | Registro por pedido |
| Normas sanitarias | No aplica | Cumplimiento NOM |

**Mensaje de venta:**

> "Tus carnes, pescados y lacteos viajan con protocolo sanitario.
> No en una mochila termica de moto."

### Alcance de productos

Con este protocolo, la plataforma puede manejar sin restricciones:

- Carnes rojas, pollo y cerdo (crudas y cocidas).
- Pescados y mariscos frescos.
- Lacteos y derivados.
- Embutidos y quesos.
- Alimentos preparados de restaurantes.
- Farmacia con requerimiento de temperatura.
- Frutas y verduras.
- Abarrotes no perecederos.

## Cadena de frio

**Solucion operativa:** el comercio provee la hielera + geles
refrigerantes al chofer cuando entrega el pedido.

**Por que funciona:**

- El comercio ya tiene hieleras para su operacion normal (uso diario).
- Costo marginal por pedido: ~$0 (reutilizables 1000+ veces).
- Al comercio le conviene: garantiza calidad, evita devoluciones,
  fideliza al cliente.
- El chofer solo transporta la hielera; no invierte en equipo.
- La hielera se devuelve al comercio en la siguiente recoleccion.

**Cobertura de productos:**

- Abarrotes no perecederos: sin restriccion.
- Lacteos, carnes, embutidos: hielera con geles.
- Alimentos preparados (tortas, hamburguesas, pozole): hielera +
  empaque del restaurante.
- Farmacia (medicamentos que requieren temperatura): hielera pequena.
- Helados / congelados: hielera + hielo seco si el comercio lo provee.

**Regla operativa:**
- El comercio decide si su producto requiere hielera.
- Si el chofer llega sin hielera, el comercio puede rechazar el retiro.
- La app registra "requiere hielera" por producto en el catalogo.

## Unit economics (por pedido)

Ejemplo tipico de abarrotes:

    Ticket promedio familiar:            $400 MXN
    Envio cobrado a la familia:          $30 MXN
    Comision AutoElekIA al comercio:     $20 MXN (5%)
    ----------------------------------------
    Total ingreso bruto:                 $50 MXN

    Pago al chofer por entrega:          -$30 MXN
    Costo Stripe (~3.6% + $3):           -$17 MXN
    ----------------------------------------
    Neto AutoElekIA por pedido:          ~$3 MXN

Nota: si la familia paga con SPEI (mas barato que tarjeta), el neto
sube a ~$15-18 MXN por pedido.

Ejemplo de restaurantes (ticket $250):

    Ticket promedio:                     $250 MXN
    Envio cobrado a la familia:          $30 MXN
    Comision AutoElekIA (8%):            $20 MXN
    ----------------------------------------
    Total ingreso bruto:                 $50 MXN

    Pago al chofer:                      -$30 MXN
    Stripe:                              -$12 MXN
    ----------------------------------------
    Neto AutoElekIA por pedido:          ~$8 MXN

Proyeccion por volumen (mix 50% abarrotes / 50% restaurantes):

| Pedidos/dia | AutoElekIA/mes | Choferes/mes | GMV/mes |
|-------------|---------------|--------------|---------|
| 10 | $1,650 | $9,000 | $97,500 |
| 50 | $8,250 | $45,000 | $487,500 |
| 100 | $16,500 | $90,000 | $975,000 |
| 300 | $49,500 | $270,000 | $2,925,000 |

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
