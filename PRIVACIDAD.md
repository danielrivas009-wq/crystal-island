# Política de privacidad — Dynamic Island para Windows

Versión de la app: 1.1.1 · Última actualización: 17 de noviembre de 2026
Responsable: [NOMBRE DE LA EMPRESA O PERSONA] · Contacto: [CORREO DE CONTACTO]

## En resumen

- La app **no pide cuenta, no recopila datos personales y no tiene publicidad, analítica ni telemetría**.
- **Todo se guarda en tu equipo.** Nosotros no recibimos nada y no tenemos servidores con datos tuyos.
- Lo único que sale a internet es la búsqueda de la **letra** de la canción, y **sólo si tú pulsas el botón de letras**.

## Qué datos usa la app y para qué

| Dato | De dónde sale | Para qué | ¿Se guarda? | ¿Sale de tu equipo? |
|------|---------------|----------|-------------|---------------------|
| Lo que suena (título, artista, álbum, aplicación, carátula, posición) | Windows | Mostrarlo en la isla | No (sólo en memoria) | Sólo título, artista y duración, si pulsas el botón de letras |
| Volumen y si hay una app a pantalla completa | Windows | Controlar el volumen y ocultar la isla | No | No |
| Uso de CPU, RAM, disco y tarjeta gráfica (y su nombre) | Contadores de Windows | Panel de rendimiento, sólo mientras esa página está a la vista | No | No |
| Nivel de batería, hora y formato regional | Windows | Mostrar la batería y la hora como en tu reloj | No | No |
| Agua bebida del día, sesiones de enfoque, ajustes, última página y pantalla elegida | Tú, al usar la app | Que la app recuerde tu estado | Sí, en tu equipo | No |

Los avisos del agua y del enfoque son notificaciones normales de Windows, generadas en tu equipo.

## Qué sale de tu equipo

Al pulsar el botón de letras, la app consulta el servicio gratuito **LRCLIB** (`https://lrclib.net`) por una conexión
segura (HTTPS) y le envía el **título, el artista y la duración** de la canción (el título se limpia de textos como
"Official Video"). Como en cualquier conexión a internet, ese servicio ve también tu dirección IP. No se envía ninguna
cuenta ni identificador de tu equipo.

LRCLIB es un servicio de terceros con sus propias condiciones y su propia política de conservación, que no controlamos.
Si no quieres esa consulta, no uses el botón de letras: sin él, la app no se conecta a internet.

La app tampoco comprueba actualizaciones por su cuenta: para actualizar se instala el instalador nuevo.

## Cuánto tiempo se conservan los datos

| Dato | Dónde | Cuánto tiempo |
|------|-------|---------------|
| Agua y enfoque | `%APPDATA%\Dynamic Island\state.json` | Sólo el día en curso: al empezar otro día se reinicia el contador y el registro anterior se sustituye. |
| Ajustes (meta de agua, duraciones de enfoque, última página, pantalla elegida, ocultar en pantalla completa) | `state.json` | Hasta que borres esa carpeta. |
| Tareas de la versión 1.0 (la función ya no existe) | `state.json` | Se quedan guardadas, sin mostrarse, hasta que borres la carpeta. |
| Música y rendimiento | Memoria del equipo | Sólo mientras la app está abierta; al cerrarla se descartan. |
| Letras consultadas | Memoria del equipo (hasta 100 canciones) | Hasta cerrar la app. |
| Datos enviados a LRCLIB | Servidor de LRCLIB | Según la política de LRCLIB. |

**Desinstalar la app no borra `state.json`** (así no pierdes tus ajustes si la reinstalas).

## Tu control

- **Borrar todo:** cierra la isla (bandeja → *Salir*), desinstálala y elimina la carpeta `%APPDATA%\Dynamic Island`.
- **No enviar nada a internet:** no pulses el botón de letras.
- **Inicio con Windows:** es opcional (bandeja → *Iniciar con Windows*); al activarlo, Windows guarda un elemento de inicio, que se quita desde el mismo menú.

## No compartimos ni vendemos datos

No hay terceros con acceso a tus datos, salvo LRCLIB en la consulta de letras descrita arriba. La app no está dirigida a
menores y no recopila datos de nadie.

## Seguridad

La interfaz va aislada del sistema (sandbox), no puede salir a internet por su cuenta, no recibe permisos como cámara o
micrófono, y los datos que llegan de Windows o de internet se validan antes de usarse. El detalle técnico está en la
sección *Seguridad* del [README](README.md).

## Cambios en esta política

Si una versión nueva cambia qué datos usa la app, esta política se actualizará antes y se indicará la fecha arriba.

## Contacto

Para cualquier duda sobre privacidad escribe a [CORREO DE CONTACTO].
