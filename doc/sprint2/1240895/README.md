# RCOMP 2025-2026 - Projeto 1 - Sprint 2

**Terminal 4 - 1240895**

---

## 1. Preparação

| **Parâmetro** | **Valor** |
| --- | --- |
| Software utilizado | Cisco Packet Tracer 9.0.0810 |
| Terminal | T4 |
| VTP Domain | rc2526deg6 |
| Bloco IPv4 | 10.63.128.0/17 |
| Estudante | 1240895 |

---

## 2. Convenção de Nomes

### 2.1 Switches

| **Tipo** | **Exemplo** |
| --- | --- |
| MC | MC |
| IC | IC-T4 |
| HC | HC-T4-L0-Sala4 / HC-T4-L2-Sala3 / HC-T4-L2-Sala3(2) |
| CP | CP-T4-L0-SalaX / CP-T4-L2-SalaX |

### 2.2 Router

- Rtr-T4

> Nota: Os hostnames coincidem com o Display Name (sem espaços).

---

## 3. Construção da Topologia

### 3.1 Equipamentos utilizados

- 1 Router Cisco 2811 (Rtr-T4)
- Switches modelo PT-Empty:
  - 1 IC (IC-T4)
  - 3 HC (HC-T4-L0-Sala4, HC-T4-L2-Sala3, HC-T4-L2-Sala3(2))
  - 23 CP (9 no Level 0, 14 no Level 2)

> Nota: O switch PT-Empty suporta apenas 10 portas. Como o Level 2 possui 14 CPs, foi necessário adicionar um segundo HC no Level 2 (HC-T4-L2-Sala3(2)) para suportar todas as ligações.

### 3.2 Módulos adicionados

Em cada switch PT-Empty:

1. Aba **Physical**
2. Desligar o switch
3. Adicionar módulo **PT-SWITCH-NM-1FGE** (fibra)
4. Ligar novamente

### 3.3 Ligações

- Backbone (IC → HC) → Fibra Ótica monomode
- Ligações horizontais (CP → end devices) → Cobre CAT7

---

## 4. Configuração do MC (Switch Principal)

O MC é o switch central do campus, configurado em modo VTP Server, responsável por criar e propagar todas as VLANs para os restantes switches.

### 4.1 Hostname e VTP

```
enable
configure terminal
hostname MC
vtp domain rc2526deg6
vtp mode server
vtp version 2
```

### 4.2 Criação das VLANs (no MC)

| **VLAN ID** | **Nome** | **Finalidade** |
| --- | --- | --- |
| 773 | CampusBackbone | Tráfego entre routers dos terminais |
| 774 | SwitchesDMZ | Gestão remota dos switches |
| 775 | T2-UserOutlets | Tomadas de utilizador do Terminal 2 |
| 776 | T2-WiFi | Rede Wireless do Terminal 2 |
| 777 | T2-VoIP | Telefonia IP do Terminal 2 |
| 778 | T2-ServersDMZ | Servidores do Terminal 2 |
| 779 | T3-UserOutlets | Tomadas de utilizador do Terminal 3 |
| 780 | T3-WiFi | Rede Wireless do Terminal 3 |
| 781 | T3-VoIP | Telefonia IP do Terminal 3 |
| 782 | T3-ServersDMZ | Servidores do Terminal 3 |
| 783 | T4-UserOutlets | Tomadas de utilizador do Terminal 4 |
| 784 | T4-WiFi | Rede Wireless do Terminal 4 |
| 785 | T4-VoIP | Telefonia IP do Terminal 4 |
| 786 | T4-ServersDMZ | Servidores do Terminal 4 |

```
vlan 773
 name CampusBackbone
vlan 774
 name SwitchesDMZ
vlan 775
 name T2-UserOutlets
vlan 776
 name T2-WiFi
vlan 777
 name T2-VoIP
vlan 778
 name T2-ServersDMZ
vlan 779
 name T3-UserOutlets
vlan 780
 name T3-WiFi
vlan 781
 name T3-VoIP
vlan 782
 name T3-ServersDMZ
vlan 783
 name T4-UserOutlets
vlan 784
 name T4-WiFi
vlan 785
 name T4-VoIP
vlan 786
 name T4-ServersDMZ
exit
```

