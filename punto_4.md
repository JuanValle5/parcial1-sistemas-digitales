# Punto 4: Control Digital de Tres Tanques (Reemplazo de PLC) e Implementación con Diodos

## 1. Enunciado
Realizar un circuito digital que reemplazará un PLC para el control de tres tanques que poseen:
* **Tres sensores de temperatura:** $S_1, S_2, S_3$ ($1$ cuando $T > 50^\circ\text{C}$, $0$ cuando $T \le 50^\circ\text{C}$).
* **Dos sensores de nivel alto:** $N_2, N_3$ para los dos últimos tanques ($1$ = activo, $0$ = inactivo).
* **Tres alarmas:** $A_1, A_2, A_3$ ($1$ = encendida, $0$ = apagada).
* **Tres lámparas de advertencia (bombillos):** $B_1, B_2, B_3$ ($1$ = encendido, $0$ = apagado).

Simular los circuitos con **Quartus II** e implementar el circuito **con ayuda de diodos**.

---

## 2. Análisis de las Condiciones de Diseño (`a` – `m`)

Clasificando las 32 combinaciones según la cantidad de sensores de temperatura activos ($\#S \in \{0,1,2,3\}$) y sensores de nivel activos ($\#N \in \{0,1,2\}$):

| Regla | $\#S$ activos | $\#N$ activos | Descripción del comportamiento | Salidas activas |
| :---: | :---: | :---: | :--- | :--- |
| **a** | 0 | 0 | Todo menor a $50^\circ\text{C}$ y sin nivel alto | Todo apagado (`000 000`) |
| **f** | 0 | 1 (solo $N_3$) | Solo sensor de nivel 3 activo ($m_1$) | Solo $B_1 = 1$ |
| **g** | 0 | 1 (solo $N_2$) | Solo sensor de nivel 2 activo ($m_2$) | Solo $B_3 = 1$ |
| **h** | 0 | 2 ($N_2, N_3$) | Solo los dos sensores de nivel activos ($m_3$) | Todas: $A_1, A_2, A_3, B_1, B_2, B_3 = 1$ |
| **b** | 1 ($S_i$) | 0 | Solo 1 sensor de temp. $S_i$ activo y niveles apagados | Alarma $A_i = 1$ y bombillos opuestos $B_{j \neq i} = 1$ |
| **d** | 1 ($S_i$) | 1 ($N_2 \oplus N_3$) | Solo 1 sensor de temp. $S_i$ y 1 sensor de nivel activo | Alarma $A_i = 1$ y bombillo $B_i = 1$ |
| **e** | 1 ($S_i$) | 2 ($N_2, N_3$) | Solo 1 sensor de temp. $S_i$ y los 2 niveles activos | Solo bombillos opuestos $B_{j \neq i} = 1$ (Alarmas = 0) |
| **l** | 2 ($S_i, S_j$) | 0 | Solo 2 sensores de temp. activos y ningún nivel | Todas: $A_1, A_2, A_3, B_1, B_2, B_3 = 1$ |
| **j** | 2 ($S_i, S_j$) | 1 ($N_2 \oplus N_3$) | 2 sensores de temp. y 1 sensor de nivel activo | Solo la alarma opuesta $A_{k \neq i,j} = 1$ |
| **k** | 2 ($S_i, S_j$) | 2 ($N_2, N_3$) | 2 sensores de temp. y los 2 sensores de nivel activos | Solo los bombillos correspondientes $B_i = 1, B_j = 1$ |
| **c / i** | 3 ($S_1, S_2, S_3$) | 0 | Solo los 3 sensores de temp. activos ($m_{28}$) | Todas: $A_1, A_2, A_3, B_1, B_2, B_3 = 1$ |
| **m** | 3 ($S_1, S_2, S_3$) | 1 o 2 | En cualquier otro caso ($m_{29}, m_{30}, m_{31}$) | No prende nada (`000 000`)* |

> [!NOTE]
> **Nota sobre las condiciones `c`, `i` y `m` (filas $m_{29}, m_{30}, m_{31}$):**
> * Si se toma la condición **i** (*"Si **solo** los tres sensores de temperatura están activos..."*) junto con **m** (*"En cualquier otro caso no prende nada"*), entonces $m_{28} (11100)$ enciende todo y $m_{29}, m_{30}, m_{31}$ apagan todo (**Caso A: Estricto con regla `i` y `m`**).
> * Si se toma la condición **c** de forma general para cualquier estado con $S_1 S_2 S_3 = 111$ sin importar $N_2, N_3$, entonces $m_{28}, m_{29}, m_{30}, m_{31}$ encienden todo (**Caso B: General con regla `c`**).
> Abajo se presentan las ecuaciones reducidas para ambos casos (solo difieren en sumar el término $+ S_1 S_2 S_3$).

---

## 3. Tabla de Verdad Completa (32 Combinaciones)

| $m$ | $S_1$ | $S_2$ | $S_3$ | $N_2$ | $N_3$ | Regla | $A_1$ | $A_2$ | $A_3$ | $B_1$ | $B_2$ | $B_3$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **$m_0$** | 0 | 0 | 0 | 0 | 0 | **a** | 0 | 0 | 0 | 0 | 0 | 0 |
| **$m_1$** | 0 | 0 | 0 | 0 | 1 | **f** | 0 | 0 | 0 | **1** | 0 | 0 |
| **$m_2$** | 0 | 0 | 0 | 1 | 0 | **g** | 0 | 0 | 0 | 0 | 0 | **1** |
| **$m_3$** | 0 | 0 | 0 | 1 | 1 | **h** | **1** | **1** | **1** | **1** | **1** | **1** |
| **$m_4$** | 0 | 0 | 1 | 0 | 0 | **b** ($S_3$) | 0 | 0 | **1** | **1** | **1** | 0 |
| **$m_5$** | 0 | 0 | 1 | 0 | 1 | **d** ($S_3, N_3$) | 0 | 0 | **1** | 0 | 0 | **1** |
| **$m_6$** | 0 | 0 | 1 | 1 | 0 | **d** ($S_3, N_2$) | 0 | 0 | **1** | 0 | 0 | **1** |
| **$m_7$** | 0 | 0 | 1 | 1 | 1 | **e** ($S_3$) | 0 | 0 | 0 | **1** | **1** | 0 |
| **$m_8$** | 0 | 1 | 0 | 0 | 0 | **b** ($S_2$) | 0 | **1** | 0 | **1** | 0 | **1** |
| **$m_9$** | 0 | 1 | 0 | 0 | 1 | **d** ($S_2, N_3$) | 0 | **1** | 0 | 0 | **1** | 0 |
| **$m_{10}$** | 0 | 1 | 0 | 1 | 0 | **d** ($S_2, N_2$) | 0 | **1** | 0 | 0 | **1** | 0 |
| **$m_{11}$** | 0 | 1 | 0 | 1 | 1 | **e** ($S_2$) | 0 | 0 | 0 | **1** | 0 | **1** |
| **$m_{12}$** | 0 | 1 | 1 | 0 | 0 | **l** ($S_2, S_3$) | **1** | **1** | **1** | **1** | **1** | **1** |
| **$m_{13}$** | 0 | 1 | 1 | 0 | 1 | **j** ($S_2, S_3$) | **1** | 0 | 0 | 0 | 0 | 0 |
| **$m_{14}$** | 0 | 1 | 1 | 1 | 0 | **j** ($S_2, S_3$) | **1** | 0 | 0 | 0 | 0 | 0 |
| **$m_{15}$** | 0 | 1 | 1 | 1 | 1 | **k** ($S_2, S_3$) | 0 | 0 | 0 | 0 | **1** | **1** |
| **$m_{16}$** | 1 | 0 | 0 | 0 | 0 | **b** ($S_1$) | **1** | 0 | 0 | 0 | **1** | **1** |
| **$m_{17}$** | 1 | 0 | 0 | 0 | 1 | **d** ($S_1, N_3$) | **1** | 0 | 0 | **1** | 0 | 0 |
| **$m_{18}$** | 1 | 0 | 0 | 1 | 0 | **d** ($S_1, N_2$) | **1** | 0 | 0 | **1** | 0 | 0 |
| **$m_{19}$** | 1 | 0 | 0 | 1 | 1 | **e** ($S_1$) | 0 | 0 | 0 | 0 | **1** | **1** |
| **$m_{20}$** | 1 | 0 | 1 | 0 | 0 | **l** ($S_1, S_3$) | **1** | **1** | **1** | **1** | **1** | **1** |
| **$m_{21}$** | 1 | 0 | 1 | 0 | 1 | **j** ($S_1, S_3$) | 0 | **1** | 0 | 0 | 0 | 0 |
| **$m_{22}$** | 1 | 0 | 1 | 1 | 0 | **j** ($S_1, S_3$) | 0 | **1** | 0 | 0 | 0 | 0 |
| **$m_{23}$** | 1 | 0 | 1 | 1 | 1 | **k** ($S_1, S_3$) | 0 | 0 | 0 | **1** | 0 | **1** |
| **$m_{24}$** | 1 | 1 | 0 | 0 | 0 | **l** ($S_1, S_2$) | **1** | **1** | **1** | **1** | **1** | **1** |
| **$m_{25}$** | 1 | 1 | 0 | 0 | 1 | **j** ($S_1, S_2$) | 0 | 0 | **1** | 0 | 0 | 0 |
| **$m_{26}$** | 1 | 1 | 0 | 1 | 0 | **j** ($S_1, S_2$) | 0 | 0 | **1** | 0 | 0 | 0 |
| **$m_{27}$** | 1 | 1 | 0 | 1 | 1 | **k** ($S_1, S_2$) | 0 | 0 | 0 | **1** | **1** | 0 |
| **$m_{28}$** | 1 | 1 | 1 | 0 | 0 | **c / i** | **1** | **1** | **1** | **1** | **1** | **1** |
| **$m_{29}$** | 1 | 1 | 1 | 0 | 1 | **m** *(o **c**)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* |
| **$m_{30}$** | 1 | 1 | 1 | 1 | 0 | **m** *(o **c**)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* |
| **$m_{31}$** | 1 | 1 | 1 | 1 | 1 | **m** *(o **c**)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* | 0 *(1)* |

---

## 4. Reducción por Mapas de Karnaugh / Quine-McCluskey

### 4.1. Minitérminos de cada Salida
* **$A_1$:** $\sum m(3, 12, 13, 14, 16, 17, 18, 20, 24, 28)$ *(+$29, 30, 31$ en Caso B)*
* **$A_2$:** $\sum m(3, 8, 9, 10, 12, 20, 21, 22, 24, 28)$ *(+$29, 30, 31$ en Caso B)*
* **$A_3$:** $\sum m(3, 4, 5, 6, 12, 20, 24, 25, 26, 28)$ *(+$29, 30, 31$ en Caso B)*
* **$B_1$:** $\sum m(1, 3, 4, 7, 8, 11, 12, 17, 18, 20, 23, 24, 27, 28)$ *(+$29, 30, 31$ en Caso B)*
* **$B_2$:** $\sum m(3, 4, 7, 9, 10, 12, 15, 16, 19, 20, 24, 27, 28)$ *(+$29, 30, 31$ en Caso B)*
* **$B_3$:** $\sum m(2, 3, 5, 6, 8, 11, 12, 15, 16, 19, 20, 23, 24, 28)$ *(+$29, 30, 31$ en Caso B)*

### 4.2. Ecuaciones Reducidas en Suma de Productos (SOP) y Factorizadas

#### Caso A (Aplicando Regla `i` + `m`: $m_{29}, m_{30}, m_{31} = 0$)
1. **Alarma $A_1$:**
   $$A_1 = \overline{S_1}\overline{S_2}\overline{S_3}N_2N_3 + S_1\overline{N_2}\overline{N_3} + \overline{(N_2N_3)}\cdot\left(S_1\overline{S_2}\overline{S_3} + \overline{S_1}S_2S_3\right)$$
2. **Alarma $A_2$:**
   $$A_2 = \overline{S_1}\overline{S_2}\overline{S_3}N_2N_3 + S_2\overline{N_2}\overline{N_3} + \overline{(N_2N_3)}\cdot\left(\overline{S_1}S_2\overline{S_3} + S_1\overline{S_2}S_3\right)$$
3. **Alarma $A_3$:**
   $$A_3 = \overline{S_1}\overline{S_2}\overline{S_3}N_2N_3 + S_3\overline{N_2}\overline{N_3} + \overline{(N_2N_3)}\cdot\left(\overline{S_1}\overline{S_2}S_3 + S_1S_2\overline{S_3}\right)$$
4. **Bombillo $B_1$:**
   $$B_1 = (S_2 + S_3)\overline{N_2}\overline{N_3} + (\overline{S_1}\overline{S_3} + \overline{S_2}S_3 + S_2\overline{S_3})N_2N_3 + \overline{S_2}\overline{S_3}\overline{N_2}N_3 + S_1\overline{S_2}\overline{S_3}N_2\overline{N_3}$$
5. **Bombillo $B_2$:**
   $$B_2 = (S_1 + S_3)\overline{N_2}\overline{N_3} + (\overline{S_2}\overline{S_3} + \overline{S_1}S_3 + S_1\overline{S_3})N_2N_3 + \overline{S_1}S_2\overline{S_3}(N_2 \oplus N_3)$$
6. **Bombillo $B_3$:**
   $$B_3 = (S_1 + S_2)\overline{N_2}\overline{N_3} + (\overline{S_1}\overline{S_3} + S_1\overline{S_2} + \overline{S_1}S_2)N_2N_3 + \overline{S_1}\overline{S_2}N_2\overline{N_3} + \overline{S_1}\overline{S_2}S_3\overline{N_2}N_3$$

#### Caso B (Si $S_1 S_2 S_3 = 1$ activa todo sin importar $N_2, N_3$)
Simplemente se suma el término $+ S_1 S_2 S_3$ a cada una de las 6 ecuaciones anteriores (lo cual además simplifica $\overline{S_1}S_2S_3$ en $A_1$ a $S_2S_3$, $\overline{S_2}$ en $A_2$ a $S_1S_3$, y $\overline{S_3}$ en $A_3$ a $S_1S_2$).

---

## 5. Implementación del Circuito con Ayuda de Diodos (Matriz de Diodos)

### ¿Por qué implementar este punto con diodos?
Cada una de las 6 salidas ($A_1, A_2, A_3, B_1, B_2, B_3$) es la suma lógica (OR) de múltiples condiciones, y muchas condiciones activan **varias salidas simultáneamente** (por ejemplo, las reglas `h`, `l` y `c/i` encienden las **6 salidas a la vez**, y la regla `b` enciende **1 alarma y 2 bombillos**).

Usando **diodos `1N4148` (o `1N4007`) en configuración OR (Matriz de Diodos)**:
1. Se crean **6 líneas de bus verticales** en el protoboard: una para cada salida ($A_1, A_2, A_3, B_1, B_2, B_3$), cada una con una resistencia *pull-down* de $2.2\text{ k}\Omega$ a $\text{GND}$.
2. Cada línea de condición activa en alto (`1` lógico = $+5\text{ V}$) se conecta mediante el **ánodo** de un diodo hacia los buses de las salidas que debe encender (el **cátodo** —franja del diodo— va conectado al bus de la salida).
3. Los diodos bloquean el retorno de corriente entre distintas salidas (evitando cortocircuitos) y realizan todas las sumas OR sin gastar circuitos integrados `74LS32`.

### Tabla de Conexión de la Matriz de Diodos (15 Líneas de Acción $\rightarrow$ 6 Salidas)

Si agrupamos las condiciones que producen exactamente el mismo patrón de encendido, solo existen **15 líneas de acción** (o pueden tomarse directamente de las salidas de decodificadores / compuertas AND):

| Línea de Condición / Minitérminos | Expresión de la Línea | Diodo a $A_1$ | Diodo a $A_2$ | Diodo a $A_3$ | Diodo a $B_1$ | Diodo a $B_2$ | Diodo a $B_3$ |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **$L_1$** ($m_1$: Regla **f**) | $\overline{S_1}\overline{S_2}\overline{S_3}\overline{N_2}N_3$ | · | · | · | **D** | · | · |
| **$L_2$** ($m_2$: Regla **g**) | $\overline{S_1}\overline{S_2}\overline{S_3}N_2\overline{N_3}$ | · | · | · | · | · | **D** |
| **$L_3$** ($m_3, m_{12}, m_{20}, m_{24}, m_{28}$: **Todo ON**) | Reglas **h, l, c/i** | **D** | **D** | **D** | **D** | **D** | **D** |
| **$L_4$** ($m_4$: Regla **b** con $S_3$) | $\overline{S_1}\overline{S_2}S_3\overline{N_2}\overline{N_3}$ | · | · | **D** | **D** | **D** | · |
| **$L_5$** ($m_8$: Regla **b** con $S_2$) | $\overline{S_1}S_2\overline{S_3}\overline{N_2}\overline{N_3}$ | · | **D** | · | **D** | · | **D** |
| **$L_6$** ($m_{16}$: Regla **b** con $S_1$) | $S_1\overline{S_2}\overline{S_3}\overline{N_2}\overline{N_3}$ | **D** | · | · | · | **D** | **D** |
| **$L_7$** ($m_5, m_6$: Regla **d** con $S_3$) | $\overline{S_1}\overline{S_2}S_3(N_2 \oplus N_3)$ | · | · | **D** | · | · | **D** |
| **$L_8$** ($m_9, m_{10}$: Regla **d** con $S_2$) | $\overline{S_1}S_2\overline{S_3}(N_2 \oplus N_3)$ | · | **D** | · | · | **D** | · |
| **$L_9$** ($m_{17}, m_{18}$: Regla **d** con $S_1$) | $S_1\overline{S_2}\overline{S_3}(N_2 \oplus N_3)$ | **D** | · | · | **D** | · | · |
| **$L_{10}$** ($m_7, m_{27}$: Reglas **e/k** $\rightarrow B_1, B_2$) | $(\overline{S_1}\overline{S_2}S_3 + S_1S_2\overline{S_3})N_2N_3$ | · | · | · | **D** | **D** | · |
| **$L_{11}$** ($m_{11}, m_{23}$: Reglas **e/k** $\rightarrow B_1, B_3$) | $(\overline{S_1}S_2\overline{S_3} + S_1\overline{S_2}S_3)N_2N_3$ | · | · | · | **D** | · | **D** |
| **$L_{12}$** ($m_{15}, m_{19}$: Reglas **e/k** $\rightarrow B_2, B_3$) | $(S_1\overline{S_2}\overline{S_3} + \overline{S_1}S_2S_3)N_2N_3$ | · | · | · | · | **D** | **D** |
| **$L_{13}$** ($m_{13}, m_{14}$: Regla **j** $\rightarrow A_1$) | $\overline{S_1}S_2S_3(N_2 \oplus N_3)$ | **D** | · | · | · | · | · |
| **$L_{14}$** ($m_{21}, m_{22}$: Regla **j** $\rightarrow A_2$) | $S_1\overline{S_2}S_3(N_2 \oplus N_3)$ | · | **D** | · | · | · | · |
| **$L_{15}$** ($m_{25}, m_{26}$: Regla **j** $\rightarrow A_3$) | $S_1S_2\overline{S_3}(N_2 \oplus N_3)$ | · | · | **D** | · | · | · |

*(Donde **D** indica colocar un diodo `1N4148` con el **ánodo** en la línea $L_k$ y el **cátodo** en la columna de la salida respectiva).*

> [!TIP]
> **Tip de Montaje con Decodificadores (`74LS138`):**
> Observe que las entradas se dividen naturalmente en dos grupos pequeños:
> 1. **Estado de temperaturas $(S_1, S_2, S_3)$:** 3 bits $\rightarrow$ **1 decodificador 3 a 8** (`74LS138`).
> 2. **Estado de niveles $(N_2, N_3)$:** 2 bits $\rightarrow$ tres casos: $\overline{N_2}\overline{N_3}$ (0 niveles), $N_2 \oplus N_3$ (1 nivel) y $N_2N_3$ (2 niveles).
> Combinando ambos con compuertas y la matriz de diodos `1N4148`, el montaje en protoboard se reduce drásticamente en cableado.

---

## 6. Código VHDL para Simulación en Quartus II

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity punto4_tanques is
    Port (
        S1, S2, S3 : in  STD_LOGIC; -- Sensores de temperatura (>50°C = '1')
        N2, N3     : in  STD_LOGIC; -- Sensores de nivel alto
        A1, A2, A3 : out STD_LOGIC; -- Alarmas de los tanques 1, 2 y 3
        B1, B2, B3 : out STD_LOGIC  -- Bombillos de los tanques 1, 2 y 3
    );
end punto4_tanques;

architecture Behavioral of punto4_tanques is
    signal entradas : STD_LOGIC_VECTOR(4 downto 0);
    signal salidas  : STD_LOGIC_VECTOR(5 downto 0); -- (A1, A2, A3, B1, B2, B3)
begin
    entradas <= S1 & S2 & S3 & N2 & N3;

    with entradas select
        salidas <=
            "000000" when "00000", -- m0  (a)
            "000100" when "00001", -- m1  (f)
            "000001" when "00010", -- m2  (g)
            "111111" when "00011", -- m3  (h)
            "001110" when "00100", -- m4  (b)
            "001001" when "00101", -- m5  (d)
            "001001" when "00110", -- m6  (d)
            "000110" when "00111", -- m7  (e)
            "010101" when "01000", -- m8  (b)
            "010010" when "01001", -- m9  (d)
            "010010" when "01010", -- m10 (d)
            "000101" when "01011", -- m11 (e)
            "111111" when "01100", -- m12 (l)
            "100000" when "01101", -- m13 (j)
            "100000" when "01110", -- m14 (j)
            "000011" when "01111", -- m15 (k)
            "100011" when "10000", -- m16 (b)
            "100100" when "10001", -- m17 (d)
            "100100" when "10010", -- m18 (d)
            "000011" when "10011", -- m19 (e)
            "111111" when "10100", -- m20 (l)
            "010000" when "10101", -- m21 (j)
            "010000" when "10110", -- m22 (j)
            "000101" when "10111", -- m23 (k)
            "111111" when "11000", -- m24 (l)
            "001000" when "11001", -- m25 (j)
            "001000" when "11010", -- m26 (j)
            "000110" when "11011", -- m27 (k)
            "111111" when "11100", -- m28 (c / i)
            "000000" when others;  -- m29, m30, m31 (m)

    A1 <= salidas(5);
    A2 <= salidas(4);
    A3 <= salidas(3);
    B1 <= salidas(2);
    B2 <= salidas(1);
    B3 <= salidas(0);
end Behavioral;
```

---

## 7. Acondicionamiento de Corriente a la Salida de la Matriz de Diodos
Dado que cada diodo de silicio (`1N4148`) introduce una caída de tensión en directo de $V_D \approx 0.7\text{ V}$ y una salida TTL alta entrega alrededor de $3.4\text{ V}$, en el bus de cátodos se tendrá aproximadamente $3.4\text{ V} - 0.7\text{ V} = 2.7\text{ V}$.
* **Por qué es indispensable el transistor NPN:** $2.7\text{ V}$ con corriente limitada por las compuertas y múltiples LEDs encendidos al mismo tiempo (como en las reglas `h`, `l`, `c/i` donde se encienden los **6 LEDs simultáneamente**) sobrecargaría las compuertas si se conectaran los LEDs directo.
* **Conexión:** Cada uno de los 6 buses de cátodos ($A_1, A_2, A_3, B_1, B_2, B_3$) debe ir mediante una resistencia de base de $2.2\text{ k}\Omega$ a la base de un transistor **2N2222** (6 transistores en total, o un arreglo integrado **ULN2003A** / **ULN2803A** que ya incluye 7 u 8 drivers Darlington con sus resistencias de base en un solo chip DIP).
