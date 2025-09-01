# Evidencias de la unidad 4

Proyecto Seek 🎶✨
Actividad 01 — Investigación
Revisión y refinamiento del concepto original

1. Recuperación del concepto original
Mi concepto raíz se basa en la idea de un organismo visual colectivo que responde al cuerpo, a la energía y a la atención del público.
El público modula este organismo a través de los dispositivos móviles, el navegador web y una consola de control remoto.

Fases emocionales
Exploración → fase inicial, curiosa y flotante.
Tensión → acumulación de energía, patrones que se comprimen.
Catarsis → liberación expansiva, partículas en explosión.
Mapeo IPO original
Inputs: inclinación y toques de móviles, audio ambiente, movimiento frente a cámara, interacción con mouse/teclado.
Process: ruido Perlin, sistemas de partículas, deformaciones de rejilla.
Outputs: visuales generativos en p5.js (formas, halos, campos).
2. Mapeo concepto → implementación técnica
2.1 Estados ↔ fases emocionales
Estado 0 — Exploración
Círculos orbitando y halos suaves. Paleta fría.

Estado 1 — Tensión
Rejilla deformada y vibrante que responde al audio. Paleta intermedia.

Estado 2 — Catarsis
Partículas en expansión con halo fuerte alimentado por motion de cámara. Paleta cálida/intensa.

2.2 Inputs reales y uso en cada estado
Cliente	Dato (Socket)	Estado 0	Estado 1	Estado 2
Mobile1	Gyro (β,γ)	Centro del organismo	Inclina rejilla	Dirección inicial de partículas
Accel	Pulso secundario	Amplitud ondas	Energía inicial
Touch	Color/acento	Contraste	Estelas
Mobile2	Orientación	Parallax ligero	Skew rejilla	Viento de partículas
Mag sim.	Grosor borde	Densidad celdas	Fuerza campo
Touch	Borde brillante	Más ruido	Más estelas
Webclient	Mouse	Atracción suave	Cursor desplaza cresta	Foco de partículas
AudioLevel	Halo/pulso leve	Amplitud ondas	Explosiones periódicas
VideoMotion	Halo ambiental	Vibración extra	Halo/fog intenso
Keys	Cambiar estado (debug)	Cambiar estado	Cambiar estado
Control	a, b, c	a: tamaño base, b: lerp mouse, c: alpha	a: amplitud, b: densidad, c: contraste	a: cantidad, b: velocidad, c: tamaño
2.3 Procesos generativos concretos
Estado 0 (Exploración)
Orbitas con ruido Perlin, halo dependiente del audio, mezcla hacia mouse con b.

Estado 1 (Tensión)
Rejilla sinusoidal; amplitud controlada por a y audio; densidad ligada al mag de Mobile2; contraste por c.

Estado 2 (Catarsis)
Partículas lanzadas desde el centro; dirección = tilt Mobile1 + viento Mobile2; explosiones con audio; halo según motion; cantidad/velocidad/tamaño con a,b,c.

2.4 Outputs factibles
Resolución full window (≥ 60FPS en desktop).
Paletas diferenciadas por estado (0 fríos, 1 morados/ámbar, 2 cálidos).
Transiciones suaves (400–800 ms, interpolando parámetros).
3. Refinamiento técnico
Limitaciones
iOS necesita permiso para sensores → resolví con botón “Solicitar permisos”.
Magnetómetro real no disponible → simulado con orientation.
Websocket bloqueado en túnel a veces → habilité polling como fallback.
Oportunidades
Inputs adicionales del webclient: teclas como trigger, motion de cámara como energía extra.
Parámetros remotos (a,b,c) expandibles a color themes o velocidad de transición.
Coherencia narrativa
0 → 1: crossfade de color + densidad creciente, triggered por audioLevel alto o botón de control.
1 → 2: flash + expansión de partículas, triggered por parámetros o control.
2 → 0: decaimiento, partículas vuelven al centro, halo baja, colores regresan a fríos.
4. Estado actual
 Servidor con Node.js + Socket.IO.
 Control remoto (cambio de estados y parámetros).
 Mobile1 (acelerómetro, giroscopio, touch).
 Mobile2 (orientación, mag simulado, touch).
 Webclient (mouse, teclado, audio, video).
 Desktop visual integrando los 3 clientes + control.
Próximo paso: refinar cada estado con visuales específicos y pulir las transiciones narrativas.