---

## 5. Configuração dos Restantes Switches (IC, HC, CP)

Todos os switches exceto o MC são configurados em modo VTP Client, recebendo automaticamente as VLANs criadas no MC.

```
enable
configure terminal
hostname [nome do switch]
vtp domain rc2526deg6
vtp mode client
vtp version 2
```

---

## 6. Configuração de Trunk

Os trunks permitem transportar tráfego de múltiplas VLANs entre switches. São configurados em todas as ligações entre switches da hierarquia: MC → IC → HC → CP.

### 6.1 MC ↔ IC-T4

No MC (porta para IC-T4):

```
interface GigabitEthernet1/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 switchport trunk native vlan 1
 no shutdown
```

No IC-T4 (porta para MC):

```
interface GigabitEthernet8/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 switchport trunk native vlan 1
 no shutdown
```

### 6.2 IC-T4 ↔ HC-T4-L0-Sala4

No IC-T4:

```
interface GigabitEthernet9/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 switchport trunk native vlan 1
 no shutdown
```

No HC-T4-L0-Sala4:

```
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 switchport trunk native vlan 1
 no shutdown
```

### 6.3 IC-T4 ↔ HC-T4-L2-Sala3 e HC-T4-L2-Sala3(2)

Mesma lógica de trunk em ambos os lados de cada ligação, com `switchport trunk allowed vlan 773-786`.

### 6.4 HC → CP (uplinks)

Todas as ligações HC → CP são configuradas como trunk em ambos os lados:

```
interface GigabitEthernetX/X
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 switchport trunk native vlan 1
 no shutdown
```

### 6.5 Porta do IC-T4 para o Rtr-T4 (CampusBackbone)

A porta do IC-T4 ligada ao Rtr-T4 é configurada como trunk para suportar o router-on-a-stick:

```
interface FastEthernet7/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 switchport trunk native vlan 1
 no shutdown
```

### 6.6 Verificação

```
show interfaces trunk
```

Resultado esperado: todas as portas em estado `trunking`, modo `on`, VLANs `773-786` permitidas.

---

## 7. Gestão dos Switches (SwitchesDMZ - VLAN 774)

Cada switch possui uma interface virtual (SVI) associada à VLAN 774, permitindo gestão remota. A VLAN 774 é utilizada exclusivamente para administração e não possui gateway Layer 3 no router, garantindo o seu isolamento.

| **Switch** | **IP de Gestão** | **Máscara** |
| --- | --- | --- |
| IC-T4 | 10.63.165.1 | 255.255.254.0 |
| HC-T4-L0-Sala4 | 10.63.165.2 | 255.255.254.0 |
| HC-T4-L2-Sala3 | 10.63.165.3 | 255.255.254.0 |
| HC-T4-L2-Sala3(2) | 10.63.165.4 | 255.255.254.0 |

Configuração aplicada em cada switch:

```
interface vlan 774
 ip address 10.63.165.X 255.255.254.0
 no shutdown
```

---

## 8. Configuração de Layer 3 (Router-on-a-Stick)

O encaminhamento entre VLANs é realizado pelo router Cisco 2811 (Rtr-T4) através da técnica Router-on-a-Stick, utilizando sub-interfaces com encapsulamento IEEE 802.1Q na FastEthernet0/0.

### 8.1 Ativação da interface física

```
enable
configure terminal
interface FastEthernet0/0
 no ip address
 no shutdown
```

### 8.2 Sub-interfaces (VLANs do Terminal 4)

| **Sub-interface** | **VLAN** | **IP Gateway** | **Máscara** |
| --- | --- | --- | --- |
| Fa0/0.773 | 773 - CampusBackbone | 10.63.172.3 | 255.255.255.0 |
| Fa0/0.783 | 783 - T4-UserOutlets | 10.63.160.1 | 255.255.252.0 |
| Fa0/0.784 | 784 - T4-WiFi | 10.63.136.1 | 255.255.248.0 |
| Fa0/0.785 | 785 - T4-VoIP | 10.63.168.1 | 255.255.254.0 |
| Fa0/0.786 | 786 - T4-ServersDMZ | 10.63.174.1 | 255.255.255.128 |

