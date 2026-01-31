## Práctica: Transmisión de video

Para empezar, ya que esto es una práctica realizada con VM's dejo las especificaciones utilizadas en mi caso

| CPU | RAM | Almacenamiento | ISO |
|-----|-----|----------------|-----|
|3 nucleos (Hilos) | 4GB | 60GB | ubuntu-24.04.3-desktop-amd64 |

Ahora instalamos **FFmpeg**

```bash
    sudo apt install ffmpeg
```

Ya instalado podemos instalar el [**video**](big-buck-bunny-1080p-30sec.mp4) que se encuentra en este repositorio que sera utilizado para hacer las pruebas.

Ahora podemos realizar una revisión de los datos del video con el siguiente comando:
```bash
    ffprobe -v error -show_streams fichero.mp4
```

Con la salida que nos da el comando anterior podemos ver algunos de los siguientes datos del video:
```bash
- width=1920
- height=1080
- duration=30.000000
- avg_frame_rate=24/1       #frame/segundos = FPS
- codec_name=H264           #Compresión
- bits_per_raw_sample=8     #Esta es la profundidad de color en bits
```

Ahora haremos un cambio de contenedor con lo siguiente:
```bash
ffmpeg -i original.mp4 -c:v copy -c:a copy salida.mkv
```

Al hacer esto obtendremos un archivo `.mkv` que tiene un peso un poco menor al original que **no llega a ser significativo**, ademas de esto vamos que **sube muy poco el consumo de la CPU y es rapido** (ya que no es una alta compresión).

Ahora haremos cambios de códecs y comparación.

Crear el fichero en H.264 con un bitrate de 2Mbps:
```bash
ffmpeg -i original.mp4 -c:v libx264 -b:v 2M -c:a copy
h264_2mbps.mp4
```
>[!Note]
> Este caso si hara una exigencia importante para la CPU y un poco más de tiempo.

Ahora hay que hacerlo en H.265 con el mismo bitrate:
```bash
ffmpeg -i original.mp4 -c:v libx265 -b:v 2M -c:a copy
h265_2mbps.mp4
```
>[!Note]
> Este caso también hara una exigencia importante para la CPU y un poco más de tiempo.

Ahora para hacer comparaciones de rendimiento reproducimos los videos y pausamos en cualquier escena con mucho movimiento al hacer esto veremos que el archivo `h264_2mbps.mp4` Tiene más artifacts (pixelado), ademas si revisamos sus pesos veremos que `h264_2mbps.mp4` pesa un poco menos.

![Imagen de la comparación](Imagen_Video.png)

> Esto se debe a que el `h264_2mbps.mp4` es más comprimido generando más fallos visuales pero tiene un menor peso.

## Simulación de streaming con diferentes tipos de fichero.
1. Low (móvil):
    - Resolución 240p
    - Bitrate: 400k
2. High (fibra):
    - Resolución: 1080p
    - Bitrate: 2Mbps

Preguntas finales:
1. Almacenamiento: Si tu servidor tiene un disco de 500 GB, ¿cuántas horas de vídeo del perfil "HD" (2 Mbps) podrías alojar?
    
    **Espacio disponible:** 500 * 8 = 4.000Gb = 4.000.000Mb
    > GB a Gb (bits a bytes)

    **Horas que se puede grabar:** (4.000.000 / 2) / 3.600 = 555,5 Horas
    > (Mb / Mbps) / hora en segundos 

3. Red: Tienes una línea de 100 Mbps simétricos. ¿Cuántos usuarios podrían ver el perfil "Móvil" (400 kbps) simultáneamente antes de saturar el 80% de la línea?

    **80% de la línea:** 80Mbps = 80.000kbps
    > Mb a Kb 

    **Usuarios antes de saturar el 80%:** 80.000 / 400 = 200 Usuarios
    > Ancho de banda / consumo por móvil
