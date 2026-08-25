# Firma de correo — Integria

Hosting de las imágenes y del HTML de la firma de correo corporativa.

## Estructura

```
firma/              PNG servidos por URL desde el HTML de la firma
index.html          página de verificación del deploy
firma-urls.html     firma para producción (6 KB) — la que se pega en el cliente de correo
firma-base64.html   misma firma autocontenida (42 KB) — respaldo, no requiere hosting
```

## Uso

1. Deployar en Vercel (proyecto estático, sin framework).
2. Abrir `https://integria-khaki.vercel.app/` para verificar que las imágenes cargan.
3. Reemplazar el placeholder de las URLs:

   ```bash
   sed -i 's|https://integria-khaki.vercel.app/firma|https://integria-khaki.vercel.app/firma|g' firma-urls.html index.html
   ```

4. Pegar el contenido de `firma-urls.html` como firma en el cliente de correo.

## Importante

- **Deployment Protection debe estar desactivado** en Vercel (Settings → Deployment Protection), o las imágenes devuelven 401 desde el correo.
- **Las URLs son permanentes**: cada correo enviado queda apuntando a ellas para siempre. Conviene usar un dominio propio (`firma.integria.pro`) en lugar de `*.vercel.app`, para poder mover el hosting sin romper el histórico.
- El badge lleva la vigencia `JUN 2026-JUN 2027` dentro del PNG: hay que reemplazar `firma/badge-gptw.png` cada año.

## Pendiente

- Las imágenes están a 1x; en pantallas HiDPI se ven borrosas. Reexportar al doble desde los originales.
