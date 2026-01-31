# Práctica: Radio online

Para empezar, ya que esto es una práctica realizada con VM's dejo las especificaciones utilizadas en mi caso

| CPU | RAM | Almacenamiento | ISO | Nº de VM's |
|-----|-----|----------------|-----|------------|
|3 nucleos (Hilos) | 4GB | 60GB | ubuntu-24.04.3-desktop-amd64 | 2 |

> [!Important]
> Una VM hara de servidor de **streaming** y la otra de **DJ** así me referire a cada una

Cuando esten instaladas las maquinas podremos ver la IP asignada por DHCP
```bash
    ip a
```
Esta IP la utilizaremos para establecer la IP estática de cada VM en la configuración del cableado asignamos la IPv4 como manual y colocamos la IP, su mascara y puerta de enlace (Gateway)

> [!Note]
> Ya que yo he utilizado "Red NAT" para la práctica mis IP`s son de la red 10.0.2.0/24 (red establecida por mi en VirtualBox)

**Una vez instaladas y configuradas las máquinas podemos proceder a instalar los software necesarios y útiles**

## Softwares a utilizar

| Nombre | Necesario/Útil | Descripción | Comentario | VM |
|--------|----------------|-------------|------------|----|
| OpenSSH Server | Útil |  Sistema que permite acceso remoto seguro al CLI de otro ordenador a través de una red | Esta es útil ya que nos permite un acceso mas directo al CLI de las VM's | Ambas |
| Icecast2 | Necesario | Servidor de transmisión de medios (streaming) de código abierto | Lo utilizaremos para transmitir nuestra "estación de radio" | Streaming |
| Mixxx | Necesario | Software de DJ gratuito y de código abierto para mezclar música | Sera usado para reproducir la música a transmitir | DJ |

## Instalaciones

 1. OpenSSH Server

    ```bash
        sudo apt update
        sudo apt install openssh-server -y     #Tambien puede ser acortar a "sudo apt install ssh -y"
    ```
> [!Important]
> Recordar que no es necesario para la práctica y puede instalarse en ambas VM's

 2. Icecast2

    ```bash
        sudo apt update
        sudo apt install icecast2 -y
    ```
    Al momento de ejecutar el comando de instalación este nos preguntara si queremos configurar la contraseña de Icecast2 la cual es necesaria para el funcionamiento de Icecast2 por esto seleccionaremos que si

    Una vez establecemos los datos solicitados por Icecast2 (dominio, clave y clave de admin), ya podremos acceder por el navegador al puerto 8000 Utilizando las siguientes URL's

    | Local | En la red |
    |-------|-----------|
    | http://localhost:8000 | http://IP:8000 |

    Ahora podremos acceder al panel de administrador en caso de querer administrar los emisores

> [!Important]
> Solo en la VM que hara de servidor de streaming

 2. Mixxx

    ```bash
        sudo apt update
        sudo apt install mixxx -y
    ```
    > [!Important]
    > Solo en la VM que hara de DJ

    Una vez instalado podremos acceder a Mixxx como una aplicación común (gráficamente o por su nombre en el CLI)

    Ya en mixxx nos preguntara por una carpeta donde tendras las musicas y pasaremos a configurar en la pestaña ``Opciones`` apartado ``Preferencias``

    Ahora nos dirigiremos a ``Emisión en vivo`` donde configuraremos lo siguiente:

    |Tipo|Montar|Servidor|Puerto|Identificación|Contraseña|
    |----|------|--------|------|--------------|----------|
    | Icecast2 | /Nombre | IP (VM streaming) | 8000 (O personalizado) | source | La asignada al instalar Icecast2 |

    Una vez establecidos los valores solo aplicamos y volvemos a la consola de Mixxx

    Ahora solo debemos Pulsar ``ON AIR`` colocar una canción instalada Y acceder desde el navegador al servidor de streaming con la ruta de montado (``/Nombre``)

    | Local | En la red |
    |-------|-----------|
    | http://localhost:8000/Nombre | http://IP:8000/Nombre |

    Dejo un [video](Streaming%20de%20radio.mp4) de como debe quedar.

> [!Note]
> Deberiamos poder escuchar la emisora desde cualquiera de las dos VM's
