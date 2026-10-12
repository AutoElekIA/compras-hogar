# Fiscal y Legal

Consideraciones fiscales y legales para operar Compras del Hogar en
Mexico. **Este documento es orientativo y debe validarse con un
contador publico antes de operar.**

## Modelo fiscal elegido

**AutoElekIA actua como plataforma tecnologica (SaaS), NO como
intermediario financiero.**

Esto significa:
- AutoElekIA no recibe el dinero de las compras.
- AutoElekIA no custodia fondos de terceros.
- AutoElekIA no es fintech ni esta regulada por CNBV.
- AutoElekIA solo cobra una comision por servicio B2B.

## Flujo del dinero (Stripe Connect)

    FAMILIA
      │ paga $500 MXN por pedido
      ▼
    STRIPE (merchant of record)
      │
      ├─► $475 al COMERCIO (cuenta conectada)
      │     emite CFDI a la familia
      │
      └─► $25 a AUTOLEKIA (application fee)
            emite CFDI al comercio por comision

**Nadie mas toca el dinero.** Stripe maneja el split automatico.

## Obligaciones fiscales de AutoElekIA

### 1. Facturacion al comercio

Cada mes AutoElekIA emite CFDI al comercio por el total de comisiones
del periodo.

| Campo CFDI | Valor |
|-----------|-------|
| Emisor | AutoElekIA (RFC propio) |
| Receptor | Comercio (RFC del comercio) |
| Concepto | "Comision por servicio de plataforma" |
| ClaveProdServ | 81112100 (Servicios de plataformas) |
| ClaveUnidad | E48 (Unidad de servicio) |
| IVA | 16% trasladado |
| Forma de pago | 03 (Transferencia electronica) |
| Metodo de pago | PUE (Pago en una sola exhibicion) |

### 2. Declaraciones mensuales

- **ISR provisional:** sobre utilidad del mes
- **IVA mensual:** trasladado (cobrado) - acreditable (pagado)
- **DIOT:** proveedores del mes (si aplica)
- **Contabilidad electronica:** enviar a SAT

### 3. Declaracion anual

- ISR anual por ejercicio fiscal
- Conciliacion de ingresos vs facturas emitidas
- Conciliacion con Stripe (reporte anual de Stripe)

## Regimen fiscal recomendado

### RESICO (Regimen Simplificado de Confianza)

Aplica si:
- Ingresos anuales < $3.5 millones MXN
- Todos los ingresos facturados
- Pagos electronicos

Ventajas:
- ISR bajo (1-2.5% sobre ingresos)
- Contabilidad simplificada
- Facturacion electronica facilitada

**Recomendacion:** aplicar a RESICO mientras se pueda.

### Cuando supere RESICO

- Regimen general de personas morales (PM)
- ISR corporativo (30% sobre utilidad)
- Requiere contador de tiempo completo o despacho

## Obligaciones de los comercios

Los comercios son **responsables de**:
- Emitir CFDI a la familia por cada venta
- Declarar sus ingresos
- Cobrar IVA al consumidor final
- Su propio ISR
- Stripe les retiene ISR si no estan en RESICO

**AutoElekIA NO es responsable de la facturacion del comercio.**
Esto se documenta en el contrato de onboarding.

## Obligaciones de los choferes

Los choferes son **aliados independientes**, no empleados.

- Emiten CFDI a AutoElekIA (o al comercio) por sus servicios
- Estan en RESICO individual o regimen simplificado
- No hay relacion laboral
- AutoElekIA les retiene ISR/IVA si su regimen lo requiere

**Alternativa:** contratar a los choferes como prestadores de servicios
con contrato de prestacion de servicios profesionales.

## Documentos legales requeridos

### 1. Terminos y Condiciones (T&C)

