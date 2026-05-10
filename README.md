# 🐧 Arch Linux + Qtile: Configuración desde cero 🚀

¡Bienvenido al repositorio **ArchLinux-Qtile**! 

Este repositorio contiene una guía paso a paso, exhaustiva y en español, para la instalación de **Arch Linux** desde cero (Bare Metal/VM) y la configuración de **Qtile** como Window Manager. 

El objetivo de este proyecto es lograr un entorno minimalista, sumamente rápido, completamente funcional y estéticamente profesional, utilizando una paleta de colores **Nord**, tipografías Nerd Fonts y un control total del sistema mediante atajos de teclado y scripts en Python.

---

## 📑 Índice de Fases

La guía está dividida en 5 fases secuenciales que te llevarán desde el disco en blanco hasta la personalización de cada píxel en tu pantalla:

- 🗺️ [**00_Roadmap**](00_Roadmap.md): Planificación general del entorno y consideraciones iniciales.
- 💻 [**01_Fase1_Base**](01_Fase1_Base.md): Particionado UEFI, formateo, `pacstrap`, `chroot`, red y configuración de GRUB.
- 🎨 [**02_Fase2_Entorno**](02_Fase2_Entorno.md): Servidor Xorg, PipeWire para audio, instalación de Qtile y gestor de sesiones LightDM.
- 🖌️ [**03_Fase3_Estetica**](03_Fase3_Estetica.md): Instalación del gestor AUR (`yay`), aplicación del tema Nordic, iconos Papirus y tipografías avanzadas.
- ⚙️ [**04_Fase4_Hardware_y_Atajos**](04_Fase4_Hardware_y_Atajos.md): Mapeo de teclas físicas (volumen, brillo) y configuración del Touchpad para netbooks/laptops.
- 📊 [**05_Fase5_Barra_Personalizada**](05_Fase5_Barra_Personalizada.md): Diseño y código de la barra superior de Qtile en Python con widgets de monitorización de hardware en vivo.

---

## 🛠️ Tecnologías y Herramientas Clave

* **OS:** [Arch Linux](https://archlinux.org/)
* **Window Manager:** [Qtile](http://www.qtile.org/) (Configurado 100% en Python)
* **Compositor:** Picom (transparencias y sombras)
* **Terminal:** Alacritty
* **Lanzador de Apps:** Rofi
* **Gestor de Login:** LightDM + Slick Greeter
* **Servidor de Audio:** PipeWire
* **Estética Principal:** Nordic Theme

---

## ⚠️ Recomendaciones

* **La configuración es en Python:** Todos los ajustes visuales y atajos de Qtile se definen en el archivo `config.py`. Debes tener especial cuidado con la sintaxis y la indentación.
* **Respaldo:** Antes de hacer cambios drásticos en tu `config.py`, es muy recomendable guardar una copia de seguridad que sepas que funciona.
* **Arch Wiki:** Aunque esta guía es muy detallada, recuerda que la [Arch Wiki](https://wiki.archlinux.org/) sigue siendo tu mejor fuente de información para resolver incidencias con tu hardware específico.

---
*Desarrollado para la comunidad. Modifica y experimenta a tu gusto.*
