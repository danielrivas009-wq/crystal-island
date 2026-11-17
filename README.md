# Dynamic Island para Windows

Una "isla dinámica" estilo iPhone que vive en la parte superior central de tu pantalla de Windows.
Contraída muestra **batería · hora · ondas de audio**; al pasar el mouse se expande y tiene 4 páginas.
No necesita cuenta, contraseña ni conexión a ningún servicio propio: todo se guarda en tu equipo.

| # | Página | Qué hace |
|---|--------|----------|
| 1 | **Música** | Carátula, título, artista, barra de progreso (clic para adelantar), anterior / play-pausa / siguiente, **volumen del sistema** y **letra sincronizada**. Funciona con Spotify, YouTube en Chrome/Edge, Apple Music, VLC, etc. |
| 2 | **Recordatorio de agua** | Meta diaria, botón *Beber*, medidor circular, recordatorios cada X minutos y ajustes (meta, tamaño de vaso, intervalo). |
| 3 | **Rendimiento del equipo** | Cuatro medidores en vivo: **CPU** (uso e hilos), **RAM** (usada / total), **Disco** (espacio usado del disco del sistema y su actividad) y **GPU** (uso y memoria de la tarjeta gráfica; si hay dos, muestra la más ocupada). El anillo pasa a ámbar desde el 80 % y a rojo desde el 92 %. Pasa el mouse sobre una tarjeta para ver el nombre completo del procesador o de la tarjeta. Sólo mide mientras la página está a la vista, así que no gasta nada con la isla en reposo. |
| 4 | **Enfoque** | Temporizador Pomodoro: 25 min de enfoque, 5 de descanso y un descanso largo de 15 cada 4 sesiones (todo ajustable). La **cuenta regresiva se ve en la isla contraída** y al terminar cada etapa avisa con destello, sonido, una tarjeta bajo la isla y una notificación de Windows. Cuenta las sesiones del día. |

## 1. Requisitos

- **Windows 10 (1809+) u 11**, 64 bits.
- Para usar el instalador no hace falta nada más. Para desarrollar: **Node.js 20 o superior**.

## 2. Ejecutar en modo desarrollo

```powershell
npm install
npm start
```

`npm run dev` hace lo mismo pero abre las DevTools.

## 3. Crear el instalador `.exe`

```powershell
npm run dist
```

En `dist/` aparece `Dynamic Island Setup 1.1.1.exe` (antes recalcula `src/main/script-hashes.json`, ver *Seguridad*). Se instala por usuario (no pide administrador), deja acceso
directo en el escritorio y en el menú Inicio, y abre la app al terminar. `npm run dist:portable` crea una versión
que no se instala.

**Actualizar.** Antes de compilar sube `version` en `package.json` (la 1.1.0 trae el panel de rendimiento y quita las
tareas; la 1.1.1 endurece la seguridad y actualiza Electron). Quien ya tenga la app instalada sólo ejecuta el instalador nuevo: se instala encima (mismo `appId`), conserva
sus datos y la app se reinicia con la versión nueva. Cierra antes la isla (bandeja → *Salir*) si el instalador lo pide.

> Windows SmartScreen puede advertir porque el ejecutable no está firmado: *Más información → Ejecutar de todas formas*.

**Si falla con `Cannot create symbolic link`**: activa *Configuración → Sistema → Para programadores → Modo de
programador* y vuelve a compilar; o copia la carpeta ya extraída a
`%LOCALAPPDATA%\electron-builder\Cache\winCodeSign\winCodeSign-2.6.0` y reintenta.

## 4. Uso

- **Pasa el mouse** sobre la muesca para expandir; sal de ella para contraer.
- Cambia de página con las **flechas**, los **puntos**, la **rueda del mouse** o **← →** del teclado. `Esc` contrae.
- **Arrastra la isla** por su barra superior para moverla a otra pantalla; recuerda dónde la dejaste.
- Icono en la **bandeja del sistema**: mostrar/ocultar, abrir una página, *Iniciar con Windows*, *Ocultar en pantalla completa*, salir.
- Atajo global: **Ctrl + Alt + I** muestra/oculta la isla.

