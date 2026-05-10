# Fase 2: Creación de Usuario y Entorno Gráfico Base

Has reiniciado y estás en la consola negra de tu nuevo sistema. Inicia sesión como `root` con la contraseña que creaste.

**[SOLO PARA NETBOOK/PC REAL] Conectarse al Wi-Fi:**
A diferencia del pendrive (donde usabas `iwctl`), tu nuevo sistema oficial usa `NetworkManager`. Para conectarte al Wi-Fi simplemente escribe `nmtui`, ve a "Activate a connection", elige tu red, pon la contraseña y sal del menú. Haz un `ping -c 3 archlinux.org` para comprobar.

---

### 1. Creación de tu Usuario Principal
Reemplaza `tu_usuario` por tu nombre:
```bash
useradd -m -G wheel,video,audio,storage tu_usuario
passwd tu_usuario
```

### 2. Configurar sudo
```bash
pacman -S sudo nano git base-devel
EDITOR=nano visudo
```
*Busca la línea `# %wheel ALL=(ALL:ALL) ALL` y bórrale el `#` inicial para descomentarla. Guarda y sal (Ctrl+O, Ctrl+X).*

Escribe `exit` y vuelve a iniciar sesión, pero ahora con `tu_usuario`.

### 3. Servidor Gráfico, Audio y Drivers de VM
```bash
sudo pacman -S xorg-server xorg-xinit pipewire pipewire-pulse wireplumber pavucontrol
```
Configura tu teclado para el entorno gráfico (LightDM y Qtile). Ejecuta **solo uno** de estos comandos según el teclado físico de tu computadora:

*   **Para Inglés Internacional (tu PC principal):**
```bash
localectl set-x11-keymap us pc105 intl
```
*   **Para Español Latinoamericano (la Netbook):**
```bash
localectl set-x11-keymap latam
```
*   **Para Español de España:**
```bash
localectl set-x11-keymap es
```

Si usas VirtualBox (para habilitar el portapapeles compartido y redimensionamiento de pantalla):
```bash
sudo pacman -S virtualbox-guest-utils
sudo systemctl enable vboxservice
```
Si usas VMWare:
```bash
sudo pacman -S open-vm-tools
sudo systemctl enable vmtoolsd
```
*(Nota: Ya no se necesitan paquetes de video extra como en el pasado, el kernel de Linux ya los trae incorporados por defecto para Máquinas Virtuales).*

### 4. Instalación de Qtile y Utilidades
```bash
sudo pacman -S qtile alacritty rofi thunar picom feh polkit-gnome dunst xdg-utils
```
*(Nota: `xdg-utils` es el "pegamento" vital para que la consola sepa cómo abrir enlaces externos en un navegador).*

### 5. Gestor de Inicio de Sesión (LightDM)
```bash
sudo pacman -S lightdm lightdm-slick-greeter
sudo nano /etc/lightdm/lightdm.conf
```
*Busca la sección `[Seat:*]` y la línea `#greeter-session=example-gtk-gnome`. Descoméntala y cámbiala a:*
`greeter-session=lightdm-slick-greeter`

Habilita LightDM para que arranque siempre con el sistema:
```bash
sudo systemctl enable lightdm
```

¡Si reinicias ahora (`sudo reboot`), te recibirá una interfaz gráfica para poner tu usuario y contraseña!

---

### 6. Instalación de Navegador Web y Asistente (Antigravity)
Una vez que hayas iniciado sesión en Qtile, es altamente recomendable instalar tu navegador de preferencia y configurar Antigravity para que la Inteligencia Artificial te asista y modifique los archivos de configuración desde adentro.

1. Abre tu terminal (Alacritty) y descarga un navegador web (ej. Firefox):
```bash
sudo pacman -S firefox
```
*(Si prefieres Brave, puedes instalarlo más adelante en la Fase 3 usando `yay -S brave-bin`).*

2. Dile a la terminal cuál es tu navegador:
```bash
export BROWSER=firefox
```

3. Ejecuta el comando de instalación de Antigravity. Gracias a `xdg-utils` y a tu nuevo navegador, la ventana de Google Login se abrirá automáticamente para que conectes el asistente a tu máquina virtual.
