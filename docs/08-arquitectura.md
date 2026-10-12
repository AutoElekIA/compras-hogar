# Arquitectura Tecnica

## Vision general

Plataforma de 3 PWAs + backend serverless (Firebase) + Stripe Connect
para procesamiento de pagos. Sin servidor propio en Fase 1.

## Stack tecnologico

| Capa | Tecnologia | Razon |
|------|-----------|-------|
| **PWA Familia** | HTML/CSS/JS vanilla (single-file) | Simple, sin build, ya funciona |
| **PWA Comercio** | Vue 3 + Vite | CRUDs, dashboard, mas complejo |
| **PWA Chofer** | Vue 3 + Vite | Similar a Admin de transporte |
| **Backend** | Firebase (Firestore + Auth + Functions) | Serverless, real-time, gratis al inicio |
| **Pagos** | Stripe Connect (Express accounts) | Split automatico, KYC delegado |
| **Hosting** | GitHub Pages / Cloudflare Pages | Gratis, CDN, SSL automatico |
| **Notificaciones** | Firebase Cloud Messaging (FCM) | Push nativo, gratis |

## Modelo de datos (Firestore)

### Coleccion: hogares

    {
      id: "auto-generado",
      nombre: "Familia Aleman",
      codigoInvitacion: "ABC123",       // unico, 6 chars
      miembros: ["uid1", "uid2"],        // uids de auth anonima
      creadoEn: timestamp,
      activo: true
    }

### Coleccion: items

    {
      id: "auto-generado",
      hogarId: "abc123",                 // referencia al hogar
      name: "Leche entera",
      qty: "2 litros",
      price: 24.00,
      cat: "Carnes y lacteos",
      store: "Soriana",
      notes: "Marca Lala",
      done: false,
      creadoPor: "uid1",                 // quien lo agrego
      at: timestamp
    }

### Coleccion: catalog (por hogar)

    {
      id: "auto-generado",
      hogarId: "abc123",
      name: "Leche entera",
      cat: "Carnes y lacteos",
      lastPrice: 24.00,
      lastQty: "2 litros",
      lastStore: "Soriana",
      uses: 5,
      updatedAt: timestamp
    }

### Coleccion: comercios

    {
      id: "auto-generado",
      nombre: "Abarrotes Don Pepe",
      stripeAccountId: "acct_xxx",       // cuenta Stripe Connect
      direccion: "...",
      telefono: "...",
      categorias: ["Abarrotes", "Frutas"],
      horario: {...},
      activo: true,
      comisionPct: 5.0,                  // % que cobra AutoElekIA
      creadoEn: timestamp
    }

### Coleccion: pedidos

    {
      id: "auto-generado",
      hogarId: "abc123",
      familiaUid: "uid1",
      comercioId: "com_xxx",
      items: [                           // snapshot de la lista
        {name: "Leche", qty: "2L", price: 24, cat: "..."}
      ],
      montoTotal: 250.00,
      comisionAutoElekia: 12.50,         // 5%
      montoComercio: 237.50,
      stripePaymentIntentId: "pi_xxx",
      stripeTransferId: "tr_xxx",
      estado: "pendiente"|"pagado"|"preparando"|"listo"|"en_camino"|"entregado"|"cancelado",
      direccionEntrega: "...",
      choferId: null|"uid_chofer",
      recogidoEn: null|timestamp,
      entregadoEn: null|timestamp,
      ticketUrl: null|"...",
      firmaCliente: null|"base64...",
      creadoEn: timestamp
    }

### Coleccion: choferes

    {
      id: "auto-generado",
      nombre: "Juan Perez",
      telefono: "2221234567",
      email: "...",
      stripeAccountId: "acct_xxx",       // para recibir pagos por entrega
      activo: true,
      zona: "Puebla Norte",
      disponible: true,
      rating: 4.8,
      creadoEn: timestamp
    }