Exemplo de configuração de uma sub-interface:

```
interface FastEthernet0/0.783
 encapsulation dot1Q 783
 ip address 10.63.160.1 255.255.252.0
 no shutdown
```

> Nota: A VLAN 774 (SwitchesDMZ) não possui sub-interface no router, garantindo o seu isolamento. O acesso a esta VLAN é apenas de camada 2.

---

## 9. Configuração dos End Devices

### 9.1 Equipamentos adicionados

- 2 PCs utilizadores (VLAN 783)
- 2 PCs gestão DMZ (VLAN 774)
- 2 Laptops WiFi (VLAN 784)
- 2 Access Points (VLAN 784)
- 2 Servers (VLAN 786)
- 2 Telefones VoIP 7960 (VLAN 785)

### 9.2 Configuração das portas nos CPs

Cada porta do CP é configurada em modo access na VLAN correspondente ao dispositivo ligado:

```
interface FastEthernetX/X
 switchport mode access
 switchport access vlan [VLAN ID]
 no shutdown
```

Para telefones VoIP:

```
interface FastEthernetX/X
 switchport mode access
 switchport voice vlan 785
 no shutdown
```

### 9.3 Endereçamento IPv4 - Level 0

| **Dispositivo** | **VLAN** | **IP** | **Máscara** | **Gateway** |
| --- | --- | --- | --- | --- |
| PC-T4-Users-1 | 783 - T4-UserOutlets | 10.63.160.2 | 255.255.252.0 | 10.63.160.1 |
| PC-T4-DMZ-1 | 774 - SwitchesDMZ | 10.63.165.20 | 255.255.254.0 | — |
| T4-Server-1 | 786 - T4-ServersDMZ | 10.63.174.2 | 255.255.255.128 | 10.63.174.1 |
| T4-F0-AP1 | 784 - T4-WiFi | SSID: T4-WiFi-L0 | — | — |
| T4-Laptop-WiFi-1 | 784 - T4-WiFi | 10.63.136.3 | 255.255.248.0 | 10.63.136.1 |

### 9.4 Endereçamento IPv4 - Level 2

| **Dispositivo** | **VLAN** | **IP** | **Máscara** | **Gateway** |
| --- | --- | --- | --- | --- |
| PC-T4-Users-2 | 783 - T4-UserOutlets | 10.63.160.3 | 255.255.252.0 | 10.63.160.1 |
| PC-T4-DMZ-2 | 774 - SwitchesDMZ | 10.63.165.21 | 255.255.254.0 | — |
| T4-Server-2 | 786 - T4-ServersDMZ | 10.63.174.3 | 255.255.255.128 | 10.63.174.1 |
| T4-F2-AP2 | 784 - T4-WiFi | SSID: T4-WiFi-L2 | — | — |
| T4-Laptop-WiFi-2 | 784 - T4-WiFi | 10.63.136.4 | 255.255.248.0 | 10.63.136.1 |

> Nota: Os PCs DMZ (VLAN 774) não têm gateway configurado pois a VLAN 774 não possui sub-interface no router — é uma rede de gestão isolada a Layer 2.

### 9.5 Configuração do Laptop (WiFi)

Para ligar o laptop ao Access Point no Packet Tracer:

1. Abrir o laptop → separador **Physical**
2. Desligar o laptop
3. Remover a placa de rede com fio
4. Inserir a placa wireless **WPC300N**
5. Ligar o laptop
6. Aceder a **Desktop → PC Wireless** e ligar ao SSID do AP
7. Configurar o IP manualmente em **Desktop → IP Configuration**

---

## 10. Routing Estático

Foi utilizado routing estático em todos os routers, conforme definido no planning do Sprint 2.

### 10.1 Rtr-T4 - Rotas configuradas

As rotas estáticas foram configuradas para garantir comunicação com as redes dos Terminais 2 e 3, utilizando como next-hop o endereço IP do router de destino na rede de backbone (VLAN 773).

