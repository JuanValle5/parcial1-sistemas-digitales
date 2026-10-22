# Lista Completa de Materiales para el Montaje Práctico (Puntos 1 al 7)

Esta lista está organizada para que puedas ir directamente a la tienda de electrónica. Incluye dos columnas de cantidades:
1. **Cantidad Reutilizando (Mínima recomendada):** Si armas y pruebas un circuito (o un par de circuitos) a la vez reutilizando los integrados y LEDs (incluye unidades extra de repuesto).
2. **Cantidad Simultánea (Todo armado a la vez):** Si deseas llevar **los 7 puntos armados al mismo tiempo** sin desarmar ninguno.

---

## 1. Circuitos Integrados Digitales (Tecnología TTL Serie 74LS o CMOS 74HC)

> [!TIP]
> **Consejo al comprar:** Pide todos los integrados de la misma familia (preferiblemente **74LSxx** o **74HCxx**). Los precios son muy bajos, por lo que siempre conviene llevar 1 unidad adicional de `74LS86`, `74LS08` y `74LS04` por si algún pin se daña durante el montaje.

| Referencia | Descripción | En qué puntos se usa | Cant. Reutilizando (con repuesto) | Cant. para dejar los 7 armados a la vez |
| :--- | :--- | :--- | :---: | :---: |
| **74LS00** | 4 Compuertas **NAND** de 2 entradas | Punto 1 (y opc. Punto 5) | **2 uds.** | **2 uds.** |
| **74LS04** | 6 Inversores (**NOT**) | Puntos 2, 4, 5 y 7 | **3 uds.** | **5 uds.** |
| **74LS08** | 4 Compuertas **AND** de 2 entradas | Puntos 2, 3, 4 y 5 | **4 uds.** | **6 uds.** |
| **74LS32** | 4 Compuertas **OR** de 2 entradas | Puntos 2, 3 y 5 | **2 uds.** | **3 uds.** |
| **74LS86** | 4 Compuertas **XOR** de 2 entradas | Puntos 4, 5, 6 y 7 | **6 uds.** | **9 uds.** |
| **74LS138** | Decodificador / Demultiplexor **3 a 8** | Puntos 4 y 7 | **2 uds.** | **3 uds.** |
| **ULN2003A** *(o ULN2803A)* | Arreglo de 7 *(u 8)* drivers Darlington con resistencias internas *(ahorra cablear 7 transistores y 7 resistencias por chip)* | Puntos 4 y 7 *(acondicionamiento de corriente de salidas)* | **2 uds.** | **2 uds.** |

*(Nota: Si prefieres usar solo transistores individuales `2N2222` en lugar del `ULN2003A`, no necesitas comprar el `ULN2003A`, pero el `ULN2003A` te ahorra muchísimo espacio y cables en los Puntos 4 y 7).*

---

## 2. Semiconductores Discretos (Diodos y Transistores)

| Componente | Especificación | Función en la práctica | Cant. Reutilizando | Cant. Todo Armado |
| :--- | :--- | :--- | :---: | :---: |
| **Diodos 1N4148** *(o 1N4007)* | Diodo rápido de señal (encapsulado DO-35) | Matriz de diodos (compuertas OR con diodos) del **Punto 4** | **40 uds.** *(venden por tiras)* | **40 uds.** |
| **Transistores 2N2222A** *(o 2N3904 / BC548)* | Transistor bipolar NPN (encapsulado TO-92) | Etapa de potencia (*driver* de corriente) para encender los LEDs sin sobrecargar las compuertas TTL | **10 uds.** *(o 15 si no usas ULN2003A)* | **15 uds.** *(o 25 si no usas ULN2003A)* |

---

## 3. Diodos LED (Indicadores de Salida y Estado)

| Color y Tamaño | Uso específico en los circuitos | Cant. Reutilizando | Cant. Todo Armado |
| :--- | :--- | :---: | :---: |
| **LEDs Rojos (5 mm o 3 mm)** | Punto 1 (1), Punto 2 (1), Punto 3 (1), Punto 4 Alarmas $A_1..A_3$ (3), Punto 5 (1), Punto 6 Error (1), Punto 7 Síndrome $C_4..C_1$ (3) | **10 uds.** | **15 uds.** |
| **LEDs Verdes (5 mm o 3 mm)** | Punto 2 (1), Punto 4 Bombillos $B_1..B_3$ (3), Punto 5 (1), Punto 6 OK (1), Punto 7 Datos corregidos $D_1..D_4$ (4) | **10 uds.** | **15 uds.** |
| **LEDs Amarillos / Azules (5 mm o 3 mm)** | Opcionales para diferenciar bombillos $B_1..B_3$ del Punto 4 o testigos de entrada | **6 uds.** | **10 uds.** |

---

## 4. Resistencias (todas a $1/4\text{ W}$, tolerancia $5\%$)

| Valor Óhmico | Código de Colores (4 bandas) | Función en el circuito | Cant. Reutilizando | Cant. Todo Armado |
| :--- | :--- | :--- | :---: | :---: |
| **$150\ \Omega$** | Marrón - Verde - Marrón - Dorado | Resistencia exigida en el diagrama del **Punto 1** | **2 uds.** | **2 uds.** |
| **$220\ \Omega$** *(o $330\ \Omega$)* | Rojo - Rojo - Marrón - Dorado | Resistencias limitadoras de corriente para todos los LEDs | **15 uds.** | **30 uds.** |
| **$1\text{ k}\Omega$** | Marrón - Negro - Rojo - Dorado | Resistencias *pull-down* a $\text{GND}$ para los DIP switches y pulsadores | **15 uds.** | **30 uds.** |
| **$2.2\text{ k}\Omega$** *(o $4.7\text{ k}\Omega$)* | Rojo - Rojo - Rojo - Dorado | Resistencias de base ($R_B$) para los transistores `2N2222` y *pull-down* de los buses de diodos del Punto 4 | **20 uds.** | **35 uds.** |

