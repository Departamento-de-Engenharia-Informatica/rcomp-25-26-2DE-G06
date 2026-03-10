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

![Medidas do level 4 - Departures](level4_departures/medidas_level4.png)

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
As dimensões reais do piso, após subtrair os comprimentos das salas que já possuem tomadas internas, são:
- **Level 1 – Arrivals:** 177,6 m × 177,6 m
- **Level 4 – Departures:** 175,9 m × 177,6 m

Aplicando a regra de uma tomada a cada 5 metros:

Outlets = $\left\lceil \frac{\text{Comprimento útil da parede}}{5} \right\rceil$

### Level 1 - Arrivals
![Medidas parede externa do level 1 - Arrivals](level1_arrivals/paredeExterna_level1.png)
- Parede direita: 36 tomadas
- Parede inferior: 36 tomadas
- Total paredes externas: 72 tomadas

### Level 2 - Departures
![Medidas parede externa do level 4 - Departures](level4_departures/paredeExterna_level4.png)
- Parede direita: 36 tomadas
- Parede inferior: 36 tomadas
- Total paredes externas: 72 tomadas


## Posicionamento das tomadas de rede 
### Level 1 - Arrivals
![Outlets do level 1 - Arrivals](level1_arrivals/level1_outlets.png)

### Level 4 - Departures
![Outlets do level 4 - Departures](level4_departures/level4_outlets.png)

---

## 5. Pontos de Acesso Wireless (Wi-Fi)

Ambos os pisos do Terminal 2 requerem cobertura de rede sem fios.

Cada Wireless Access Point (WAP) garante uma cobertura circular aproximada com **50 metros de diâmetro (25 metros de raio)**.

### Cálculo da Área de Cobertura

A área de cobertura de cada WAP pode ser estimada através da expressão:
A = πr² (Onde: r = 25 m)

Logo:
A ≈ 3.14 × 25² ≈ 1963 m²

### Número de WAPs Necessários

A área total aproximada de cada piso é:
200 × 200 = 40000 m²

Número mínimo de WAPs:
N = Área do piso / Área de cobertura do WAP
N = 40000 / 1963  
N ≈ **20.38**

Arredondando por excesso:
N ≈ **21 WAPs**

### Ajuste para Condições Reais

O valor anterior corresponde apenas a um cenário **teórico ideal**, assumindo cobertura circular perfeita e ausência de obstáculos.
Na prática, diversos fatores reduzem a eficácia da cobertura:

- Atenuação do sinal provocada por **paredes, pilares e estruturas metálicas**
- **Elevada densidade de utilizadores** típica de um terminal aeroportuário
- Necessidade de **sobreposição entre células Wi-Fi** para permitir roaming contínuo
- Planeamento de canais para evitar interferências entre pontos de acesso

Para compensar estes fatores, foi aplicado um **fator de segurança de aproximadamente 40%**.

Cálculo:
N_real = 21 × 1.4  
N_real ≈ **29.4**

Arredondando por excesso:
N_real ≈ **30 WAPs por piso**

### Posicionamento no teto falso

#### Level 1 - Arrivals
![Access Points do level 1 - Arrivals](level1_arrivals/level1_aps.png)

**Nota**: Cada WAP requer uma **tomada de rede RJ45 (ISO 8877)** instalada no **teto falso**, localizado a 4 metros de altura.

#### Level 4 - Departures
![Access Points do level 4 - Arrivals](level4_departures/level4_aps.png)

**Nota**: Cada WAP requer uma **tomada de rede RJ45 (ISO 8877)** instalada no **teto falso**, localizado a 4 metros de altura.


---

## 6. Localização dos Distribuidores de Rede

De acordo com as normas de cablagem estruturada, a distância máxima entre um Horizontal Cross-Connect (HC) e qualquer tomada não deve exceder **90 metros de cabo**, sendo recomendada uma distância máxima de **80 metros em linha reta**.
Considerando que cada piso do Terminal 2 possui aproximadamente **200 × 200 metros**, um único HC não seria suficiente para cobrir toda a área.
Assim, foram definidos **dois Horizontal Cross-Connects por piso**, dividindo cada nível em duas zonas de cobertura.

### Level 1 – Arrivals
Os HCs foram instalados nas seguintes salas:
- **HC-L1-A:** Sala 6
- **HC-L1-B:** Sala 11

Estas salas foram selecionadas por estarem identificadas no enunciado como adequadas para alojar equipamentos de infraestrutura.

### Level 4 – Departures
Os HCs foram instalados em:
- **HC-L4-A:** Sala 8
- **HC-L4-B:** Sala 17

Estas salas foram selecionadas por estarem identificadas no enunciado como adequadas para alojar equipamentos de infraestrutura.

### Intermediate Cross-Connect (IC)

O edifício Terminal 2 possui um **Intermediate Cross-Connect (IC)** responsável por interligar os distribuidores horizontais ao backbone da rede do aeroporto.
O IC foi instalado na **Sala 12 do Level 1**, devido à sua adequação para infraestrutura de rede e proximidade às passagens verticais de cabos.

### Backbone

A ligação entre os HCs e o IC é realizada através de **fibra ótica**, utilizando cabos com pelo menos **8 fibras**, garantindo capacidade suficiente e redundância.

### Cablagem Horizontal

A cablagem horizontal utiliza **cabos CAT7**, que percorrem as **calhas sob o piso (underfloor raceways)** e o **teto falso localizado a 4 metros de altura**, ligando os HCs às tomadas de rede e aos pontos de acesso wireless.

---