Rotas para o Terminal 2 (via 10.63.172.1):

```
ip route 10.63.128.0 255.255.248.0 10.63.172.1
ip route 10.63.152.0 255.255.252.0 10.63.172.1
ip route 10.63.166.0 255.255.254.0 10.63.172.1
ip route 10.63.174.128 255.255.255.128 10.63.172.1
```

Rotas para o Terminal 3 (via 10.63.172.2):

```
ip route 10.63.144.0 255.255.248.0 10.63.172.2
ip route 10.63.156.0 255.255.252.0 10.63.172.2
ip route 10.63.170.0 255.255.254.0 10.63.172.2
ip route 10.63.173.0 255.255.255.0 10.63.172.2
```

Rota por defeito (para Internet, via Rtr-T2):

```
ip route 0.0.0.0 0.0.0.0 10.63.172.1
```

### 10.2 Campus Backbone

| **Router** | **IP no Backbone** | **Interface** |
| --- | --- | --- |
| Rtr-T2 | 10.63.172.1 | Fa0/0.773 |
| Rtr-T3 | 10.63.172.2 | Fa0/0.773 |
| Rtr-T4 | 10.63.172.3 | Fa0/0.773 |

---

## 11. Testes de Validação Final

Após a configuração completa da infraestrutura de rede do Terminal 4, foram realizados testes de validação para garantir o correto funcionamento de toda a rede.

---

### 11.1 Teste de conectividade com o gateway

**Objetivo:** Validar se os dispositivos conseguem comunicar com o gateway da sua VLAN.

**Dispositivo:** PC-T4-Users-1 — VLAN T4-UserOutlets (783)

**Comando:**
```
ping 10.63.160.1
```

**Resultado:** ✔ 0% packet loss

---

### 11.2 Teste inter-VLAN — Users → ServersDMZ

**Objetivo:** Confirmar o encaminhamento entre VLANs através do Router-on-a-Stick.

**Dispositivo:** PC-T4-Users-1

**Comando:**
```
ping 10.63.174.2
```

**Resultado:** ✔ 0% packet loss

---

### 11.3 Teste inter-piso — Users L0 → Users L2

**Objetivo:** Validar comunicação entre dispositivos da mesma VLAN em pisos diferentes.

**Dispositivo:** PC-T4-Users-1

**Comando:**
```
ping 10.63.160.3
```

**Resultado:** ✔ 0% packet loss

---

### 11.4 Teste inter-VLAN inter-piso — Users L0 → Server L2

**Objetivo:** Confirmar encaminhamento entre VLANs diferentes em pisos diferentes.

**Dispositivo:** PC-T4-Users-1

**Comando:**
```
ping 10.63.174.3
```

**Resultado:** ✔ 0% packet loss

---

### 11.5 Teste inter-VLAN — Users → SwitchesDMZ

**Objetivo:** Confirmar comunicação com a rede de gestão a nível Layer 3.

**Dispositivo:** PC-T4-Users-1

**Comando:**
```
ping 10.63.165.2
```

**Resultado:** ✔ 0% packet loss (primeiro pacote perdido por ARP — normal)

---

### 11.6 Teste de isolamento da VLAN SwitchesDMZ (774)

**Objetivo:** Confirmar que a VLAN 774 está isolada sem gateway Layer 3, sendo acessível apenas a nível de Layer 2.

**Dispositivo:** PC-T4-Users-1

**Comando:**
```
ping 10.63.165.1
```

**Resultado:** ✘ 100% packet loss — Destination host unreachable

**Conclusão:** ✔ VLAN 774 corretamente isolada — sem routing para esta rede

---

### 11.7 Teste WiFi — Laptop → Gateway

**Objetivo:** Validar ligação wireless e comunicação com o gateway da VLAN WiFi.

**Dispositivo:** Laptop1 — SSID T4-WiFi-L0

**Comando:**
```
ping 10.63.136.1
```

**Resultado:** ✔ 0% packet loss

---

### 11.8 Teste WiFi inter-VLAN — Laptop → PC-Users-1

**Objetivo:** Confirmar comunicação entre dispositivo wireless e VLAN de utilizadores.

