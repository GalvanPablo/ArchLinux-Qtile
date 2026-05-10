# Fase 3: Personalización Estética (Tema Nord y LightDM estilo Ubuntu)

Ahora que tienes interfaz gráfica (Qtile) y puedes usar la terminal Alacritty, vamos a embellecer el sistema.

---

### 1. Instalar un Gestor para AUR (yay)
AUR tiene temas creados por la comunidad que no están en los repositorios oficiales.
```bash
cd ~
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```
*(Puedes borrar la carpeta después con `cd ~ && rm -rf yay-bin`).*

### 2. Descargar Tema Nordic, Iconos y Fuentes
```bash
yay -S nordic-theme papirus-icon-theme ttf-jetbrains-mono-nerd
```

### 3. Configurar LightDM para que luzca como GNOME/Ubuntu
```bash
sudo nano /etc/lightdm/slick-greeter.conf
```
Pega lo siguiente:
```ini
[Greeter]
theme-name=Nordic
icon-theme-name=Papirus-Dark
cursor-theme-name=Nordic-Cursor

# Formato del reloj centrado arriba
clock-format=%a %d %b, %H:%M

# Para ocultar un fondo por defecto de usuario si lo hubiera
draw-user-backgrounds=false

# (Opcional) Ruta a tu fondo de pantalla Nord
# background=/usr/share/backgrounds/nord_wallpaper.jpg
```
Para mostrar tu foto de perfil (Avatar circular) en el centro, simplemente guarda una imagen tuya en formato `.png` o `.jpg` en tu carpeta home con el nombre `.face` (Ej: `~/.face`).

### 4. Configurar Qtile con los colores Nord
Qtile no crea su archivo de configuración por defecto automáticamente. Debes copiar la plantilla:
```bash
mkdir -p ~/.config/qtile
cp /usr/share/doc/qtile/default_config.py ~/.config/qtile/config.py
```

Ahora puedes abrir `~/.config/qtile/config.py` con tu editor favorito y aplicar la paleta **Nord**:
*   **Fondo Base:** `#2E3440`
*   **Fondo de Paneles:** `#3B4252`
*   **Texto Principal:** `#ECEFF4`
*   **Acento (Cian):** `#88C0D0`
*   **Acento (Azul):** `#81A1C1`
*   **Color de Error (Rojo):** `#BF616A`

Busca la variable `colors` en el código Python y reemplaza los colores por defecto con estos códigos hexadecimales para unificar todo tu sistema.

---

### 5. La Terminal "Mágica" (Zsh + Oh My Zsh + Powerlevel10k)
Para tener la terminal exactamente como en la imagen (con las flechas, iconos y autocompletado gris inteligente), debes hacer lo siguiente cuando ya estés dentro de tu entorno gráfico:

**A. Instalar la Shell moderna y los plugins:**
```bash
sudo pacman -S zsh zsh-autosuggestions zsh-syntax-highlighting
```
*(Cambia tu terminal por defecto ejecutando `chsh -s /usr/bin/zsh` y poniendo tu contraseña).*

**B. Instalar Oh My Zsh (El motor que maneja los temas):**
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

**C. Instalar el Tema Powerlevel10k (El de la foto):**
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

**D. Aplicar la configuración:**
Abre el archivo de configuración oculto de Zsh:
```bash
nano ~/.zshrc
```
1. Busca la línea `ZSH_THEME="robbyrussell"` y cámbiala por `ZSH_THEME="powerlevel10k/powerlevel10k"`.
2. Ve al final del todo de ese archivo y pega estas dos líneas para activar el autocompletado y los colores:
```bash
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```
Guarda y sal (Ctrl+O, Ctrl+X).

Cuando cierres Alacritty y la vuelvas a abrir, **automáticamente se lanzará el asistente visual de Powerlevel10k**, el cual te irá haciendo preguntas (ej. "¿Ves un candado?", "¿Qué estilo prefieres?") para que elijas exactamente el "Rainbow Style" de tu captura de pantalla.

---

### 6. Configurar Alacritty (Para que los iconos funcionen)
Como bien intuiste, de nada sirve tener una terminal potente si no puede leer los iconos. Debemos decirle a Alacritty que use la Nerd Font que instalamos en el Paso 2, de lo contrario verás "cuadraditos rotos" en lugar del logo de Git o las flechas.

Crea el archivo de configuración de Alacritty:
```bash
mkdir -p ~/.config/alacritty
nano ~/.config/alacritty/alacritty.toml
```

Pega esta configuración básica para enlazar la fuente especial e integrar los colores de la paleta Nord:
```toml
[font]
size = 11.0

[font.normal]
family = "JetBrainsMono Nerd Font"
style = "Regular"

[colors.primary]
background = "#2E3440"
foreground = "#D8DEE9"

[colors.normal]
black   = "#3B4252"
red     = "#BF616A"
green   = "#A3BE8C"
yellow  = "#EBCB8B"
blue    = "#81A1C1"
magenta = "#B48EAD"
cyan    = "#88C0D0"
white   = "#E5E9F0"
```
*(Guarda con Ctrl+O y sal con Ctrl+X. El cambio se aplicará al instante y tu terminal Alacritty ya será capaz de dibujar todos los símbolos de Powerlevel10k).*