Diseño Técnico Detallado 🎛️🎨
1. Especificación técnica por estado
Estado 0 — Exploración 🌌
Algoritmo generativo: órbitas con ruido Perlin + halo suave.
Elementos visuales clave:
Círculos orbitando el centro.
Paleta fría (azules, violetas suaves).
Halo pulsante según audio ambiente.
Mapeo detallado de datos:
Mobile1 (gyro) → posición del centro.
Mobile1 (accel) → amplitud de respiración secundaria.
Mobile1 (touch) → cambio de color/acento.
Mobile2 (orientación) → parallax ligero del borde.
Mobile2 (mag sim.) → grosor de los bordes.
Webclient (mouse) → atracción del organismo.
Webclient (audio) → intensidad del halo.
Control remoto (a,b,c) → a: tamaño base, b: lerp hacia mouse, c: transparencia.
Estado 1 — Tensión ⚡
Algoritmo generativo: deformación de rejilla con funciones seno/coseno, amplitud modulada por audio.
Elementos visuales clave:
Rejilla vibrante que se comprime.
Paleta intermedia (morados, ámbar).
Contraste visual creciente.
Mapeo detallado de datos:
Mobile1 (gyro) → inclinación global de la rejilla.
Mobile1 (accel) → intensidad de vibración.
Mobile1 (touch) → aumenta contraste.
Mobile2 (orientación) → skew lateral de la rejilla.
Mobile2 (mag sim.) → densidad de celdas.
Webclient (mouse) → desplaza la cresta de la rejilla.
Webclient (audio) → amplitud principal de la onda.
Control remoto (a,b,c) → a: amplitud base, b: rigidez (menos deformación), c: contraste global.
Estado 2 — Catarsis 🌟
Algoritmo generativo: sistema de partículas lanzadas con dirección + explosiones periódicas según audio.
Elementos visuales clave:
Nubes de partículas expansivas.
Halos intensos ligados a videoMotion (cámara).
Paleta cálida (rojos, naranjas brillantes).
Mapeo detallado de datos:
Mobile1 (gyro) → dirección inicial de partículas.
Mobile1 (accel) → energía/velocidad de partículas.
Mobile1 (touch) → activa estelas.
Mobile2 (orientación) → viento extra que guía partículas.
Mobile2 (mag sim.) → fuerza de campo magnético.
Webclient (mouse) → foco de atracción de partículas.
Webclient (audio) → explosiones periódicas.
Webclient (videoMotion) → tamaño del halo global.
Control remoto (a,b,c) → a: cantidad de partículas, b: velocidad, c: tamaño.
2. Tablas de mapeo técnico detallado
Estado 0 — Exploración
Cliente	Dato técnico	Parámetro visual	Rango de transformación
Móvil 1	Gyro β,γ	Centro (cx,cy)	-90..90 → -0.4..+0.4 pantalla
Móvil 1	Accel z	Amplitud halo	0..20 → 0..80 px
Móvil 1	Touch	Color/acento	false/true → azul/blanco
Móvil 2	Orientación β,γ	Parallax borde	-30..30 → -20..+20 px
Móvil 2	Mag sim.	Grosor borde	0..20 → 1..10 px
Webclient	Mouse x,y	Atracción centro	0..width/height
Webclient	AudioLevel	Intensidad halo	0..1 → 0..100% alpha
Remoto	a	Tamaño base	0..1 → 60..180 px
Remoto	b	Lerp mouse	0..1 → 0..0.2
Remoto	c	Transparencia	0..1 → 50..255 alpha
Estado 1 — Tensión
Cliente	Dato técnico	Parámetro visual	Rango de transformación
Móvil 1	Gyro β	Inclinación rejilla	-90..90 → -20..20°
Móvil 1	Accel z	Intensidad vibración	0..20 → 0..50 px
Móvil 1	Touch	Contraste	false/true → +30%
Móvil 2	Orientación γ	Skew rejilla	-30..30 → -0.3..0.3
Móvil 2	Mag sim.	Densidad celdas	0..20 → 10..50 líneas
Webclient	Mouse x	Desplazamiento onda	0..width → -PI..PI
Webclient	AudioLevel	Amplitud onda	0..1 → 0..100 px
Remoto	a	Amplitud base	0..1 → 0..50 px
Remoto	b	Rigidez	0..1 → más recta/curva
Remoto	c	Contraste global	0..1 → 50..200%
Estado 2 — Catarsis
Cliente	Dato técnico	Parámetro visual	Rango de transformación
Móvil 1	Gyro β,γ	Dirección partículas	-90..90 → -45..+45°
Móvil 1	Accel	Velocidad	0..20 → 1..10 px/frame
Móvil 1	Touch	Estelas	false/true → activadas
Móvil 2	Orientación β,γ	Viento	-30..30 → -5..+5 px/frame
Móvil 2	Mag sim.	Fuerza campo	0..20 → 0..1
Webclient	Mouse x,y	Foco atracción	0..width/height
Webclient	AudioLevel	Explosiones	0..1 → spawnear bursts
Webclient	VideoMotion	Tamaño halo	0..1 → 0..200 px extra
Remoto	a	Cantidad	0..1 → 100..1000 partículas
Remoto	b	Velocidad base	0..1 → 1..15 px/frame
Remoto	c	Tamaño partícula	0..1 → 2..12 px
3. Diseño de transiciones narrativas
0 → 1 (Exploración → Tensión):

Interpolar paleta de fría a intermedia.
Aumentar densidad de líneas progresivamente (300–500 ms).
Contraste crece con audioLevel.
Trigger: botón del control o threshold de audio.
1 → 2 (Tensión → Catarsis):

Flash breve (opacidad máxima + blanco).
Rejilla se disuelve en partículas.
Halo se intensifica con motion de cámara.
Trigger: control remoto o pico de audio.
2 → 0 (Catarsis → Exploración):

Partículas convergen al centro.
Desaturación progresiva.
Halo decrece.
Trigger: control remoto o inactividad prolongada (sin inputs > 5s).
✅ Checklist de implementación
 Especificación clara por estado (algoritmo + elementos + mapeo).
 Tablas de mapeo técnico detallado.
 Diseño de transiciones narrativas coherente.
