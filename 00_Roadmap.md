# Hoja de Ruta: Instalación de Arch Linux + Qtile

Instalar Arch Linux y configurarlo desde cero con un Window Manager (WM) como Qtile es un proyecto excelente. Aprenderás exactamente cómo funciona tu sistema operativo.

A continuación, presento la hoja de ruta estructurada en 3 fases principales, adaptadas específicamente para un entorno de Máquina Virtual (UEFI), con una estética "Nord" y un gestor de inicio de sesión estilo Ubuntu (GDM).

---

## Fase 1: Instalación del Sistema Base
Esta es la fase de instalación clásica desde la consola (Live ISO). Su objetivo es dejar el disco preparado y el sistema base listo para arrancar de forma independiente.
*   Verificación de conexión a internet y actualización de reloj.
*   Particionado de discos (UEFI/GPT: boot, swap, root).
*   Formateo y Montaje.
*   Instalación del sistema base mediante `pacstrap`.
*   Configuración del sistema (fstab, chroot, zona horaria, idiomas/locales, hostname y root).
*   Instalación del Bootloader (GRUB) y NetworkManager.

## Fase 2: Creación de Usuario y Entorno Gráfico Base
Aquí transformamos la consola negra en un sistema operativo con interfaz gráfica lista para usarse.
*   Creación de usuario estándar y configuración de privilegios `sudo`.
*   Servidor gráfico: Xorg (X11) y Drivers específicos de Máquina Virtual.
*   Sistema de Audio moderno: PipeWire.
*   Window Manager: Qtile (y sus dependencias base).
*   Utilidades esenciales del "escritorio": Alacritty (Terminal), Rofi (Lanzador), Thunar (Archivos), Picom (Transparencias).
*   Display Manager (Gestor de inicio de sesión): Instalación de LightDM y Slick Greeter.

## Fase 3: Personalización Estética ("Ricing" estilo Nord)
La fase final para darle el aspecto visual elegante, integrando la paleta de colores fríos y configurando el inicio de sesión para que luzca limpio y centrado.
*   Preparación del repositorio comunitario (AUR) mediante la herramienta `yay`.
*   Descarga de Tema Nordic, paquete de iconos Papirus y fuentes especiales Nerd Fonts.
*   Configuración detallada de LightDM (`slick-greeter.conf`) para replicar el estilo de Ubuntu.
*   Generación del archivo de configuración de Qtile (`config.py`) para comenzar a aplicar los colores hexadecimales Nord en la barra y bordes.

## Fase 4: Configuración de Hardware Físico y Atajos de Qtile
Como el sistema está en hardware real (Bare Metal), es necesario vincular los botones físicos de la computadora con el entorno gráfico.
*   Configuración de las teclas de brillo (XF86MonBrightness) mediante `brightnessctl`.
*   Configuración de las teclas de volumen y silencio usando PipeWire (`wpctl`).
*   Configuración del Touchpad (activación del Tap-to-click y Scroll Natural en Xorg).

## Fase 5: Creación de la Barra de Tareas Personalizada
Reemplazo de la barra por defecto de Qtile por una barra superior completamente adaptada a tus necesidades en la Netbook (Widget por Widget).
*   Reubicación a la parte superior.
*   *Configuración interactiva de widgets en progreso...*

---

## Consideraciones Críticas
*   **La Arch Wiki es tu Biblia:** Ante cualquier error imprevisto, la [Arch Wiki](https://wiki.archlinux.org/) tiene siempre la respuesta exacta.
*   **Python es clave en Qtile:** La configuración de Qtile se escribe íntegramente en Python. Un simple error de indentación o una coma faltante puede evitar que inicie la interfaz gráfica.
*   **Máquinas Virtuales:** Hacerlo aquí es ideal para aprender. Sin embargo, ten en cuenta que debido a la emulación gráfica, herramientas pesadas de sombreado (como `picom`) pueden no verse tan fluidas como en hardware real.
