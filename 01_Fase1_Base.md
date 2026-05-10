# Fase 1: Instalación del Sistema Base (VM y Hardware Real)

## 0. Preparativos y Consideraciones Previas

### Si usas VirtualBox (Máquina Virtual):
Antes de arrancar la ISO, debes configurar la VM con las siguientes especificaciones mínimas para que el entorno corra fluido:
*   **Memoria RAM:** Al menos 4096 MB (4 GB).
*   **Procesador:** Al menos 2 núcleos (Cores).
*   **Almacenamiento:** Disco de 30 GB o más.
*   **Pantalla:** Memoria de video al máximo (128 MB) y marca "Habilitar aceleración 3D" (opcional, si te da problemas gráficos luego, desmárcala).
*   **Sistema (CRÍTICO):** Ve a Configuración > Sistema > Placa base, y **marca la casilla "Habilitar EFI (sólo SO especiales)"**.

### Diferencias al instalar en PC Real (Notebook / Escritorio):
Si en el futuro decides instalar esto directamente en tu computadora física, debes tener en cuenta estas diferencias respecto a la guía de abajo:
1.  **Internet Wi-Fi:** En la VM, el internet funciona automáticamente. En una netbook real, deberás conectarte al Wi-Fi manualmente usando `iwctl` (he añadido los comandos exactos dentro del **Paso 1** más abajo).
2.  **Nombre del Disco:** En la VM tu disco se llamará `/dev/sda`. En una PC moderna el disco probablemente se llame `/dev/nvme0n1`. Deberás ajustar el comando `cfdisk` (Paso 2) a tu disco real.
3.  **Microcódigo del Procesador:** En una PC física, en el **Paso 5 (Pacstrap)**, debes agregar el paquete `intel-ucode` (si tienes procesador Intel) o `amd-ucode` (si es AMD) a la lista del comando para asegurar la estabilidad del procesador.
4.  **Dual Boot (Con Windows):** Si tienes Windows, **NO formatees la partición EFI** en el Paso 3 (el comando `mkfs.fat -F 32`). Solo debes montarla. Si la formateas, romperás el arranque de Windows.
5.  **Drivers de Video (Fase 2):** En lugar de instalar los paquetes de `vmware` o `virtualbox`, en el entorno real instalarás los paquetes de tu gráfica (`nvidia`, `xf86-video-amdgpu` o `xf86-video-intel`).

---

### Comienzo de la instalación (Estás en la consola como `root` tras arrancar la ISO)

### 1. Configurar el Teclado y Verificar Internet
Por defecto, la consola está en inglés tradicional. El comando de teclado (`loadkeys`) depende del hardware físico que estés usando:

*   **Para Inglés Internacional (tu PC principal):** `loadkeys us-acentos`
*   **Para Español Latinoamericano (la Netbook):** `loadkeys la-latin1`
*   **Para Español de España:** `loadkeys es`

Ejecuta el comando que corresponda a tu computadora física.

**[SOLO PARA NETBOOK/PC REAL] Conectarse al Wi-Fi:**
En la máquina virtual el internet viene configurado como cable, pero en tu netbook debes conectarte a tu red Wi-Fi manualmente. Ejecuta estos comandos en orden:
```bash
iwctl
device list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect "TU_RED_WIFI"
```
*(Escribe tu contraseña cuando te la pida y luego escribe `exit` para volver a la consola roja).*

Luego, verifica tu conexión a internet y sincroniza el reloj:
```bash
ping -c 3 archlinux.org
timedatectl set-ntp true
```

### 2. Particionado del Disco
Abre el gestor de particiones para `/dev/sda` (el disco virtual):
```bash
cfdisk /dev/sda
```
Selecciona etiqueta **gpt**. Crea 3 particiones:
1.  **512M** -> Type: `EFI System`
2.  **4G** -> Type: `Linux swap`
3.  **Resto del espacio** -> Type: `Linux filesystem`
Selecciona **Write**, escribe `yes` y luego **Quit**.

### 3. Formateo de las particiones
```bash
mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.ext4 /dev/sda3
```

### 4. Montaje de las particiones
```bash
mount /dev/sda3 /mnt
mount --mkdir /dev/sda1 /mnt/boot
```

### 5. Instalación Base (Pacstrap)
```bash
pacstrap -K /mnt base linux linux-firmware nano
```

### 6. Fstab y Chroot
```bash
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

### 7. Zona Horaria y Reloj (Ajusta la zona si es necesario)
```bash
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
hwclock --systohc
```

### 8. Idioma (Locales) y Teclado Definitivo
```bash
nano /etc/locale.gen
```
*Descomenta (borra el #) en `en_US.UTF-8 UTF-8` y `es_AR.UTF-8 UTF-8` (o tu país).*
```bash
locale-gen
echo "LANG=es_AR.UTF-8" > /etc/locale.conf
echo "KEYMAP=us-acentos" > /etc/vconsole.conf
```
*(Nota: Asegúrate de cambiar `us-acentos` por `la-latin1` o `es` si estás en otra computadora. Esto configura el teclado de la consola negra. En la Fase 2 lo configuraremos para el entorno gráfico).*

### 9. Hostname y Contraseña Root
```bash
echo "arch-vm" > /etc/hostname
passwd
```

### 10. Bootloader (GRUB) y Red
```bash
pacman -S grub efibootmgr networkmanager
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
systemctl enable NetworkManager
```

### 11. Salir y Reiniciar
```bash
exit
umount -R /mnt
reboot
```
*(Asegúrate de quitar el ISO virtual para arrancar desde el disco duro).*
