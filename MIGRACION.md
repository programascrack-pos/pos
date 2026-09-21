# MIGRACIÓN — de victoralvarezojeda@gmail.com a programascrack@gmail.com

**Importante:** no tengo acceso a ninguna de las dos cuentas de Google, ni a tu GitHub, ni a tu Vercel. Todo lo que sigue lo tenés que hacer vos, en este orden exacto. Yo ya dejé listo lo único que sí puedo entregar: el `Code.gs` con la metadata actualizada (ver `CAMBIOS.md`) y esta guía.

## Orden de pasos

### 1. Copiar la planilla a la cuenta nueva
- Con `victoralvarezojeda@gmail.com`, abrí la planilla de staging actual.
- Compartila con `programascrack@gmail.com` como Editor (o simplemente hacé `Archivo → Crear una copia` estando logueado como `programascrack@gmail.com`, si tenés acceso desde ahí).
- Poné el nombre exacto: `SmartPOS_VAO_Sistemas_Planilla_Staging_FINAL`.
- **No borres ni toques la planilla vieja** — queda como respaldo, tal como pediste.

### 2. Crear el Apps Script nuevo, vinculado a la copia
- Ya logueado como `programascrack@gmail.com`, abrí la planilla nueva → `Extensiones → Apps Script`. Esto crea un proyecto de Apps Script **vinculado automáticamente** a esa planilla — no hace falta configurar ningún ID a mano (el `Code.gs` usa `getActiveSpreadsheet()`, no `openById`, así que se conecta solo).
- Borrá el contenido default del editor y pegá el `Code.gs` completo que te dejo en este paquete (ya tiene la metadata de `programascrack@gmail.com`).
- Guardá.

### 3. Desplegar como aplicación web
- `Implementar → Nueva implementación → Aplicación web`.
- Ejecutar como: **Yo** (tu cuenta nueva).
- Quién tiene acceso: **Cualquier usuario**.
- Copiá la URL que te da (termina en `/exec`). Esa es tu **Web App nueva**.

### 4. Configurar Script Properties — SOLO lo necesario
Desde el editor de Apps Script (`⚙️ Configuración del proyecto → Propiedades del script`), agregá:
- `ADMIN_CLAVE_HASH` — **no la copies desde la cuenta vieja tal cual si no hace falta**: lo más simple y seguro es generarla de nuevo acá. Ejecutá una vez, a mano, desde el editor: `setAdminClave('tu-clave-real')`.
- `PIN_XX`, `PIN_LP` (y cualquier otro prefijo que ya tengas) — mismo criterio: podés copiar el hash tal cual desde la planilla/proyecto viejo si querés conservar los mismos PIN, o generarlos de nuevo con `setPIN('XX','1234')` etc. Si los copiás, copiá el **hash**, nunca el PIN en texto plano, y no lo pegues en ningún chat, commit ni documento.
- `MP_TOKEN_XX` (y demás prefijos con Mercado Pago configurado) — mismo criterio que el PIN: copiá el valor real solo si es indispensable, y hacelo directo entre los dos paneles de Script Properties (Google → Google), nunca pegándolo en un archivo, commit, o acá en el chat.
- **No copies nada que no uses.** Si un prefijo no tiene Mercado Pago configurado, no crees esa propiedad.

### 5. Actualizar la URL en el frontend
Editá estos 3 archivos (te los dejo listos, con la línea marcada) y reemplazá `API_URL` por la URL que te dio el paso 3:
- `pos.html`
- `admin.html`
- `reportes.html`

Buscá la línea `const API_URL = '...'` en cada uno — es la única línea que cambia en los 3 archivos.

### 6. Subir a GitHub y dejar que Vercel despliegue
- Subí los 4 HTML + lo que ya tenías al repo `https://github.com/programascrack-pos/pos`.
- **Antes de subir, revisá que ningún archivo tenga**: contraseñas, PIN, hashes, tokens de Mercado Pago, ni ningún Spreadsheet ID (no debería haber ninguno, porque el sistema no usa IDs — pero conviene revisar igual antes del primer push a un repo nuevo).
- Vercel debería redesplegar solo al detectar el push (si ya está conectado a ese repo).

### 7. Prueba real — la tenés que hacer vos
No puedo acceder a `pos-dun-five.vercel.app` ni a ninguna de las dos cuentas de Google desde acá, así que esta parte no la puedo confirmar yo. Hacé, en este orden:
1. Login como XX en el sitio nuevo.
2. Una venta de prueba.
3. Confirmá en `SmartPOS_VAO_Sistemas_Planilla_Staging_FINAL` (la nueva) que: bajó el stock, apareció la venta.
4. Confirmá en la planilla **vieja** que esa venta **no** aparece ahí.
5. Login ADMIN, revisá el panel, probá activar/suspender, reportes, logout y volver a entrar.

Si algo de esto falla, contame exactamente en qué paso y qué viste — con eso puedo seguir ayudando a diagnosticar, aunque no pueda entrar yo mismo a probarlo.

## Resumen — qué se migra y qué no
| Elemento | Se migra | Cómo |
|---|---|---|
| Datos de todas las hojas | Sí | Copiando la planilla completa (paso 1) |
| Lógica de `Code.gs` | Sí, sin cambios | Pegado tal cual (paso 2) — ni un cambio funcional |
| `ADMIN_CLAVE_HASH`, `PIN_*` | A tu criterio | Copiar el hash o generar de nuevo (paso 4) |
| `MP_TOKEN_*` | Solo los que uses | Copiar directo entre paneles de Google, nunca por archivo/chat |
| Spreadsheet ID | No aplica | El sistema no usa IDs — se vincula solo al crear el Apps Script en la planilla nueva |
