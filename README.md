# Firma de correo — Integria

Hosting de las imágenes y del HTML de la firma de correo corporativa.

## Estructura

```
imagenes/           originales de alta resolución (fuente, no se deployan)
firma/              PNG a 2x servidos por URL desde el HTML de la firma
opciones/           las dos escalas en revisión, autocontenidas (ver más abajo)
index.html          página de verificación del deploy
firma-outlook.html  documento completo listo para instalar en Outlook
firma-urls.html     fragmento de la firma (solo la <table>)
firma-base64.html   misma firma autocontenida (61 KB) — respaldo, no requiere hosting
```

La firma activa mide **509 px** de ancho (75%). Los tres HTML comparten
exactamente la misma `<table>`: si se edita uno, hay que replicar el cambio en
los otros dos y en `index.html`.

### Decisión de tamaño pendiente

`opciones/opcion-75.html` y `opciones/opcion-85.html` son las dos escalas a
revisar, cada una autocontenida (se abren en cualquier navegador, no dependen
del deploy). **Cuando se elija una hay que consolidar**: si gana la de 85%, hay
que pasar sus medidas a `firma-urls.html`, `firma-outlook.html`,
`firma-base64.html` e `index.html`, y borrar `opciones/`. No pueden convivir
dos firmas activas.

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

## Imágenes

Los PNG de `firma/` se generan desde los originales de `imagenes/` al **doble** del
tamaño con el que se muestran, y el HTML los baja con `width`/`height`. Así se ven
nítidos en pantallas HiDPI. Si se cambia algún tamaño de la firma, hay que
reexportar el PNG correspondiente al doble del nuevo tamaño.

Los PNG están dimensionados a 2x de la versión **85%**, que es la más grande. El
mismo juego sirve para la de 75% (ahí quedan a 2.26x, sobra resolución y no
molesta). Si se descarta el 85%, se pueden regenerar más chicos, pero no es
necesario: la diferencia de peso es de pocos KB.

| `firma/`            | 75%   | 85%   | archivo | origen en `imagenes/` | recorte |
|---------------------|------:|------:|--------:|-----------------------|---------|
| `logo-integria.png` |139×60 |157×68 | 314×136 | `logos-28.png`        | 38,47 548×238 |
| `badge-gptw.png`    | 48×81 | 54×92 | 108×184 | `GPTW-28.png`         | 15,0 154×261 |
| `icon-flecha.png`   | 35×29 | 39×33 |   78×66 | `icono flecha-23.png` | — |
| `icon-email.png`    | 11×11 | 13×13 |   26×26 | `firmas-26.png`       | — |
| `icon-telefono.png` | 11×11 | 13×13 |   26×26 | `firmas-25.png`       | — |
| `icon-direccion.png`| 10×13 | 11×14 |   22×28 | `firmas-23.png`       | — |
| `icon-web.png`      | 11×11 | 13×13 |   26×26 | `firmas-24.png`       | — |
| `icon-instagram.png`| 11×11 | 13×13 |   26×26 | `firmas-27.png`       | — |
| `icon-linkedin.png` | 10×10 | 11×11 |   22×22 | `firmas-28.png`       | — |

El recorte quita el margen transparente del original. El badge de Great Place to
Work tiene proporción 0.59 (alto/ancho): forzarlo a otra proporción lo deja
achatado.

## Importante

- **Deployment Protection debe estar desactivado** en Vercel (Settings → Deployment Protection), o las imágenes devuelven 401 desde el correo.
- **Las URLs son permanentes**: cada correo enviado queda apuntando a ellas para siempre. Conviene usar un dominio propio (`firma.integria.pro`) en lugar de `*.vercel.app`, para poder mover el hosting sin romper el histórico.
- El badge lleva la vigencia `JUN 2026-JUN 2027` dentro del PNG: hay que reemplazar `firma/badge-gptw.png` cada año.

## Estado actual

Última tanda de cambios pedidos por el cliente, ya aplicada:

1. **Tamaño** — la firma se achicó al 75%: de 679 px a 509 px de ancho. Todos los
   textos, iconos y espaciados se escalaron proporcionalmente (nombre 17→13 px,
   datos de contacto 13→10 px). Se armó además una variante al 85% (577 px) para
   que la diseñadora elija; ver "Decisión de tamaño pendiente" más arriba.
2. **Resolución** — los PNG se regeneraron a 2x desde los originales de
   `imagenes/` (ver tabla arriba). De paso se corrigió el badge de Great Place to
   Work, que estaba deformado: se mostraba a 72×96 (proporción 0.75) cuando el
   original es 0.59.
3. **Texto** — donde decía `INTEGRIA` en naranja ahora dice `INTEGRIA CONSULTING`.
   Como el texto es más largo, el salto de línea de la dirección se movió: antes
   cortaba después de `CABA`, ahora corta después de `Juana Azurduy 2440`.
4. **Dirección clickeable** — el pin y el texto de la dirección abren Google Maps
   (`https://www.google.com/maps/search/?api=1&query=...`).

### Pendiente

- Falta deployar a Vercel para que las imágenes nuevas queden publicadas. Hasta
  entonces, los correos ya enviados siguen apuntando a los PNG viejos (mismos
  nombres de archivo, así que el deploy los actualiza a todos).
- Sigue vigente lo del dominio propio (ver "Importante").
- Probar en Outlook de escritorio: es el que peor renderiza y conviene confirmar
  que el link de la dirección no se rompa y que no aparezcan líneas de 1px.
