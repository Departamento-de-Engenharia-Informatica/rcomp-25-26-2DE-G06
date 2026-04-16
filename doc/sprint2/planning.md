RCOMP 2025-2026 Project 1 - Sprint 2 planning
===========================================
### Sprint master:  1240895###

# 1. Sprint's backlog #

>  T.2.1 - Development of a layer two and layer three Packet Tracer simulation for Terminal 2 encompassed floors, the campus backbone, and including the internet connection.

>  T.2.2 - Development of a layer two and layer three Packet Tracer simulation for Terminal 3 encompassed floors and the campus backbone. Integration of every member’s Packet Tracer simulation into a single simulation.

>  T.2.3 - Development of a layer two and layer three Packet Tracer simulation for Terminal 5 encompassed floors and the campus backbone.

>  T.2.4 - Development of a layer two and layer three Packet Tracer simulation for Terminal 4 encompassed floors and the campus backbone.

### Nota sobre a tarefa T.1.4
A tarefa T.2.3 (Terminal 5) é ignorada por a equipa possuir apenas 3 elementos, e ter sido acordado anteriormente com a docente da unidade curricular que a tarefa T.1.4 (Terminal 4) seria continuada pelo estudante 1240895, mesmo após a reorganização dos grupos.

# 2. Technical decisions and coordination #

#### Decisions: ####
* Packet Tracer Version: 9.0.0810 para garantir compatibilidade na integração final.
* VTP Domain: rc2526deg6
* VTP Mode: Pelo menos um switch em modo Server e os restantes em modo Client.
* VLAN IDs Range: 773 a 793.
* Spanning Tree Protocol (STP): Mantido ativo por defeito em todos os switches para gerir links redundantes.
* Routing: Static routing em todos os routers (modelo 2811 obrigatoriamente).
* ISP router’s IPv4 node address: 89.73.67.134/30
* IPv4 address space to use (Block of IPv4 addresses): 10.63.128.0/17

---

## 2.1 VLAN Database (Global)

Todas as VLANs deverão ser configuradas em todos os switches (via VTP ou manualmente), permitindo comunicação através de ligações **trunk**.

| VLAN Name      | VLAN ID | Finalidade                          |
|----------------|---------|-------------------------------------|
| CampusBackbone | 773     | Tráfego entre routers dos terminais |
| SwitchesDMZ    | 774     | Gestão remota dos switches          |
| T2-UserOutlets | 775     | Tomadas de utilizador do Terminal 2 |
| T2-WiFi        | 776     | Rede Wireless do Terminal 2         |
| T2-VoIP        | 777     | Telefonia IP do Terminal 2          |
| T2-ServersDMZ  | 778     | Servidores do Terminal 2            |
| T3-UserOutlets | 779     | Tomadas de utilizador do Terminal 3 |
| T3-WiFi        | 780     | Rede Wireless do Terminal 3         |
| T3-VoIP        | 781     | Telefonia IP do Terminal 3          |
| T3-ServersDMZ  | 782     | Servidores do Terminal 3            |
| T4-UserOutlets | 783     | Tomadas de utilizador do Terminal 4 |
| T4-WiFi        | 784     | Rede Wireless do Terminal 4         |
| T4-VoIP        | 785     | Telefonia IP do Terminal 4          |
| T4-ServersDMZ  | 786     | Servidores do Terminal 4            |


## Justification of VLAN ID selection

A atribuição dos VLAN IDs segue requisitos técnicos definidos pelo enunciado do projeto e não resulta de uma escolha arbitrária.

### Cumprimento do intervalo obrigatório

- O enunciado define para o Grupo 6 da Turma 2DE o intervalo obrigatório de VLAN IDs:
  - **773 a 793**
- Todos os VLAN IDs utilizados no Terminal 2 encontram-se dentro deste intervalo, garantindo conformidade com as regras globais do projeto.

### Consistência global do campus

- A utilização de um intervalo comum entre todos os terminais (T2, T3, T4) assegura:
  - integração sem conflitos na fase final (`campus.pkt`)
  - compatibilidade com VTP (VLAN Trunking Protocol)
  - propagação consistente das VLANs entre switches

### Segmentação funcional da rede

As VLANs foram organizadas por função lógica, permitindo:

- separação de tráfego por serviço (Users, WiFi, VoIP, Servers)
- redução de broadcast domains
- aumento da segurança lógica entre serviços
- melhor escalabilidade da infraestrutura

### VLAN de backbone e gestão

- VLAN 773 (CampusBackbone) foi reservada para interligação entre routers dos terminais
- VLAN 774 (SwitchesDMZ) foi reservada para gestão de equipamentos de rede

Esta separação garante isolamento entre:
- tráfego de gestão (administração de switches)
- tráfego de routing inter-terminal
- tráfego de utilizadores finais

---

## 2.2 IPv4 Addressing Plan (VLSM)

O bloco **10.63.128.0/17** foi dividido utilizando VLSM, começando pelas redes com maior número de nós para otimizar o espaço de endereçamento.

