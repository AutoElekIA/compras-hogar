# Compras del hogar — PWA con Firebase

## 1. Crear el proyecto de Firebase (una sola vez)

1. Entra a https://console.firebase.google.com con tu cuenta de Google.
2. "Agregar proyecto" → dale un nombre (ej. `compras-hogar`) → sigue el asistente.
3. En el menú lateral: **Compilación → Firestore Database** → "Crear base de datos" → modo **producción** → elige la región más cercana (ej. `us-central`).
4. En **Firestore → Reglas**, reemplaza el contenido por:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /items/{doc} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```
   Esto permite leer/escribir solo a quien haya iniciado sesión (aunque sea anónima) en tu app — no a cualquiera en internet.

5. En el menú lateral: **Compilación → Authentication** → "Comenzar" → pestaña "Sign-in method" → habilita **Anónimo**.
6. En el menú lateral: ⚙️ **Configuración del proyecto** → baja hasta "Tus apps" → ícono `</>` (Web) → dale un apodo → "Registrar app".
7. Copia el objeto `firebaseConfig` que te muestra (apiKey, authDomain, projectId, etc.).
8. Abre `index.html` en este paquete y pega esos valores donde dice `PEGA_AQUI_TU_...` (busca `firebaseConfig` cerca del inicio del `<script type="module">`).

## 2. Subir a GitHub Pages

1. En GitHub, crea un repositorio nuevo (ej. `compras-hogar`), público o privado — ambos funcionan con Pages.
2. Sube estos 4 archivos a la raíz del repositorio: `index.html`, `manifest.json`, `service-worker.js`, `icon-192.png`, `icon-512.png`.
3. En el repositorio: **Settings → Pages** → en "Source" elige la rama `main` y carpeta `/ (root)` → Guardar.
4. Espera 1–2 minutos. GitHub te dará una URL tipo `https://tu-usuario.github.io/compras-hogar/`.
5. Abre esa URL en tu celular y en el de tu esposa. En Chrome Android aparecerá la opción "Instalar app" o "Agregar a pantalla de inicio" — así queda como app normal, sin necesidad de navegador.

## 3. Traer tus 53 productos actuales (sin volver a capturarlos)

1. Abre el artefacto viejo en claude.ai → menú (▾) → "Copiar como Markdown".
2. Pega ese texto en el chat con Claude y pídele que te lo convierta al formato `nombre; cantidad; precio; categoría`, una línea por producto.
3. Pega el resultado en el cuadro "Importar lista" de la nueva app (aparece debajo de la lista) → botón "Importar lista".
4. Hecho una vez, borra ese texto del cuadro — no vuelvas a usarlo salvo que necesites importar algo más.

## Notas

- La lista se sincroniza en tiempo real entre todos los que tengan la URL e inicien sesión (anónima) — pensado para uso familiar, no para compartir públicamente el link.
- Si algún día quieres cerrar el acceso a alguien, tendrías que rotar las reglas de Firestore o borrar la base — no hay manejo de usuarios individuales en esta versión simple.
- El service worker cachea el cascarón de la app para que abra rápido y funcione parcialmente sin conexión; los datos en sí siempre requieren internet para sincronizar.