## 5. Datos y privacidad

Todo queda en `%APPDATA%\Dynamic Island\state.json` de tu equipo: agua, enfoque, ajustes y la pantalla elegida.
La app sólo sale a internet para buscar la **letra** de la canción que suena (página Música). El panel de rendimiento
lee los contadores de Windows de tu propio equipo y no envía nada. Si venías de la 1.0, las tareas que escribiste
siguen guardadas en ese archivo (clave `tasks`) aunque la página ya no exista.

## 6. Personalizar

| Quiero… | Dónde |
|---------|-------|
| Cambiar el tamaño de la isla, tiempos de hover o el atajo | `src/main/config.js` |
| Forzar la hora de 24 h o de 12 h (por defecto sigue a Windows) | `clock24h` en `config.js` |
| Forzar el idioma (por defecto el de Windows; hay español e inglés) | `language` en `config.js`: `'auto'`, `'es'` o `'en'` |
| Apagar el sonido de los avisos | `notificationSound` en `config.js` |
| Cambiar un texto o agregar un idioma | `src/main/messages.js`; luego `npm run check:i18n` |
| Duraciones del temporizador | en la propia página Enfoque (botón de ajustes) |
| Cada cuánto se actualiza el panel de rendimiento | `INTERVAL_MS` en `src/main/perf.js` (1500 ms) |
| Colores | variables `:root` al inicio de `src/renderer/styles.css` |

## 7. Estructura

```
src/main/        proceso principal: ventana, bandeja, puente con Windows (bridge.ps1), rendimiento (perf.js + perf.ps1), letras, idioma, seguridad (security.js)
src/preload.js   lo único que la interfaz puede usar del sistema (window.island)
src/renderer/    interfaz: app.js (isla y navegación), pages/ (media, water, perf, focus), styles.css
assets/          iconos de la app y de la bandeja
scripts/         check-i18n.js, gen-hashes.js (hashes de los .ps1) y security-test.js (prueba de seguridad)
```

## 8. Seguridad

Se prueba con `npm run test:security` (20 pruebas que atacan la isla por la interfaz y por el proceso principal; salen
PASS/FAIL y código 1 si algo falla) y con `npm audit --omit=dev` (0 vulnerabilidades: la app no lleva dependencias en ejecución).
Repite ambas antes de cada versión.

| Protección | Cómo |
|------------|------|
| Electron al día | 45.x (Chromium 156). `MIN_ELECTRON` en `scripts/security-test.js` avisa cuando una versión pierde soporte. |
| Interfaz aislada | Sandbox, `contextIsolation`, sin Node, sin `<webview>`, sin DevTools en la app instalada; no puede navegar ni abrir ventanas. |
| Sin permisos ni red desde la interfaz | CSP estricta, peticiones http(s) bloqueadas y permisos web denegados (cámara, micrófono, ubicación…). |
| IPC validado | `src/main/security.js`: sólo se acepta al marco de la isla; claves, tipos y tamaños limitados; los comandos de música salen de una lista fija. |
| Datos externos | Los títulos y la carátula que llegan de Windows se validan y acotan. Las letras sólo se piden a lrclib.net (8 s y 2 MB como máximo). |
| PowerShell | Se lanza por ruta absoluta y, con la app instalada, sólo si el SHA-256 del script coincide con el empaquetado (`script-hashes.json`). |
| Ejecutable | Fusibles de Electron: sin `ELECTRON_RUN_AS_NODE`, sin `--inspect` y con comprobación de integridad de `app.asar`. |

Límites conocidos:

- El instalador **no está firmado** (SmartScreen avisa). Firmarlo requiere un certificado de firma de código.
- `npm audit` completo marca avisos en las herramientas de empaquetado (picomatch y minimatch, sin parche publicado); no se instalan con la app.
- Se protege de contenido y datos externos, no de otro programa que ya corra con tu usuario en tu PC.
- La interfaz se carga desde `file://` (necesario para los módulos ES).
- Si editas `bridge.ps1` o `perf.ps1`, recompila con `npm run dist` (regenera los hashes); si no, la app instalada no los ejecuta.
