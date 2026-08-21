# Señal y Silencio — Dashboard de métricas de WhatsApp

Tablero interactivo del canal de WhatsApp de la subcuenta de GoHighLevel de Bernard Kizer
(`qv9rF8ulG9G4xN53nl3v`), reconstruido mensaje por mensaje desde la API REST v2 de
LeadConnector.

**Ventana de datos: 22/07/2026 – 21/08/2026.** Las cifras están congeladas en el HTML;
el dashboard no consulta la API en tiempo de ejecución.

## Cómo se obtuvieron los datos

Se barrieron las 18.593 conversaciones de la subcuenta, se aislaron las 795 con actividad
de WhatsApp o tag de campaña `[whatsapp] - campana-*`, y se leyeron sus 3.522 mensajes uno
a uno. Cada tiempo de respuesta se calculó comparando el sello de tiempo de un mensaje con
el del mensaje inmediatamente anterior en el hilo — no son estimaciones.

## Qué mide

| Bloque | Contenido |
|---|---|
| Envío y entrega | Enviados, entregados, leídos y fallidos, con embudo en contactos y volumen diario |
| Tiempo de respuesta | Primera respuesta al mensaje frío **vs.** respuesta dentro de la conversación (son métricas distintas), más el tiempo de reacción del bot |
| Conversación | Promedio y mediana de mensajes, embudo de profundidad, toques hasta la primera respuesta |
| Agendamiento | Tiempo hasta la cita y atribución real al bot |
| Países | Tasa de respuesta y tasa de fallo por prefijo E.164 |
| Campañas | Rendimiento por tag, con los leads entrantes separados |
| Timing | Hora y día de disparo |
| Recomendación | Ocho métricas adicionales que el dataset ya soporta |

## Hallazgos principales

- **8 % de los envíos falla** (129 de 1.607), concentrado en Brasil (73,9 %) y Chile (64,7 %).
- **La campaña saliente convierte al 7,6 %**; el lead entrante responde al 100 %.
- **La demora es solo del primer contacto**: mediana de 23,2 min para la primera respuesta,
  pero 1,6 min una vez que la conversación está viva.
- **Solo 2 de las 7 citas las cerró el bot**; las otras 5 ya existían antes del primer
  mensaje de WhatsApp.

## Despliegue

Sitio estático sin build. `vercel.json` activa `cleanUrls` y mantiene viva la ruta
`/dashboard-whatsapp-metricas`. En Vercel basta importar el repo y desplegar: no hay
comando de build, dependencias ni variables de entorno.

El HTML es autocontenido — la única petición externa es a Google Fonts.
Funciona en tema claro y oscuro según la preferencia del navegador.

## Aviso sobre datos personales

La tabla «Las agendas, una por una» contiene **nombres reales de leads**. El repositorio es
público. Las cabeceras incluyen `X-Robots-Tag: noindex, nofollow` para desalentar la
indexación, pero eso no restringe el acceso: cualquiera con la URL puede leerlo. Si el
tablero va a circular fuera del equipo, conviene anonimizar esa tabla antes.

## Regenerar con datos frescos

El HTML se compila desde una plantilla con marcador `/*__DATA__*/` que se sustituye por el
JSON de métricas. La plantilla y el pipeline de extracción viven en el proyecto `smg`, no en
este repositorio.
