# Fase 4: Configuración de Hardware Físico y Atajos de Qtile

Cuando instalas Arch Linux en hardware real (como una Notebook), es necesario "enseñarle" al entorno gráfico cómo comunicarse con los botones físicos de tu computadora (como las teclas de Función Fn). 

La mayoría de estos ajustes se realizan modificando el "cerebro" de tu entorno:
`~/.config/qtile/config.py`

---

### 1. Control de Brillo de Pantalla
Primero, instala el controlador del brillo de la pantalla (que no requiere permisos de root):
```bash
sudo pacman -S brightnessctl
```

Abre tu archivo de Qtile (`nano ~/.config/qtile/config.py`), busca la lista llamada `keys = [` (suele estar casi arriba de todo) y agrega estas líneas para mapear los botones del "sol" de tu teclado:
```python
    # Control de brillo
    Key([], "XF86MonBrightnessUp", lazy.spawn("brightnessctl set +5%"), desc="Subir brillo"),
    Key([], "XF86MonBrightnessDown", lazy.spawn("brightnessctl set 5%-"), desc="Bajar brillo"),
```

### 2. Control de Volumen y Audio
En la Fase 2 ya instalamos `pipewire` y `wireplumber` (el sistema de audio moderno de Linux). Para que las teclas de volumen físico (parlantes) funcionen, agrega estas líneas en la misma lista de `keys`:
```python
    # Control de volumen
    Key([], "XF86AudioRaiseVolume", lazy.spawn("wpctl set-volume -l 1.5 @DEFAULT_AUDIO_SINK@ 5%+"), desc="Subir volumen"),
    Key([], "XF86AudioLowerVolume", lazy.spawn("wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-"), desc="Bajar volumen"),
    Key([], "XF86AudioMute", lazy.spawn("wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle"), desc="Silenciar audio"),
```

**Solución de Problemas de Audio (Si no tienes sonido o pavucontrol falla):**
A veces el motor de audio no arranca automáticamente la primera vez. Si no tienes sonido o el panel de volumen se queda congelado en "Establishing connection", fórza su encendido así:

1. Asegúrate de tener todas las piezas (si te olvidaste alguna en la instalación base):
```bash
sudo pacman -S pipewire pipewire-pulse wireplumber pavucontrol
```
2. Enciende el motor de audio de tu usuario (este comando va **SIN sudo**, ya que el audio pertenece al usuario normal):
```bash
systemctl --user enable --now pipewire pipewire-pulse wireplumber
```
3. Ahora escribe `pavucontrol` en la consola. Se abrirá el panel gráfico y en la pestaña "Output Devices" podrás quitarle el 'Mute' a tu netbook.

### 3. Touchpad (Toque para hacer clic)
Por defecto, Arch Linux requiere que hundas físicamente el touchpad para hacer clic. Si quieres activar el "toque suave para hacer clic" (Tap to click) y el desplazamiento natural con dos dedos, debes crear un archivo de configuración del servidor gráfico Xorg:

1. Crea la carpeta si no existe:
```bash
sudo mkdir -p /etc/X11/xorg.conf.d/
```
2. Crea el archivo de configuración:
```bash
sudo nano /etc/X11/xorg.conf.d/30-touchpad.conf
```
3. Pega este contenido exacto:
```conf
Section "InputClass"
    Identifier "touchpad"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "NaturalScrolling" "true"
EndSection
```
*(Guarda con Ctrl+O y sal con Ctrl+X. El cambio del touchpad se aplicará la próxima vez que reinicies tu computadora).*

---

### Nota Crítica sobre Qtile
Recuerda que cada vez que edites y guardes el archivo `~/.config/qtile/config.py`, los cambios no aplicarán automáticamente. Debes presionar **`Super + Control + R`** (Tecla Windows + Ctrl + R) para recargar el entorno visual en vivo. Si el código de Python tiene algún error (como una coma faltante), la pantalla no parpadeará y el atajo no funcionará.

---

### 4. Resumen de Atajos Nativos de Qtile
Para que no tengas que leer el código fuente de `config.py` cada vez que olvides cómo moverte, aquí tienes el resumen de los atajos que vienen por defecto (donde **`Super`** es la tecla **Windows**):

**Gestión del Sistema y Apps:**
*   **`Super + Control + r`**: Recargar Qtile (aplicar cambios del código).
*   **`Super + Control + q`**: Cerrar sesión / Salir de Qtile.
*   **`Super + Enter`**: Abrir una nueva Terminal.
*   **`Super + r`**: Abrir el lanzador rápido de comandos en la barra superior.

**Manejo de Ventanas:**
*   **`Super + w`**: Cerrar la ventana que tengas enfocada.
*   **`Super + f`**: Alternar Pantalla Completa (Fullscreen).
*   **`Super + t`**: Alternar ventana Flotante (para poder arrastrarla libremente).
*   **`Super + Tab`**: Cambiar entre los diferentes modos de organización visual (Columnas, Maximizado, etc.).
*   **`Super + Barra Espaciadora`**: Cambiar el foco hacia la siguiente ventana.

**Movimiento Direccional:**
*   **`Super + Flechas`**: Mover tu selección hacia Izquierda, Abajo, Arriba o Derecha.
*   **`Super + Shift + Flechas`**: Mover/Intercambiar la ventana entera de lugar.
*   **`Super + Control + Flechas`**: Agrandar o encoger la ventana.

**Escritorios Virtuales (Grupos):**
*   **`Super + [1 al 9]`**: Viajar al escritorio número 1, 2, 3...
*   **`Super + Shift + [1 al 9]`**: Enviar la ventana que estás usando a ese escritorio.
