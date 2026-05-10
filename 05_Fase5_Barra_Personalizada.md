# Fase 5: Creación de la Barra de Tareas (Diseño Figma)

Vamos a replicar tu diseño de Figma al milímetro. Hemos invertido el paradigma clásico: la izquierda tendrá el reloj y clima, el centro la paginación por puntos, y la derecha toda la información del hardware en un formato ultra comprimido.

---

### 1. Los Grupos (Puntos de Paginación)
Para lograr el efecto de "puntos" (estilo iOS/Android) en lugar de números en el centro de tu pantalla, vamos a usar el icono de círculo lleno (``). Edita la lista de tus grupos en `config.py` para que quede así:

```python
groups = [
    Group("1", label=""),
    Group("2", label=""),
    Group("3", label=""),
    Group("4", label=""),
    Group("5", label=""),
    Group("6", label=""),
]
# Mantenemos el bloque "for i in groups:" intacto, igual que siempre.
```

### 2. Estructura Completa de la Barra (Figma Design)
Ve a la sección donde se dibuja tu barra (`screens = [ Screen( top=bar.Bar([ ... `) y **reemplaza TODO el contenido** de los corchetes de la barra por el código de abajo.

*He programado cada bloque para que funcione y luzca exactamente como tu imagen:*

```python
                # ==========================================
                # --- IZQUIERDA: Clima y Reloj ---
                # ==========================================
                widget.Spacer(length=15),
                widget.GenPollText(
                    update_interval=600, # Actualiza cada 10 min
                    # Devuelve solo el Icono Dinámico (%c) y la Temperatura (%t) sin la ciudad
                    func=lambda: subprocess.check_output("curl -s --max-time 5 'wttr.in/?format=☁%20%t%20%C&lang=es'", shell=True, text=True).strip(),
                    font="JetBrainsMono Nerd Font Bold",
                    fontsize=14,
                    foreground="#ECEFF4",
                ),
                widget.Spacer(length=20),
                widget.Clock(
                    format=" %H:%M  %d/%m/%Y",
                    font="JetBrainsMono Nerd Font",
                    fontsize=14,
                    foreground="#ECEFF4",
                ),

                # ==========================================
                # --- CENTRO: Puntos de Paginación ---
                # ==========================================
                widget.Spacer(), # Empuja hacia la izquierda
                
                widget.GroupBox(
                    font="JetBrainsMono Nerd Font",
                    fontsize=14,
                    margin_y=3,
                    margin_x=0,
                    padding_y=5,
                    padding_x=5,
                    borderwidth=0, # Sin bordes ni líneas
                    active="#ECEFF4",   # Blanco brillante (Si el escritorio tiene ventanas abiertas)
                    inactive="#4C566A", # Gris oscuro (Si el escritorio está vacío)
                    highlight_method="text", # Solo cambia el color del círculo
                    this_current_screen_border="#88C0D0", # Cian (El escritorio que estás viendo ahora)
                ),

                widget.Spacer(), # Empuja hacia la derecha

                # ==========================================
                # --- DERECHA: Hardware y Quick Settings ---
                # ==========================================
                
                # 1. Wi-Fi (Placeholder temporal para evitar errores de librerías)
                widget.TextBox(
                    text=" Mi-Red-WiFi",
                    font="JetBrainsMono Nerd Font",
                    foreground="#E5E9F0",
                ),
                widget.Spacer(length=15),

                # 2. USB (Placeholder visual por ahora)
                widget.TextBox(
                    text="󰕓 Nombre",
                    font="JetBrainsMono Nerd Font",
                    foreground="#E5E9F0",
                ),
                widget.Spacer(length=15),

                # 3. El Botón "Quick Settings" (Tu recuadro con iconos)
                widget.TextBox(
                    text="  󰕾  ",
                    font="JetBrainsMono Nerd Font",
                    fontsize=16,
                    background="#BF616A", # Fondo rojo tipo Nord
                    foreground="#2E3440", # Iconos en negro/gris oscuro
                    mouse_callbacks={'Button1': lazy.spawn("pavucontrol")}, # Clic abre el mezclador de audio
                ),
                widget.Spacer(length=15),

                # 4. Hardware Stats reales en vivo
                widget.CPU(
                    format="⚙ {load_percent:04}%",
                    font="JetBrainsMono Nerd Font",
                    foreground="#D8DEE9",
                ),
                widget.Spacer(length=10),
                widget.Memory(
                    format="🎫 {MemPercent:04}%",
                    font="JetBrainsMono Nerd Font",
                    foreground="#D8DEE9",
                ),
                widget.Spacer(length=10),
                # Nota: El widget de Batería fue omitido porque la netbook no tiene la batería física conectada.
                # Bandeja del sistema (Para iconos extra de fondo)
                widget.Systray(),
                widget.Spacer(length=10),
```

### 3. El Fondo de la Barra
Para lograr esa elegancia oscura de tu diseño (y no el bloque gris feo), fíjate que justo debajo del corchete final `]` de la lista de widgets, están las propiedades de la barra entera. Cámbialas a esto:

```python
            ],
            30, # Grosor de la barra un poco más ancho (30 píxeles)
            background="#0F1419", # Un negro/azul súper profundo
            opacity=0.95, # Una leve transparencia espectacular
        ),
    ),
]
```
