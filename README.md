# Wedding RSVP Page

Pagina web gratuita para confirmar asistencia a la boda. Las respuestas se guardan en Google Sheets.

## Setup

### 1. Crear la Google Sheet

1. Ve a [sheets.google.com](https://sheets.google.com) y crea una hoja nueva
2. En la primera fila, pon estos encabezados (uno por columna):

   | A | B | C | D | E | F |
   |---|---|---|---|---|---|
   | Timestamp | Nombre | Asistencia | Num Invitados | Bus | Mensaje |

### 2. Crear el Google Apps Script

1. En la Google Sheet, ve a **Extensiones > Apps Script**
2. Borra el contenido del editor y pega este codigo:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    new Date().toLocaleString('es-ES'),
    data.nombre,
    data.asistencia,
    data.numInvitados,
    data.bus,
    data.mensaje
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Guarda el proyecto (Ctrl+S) y ponle un nombre (ej: "RSVP Boda")
4. Haz clic en **Implementar > Nueva implementacion**
5. Tipo: **Aplicacion web**
6. Ejecutar como: **Yo**
7. Quien tiene acceso: **Cualquier persona**
8. Haz clic en **Implementar** y copia la URL que te da

### 3. Conectar la pagina con el script

Abre `index.html` y busca esta linea cerca del final:

```javascript
const SCRIPT_URL = 'TU_URL_DE_GOOGLE_APPS_SCRIPT_AQUI';
```

Reemplaza `TU_URL_DE_GOOGLE_APPS_SCRIPT_AQUI` con la URL que copiaste en el paso anterior.

### 4. Personalizar el contenido

En `index.html`, busca y reemplaza:
- `Nombre` y `Nombre` (en el hero) por vuestros nombres
- `15 de junio de 2026` por la fecha de la boda
- `Nombre del Lugar · Ciudad` por el lugar y la ciudad
- `1 de mayo de 2026` por la fecha limite de confirmacion

### 5. Publicar en GitHub Pages (gratis)

1. Crea un repositorio en [github.com/new](https://github.com/new) (puede ser privado)
2. Sube el archivo `index.html`
3. Ve a **Settings > Pages**
4. Source: **Deploy from a branch** > **main** > **/ (root)**
5. Guarda y espera unos minutos. Tu pagina estara en `https://tu-usuario.github.io/nombre-repo/`

## Estructura

```
wedding-rsvp/
  index.html   ← La pagina completa (HTML + CSS + JS)
  README.md    ← Este archivo
```
