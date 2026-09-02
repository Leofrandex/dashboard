# Señal y Silencio — Reporte de conversaciones

Tablero del embudo conversacional de la subcuenta de GoHighLevel de Bernard Kizer
(`qv9rF8ulG9G4xN53nl3v`), reconstruido hilo por hilo desde la API REST v2 de LeadConnector.

**Ventana de datos: 19/08/2026 – 02/09/2026 (14 días).** Las cifras están congeladas en el
HTML; el dashboard no consulta la API en tiempo de ejecución.

## Cómo se obtuvieron los datos

Se paginaron las conversaciones de la subcuenta por fecha de creación y se leyeron los
**6.270 hilos** abiertos dentro de la ventana, mensaje por mensaje. Cada latencia se calcula
comparando el sello de tiempo de un mensaje con el del anterior en el mismo hilo — no son
estimaciones.

La **fuente de tráfico** se deduce de la firma del primer mensaje del lead. Los 15 trigger
links de la subcuenta redirigen a `wa.me` con un texto pre-cargado distinto cada uno, así que
el origen queda escrito en el propio mensaje. Lo que no llega por trigger link se clasifica
por canal, que dice por dónde entró pero no qué lo motivó.

## Qué mide

| Bloque | Contenido |
|---|---|
| Conversaciones | Abiertas, respondidas y continuadas (≥4, ≥6, ≥20 mensajes), timeline diaria apilada por canal, canal de entrada y número de entrada |
| Fuentes de tráfico | Conversaciones, profundidad, tasa de respuesta, calificadas y agendadas por cada fuente |
| Calificación | Embudo acumulado del agente de IA por etiqueta |
| Ritmo | Latencia del lead para contestar la primera vez y la nuestra para responderle |
| Cuándo | Hora y día de apertura, con la porción que recibió respuesta |
| Campañas y citas | Contactos por campaña saliente y agenda por closer |

## Hallazgos principales

- **Se abren 448 conversaciones por día**, pero solo el **27,9 % llega a cuatro mensajes**.
  Ahí se separa el saludo de la conversación real.
- **Instagram es el canal principal con el 51 %** del volumen, por encima de WhatsApp (26 %).
- **La atribución cubre el 2,4 %**: solo 153 de 6.270 conversaciones traen firma de trigger
  link. El resto se clasifica apenas por canal.
- **Esa minoría se comporta de otra manera**: el 98 % de las conversaciones con firma llega a
  cuatro mensajes, contra el 26 % del resto.
- **La campaña saliente trae 10 veces más volumen y una décima parte de la profundidad**
  (11,7 % llega a cuatro mensajes, contra 98 % de los trigger links de Instagram).
- **IG Orgánico son 3.189 conversaciones y cero agendas.** O no convierte, o el flujo del bot
  no corre sobre Instagram y por eso no se etiqueta. Hay que distinguir cuál de las dos.

## Despliegue

Sitio estático sin build. `vercel.json` activa `cleanUrls` y mantiene viva la ruta
`/dashboard-whatsapp-metricas`. No hay comando de build, dependencias ni variables de entorno.

El HTML es autocontenido — la única petición externa es a Google Fonts. Funciona en tema claro
y oscuro según la preferencia del navegador.

## Datos personales

Esta versión **no publica nombres, teléfonos ni correos de leads**: todas las cifras son
agregadas. La tabla «Las agendas, una por una» de la versión anterior, que sí los incluía, fue
retirada. Las cabeceras mantienen `X-Robots-Tag: noindex, nofollow`.

## Regenerar con datos frescos

El HTML se compila desde una plantilla con el marcador `/*__DATA__*/`, que se sustituye por el
JSON de métricas. El pipeline vive en el proyecto `smg`:

```bash
python scripts/etl.py 14     # extrae de la API v2 -> data.json  (~13 min)
python scripts/build.py      # inyecta en la plantilla -> dashboard-whatsapp-metricas.html
```

Luego se copia el resultado a este repositorio como `index.html`.