## Flujo de un pedido (end-to-end)

    1. FAMILIA abre PWA Familia
       └─ Arma su lista en el hogar

    2. FAMILIA pulsa "Comprar"
       ├─ App agrupa items por categoria
       ├─ Sugiere comercio(s) que cubren la lista
       └─ Muestra precio total + comision

    3. FAMILIA elige comercio
       └─ App crea pedido en Firestore (estado=pendiente)

    4. CLOUD FUNCTION (trigger on pedido create)
       ├─ Stripe crea PaymentIntent
       └─ Devuelve client_secret al frontend

    5. FAMILIA confirma pago
       ├─ Stripe procesa pago
       ├─ Split automatico:
       │  ├─ application_fee -> AutoElekIA
       │  └─ resto -> comercio
       └─ Webhook confirma -> estado=pagado

    6. COMERCIO recibe notificacion (PWA Comercio)
       ├─ Ve pedido pagado
       ├─ Prepara productos
       ├─ Genera ticket con folio
       └─ Marca "listo para recoger"

    7. AUTOLEKIA asigna chofer
       ├─ Sistema busca chofer disponible en zona
       ├─ Envia notificacion PUSH al chofer
       └─ Chofer acepta

    8. CHOFER recoge
       ├─ Llega al comercio
       ├─ Escanea ticket (QR)
       └─ App marca "recogido" con timestamp

    9. CHOFER entrega
       ├─ Llega a la familia
       ├─ Pide firma digital (canvas en app)
       └─ Marca "entregado"

    10. CIERRE
        ├─ Ticket firmado se guarda
        ├─ Comision se registra para reportes
        └─ Ambas partes ven el status finalizado

## Integracion Stripe Connect

### Onboarding de comercios

    const account = await stripe.accounts.create({
      type: "express",
      country: "MX",
      email: "...",
      capabilities: {
        card_payments: {requested: true},
        transfers: {requested: true}
      }
    });

    // Redirigir a Stripe para que complete KYC
    const link = await stripe.accountLinks.create({
      account: account.id,
      refresh_url: "https://compras-hogar.com/onboarding/refresh",
      return_url: "https://compras-hogar.com/onboarding/complete",
      type: "account_onboarding"
    });

### Cobro con application fee

    const paymentIntent = await stripe.paymentIntents.create({
      amount: 25000,                     // $250.00 MXN
      currency: "mxn",
      application_fee_amount: 1250,      // $12.50 comision AutoElekIA
      transfer_data: {
        destination: "acct_comercio"     // cuenta del comercio
      },
      metadata: {pedidoId: "..."}
    });

## Cloud Functions necesarias

| Funcion | Trigger | Que hace |
|---------|---------|----------|
| `crearPaymentIntent` | HTTPS callable | Crea intent + application_fee |
| `webhookStripe` | HTTPS (webhook) | Recibe eventos de Stripe |
| `actualizarEstadoPedido` | Firestore onUpdate | Notifica a choferes cuando pagado |
| `asignarChofer` | Firestore onCreate | Busca chofer y asigna |
| `notificarComercio` | Firestore onUpdate | Push al comercio cuando pedido pagado |
| `notificarFamilia` | Firestore onUpdate | Push a familia cuando entregado |
| `generarTicket` | HTTPS callable | PDF del ticket firmado |

## Seguridad

### Reglas Firestore (a implementar)

    rules_version = '2';
    service cloud.firestore {
      match /databases/{database}/documents {
        // Hogares: solo miembros
        match /hogares/{hogarId} {
          allow read, write: if request.auth.uid in resource.data.miembros;
        }

        // Items: solo miembros del hogar
        match /items/{itemId} {
          allow read: if request.auth != null &&
            request.auth.uid in get(/databases/$(database)/documents/hogares/$(resource.data.hogarId)).data.miembros;
        }

        // Comercios, pedidos, choferes: solo via Cloud Functions
        match /{document=**} {
          allow read, write: if false;
        }
      }
    }

### Firebase App Check

- Activar en produccion
- Provider: reCAPTCHA v3 para web
- Impide bots escribiendo sin pasar por la app

## Hosting

| Componente | Hosting | URL |
|-----------|---------|-----|
| PWA Familia | Cloudflare Pages | compras.autoelekia.lat |
| PWA Comercio | Cloudflare Pages | comercios.autoelekia.lat |
| PWA Chofer | Cloudflare Pages | choferes.autoelekia.lat |
| Admin (futuro) | Cloudflare Pages | admin-compras.autoelekia.lat |

## Costos por fase

| Fase | Usuarios | Comercios | Costo Firebase | Stripe | Total |
|------|----------|-----------|---------------|--------|-------|
| MVP | 50 | 5 | $0 (free tier) | % por tx | $0 fijo |
| Crecimiento | 500 | 30 | ~$25/mes | % por tx | ~$25/mes |
| Escala | 5,000 | 200 | ~$100/mes | % por tx | ~$100/mes |

Stripe cobra ~3.6% + $3 MXN por transaccion. Ese costo se suma al
total o se descuenta de la parte del comercio (decision pendiente).

## Migracion futura (si es necesario)

Si Firebase se queda corto:
- Migrar a backend propio (Flask + MySQL, similar a transporte escolar)
- Mantener Firestore para real-time si es necesario
- Alternativa: Firebase + Cloud Run (compute sin servidor)
