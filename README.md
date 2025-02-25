# grml-live-comp

`grml-live-comp` es `grml-live`, pero con las clases (mini)FAI y otros ajustes para obtener un sistema especializado para competencias de programación.

`grml-live` es el sistema de creación de imágenes vivas usado por `grml` (basado en Debian, con Systemd).

## Modo de uso

Ejecute como usuario root el archivo `./run`:

``` bash
# ./run
```

## Dependencias

### Para crear una imagen
 + Sistema Linux basado en Debian/Ubuntu de 64 bits.
 + 10 Gb de almacenamiento para los archivos de la imagen
 + Paquetes: `mmdebstrap socat dosfstools mtools squashfs-tools xorriso debian-archive-keyring`

### Para ejecutar el sistema
 + Mínimo de memoria RAM: 4 Gb (teórico), en práctica, probablemente 6 Gb.
 + Procesador de 64 bits
 + Memoria USB/Pendrive con capacidad mínima de 2 Gb (se recomienda usar uno con velocidades de lectura mayor a 30Mb/s, ya que acelera sustancialmente el arranque).

### Archivos externos requeridos para creación de imagen

Estos deben estar en la raíz del repositorio. Sólo las llaves se pueden eliminar una vez instaladas.

 + Llaves para repositorios APT:
   + [Microsoft](https://code.visualstudio.com/docs/setup/linux) en archivo `packages.microsoft.gpg`
   + [Sublime Text](https://www.sublimetext.com/docs/linux_repositories.html) en archivo `sublimehq-pub.gpg`
 + Paquete de instalación de [NetBeans](https://netbeans.apache.org/front/main/download/) en archivo `netbeans.deb`. Actualmente, esta probada la versión 24.

## Firewall, contraseña de administrador y extensiones para VS Code

### Firewall y contraseña de administrador

Ambos requieren la presencia de ciertos archivos en la raíz de la segunda partición en la memoria USB (la que tiene un sistema de archivos FAT y etiqueta 'GRML'):

 + `/whitelist.txt`: Tiene los dominios e IPs a los que el sistema será permitido a conectarse.
   + Formato: Una dirección por línea, sin comentarios o texto extra.
   + Validación: No hay. Si la dirección no es correcta, se seguirá a la siguiente con un pequeño retraso (excepto por la primera).
   + Otros: La primera dirección del archivo será la página de inicio del navegador.
   + NOTA: En caso de estar vacío o no existir, se deshabilitará el firewall, borrando cualquier regla aplicada anteriormente.

   + Ejemplo (ignore comentarios iniciados por '#'):
     ```
     google.com                 # Dominio, primera dirección del archivo, y página de inicio para el navegador
     dominio.no.existente.com   # Dirección no existente, será ignorado
     8.8.8.8                    # Ip
     ...
     ```
 + `/rootpwd.txt`: Contiene la contraseña para el usuario root.
   + Formato: Una sola línea de texto plano (ojo con los saltos de línea de otros sistemas operativos).
   + Validación: Ninguna, así que cuidado con los caracteres especiales y saltos de línea.
   + NOTA: En caso de estar vacío o no existir, la cuenta root permanecerá BLOQUEADA, ya que es su única forma de acceso (el usuario por defecto no tiene acceso a sudo).
   + Ejemplo:
     ```
     ContraseñaSuperSegura
     ```
### Instalación de extensiones

En la raíz del repositorio, crear una carpeta `extensiones/` y poner en esta los archivos `.vsix` requeridos. Estos serán copiados e instalados en el sistema al crear la imagen.

## Suite

**Lenguajes**
 + C/C++ (gcc/g++)
 + Java (jdk/jre)
 + Python 3

**Editores/IDEs**
 + VS Code (extensiones opcionales)
 + Sublime Text
 + Code::Blocks
 + NetBeans
 + Gedit
 + ViM/gViM
 + Emacs (gtk/consola)

**Entorno de escritorio (Xfce)**
 + Navegador (NetSurf)
 + Calculadora
 + Gestor de tareas y Htop
 + Lector de documentos (zathura)

**Otras funcionalidades**
 + Firewall por inclusión mediante IPs (o desde la resolución de dominios). Soporta HTTP y HTTPS.
 + Se puede establecer la contraseña de administrador durante arranque

**Extensiones probadas/configuradas**
 + `ms-python.vscode-pylance` (se puede usar en vez de `ms-python.python` si sólo se requiere IntelliSense)
 + `ms-vscode.cpptools`
 + `redhat.java` (para ahorrar espacio, usar la versión universal; el sistema ya tiene los binarios necesarios para ejecutarla)
 + `formulahendry.code-runner` (el por defecto para ejecutar código, muy recomendado)

### Notas

 + El usuario por defecto (`grml`) no tiene acceso `sudo`.
 + El sistema se ejecuta totalmente en RAM: No hace cambios en el equipo, y basta con esperar a que copie los archivos necesarios desde la memoria para retirarla.
 + No hay soporte para redes inalámbricas, solo cableadas (desde el arranque, para que sea configurada automáticamente)
 + Localización por defecto para Chile continental (interfaz, hora, teclado).
 + Si las extensiones de VS Code requieren conexión a Internet, agregue las IPs y dominios necesarios al firewall.
 + Por un error de VS Code, si el sistema no tiene conexión durante el primer inicio del programa, las extensiones no aparecerán como instaladas (aunque funcionarán normalmente).