**Dispositivo:** Laptop1

**Comando:**
```
ping 10.63.160.2
```

**Resultado:** ✔ 0% packet loss (primeiro pacote perdido por ARP — normal)

---

### 11.9 Teste Backbone — Rtr-T4 → Rtr-T2

**Objetivo:** Validar comunicação entre routers dos terminais através do campus backbone.

**Dispositivo:** Rtr-T4

**Comando:**
```
ping 10.63.172.1
```

**Resultado:** ✔ 80% success (primeiro pacote perdido — normal)

---

### 11.10 Teste Backbone — Rtr-T4 → Rtr-T3

**Objetivo:** Validar comunicação com o Terminal 3.

**Dispositivo:** Rtr-T4

**Comando:**
```
ping 10.63.172.2
```

**Resultado:** ✔ 80% success (primeiro pacote perdido — normal)

---

### 11.11 Teste de continuidade Layer 2 (Switching Backbone)

**Objetivo:** Confirmar que os trunks entre MC, IC-T4, HCs e CPs estão operacionais.

**Comando** (em cada switch):
```
show interfaces trunk
```

**Resultado obtido:**

- ✔ VLANs 773-786 presentes em todos os trunks
- ✔ Todas as interfaces em estado `trunking` com encapsulamento `802.1q`
- ✔ Sem portas bloqueadas indevidamente pelo STP
- ✔ VLANs allowed and active confirmadas em toda a hierarquia:
  - MC → IC-T4 → HC-T4-L0-Sala4 → CPs Level 0
  - IC-T4 → HC-T4-L2-Sala3 → CPs Level 2
  - IC-T4 → HC-T4-L2-Sala3-2 → CPs Level 2

---

### 11.12 Modo Simulação

Foi utilizado o modo de simulação do Packet Tracer para visualizar o percurso dos pacotes ICMP na rede.

**Teste realizado:** PC-T4-Users-1 → T4-Server-2 (10.63.174.3)

**Caminho percorrido:**
```
PC-T4-Users-1 → CP-T4-L0-Sala14 → HC-T4-L0-Sala4 → IC-T4 → Rtr-T4
→ IC-T4 → HC-T4-L2-Sala3 → CP-T4-L2-Sala14/15 → T4-Server-2
```

A resposta percorreu o caminho inverso, confirmando comunicação bidirecional. A análise PDU no Rtr-T4 confirmou o correto encapsulamento Dot1Q (Layer 2) e o endereçamento IP (Layer 3).

---

### 11.13 Conclusão dos testes

Os testes realizados permitiram validar:

- ✔ Funcionamento correto das VLANs e trunks
- ✔ Encaminhamento inter-VLAN através do Router-on-a-Stick
- ✔ Comunicação inter-piso (Level 0 ↔ Level 2)
- ✔ Isolamento correto da VLAN 774 (SwitchesDMZ) — sem gateway Layer 3
- ✔ Conectividade wireless (SSID T4-WiFi-L0)
- ✔ Routing estático entre terminais via Campus Backbone
- ✔ Continuidade Layer 2 confirmada em toda a hierarquia de switching
- ✔ Comunicação bidirecional confirmada em modo simulação

Conclui-se que a infraestrutura de rede do Terminal 4 se encontra corretamente configurada, funcional e em conformidade com os requisitos do projeto.

---

## 12. Nota Técnica - HC Adicional no Level 2

O switch PT-Empty suporta um máximo de 10 portas GigabitEthernet. O Level 2 do Terminal 4 possui 14 CPs, o que excede a capacidade de um único HC.

Para resolver esta limitação, foi adicionado um segundo HC no Level 2:

- **HC-T4-L2-Sala3** — responsável pelos primeiros 7 CPs do Level 2
- **HC-T4-L2-Sala3(2)** — responsável pelos restantes 7 CPs do Level 2

Ambos os HCs estão ligados ao IC-T4 via trunk e configurados como VTP Client, garantindo a propagação correta de todas as VLANs.

---

*RCOMP 2025-2026 | Projeto 1 Sprint 2 | Terminal 4 | 1240895*