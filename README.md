# Firma de correo — Integria

Hosting de las imágenes y del HTML de la firma de correo corporativa.

## Estructura

```
firma/              PNG servidos por URL desde el HTML de la firma
index.html          página de verificación del deploy
firma-outlook.html  documento completo listo para instalar en Outlook
firma-urls.html     fragmento de la firma (solo la <table>)
firma-base64.html   misma firma autocontenida (42 KB) — respaldo, no requiere hosting
```

## Uso

1. Deployar en Vercel (proyecto estático, sin framework).
2. Abrir `https://integria-khaki.vercel.app/` para verificar que las imágenes cargan.
3. Reemplazar el placeholder de las URLs:

   ```bash
   sed -i 's|https://integria-khaki.vercel.app/firma|https://integria-khaki.vercel.app/firma|g' firma-urls.html index.html
   ```

4. Instalar `firma-outlook.html` como firma (ver abajo).

## Instalar la firma en Outlook

**Opción A — copiar y pegar (sirve para todas las versiones)**

1. Abrir `firma-outlook.html` en Chrome o Edge.
2. Seleccionar la firma con el mouse (o `Ctrl+A`) y copiar con `Ctrl+C`.
3. En Outlook: Archivo → Opciones → Correo → Firmas → Nueva, y pegar con `Ctrl+V`.

**Opción B — instalar el archivo (solo Outlook clásico de escritorio, más fiel)**

1. Crear una firma vacía en Outlook con el nombre deseado, por ejemplo `Integria`.
2. Abrir `%APPDATA%\Microsoft\Signatures` en el explorador.
3. Reemplazar el contenido de `Integria.htm` por el de `firma-outlook.html`.

Conviene enviarse un correo de prueba y revisarlo en Outlook de escritorio: usa el
motor de Word y puede dibujar una línea de 1px en las uniones entre celdas con
fondos distintos (el badge está en una celda con `rowspan`).

## Importante

- **Deployment Protection debe estar desactivado** en Vercel (Settings → Deployment Protection), o las imágenes devuelven 401 desde el correo.
- **Las URLs son permanentes**: cada correo enviado queda apuntando a ellas para siempre. Conviene usar un dominio propio (`firma.integria.pro`) en lugar de `*.vercel.app`, para poder mover el hosting sin romper el histórico.
- El badge lleva la vigencia `JUN 2026-JUN 2027` dentro del PNG: hay que reemplazar `firma/badge-gptw.png` cada año.

## Pendiente

- Las imágenes están a 1x; en pantallas HiDPI se ven borrosas. Reexportar al doble desde los originales.