---

## 5. Elementos de Entrada / Conmutación

| Componente | Descripción | En qué puntos se usa | Cant. Reutilizando | Cant. Todo Armado |
| :--- | :--- | :--- | :---: | :---: |
| **Pulsadores (Pushbutton)** | Pulsador táctil normalmente abierto (4 pines o 2 pines para protoboard) | **Punto 2** (pulsadores $a, b, c$) | **4 uds.** (3 + 1 repuesto) | **4 uds.** |
| **DIP Switch de 8 posiciones** | Interruptor múltiple tipo piano o deslizable para protoboard | **Punto 6** (palabra de 8 bits), **Punto 7** (4 bits datos + 4 bits error), **Punto 4** (5 sensores) | **2 uds.** | **3 uds.** |
| **DIP Switch de 4 posiciones** | Interruptor múltiple de 4 canales | **Punto 1** (2 entradas), **Punto 3** (4 detectores), **Punto 5** (2 entradas) | **2 uds.** | **3 uds.** |

---

## 6. Alimentación, Protoboards y Cableado

| Material | Especificación / Recomendación | Cantidad sugerida |
| :--- | :--- | :---: |
| **Protoboards (Breadboards)** | Protoboard estándar de 830 puntos (con buses de alimentación dobles a ambos lados) | **2 a 3 protoboards** *(el Punto 4 y el Punto 7 ocupan aprox. 1 protoboard completo cada uno)* |
| **Alambre para protoboard (Calibre 22 AWG)** | Alambre sólido telefónico / UTP unifilar calibre **22 AWG** (varios colores: rojo para $+5\text{ V}$, negro/azul para $\text{GND}$, otros colores para señales) | **2 a 3 metros por color** *(o 1 caja de jumpers preformados en U + 1 paquete de jumpers macho-macho)* |
| **Fuente de $5\text{ V DC}$ regulada** | Cargador USB de $5\text{ V}$ con cable pelado, módulo **MB102** para protoboard, o regulador **LM7805** con batería de $9\text{ V}$ / adaptador de $9\text{ V}-12\text{ V}$ | **1 fuente de $5\text{ V}$** |
| **Capacitores cerámicos $100\text{ nF}$ (código `104`)** | Capacitores de desacoplo entre $V_{CC}$ y $\text{GND}$ cerca de los integrados (evitan ruidos y falsos disparos en el Punto 7 y Punto 4) | **5 uds.** |
| **Herramientas básicas** | Pelacables / cortafríos pequeño, pinza de punta fina (muy útil para insertar los 32 diodos del Punto 4) y multímetro digital | **1 juego** |

---

## 7. Resumen Rápido por Punto (Checklist de Montaje)

* **Punto 1:** `1x 74LS00`, 2 switches, 2 resistencias $1\text{ k}\Omega$, 1 transistor `2N2222`, 1 resistencia $2.2\text{ k}\Omega$, 1 resistencia $150\ \Omega$, 1 LED.
* **Punto 2:** `1x 74LS08`, `1x 74LS32`, `1x 74LS04`, 3 pulsadores, 3 resistencias $1\text{ k}\Omega$, 2 transistores `2N2222`, 2 resistencias $2.2\text{ k}\Omega$, 2 resistencias $220\ \Omega$, 1 LED Rojo, 1 LED Verde.
* **Punto 3:** `1x 74LS08`, `1x 74LS32`, DIP switch de 4, 4 resistencias $1\text{ k}\Omega$, 1 transistor `2N2222`, 1 resistencia $2.2\text{ k}\Omega$, 1 resistencia $220\ \Omega$, 1 LED Rojo.
* **Punto 4:** `1x 74LS138`, `2x 74LS08`, `1x 74LS86`, `1x 74LS04`, **32 diodos `1N4148`**, `1x ULN2003A` (o 6 `2N2222`), DIP switch de 8 (usa 5), 5 resistencias $1\text{ k}\Omega$, 6 resistencias $2.2\text{ k}\Omega$, 6 resistencias $220\ \Omega$, 3 LEDs Rojos ($A_1..A_3$) y 3 LEDs Verdes/Amarillos ($B_1..B_3$).
* **Punto 5:** `1x 74LS04`, `1x 74LS08`, `1x 74LS32` (o `1x 74LS00`) + `1x 74LS86`, 2 switches, 2 resistencias $1\text{ k}\Omega$, 2 transistores `2N2222`, 2 resistencias $2.2\text{ k}\Omega$, 2 resistencias $220\ \Omega$, 2 LEDs.
* **Punto 6:** `2x 74LS86`, DIP switch de 8, 8 resistencias $1\text{ k}\Omega$, 2 transistores `2N2222`, 2 resistencias $2.2\text{ k}\Omega$, 2 resistencias $220\ \Omega$, 1 LED Rojo, 1 LED Verde.
* **Punto 7:** `5x 74LS86`, `1x 74LS138`, `1x 74LS04`, `1x ULN2003A` (o 7 `2N2222`), DIP switch de 8 (4 datos + 4 error), 8 resistencias $1\text{ k}\Omega$, 7 resistencias $220\ \Omega$, 3 LEDs Rojos (Síndrome) y 4 LEDs Verdes (Palabra corregida).
