# Demo 01 · Sofi — Hospital San Rafael del Norte

Demo ilustrativa (A1 del anexo): asistente de WhatsApp que entiende notas de
voz, clasifica urgencia, agenda en 4 áreas y recuerda la cita. Todo es ficticio
y corre en el navegador, sin backend ni cuentas.

**Abrir:** doble clic en `index.html`. El escudo, el avatar de Sofi y el favicon van incrustados en el HTML (base64), así que el archivo funciona solo, también si se envía al celular sin la carpeta `assets/`.

## Publicada en la web

Copia lista para Netlify en `public/demos/hospital/` → **https://novalicolombia.com.co/demos/hospital/**.
Esta carpeta (`proyectos/01-bot-hospital/`) es la fuente: si cambias algo aquí, vuelve a copiar
`index.html` y `assets/` a `public/demos/hospital/`.

| Archivo | Uso |
|---|---|
| `assets/escudo.webp` | escudo completo en la portada (recortado del `.jfif` original, 62 KB) |
| `assets/escudo-icono.webp` | avatar de WhatsApp y del panel de recepción |
| `assets/favicon.png` | ícono de la pestaña |
| `assets/og.jpg` | vista previa al compartir el enlace (WhatsApp, LinkedIn) |

Todo cumple la CSP de `netlify.toml`: imágenes propias, fuentes de Google y JS en línea.

## Qué muestra

| Pieza | Dónde está en `index.html` |
|---|---|
| Datos del hospital y las 4 áreas (médico, horario, cupo, precio) | constantes `H` y `AREAS` |
| Casos para el video y la llamada | constante `SCENARIOS` (tarjetas: `CASE_ICO`, `playCase()`) |
| Línea de proceso Escucha → Recuerda del panel | `setStage()` |
| Comprensión del mensaje (área, nombre, EPS, fiebre, edad, 1–10) | `understand()` |
| Qué responde Sofi | `reply()` |
| Cupos, recordatorios y resumen para recepción | `book()`, `forwardReminder()` |

## Reglas de negocio

- Urgencia solo en **Medicina general** y **Pediatría**. Es *prioritaria* si hay
  fiebre ≥ 38,5, malestar ≥ 7/10 o expresiones como "no se le baja" o
  "no he dormido". Esas dos áreas reservan su primer cupo del día para estos casos.
- Señales de alarma (dolor en el pecho, dificultad para respirar, convulsión,
  bebé menor de 3 meses con fiebre, etc.): **no agenda**, remite a urgencias
  y pasa el caso a una persona.
- Nunca recomienda medicamentos ni dosis.
- Recordatorios: el día anterior a las 6:00 p.m. (si aún no pasó) y 1 hora antes.
- Agenda con al menos 1 hora de anticipación.

## Para pasarla a un cliente real

En la demo, `understand()` y `reply()` son reglas simples. En producción ese
trabajo lo hacen WhatsApp Cloud API, n8n, Whisper para el audio, Claude para
entender y responder, y Google Calendar para la agenda. Lo que se reutiliza tal
cual son las reglas de negocio de arriba y la tabla `AREAS`.
