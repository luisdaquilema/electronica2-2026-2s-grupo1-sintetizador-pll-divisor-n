# Proyecto Intermedio: Sintetizador de Frecuencia con PLL (CD4046) y Divisor Programable (CD4017)

* **Asignatura:** Electrónica II (TEP220342) – Cuarto Semestre, Paralelo "A"
* **Docente:** Mgs. Giovanny Cuzco
* **Grupo:** Grupo 1
* **Proyecto asignado:** Sintetizador de frecuencia con PLL (CD4046) y divisor programable por N (CD4017)
* **Integrantes:**
  * William Saúl Torres Wilcapi (@WilliamTorres844)
  * Luis Fernando Daquilema Yambay
  * Juan José Vargas Núñez

## Objetivo del proyecto
Diseñar, simular e implementar en protoboard un sintetizador de frecuencia de lazo único con un oscilador de referencia de 50 kHz, un PLL CD4046 y un divisor programable CD4017 para generar salidas de 100 a 450 kHz ($f_{out} = N \cdot f_{ref}$, con $N$ de 2 a 9). El sistema permitirá verificar la exactitud con error relativo menor al 2 %, la resolución de paso y el tiempo de enganche.
## Principio de funcionamiento

Un PLL compara la fase de una señal de referencia con la de la salida del VCO (dividida entre N). El error de fase se filtra y se convierte en una tensión que corrige la frecuencia del VCO, hasta que ambas señales quedan enganchadas:

$$f_{out} = N \cdot f_{ref}$$

Con $f_{ref} = 50\ \text{kHz}$ y el divisor programable en la realimentación, cambiar N cambia la frecuencia de salida en pasos de 50 kHz.

## Diagrama de bloques

```
 Oscilador ──f_ref──► Comparador ──► Filtro ──► VCO ──┬──► f_out
 50 kHz               de fase       pasa-bajas  (CD4046)│
                      (CD4046)                         │
                          ▲                            │
                          └──── f_out / N ◄── CD4017 ◄─┘
                                          (divisor por N)
```

## Componentes principales

| Bloque | Dispositivo | Función |
|---|---|---|
| Referencia | Oscilador de 50 kHz | Frecuencia estable de comparación |
| Comparador de fase + VCO | CD4046 | Detecta el error de fase y genera $f_{out}$ |
| Divisor programable | CD4017 | Divide la salida del VCO entre N (2 a 9) |
| Filtro de lazo | RC pasa-bajas | Suaviza la tensión de control del VCO |

## Divisor por N con el CD4017

El CD4017 es un contador de década. Para dividir entre N, se conecta la salida $Q_N$ a la entrada RESET: el contador cuenta de 0 a N−1 y reinicia, entregando un pulso por cada N pulsos de entrada. Un selector (jumper o dip-switch) permite elegir $Q_2 \dots Q_9$.

## Frecuencias de salida esperadas

| N | $f_{out}$ (kHz) |
|---|---|
| 2 | 100 |
| 3 | 150 |
| 4 | 200 |
| 5 | 250 |
| 6 | 300 |
| 7 | 350 |
| 8 | 400 |
| 9 | 450 |

## Criterios de evaluación

- **Exactitud:** error relativo menor al 2 %:

$$e\% = \frac{|f_{medida} - f_{teórica}|}{f_{teórica}} \times 100$$

- **Resolución de paso:** 50 kHz (igual a $f_{ref}$).
- **Tiempo de enganche:** tiempo desde el cambio de N hasta que el PLL estabiliza $f_{out}$.

## Plan de trabajo

1. Cálculo de los componentes del VCO y del filtro de lazo (docs/).
2. Simulación del lazo completo para cada valor de N (src/).
3. Montaje en protoboard.
4. Mediciones con osciloscopio y comparación teórico vs. práctico (results/).

## Referencias

- Hojas de datos del CD4046 y del CD4017 (Texas Instruments).
- Guía de la práctica de Electrónica II, UNACH, 2026-2S.