| VLAN / Rede    | Máscara | Endereço de Rede | Intervalo de IPs Válidos      | Nós Máx. |
|----------------|---------|------------------|-------------------------------|----------|
| T2-WiFi        | /21     | 10.63.128.0      | 10.63.128.1 - 10.63.135.254   | 2046     |
| T4-WiFi        | /21     | 10.63.136.0      | 10.63.136.1 - 10.63.143.254   | 2046     |
| T3-WiFi        | /21     | 10.63.144.0      | 10.63.144.1 - 10.63.151.254   | 2046     |
| T2-UserOutlets | /22     | 10.63.152.0      | 10.63.152.1 - 10.63.155.254   | 1022     |
| T3-UserOutlets | /22     | 10.63.156.0      | 10.63.156.1 - 10.63.159.254   | 1022     |
| T4-UserOutlets | /22     | 10.63.160.0      | 10.63.160.1 - 10.63.163.254   | 1022     |
| SwitchesDMZ    | /23     | 10.63.164.0      | 10.63.164.1 - 10.63.165.254   | 510      |
| T2-VoIP        | /23     | 10.63.166.0      | 10.63.166.1 - 10.63.167.254   | 510      |
| T4-VoIP        | /23     | 10.63.168.0      | 10.63.168.1 - 10.63.169.254   | 510      |
| T3-VoIP        | /23     | 10.63.170.0      | 10.63.170.1 - 10.63.171.254   | 510      |
| CampusBackbone | /24     | 10.63.172.0      | 10.63.172.1 - 10.63.172.254   | 254      |
| T3-ServersDMZ  | /24     | 10.63.173.0      | 10.63.173.1 - 10.63.173.254   | 254      |
| T4-ServersDMZ  | /25     | 10.63.174.0      | 10.63.174.1 - 10.63.174.126   | 126      |
| T2-ServersDMZ  | /25     | 10.63.174.128    | 10.63.174.129 - 10.63.174.254 | 126      |

## Justification of IPv4 addressing plan (VLSM)

O plano de endereçamento IPv4 foi desenvolvido com base no bloco atribuído:

- **10.63.128.0/17**

Foi aplicada a metodologia **VLSM (Variable Length Subnet Masking)** para otimizar a utilização do espaço de endereçamento.

---

### A) Dimensionamento por requisitos de nós

Cada sub-rede foi dimensionada de acordo com o número máximo de dispositivos previstos:

- T2-WiFi: ~2000 hosts → /21 (2046 hosts utilizáveis)
- T2-UserOutlets: ~1000 hosts → /22 (1022 hosts utilizáveis)
- SwitchesDMZ: ~410 hosts → /23 (510 hosts utilizáveis)
- Backbone: ~190 hosts → /24 (254 hosts utilizáveis)

- o **primeiro endereço** da sub-rede ser reservado para identificação da rede (Network Address)
- o **último endereço** ser reservado para broadcast

Assim, estes endereços não podem ser atribuídos a hosts.

A escolha das máscaras segue diretamente a fórmula:
2^(32 - N) - 2 ≥ número de hosts necessários

---

### B) Aplicação da regra VLSM (ordenação por tamanho)

O planeamento foi feito seguindo a regra fundamental do VLSM:

- ordenar redes da maior para a menor
- alocar blocos sequencialmente no espaço disponível
- evitar sobreposição de sub-redes

Isto permite:
- utilização eficiente do espaço IPv4
- eliminação de desperdício de endereços
- facilidade de expansão futura

---

### C) Prevenção de sobreposição entre terminais

O bloco /17 foi dividido entre T2, T3 e T4 de forma estruturada:

- cada terminal recebeu blocos contíguos
- não existem interseções entre sub-redes
- garante-se compatibilidade com routing estático entre routers

Exemplo:
- T2 WiFi → 10.63.128.0/21
- T3 WiFi → 10.63.144.0/21
- T4 WiFi → 10.63.136.0/21

---

### D) Eficiência e escalabilidade

A abordagem VLSM permite:

- crescimento futuro sem reconfiguração global
- adição de novas VLANs dentro do bloco /17
- manutenção simplificada da infraestrutura

---

## 2.3 Configurações Específicas de Nós

### VLAN Nativa
- VLAN 1 (default Cisco)

---

### Routers – Campus Backbone

| Router | Endereço IP |
|--------|-------------|
| Rtr-T2 | 10.63.172.1 |
| Rtr-T3 | 10.63.172.2 |
| Rtr-T4 | 10.63.172.3 |

---

### Ligação ao ISP (Terminal 2)

- Router T2 ligado ao ISP através de:
    - **Endereço:** 89.73.67.134/30
    - **Meio:** ligação DSL (modem)

---

### Gestão de Switches (SwitchesDMZ)

| Terminal | Intervalo de IP               |
|----------|-------------------------------|
| T2       | 10.63.164.1 – 10.63.164.100   |
| T3       | 10.63.164.101 – 10.63.164.200 |
| T4       | 10.63.165.1 – 10.63.165.100   |


### Device naming convention examples:
* Switches: 
* MC: MC-[Terminal]-[Level]-[Localização/Sala] (ex: MC-T2-L1-Sala6).
* HC: HC-[Terminal]-[Level]-[Localização/Sala] (ex: HC-T2-L1-Sala6).
* CP: CP-[Terminal]-[Level]-[Localização/Sala] (ex: CP-T2-L1-Sala6).
* Routers: Rtr-[Terminal] (ex: Rtr-T2).
* Hostnames: Devem coincidir com o Display Name mas sem espaços.

# 3. Subtasks assignment #

#### Distribution: ####
* 1240914- Development of the Packet Tracer simulation for Terminal 2
* 1241138 - Development of the Packet Tracer simulation for Terminal 3
* 1240895 - Development of the Packet Tracer simulation for Terminal 4