Puntos clave:
- AutoElekIA actua como plataforma, no como vendedor
- El comercio es responsable de la venta y su facturacion
- El chofer es responsable de la entrega
- AutoElekIA solo facilita la conexion
- Limitacion de responsabilidad
- Ley aplicable: Mexico, tribunales de Puebla

### 2. Aviso de Privacidad

Cumplimiento de LFPDPPP (Ley Federal de Proteccion de Datos):
- Datos recabados: nombre, direccion, telefono, email, lista de compras
- Finalidad: conectar familia - comercio - chofer
- Transferencia a terceros (Stripe, Firebase)
- Derechos ARCO
- Contacto DPO

### 3. Contrato con Comercios

- Alta como "Merchant"
- Aceptacion de comisiones (5% o segun acuerdo)
- Aceptacion de usar Stripe Connect
- Responsabilidades del comercio
- Terminacion y efectos
- Confidencialidad

### 4. Contrato con Choferes

- Prestacion de servicios independientes
- Pago por entrega (monto definido)
- No relacion laboral
- Responsabilidades (no dañar productos, no cobrar por fuera)
- Terminacion

## Regulaciones aplicables

| Regulacion | Aplica | Notas |
|-----------|--------|-------|
| LFPDPPP | Si | Datos personales de familias |
| LFPC | Si | Proteccion al consumidor |
| CODIGO DE COMERCIO | Si | Relaciones comerciales |
| LISR | Si | Impuesto sobre la renta |
| LIVA | Si | IVA |
| LFDC | No | No hay comercio electronico propio |
| CNBV | No | No hay manejo de fondos de terceros |
| IFT | No | No hay servicios de telecomunicaciones |
| NOM-151 | Si (futuro) | Conservacion de mensajes (si hay chat) |

## Cumplimiento con Stripe

Stripe exige:
- KYC (Know Your Customer) a cada comercio
- AML (Anti Money Laundering)
- Terminos de servicio del usuario
- Verificacion de identidad de titulares

**AutoElekIA se apoya en Stripe Connect Express** para delegar KYC/AML.
El comercio completa el onboarding directamente con Stripe.

## Politica de datos

| Dato | Retencion | Quien lo ve |
|------|-----------|-------------|
| Lista de compras | 12 meses | Familia + comercio que atendio |
| Historial de pedidos | 5 años | Familia + comercio + AutoElekIA |
| Datos de pago | Gestionado por Stripe | Solo Stripe y comercio |
| Direccion de entrega | 12 meses | Familia + chofer (durante entrega) |
| Datos personales | Mientras cuenta activa | Segun aviso de privacidad |

## Riesgos legales

| Riesgo | Probabilidad | Mitigacion |
|--------|--------------|------------|
| Comercio no emite CFDI | Media | Documentado en contrato, monitoreo |
| Chofer causa daño a productos | Media | Contrato claro + seguro |
| Familia demanda por producto | Baja | T&C limita responsabilidad |
| Stripe suspende cuenta | Baja | Cumplir ToS, diversificar pasarelas |
| SAT cuestiona comision | Media | Documentar modelo claro, contador |
| Dato personal filtrado | Baja | Reglas Firestore + App Check |

## Proximos pasos (accionables)

- [ ] Contratar contador familiarizado con marketplaces
- [ ] Definir RFC y regimen fiscal de AutoElekIA
- [ ] Redactar T&C, Aviso de Privacidad, contratos
- [ ] Registrarse como Plataforma Tecnologica ante SAT (si aplica)
- [ ] Configurar Stripe Connect en modo test
- [ ] Documentar comisiones en Stripe (application fee)
- [ ] Validar modelo con un CFDI de prueba

## Referencias

- Ley Federal de Proteccion de Datos Personales en Posesion de los
  Particulares (LFPDPPP)
- Ley del Impuesto sobre la Renta (LISR)
- Ley del Impuesto al Valor Agregado (LIVA)
- Reglas de RESICO (SAT)
- Terminos de Stripe Connect
- Documentacion de Stripe Connect Mexico
