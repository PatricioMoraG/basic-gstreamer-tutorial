# Cheat Sheet de GStreamer

## 1. ¿Qué es GStreamer?
GStreamer es un framework multimedia de código abierto que permite crear aplicaciones capaces de procesar audio, video y otros tipos de medios. Utiliza una arquitectura de pipeline (tubería) compuesta por elementos que realizan tareas específicas.

## 2. Instalación
En Linux (Debian/Ubuntu)
```
sudo apt-get update
sudo apt-get install gstreamer1.0-tools gstreamer1.0-plugins-{base,good,bad,ugly} 
```

En Windows
Descarga los instaladores desde el sitio oficial de GStreamer.

## 3. Componentes Principales
- Pipeline: La tubería completa que conecta todos los elementos.
- Elementos: Bloques individuales que realizan operaciones (ej. decodificación, filtrado).
- Pads: Puntos de conexión entre elementos (src - salida, sink - entrada).
- Bins: Contenedores para agrupar múltiples elementos.

## 4. Comandos Básicos

### gst-launch

Permite construir y ejecutar pipelines desde la línea de comandos.

Sintaxis Básica:

```
gst-launch-1.0 [elemento1] ! [elemento2] ! ... ! [elementoN]
```
Ejemplos:

Reproducir un archivo de video

```
gst-launch-1.0 filesrc location=video.mp4 ! decodebin ! autovideosink
```
Reproducir un archivo de audio

```
gst-launch-1.0 filesrc location=audio.mp3 ! decodebin ! autoaudiosink
```
Capturar video desde la webcam y mostrarlo

```
gst-launch-1.0 v4l2src ! videoconvert ! autovideosink
```

Transmisión de video en RTSP

```
gst-launch-1.0 -v v4l2src ! videoconvert ! x264enc ! rtph264pay ! udpsink host=127.0.0.1 port=5000
```

### gst-inspect

Muestra información sobre elementos y plugins disponibles.

Ejemplos:

Listar todos los elementos disponibles

bash
Copiar código
gst-inspect-1.0
Obtener información detallada de un elemento específico

bash
Copiar código
gst-inspect-1.0 decodebin
5. Elementos Comunes
Sources (Fuentes)

filesrc: Lee datos desde un archivo.
v4l2src: Captura video desde dispositivos V4L2 (webcams).
Sinks (Sumideros)

autovideosink: Reproduce video utilizando el sumidero adecuado.
autoaudiosink: Reproduce audio utilizando el sumidero adecuado.
Filters (Filtros)

videoconvert: Convierte formatos de video.
audioconvert: Convierte formatos de audio.
capsfilter: Filtra flujos según capacidades especificadas.
Encoders/Decoders

x264enc: Codifica video en formato H.264.
decodebin: Detecta y decodifica automáticamente flujos multimedia.
6. Caps (Capacidades)
Definen el tipo de datos que fluyen a través de los pads.

Ejemplo de uso de capsfilter:

bash
Copiar código
gst-launch-1.0 filesrc location=video.mp4 ! decodebin ! videoconvert ! video/x-raw, width=640, height=480 ! autovideosink
7. Ejemplos de Pipelines Comunes
Grabar video desde la webcam y guardarlo en un archivo

bash
Copiar código
gst-launch-1.0 v4l2src ! videoconvert ! x264enc ! mp4mux ! filesink location=output.mp4
Reproducir video con escala de grises

bash
Copiar código
gst-launch-1.0 filesrc location=video.mp4 ! decodebin ! videoconvert ! videobalance saturation=0 ! autovideosink
Transmitir audio en vivo a través de la red

bash
Copiar código
gst-launch-1.0 pulsesrc ! audioconvert ! lamemp3enc ! shout2send ip=192.168.1.100 port=8000 mount=/stream
8. Depuración y Logs
Para obtener información detallada durante la ejecución de un pipeline:

bash
Copiar código
GST_DEBUG=3 gst-launch-1.0 [pipeline]
Niveles de depuración:

0: Error
1: Warning
2: Info
3: Debug
4: Log
5: Trace
9. Desarrollo con GStreamer
Para integrar GStreamer en aplicaciones, puedes utilizar bindings para diferentes lenguajes de programación, como C, Python, y otros.

Ejemplo en Python usando gi (GObject Introspection):

python
Copiar código
import gi
gi.require_version('Gst', '1.0')
from gi.repository import Gst

Gst.init(None)

pipeline = Gst.parse_launch("filesrc location=video.mp4 ! decodebin ! autovideosink")
pipeline.set_state(Gst.State.PLAYING)

bus = pipeline.get_bus()
msg = bus.timed_pop_filtered(Gst.CLOCK_TIME_NONE, Gst.MessageType.ERROR | Gst.MessageType.EOS)

pipeline.set_state(Gst.State.NULL)
10. Recursos Adicionales
Documentación Oficial: https://gstreamer.freedesktop.org/documentation/
Tutoriales: GStreamer Tutorials
Repositorio GitHub: GStreamer GitLab
Consejos Útiles
Explora los Plugins Disponibles: Utiliza gst-inspect-1.0 para descubrir qué plugins y elementos están disponibles en tu instalación.
Utiliza gst-launch-1.0 para Prototipar: Antes de implementar en código, prueba tus pipelines en la línea de comandos.
Mantén Actualizados los Plugins: Asegúrate de tener instalados los paquetes de plugins "good", "bad", y "ugly" para acceder a una amplia gama de funcionalidades.
Consulta la Comunidad: Participa en foros y listas de correo de GStreamer para resolver dudas y aprender de otros usuarios.
