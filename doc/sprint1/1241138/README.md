Sprint 1 - Terminal 3 - 1241138
===========================================
---

## Informações Gerais

O edifício do **Terminal 3** é composto por cinco pisos; no entanto, o presente projeto abrange apenas o **Piso 1 (Partidas)** e o **Piso 2 (Chegadas)**.

Ambos os pisos deverão garantir cobertura de rede local sem fios (**Wireless LAN – Wi-Fi**).

---

## 1. Requisitos Técnicos

### Level 1 - Departures
- Este piso dispõe de um canal técnico subterrâneo para cablagem, com pontos de acesso devidamente assinalados na planta.
- Existem quatro condutas verticais de cabos que percorrem todos os pisos do edifício. Num piso inferior, estas condutas verticais estabelecem ligação direta à passagem subterrânea exterior.
- A distância vertical entre este piso e o ponto de ligação à passagem subterrânea exterior é de **10 metros**.
- O teto do piso encontra-se a **5 metros do chão**, no entanto, existe um **teto falso a 4 metros**. O espaço técnico acima do teto falso é adequado para instalação de equipamentos de rede.

#### Tomadas de Rede
- As salas **2, 4, 5, 7, 8, 9, 10, 12, 13 e 14** deverão possuir o número padrão de tomadas de rede.
- As salas **1, 3, 6, 11 e 15** não necessitam de tomadas de rede.
- As salas **1, 11 e 15** são adequadas para a instalação de equipamentos de infraestrutura de rede.

#### Tomadas ao Longo das Paredes Exteriores
Ao longo:
- da parede exterior do lado esquerdo da planta;
- da parede exterior na parte superior da planta;

**Nota**: deverá ser instalada **uma tomada de rede (ISO 8877) a cada 5 metros**.


### Level 2 - Arrivals
- Tal como no Piso 1, existe um canal técnico subterrâneo para cablagem, com pontos de acesso assinalados na planta.
- Existem quatro condutas verticais de cabos que percorrem todos os pisos do edifício. Num piso inferior, estas condutas verticais estabelecem ligação direta à passagem subterrânea exterior.
- A distância vertical entre este piso e o ponto de ligação à passagem subterrânea exterior é de **16 metros**.
- O teto do piso encontra-se a **5 metros do chão**, existe um **teto falso a 4 metros**. O espaço técnico acima do teto falso é adequado para instalação de equipamentos de rede.

#### Tomadas de Rede
- As salas **2, 3, 4, 7, 10, 11, 12, 14, 15, 16, 17, 18 e 19** deverão possuir o número padrão de tomadas de rede.
- As salas **1, 5, 6, 8, 9, 13 e 20** não necessitam de tomadas de rede.
- As salas **1, 13 e 20** são adequadas para a instalação de equipamentos de infraestrutura de rede.

#### Tomadas ao Longo das Paredes Exteriores
Ao longo:
- da parede exterior do lado esquerdo da planta;
- da parede exterior na parte superior da planta;

**Nota**: deverá ser instalada **uma tomada de rede (ISO 8877) a cada 5 metros**.

---

## 2. Dimensionamento das Tomadas de Rede

O número de tomadas foi calculado de acordo com as normas de cablagem estruturada, considerando:

- **2 tomadas por cada 10 m²**
- Equivalente a:


Outlets = $\left\lceil \frac{\text{Área (m²)}}{10} \times 2 \right\rceil$
(**Nota**: Tudo arredondado por excesso.)


---

## 3. Medidas das Salas

### Level 1 - Departures

![Medidas do Level 1 - Departures](level1_departures/medidas_level1.png)

- O edifício tem aproximadamente 200 m x 200 m.

| Sala | Área (m²) | Número de Outlets  |
|------|-----------|--------------------|
| 1    | 112,64    | 0                  |
| 2    | 242,88    | 49                 |
| 3    | 374,40    | 0                  |
| 4    | 245,52    | 49                 |
| 5    | 252,20    | 51                 |
| 6    | 377,52    | 0                  |
| 7    | 245,52    | 49                 |
| 8    | 171,60    | 35                 |
| 9    | 208,56    | 42                 |
| 10   | 316,80    | 64                 |
| 11   | 94,09     | 0                  |
| 12   | 385,44    | 77                 |
| 13   | 416,00    | 84                 |
| 14   | 273,92    | 55                 |
| 15   | 128,00    | 0                  |

#### Total de Outlets – Level 1 (Salas)
**555 tomadas**


### Level 2 - Arrivals

![Medidas do Level 2 - Arrivals](level2_arrivals/medidas_level2.png)

- O edifício tem aproximadamente 200 m x 200 m.

| Sala | Área (m²) | Número de Outlets  |
|------|-----------|--------------------|
| 1    | —         | 0                  |
| 2    | —         | —                  |
| 3    | —         | —                  |
| 4    | —         | —                  |
| 5    | —         | 0                  |
| 6    | —         | 0                  |
| 7    | —         | —                  |
| 8    | —         | 0                  |
| 9    | —         | 0                  |
| 10   | —         | —                  |
| 11   | —         | —                  |
| 12   | —         | —                  |
| 13   | —         | 0                  |
| 14   | —         | —                  |
| 15   | —         | —                  |
| 16   | —         | —                  |
| 17   | —         | —                  |
| 18   | —         | —                  |
| 19   | —         | —                  |
| 20   | —         | 0                  |

#### Total de Outlets – Level 2 (Salas)
**— tomadas**

---

## 4. Tomadas nas Paredes Exteriores para ambos os pisos

As medidas das paredes exteriores foram obtidas através da escala da planta (**231 px = 50 m**):

Metros = $\left\lfloor \frac{\text{pixels} \times 50}{231} \right\rfloor$

**Parede superior:** 927 px → $\frac{927 \times 50}{231}$ = **200,65 m**

Outlets = $\left\lceil \frac{200,65}{5} \right\rceil = \left\lceil 40,13 \right\rceil = 41$

**Parede esquerda:** 923 px → $\frac{923 \times 50}{231}$ = **199,78 m**

Outlets = $\left\lceil \frac{199,78}{5} \right\rceil = \left\lceil 39,96 \right\rceil = 40$

Assim:

- Parede superior: **41 tomadas**
- Parede esquerda: **40 tomadas**

Total por piso:

**81 tomadas adicionais**

---
## 5. Totais finais
| Piso    | Salas | Paredes | Total   |
| ------- | ----- | ------- | ------- |
| Level 1 | 555   | 81      | **636** |
| Level 2 | —     | —       | **—**   |