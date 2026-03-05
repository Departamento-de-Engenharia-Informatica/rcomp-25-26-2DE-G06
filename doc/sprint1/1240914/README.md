Sprint 1 - Terminal 2 - 1240914
===========================================
---

## Informações Gerais

O edifício do **Terminal 2** é composto por oito pisos; no entanto, o presente projeto abrange apenas o **Piso 1 (Chegadas)** e o **Piso 4 (Partidas)**.

Ambos os pisos deverão garantir cobertura de rede local sem fios (**Wireless LAN – Wi-Fi**).

---

## 1. Requisitos Técnicos

### Level 1 - Arrivals
- Este piso dispõe de um canal técnico subterrâneo para cablagem, com pontos de acesso devidamente assinalados na planta.
- Existem quatro condutas verticais de cabos que percorrem todos os pisos do edifício. Num piso inferior, estas condutas verticais estabelecem ligação direta à passagem subterrânea exterior.
- A distância vertical entre este piso e o ponto de ligação à passagem subterrânea exterior é de **10 metros**.
- O teto do piso encontra-se a **5 metros do chão**, no entanto, existe um **teto falso a 4 metros**. O espaço técnico acima do teto falso é adequado para instalação de equipamentos de rede.

#### Tomadas de Rede
- As salas **1, 2, 3, 4, 5, 7 e 8** deverão possuir o número padrão de tomadas de rede.
- As salas **6, 9, 10, 11 e 12** não necessitam de tomadas de rede.
- As salas **6, 11 e 12** são adequadas para a instalação de equipamentos de infraestrutura de rede.

#### Tomadas ao Longo das Paredes Exteriores
Ao longo:
- da parede exterior do lado direito da planta;
- da parede exterior na parte inferior da planta;

**Nota**: deverá ser instalada **uma tomada de rede (ISO 8877) a cada 5 metros**.


### Level 4 - Departures
- Tal como no Piso 1, existe um canal técnico subterrâneo para cablagem, com pontos de acesso assinalados na planta.
- Existem quatro condutas verticais de cabos que percorrem todos os pisos do edifício. Num piso inferior, estas condutas verticais estabelecem ligação direta à passagem subterrânea exterior.
- A distância vertical entre este piso e o ponto de ligação à passagem subterrânea exterior é de **25 metros**.
- O teto do piso encontra-se a **5 metros do chão**, existe um **teto falso a 4 metros**. O espaço técnico acima do teto falso é adequado para instalação de equipamentos de rede.

#### Tomadas de Rede
- As salas **1, 2, 3, 4, 5, 6, 7, 9, 10, 11, 12, 13 e 14** deverão possuir o número padrão de tomadas de rede.
- As salas **8, 15, 16, 17 e 18** não necessitam de tomadas de rede.
- As salas **8, 17 e 18** são adequadas para a instalação de equipamentos de infraestrutura de rede.

#### Tomadas ao Longo das Paredes Exteriores
Ao longo:
- da parede exterior do lado direito da planta;
- da parede exterior na parte inferior da planta;

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

### Level 1 - Arrivals

![Medidas do Level 1 - Arrivals](level1_arrivals/medidas_level1.png)

- A sala tem 201,72 m x 201,72 m.

| Sala | Área (m²) | Número de Outlets |
|------|-----------|-------------------|
| 1    | 380,88    | 77                |
| 2    | 309,12    | 62                |
| 3    | 476,10    | 96                |
| 4    | 547,86    | 110               |
| 5    | 476,10    | 96                |
| 6    | 104,06    | 0                 |
| 7    | 474,72    | 95                |
| 8    | 474,72    | 95                |
| 9    | 655,50    | 0                 |
| 10   | 655,50    | 0                 |
| 11   | 146,41    | 0                 |
| 12   | 146,41    | 0                 |

#### Total de Outlets – Level 1 (Salas)
**631 tomadas**


### Level 4 - Departures

![Medidas do level 2 - Departures](level4_departures/medidas_level4.png)

- A sala tem 201,72 m x 201,72 m.

| Sala | Área (m²) | Número de Outlets |
|------|-----------|-------------------|
| 1    | 309,12    | 62                |
| 2    | 309,12    | 62                |
| 3    | 262,20    | 53                |
| 4    | 262,20    | 53                |
| 5    | 547,86    | 110               |
| 6    | 476,10    | 96                |
| 7    | 499,56    | 100               |
| 8    | 118,68    | 0                 |
| 9    | 262,20    | 53                |
| 10   | 380,88    | 77                |
| 11   | 262,20    | 53                |
| 12   | 262,20    | 53                |
| 13   | 262,20    | 53                |
| 14   | 190,44    | 39                |
| 15   | 524,40    | 0                 |
| 16   | 524,40    | 0                 |
| 17   | 146,41    | 0                 |
| 18   | 146,41    | 0                 |

#### Total de Outlets – Level 4 (Salas)
**864 tomadas**

---

## 4. Tomadas nas Paredes Exteriores para ambos os pisos

O comprimento total de cada parede exterior é de **201,72 metros**.

Aplicando a regra de uma tomada a cada **5 metros**:

Outlets = $\left\lceil \frac{201.72}{5} \right\rceil$

Outlets = $\left\lceil 40.34 \right\rceil = 41$

Assim:

- Parede direita: **41 tomadas**
- Parede inferior: **41 tomadas**

Total por piso:

**82 tomadas adicionais**

---
## 5. Totais finais
| Piso    | Salas | Paredes | Total   |
| ------- | ----- | ------- | ------- |
| Level 1 | 631   | 82      | **713** |
| Level 4 | 864   | 82      | **946** |
