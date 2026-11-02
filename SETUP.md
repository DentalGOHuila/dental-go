# Montaje de Dental GO (Firebase + GitHub Pages)

Tiempo estimado: 45 a 60 minutos, una sola vez. Costo: $0 (planes gratuitos).

Resultado: dos direcciones.
- Tienda pública: `https://USUARIO.github.io/dental-go/`
- Panel de ella: `https://USUARIO.github.io/dental-go/admin/`

## Parte 1. Firebase (la base de datos)

1. Entra a https://console.firebase.google.com con tu cuenta de Google y pulsa **Crear un proyecto**. Nombre: `dental-go`. Google Analytics: puedes desactivarlo.
2. **Authentication**
   1. Menú Compilación, **Authentication**, **Comenzar**.
   2. Método de acceso **Correo electrónico/contraseña**: actívalo y guarda.
   3. Pestaña **Usuarios**, **Agregar usuario**: crea uno para ella y otro para ti (correo y contraseña).
   4. Copia el **UID** de cada usuario (columna "UID de usuario").
   5. Si en Configuración de Authentication aparece la opción para desactivar el registro de usuarios nuevos, desactívala.
3. **Firestore**
   1. Menú Compilación, **Firestore Database**, **Crear base de datos**.
   2. Ubicación: la más cercana que ofrezca (por ejemplo `southamerica-east1`). No se puede cambiar después.
   3. Modo **producción**.
   4. Pestaña **Reglas**: borra todo y pega el contenido de `firestore.rules`. Reemplaza `UID_DE_ELLA` y `UID_TUYO` por los UID del paso anterior. Pulsa **Publicar**.
4. **Datos de conexión**
   1. Engranaje junto a "Descripción general del proyecto", **Configuración del proyecto**.
   2. En "Tus apps", pulsa el ícono `</>` (Web) y registra la app (nombre `dental-go`, sin Hosting).
   3. Copia los valores `apiKey`, `authDomain`, `projectId` y `appId` y pégalos en `firebase-config.js`.

## Parte 2. GitHub Pages (donde viven las páginas)

1. En https://github.com crea un repositorio público llamado `dental-go`.
2. Sube todo el contenido de esta carpeta (`index.html`, `admin/`, `firebase-config.js`, `manifest.webmanifest`, los iconos y `firestore.rules`). Con el botón **Add file, Upload files** basta; conserva la carpeta `admin`.
3. Settings, **Pages**: Source **Deploy from a branch**, rama `main`, carpeta `/ (root)`. Guarda. A los 1 o 2 minutos queda publicado.
4. Vuelve a Firebase, Authentication, Configuración, **Dominios autorizados**: agrega `USUARIO.github.io` (tu usuario de GitHub).

El repositorio es público y `firebase-config.js` queda visible. Eso es normal: esos datos no son secretos. Quien protege la información son las reglas de `firestore.rules`: solo dos cuentas pueden leer y escribir, y la tienda solo lee un documento sin costos.

## Parte 3. Primer uso

1. Abre `.../admin/`, entra con el correo de ella y registra un primer pedido.
2. En la pestaña **Catálogo** escribe el WhatsApp con código de país y las descripciones.
3. Abre la tienda pública: ya debe mostrar los productos.
4. En el celular de ella: abre `.../admin/` en el navegador, menú, **Añadir a pantalla de inicio**.

## Si algo falla

| Síntoma | Causa probable |
| --- | --- |
| "Falta conectar Firebase" | `firebase-config.js` todavía tiene `PEGAR_AQUI` |
| "Esta cuenta no tiene acceso" | Los UID de `firestore.rules` no coinciden con los de la cuenta |
| No entra con la contraseña | El usuario no existe en Authentication, o el dominio `github.io` no está autorizado |
| La tienda dice "Muy pronto" | Aún no hay productos, o el panel no se ha abierto desde que se cargaron |

## Límites del plan gratuito

Firebase Spark permite 50.000 lecturas y 20.000 escrituras por día. La tienda lee un solo documento por visita, así que alcanza para miles de visitas diarias. Si se superara una cuota, ese servicio se apaga hasta el mes siguiente.
