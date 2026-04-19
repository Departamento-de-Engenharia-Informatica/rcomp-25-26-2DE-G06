# RCOMP 2025-2026 - Projeto 1 - Sprint 2

**Terminal 3 - 1241138**

---

## 1. Preparação

| **Parâmetro** | **Valor** |
| --- | --- |
| Software utilizado | Cisco Packet Tracer 9.0.0810 |
| Terminal | T3 |
| VTP Domain | rc2526deg6 |
| Bloco IPv4 | 10.63.128.0/17 |
| Estudante | 1241138 |

---

## 2. Convenção de Nomes

### 2.1 Switches

| **Tipo** | **Exemplo** |
| --- | --- |
| MC | MC |
| IC | IC-T3-L1-Sala1 / IC-T2 / IC-T4 |
| HC | HC-T3-L1-Sala1 / HC-T3-L1-Sala11 / HC-T3-L1-Sala15 / HC-T3-L2-Sala1 / HC-T3-L2-Sala13 / HC-T3-L2-Sala20 |
| CP | CP-T3-L1-ParedeSup / CP-T3-L1-Sala13 / CP-T3-L1-Sala5 / CP-T3-L2-Sala4 / CP-T3-L2-Sala13 / CP-T3-L2-Sala20 |

### 2.2 Routers

| **Hostname** | **Função** |
| --- | --- |
| Rtr-T3-L1-Sala1 | Router principal do Terminal 3 |
| Rtr-T2 | Router representativo do Terminal 2 |
| Rtr-T4 | Router representativo do Terminal 4 |

> Nota: Os hostnames coincidem com o Display Name, sem espaços.

---

## 3. Construção da Topologia

### 3.1 Equipamentos utilizados

No ficheiro `Terminal3.pkt` foram utilizados:

- 1 router Cisco 2811 principal (`Rtr-T3-L1-Sala1`)
- 2 routers Cisco 2811 representativos dos terminais externos (`Rtr-T2` e `Rtr-T4`)
- switches modelo PT-Empty:
    - 1 MC backbone simplificado (`MC`)
    - 1 IC real do Terminal 3 (`IC-T3-L1-Sala1`)
    - 2 ICs representativos do backbone (`IC-T2` e `IC-T4`)
    - 3 HC no Level 1 (`HC-T3-L1-Sala1`, `HC-T3-L1-Sala11`, `HC-T3-L1-Sala15`)
    - 3 HC no Level 2 (`HC-T3-L2-Sala1`, `HC-T3-L2-Sala13`, `HC-T3-L2-Sala20`)
    - 3 CP representativos no Level 1 (`CP-T3-L1-ParedeSup`, `CP-T3-L1-Sala13`, `CP-T3-L1-Sala5`)
    - 3 CP representativos no Level 2 (`CP-T3-L2-Sala4`, `CP-T3-L2-Sala13`, `CP-T3-L2-Sala20`)

> Na simulação foram utilizados **CPs representativos por grupo/zona**, em vez de todos os CPs físicos do Sprint 1. Esta simplificação mantém a hierarquia IC → HC → CP e permite demonstrar trunking, VTP, gestão de switches, backbone e ligação de end devices.
>
> O switch **CP-T3-L2-Sala13** foi o CP representativo configurado com os end devices para demonstrar o funcionamento das VLANs do Terminal 3.

### 3.2 Módulos adicionados

Em cada switch PT-Empty:

1. Aba **Physical**
2. Desligar o switch
3. Adicionar módulos conforme necessário:
    - módulo Gigabit de fibra para backbone
    - módulo Gigabit de cobre para ligações horizontais
4. Ligar novamente

### 3.3 Ligações

#### Backbone simplificado

- `MC Gig0/1` ↔ `IC-T3-L1-Sala1 Gig0/1`
- `MC Gig1/1` ↔ `IC-T4 Gig0/1`
- `MC Gig2/1` ↔ `IC-T2 Gig0/1`

#### Ligações do Terminal 3

- `IC-T3-L1-Sala1 Gig1/1` ↔ `HC-T3-L1-Sala1 Gig0/1`
- `IC-T3-L1-Sala1 Gig2/1` ↔ `HC-T3-L1-Sala11 Gig0/1`
- `IC-T3-L1-Sala1 Gig3/1` ↔ `HC-T3-L1-Sala15 Gig0/1`
- `IC-T3-L1-Sala1 Gig4/1` ↔ `HC-T3-L2-Sala1 Gig0/1`
- `IC-T3-L1-Sala1 Gig5/1` ↔ `HC-T3-L2-Sala13 Gig0/1`
- `IC-T3-L1-Sala1 Gig6/1` ↔ `HC-T3-L2-Sala20 Gig0/1`

#### Ligações horizontais

- `HC-T3-L1-Sala1 Gig1/1` ↔ `CP-T3-L1-ParedeSup Gig0/1`
- `HC-T3-L1-Sala11 Gig1/1` ↔ `CP-T3-L1-Sala13 Gig0/1`
- `HC-T3-L1-Sala15 Gig1/1` ↔ `CP-T3-L1-Sala5 Gig0/1`
- `HC-T3-L2-Sala1 Gig1/1` ↔ `CP-T3-L2-Sala4 Gig0/1`
- `HC-T3-L2-Sala13 Gig1/1` ↔ `CP-T3-L2-Sala13 Gig0/1`
- `HC-T3-L2-Sala20 Gig1/1` ↔ `CP-T3-L2-Sala20 Gig0/1`

#### Router-on-a-stick e backbone representativo

- `IC-T3-L1-Sala1 Gig8/1` ↔ `Rtr-T3-L1-Sala1 Fa0/0`
- `IC-T2 Gig1/1` ↔ `Rtr-T2 Fa0/0`
- `IC-T4 Gig1/1` ↔ `Rtr-T4 Fa0/0`

> No `Terminal3.pkt`, os terminais T2 e T4 surgem de forma **simplificada**, apenas para representar a backbone do campus e validar a comunicação entre routers.

---

## 4. Base de Dados VLAN

As VLANs utilizadas no projeto são as seguintes:

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

---

## 5. Configuração de Switches

### 5.1 MC backbone

O switch `MC` funciona como **VTP Server**, sendo responsável pela criação e propagação da base de dados global de VLANs.

```bash
enable
configure terminal
hostname MC
vtp domain rc2526deg6
vtp mode server
```

Criação das VLANs:

```bash
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

Portas trunk do backbone:

```bash
interface GigabitEthernet0/1
 description -> IC-T3
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 no shutdown
exit
interface GigabitEthernet1/1
 description -> IC-T4
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 no shutdown
exit
interface GigabitEthernet2/1
 description -> IC-T2
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 no shutdown
exit
interface vlan 774
 ip address 10.63.164.1 255.255.254.0
 no shutdown
exit
```

### 5.2 Restantes switches

Todos os switches exceto o MC foram configurados em modo VTP Client.

```bash
enable
configure terminal
hostname [nome do switch]
vtp domain rc2526deg6
vtp mode client
```

### 5.3 Trunks

Todas as ligações entre switches foram configuradas em trunk. A ligação entre `IC-T3-L1-Sala1` e `Rtr-T3-L1-Sala1` também foi configurada em trunk para suportar o router-on-a-stick.
Configuração típica:

```bash
interface GigabitEthernetX/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 no shutdown
exit
```

No `IC-T3-L1-Sala1`, a porta para o router ficou:

```bash
interface GigabitEthernet8/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 no shutdown
exit
```

### 5.4 Gestão dos switches — VLAN 774

A rede de gestão foi implementada com uma SVI (`interface vlan 774`) em todos os switches. A VLAN 774 é isolada e não possui sub-interface no router.

#### Endereçamento de gestão do Terminal 3

| **Switch** | **IP de Gestão** |
| --- | --- |
| MC | 10.63.164.1 |
| IC-T3-L1-Sala1 | 10.63.164.102 |
| IC-T2 | 10.63.164.2 |
| IC-T4 | 10.63.164.3 |
| HC-T3-L1-Sala1 | 10.63.164.103 |
| HC-T3-L1-Sala11 | 10.63.164.104 |
| HC-T3-L1-Sala15 | 10.63.164.105 |
| HC-T3-L2-Sala1 | 10.63.164.106 |
| HC-T3-L2-Sala13 | 10.63.164.107 |
| HC-T3-L2-Sala20 | 10.63.164.108 |
| CP-T3-L1-ParedeSup | 10.63.164.110 |
| CP-T3-L1-Sala13 | 10.63.164.111 |
| CP-T3-L1-Sala5 | 10.63.164.112 |
| CP-T3-L2-Sala4 | 10.63.164.120 |
| CP-T3-L2-Sala13 | 10.63.164.121 |
| CP-T3-L2-Sala20 | 10.63.164.122 |

Configuração padrão:

```bash
interface vlan 774
 ip address 10.63.164.X 255.255.254.0
 no shutdown
exit
```

---

## 6. Configuração de Layer 3 (Router-on-a-Stick)

O encaminhamento entre VLANs é feito pelo router `Rtr-T3-L1-Sala1`, ligado ao `IC-T3-L1-Sala1` pela interface `Fa0/0`.

### 6.1 Ativação da interface física

```bash
enable
configure terminal
interface FastEthernet0/0
 no ip address
 no shutdown
exit
```

### 6.2 Sub-interfaces do Terminal 3

| **Sub-interface** | **VLAN** | **IP Gateway** | **Máscara** |
| --- | --- | --- | --- |
| Fa0/0.773 | 773 — CampusBackbone | 10.63.172.2 | 255.255.255.0 |
| Fa0/0.779 | 779 — T3-UserOutlets | 10.63.156.1 | 255.255.252.0 |
| Fa0/0.780 | 780 — T3-WiFi | 10.63.144.1 | 255.255.248.0 |
| Fa0/0.781 | 781 — T3-VoIP | 10.63.170.1 | 255.255.254.0 |
| Fa0/0.782 | 782 — T3-ServersDMZ | 10.63.173.1 | 255.255.255.0 |

```bash
interface FastEthernet0/0.773
 encapsulation dot1Q 773
 ip address 10.63.172.2 255.255.255.0
exit
interface FastEthernet0/0.779
 encapsulation dot1Q 779
 ip address 10.63.156.1 255.255.252.0
exit
interface FastEthernet0/0.780
 encapsulation dot1Q 780
 ip address 10.63.144.1 255.255.248.0
exit
interface FastEthernet0/0.781
 encapsulation dot1Q 781
 ip address 10.63.170.1 255.255.254.0
exit
interface FastEthernet0/0.782
 encapsulation dot1Q 782
 ip address 10.63.173.1 255.255.255.0
exit
```

> A VLAN 774 não possui sub-interface no router, mantendo-se isolada a Layer 2.

---

## 7. Routers Representativos da Backbone

Para representar a backbone do campus no `Terminal3.pkt`, foram adicionados dois routers simplificados: `Rtr-T2` e `Rtr-T4`.

### Rtr-T2

```bash
interface FastEthernet0/0
 no ip address
 no shutdown
exit
interface FastEthernet0/0.773
 encapsulation dot1Q 773
 ip address 10.63.172.1 255.255.255.0
 no shutdown
exit
```

### Rtr-T4

```bash
interface FastEthernet0/0
 no ip address
 no shutdown
exit
interface FastEthernet0/0.773
 encapsulation dot1Q 773
 ip address 10.63.172.3 255.255.255.0
 no shutdown
exit
```

### Endereços da backbone

| **Router** | **IP Backbone** |
| --- | --- |
| Rtr-T2 | 10.63.172.1 |
| Rtr-T3-L1-Sala1 | 10.63.172.2 |
| Rtr-T4 | 10.63.172.3 |

---

## 8. Plano de Endereçamento IPv4 (VLSM)

| **VLAN / Rede** | **Prefixo** | **Endereço de Rede** | **Intervalo Válido** |
| --- | --- | --- | --- |
| T2-WiFi | /21 | 10.63.128.0 | 10.63.128.1 – 10.63.135.254 |
| T4-WiFi | /21 | 10.63.136.0 | 10.63.136.1 – 10.63.143.254 |
| T3-WiFi | /21 | 10.63.144.0 | 10.63.144.1 – 10.63.151.254 |
| T2-UserOutlets | /22 | 10.63.152.0 | 10.63.152.1 – 10.63.155.254 |
| T3-UserOutlets | /22 | 10.63.156.0 | 10.63.156.1 – 10.63.159.254 |
| T4-UserOutlets | /22 | 10.63.160.0 | 10.63.160.1 – 10.63.163.254 |
| SwitchesDMZ | /23 | 10.63.164.0 | 10.63.164.1 – 10.63.165.254 |
| T2-VoIP | /23 | 10.63.166.0 | 10.63.166.1 – 10.63.167.254 |
| T4-VoIP | /23 | 10.63.168.0 | 10.63.168.1 – 10.63.169.254 |
| T3-VoIP | /23 | 10.63.170.0 | 10.63.170.1 – 10.63.171.254 |
| CampusBackbone | /24 | 10.63.172.0 | 10.63.172.1 – 10.63.172.254 |
| T3-ServersDMZ | /24 | 10.63.173.0 | 10.63.173.1 – 10.63.173.254 |
| T4-ServersDMZ | /25 | 10.63.174.0 | 10.63.174.1 – 10.63.174.126 |
| T2-ServersDMZ | /25 | 10.63.174.128 | 10.63.174.129 – 10.63.174.254 |

---

## 9. Routing Estático

O router `Rtr-T3-L1-Sala1` foi configurado com rotas estáticas para os terminais T2 e T4.

### 9.1 Rotas para T2

```bash
ip route 10.63.128.0 255.255.248.0 10.63.172.1
ip route 10.63.152.0 255.255.252.0 10.63.172.1
ip route 10.63.166.0 255.255.254.0 10.63.172.1
ip route 10.63.174.128 255.255.255.128 10.63.172.1
```

### 9.2 Rotas para T4

```bash
ip route 10.63.136.0 255.255.248.0 10.63.172.3
ip route 10.63.160.0 255.255.252.0 10.63.172.3
ip route 10.63.168.0 255.255.254.0 10.63.172.3
ip route 10.63.174.0 255.255.255.128 10.63.172.3
```

### 9.3 Default route

```bash
ip route 0.0.0.0 0.0.0.0 10.63.172.1
```

---

## 10. End Devices

Os end devices foram concentrados no CP representativo `CP-T3-L2-Sala13`.

### 10.1 Equipamentos adicionados

| **Dispositivo** | **Modelo PT** |
| --- | --- |
| PC-T3-Users-1 | PC-PT |
| T3-Server-1 | Server-PT |
| T3-AP1 | AccessPoint-PT |
| T3-Laptop-WiFi-1 | Laptop-PT + WPC300N |
| PC-T3-DMZ-1 | PC-PT |
| VoIP 7960 | Cisco IP Phone 7960 |

### 10.2 Configuração das portas no CP-T3-L2-Sala13

| **Porta** | **Dispositivo** | **Configuração** |
| --- | --- | --- |
| GigabitEthernet0/1 | Uplink | trunk 773-786 |
| GigabitEthernet1/1 | PC-T3-Users-1 | access vlan 779 |
| GigabitEthernet2/1 | T3-Server-1 | access vlan 782 |
| GigabitEthernet3/1 | T3-AP1 | access vlan 780 |
| GigabitEthernet4/1 | VoIP 7960 | voice vlan 781 |
| GigabitEthernet5/1 | PC-T3-DMZ-1 | access vlan 774 |

### 10.3 Endereçamento IPv4

| **Dispositivo** | **IP** | **Máscara** | **Gateway** |
| --- | --- | --- | --- |
| PC-T3-Users-1 | 10.63.156.10 | 255.255.252.0 | 10.63.156.1 |
| T3-Server-1 | 10.63.173.10 | 255.255.255.0 | 10.63.173.1 |
| T3-Laptop-WiFi-1 | 10.63.144.10 | 255.255.248.0 | 10.63.144.1 |
| PC-T3-DMZ-1 | 10.63.164.130 | 255.255.254.0 | — |

> O Access Point foi configurado com o SSID `T3-WIFI`.

---

## 11. Testes de Validação

### 11.1 Testes internos do Terminal 3

| **Origem** | **Comando** | **Resultado** |
| --- | --- | --- |
| T3-Laptop-WiFi-1 | ping 10.63.144.1 | ✔ |
| T3-Laptop-WiFi-1 | ping 10.63.156.1 | ✔ |
| T3-Laptop-WiFi-1 | ping 10.63.173.1 | ✔ |
| PC-T3-DMZ-1 | ping 10.63.164.1 | ✔ |
| PC-T3-DMZ-1 | ping 10.63.164.102 | ✔ |
| PC-T3-DMZ-1 | ping 10.63.144.1 | ✘ esperado |
| Rtr-T3-L1-Sala1 | ping 10.63.144.10 | ✔ |
| Rtr-T3-L1-Sala1 | ping 10.63.156.10 | ✔ |

### 11.2 Testes de backbone

| **Origem** | **Comando** | **Resultado** |
| --- | --- | --- |
| Rtr-T3-L1-Sala1 | ping 10.63.172.1 | ✔ |
| Rtr-T3-L1-Sala1 | ping 10.63.172.3 | ✔ |

### 11.3 Verificações de configuração

```bash
show ip interface brief
show ip route
show interfaces trunk
show vlan brief
show vtp status
show cdp neighbors
```

### 11.4 Conclusão dos testes

- VLANs corretamente propagadas via VTP
- Trunks operacionais em toda a hierarquia
- Router-on-a-stick funcional
- Rede WiFi funcional
- SwitchesDMZ isolada
- Backbone com T2 e T4 funcional

---

## 12. Nota Técnica — CPs Representativos

O Terminal 3 possui 22 CPs físicos no Sprint 1, mas na simulação foram utilizados CPs representativos por zona, mantendo a hierarquia lógica sem duplicar complexidade.

### Level 1

| **CP Representativo** | **Zona** | **HC ligado** |
| --- | --- | --- |
| CP-T3-L1-ParedeSup | Parede superior e canto | HC-T3-L1-Sala1 |
| CP-T3-L1-Sala13 | Zona esquerda | HC-T3-L1-Sala11 |
| CP-T3-L1-Sala5 | Zona central e direita | HC-T3-L1-Sala15 |

### Level 2

| **CP Representativo** | **Zona** | **HC ligado** |
| --- | --- | --- |
| CP-T3-L2-Sala4 | Zona superior | HC-T3-L2-Sala1 |
| CP-T3-L2-Sala13 | Zona central | HC-T3-L2-Sala13 |
| CP-T3-L2-Sala20 | Zona inferior | HC-T3-L2-Sala20 |

> O `CP-T3-L2-Sala13` foi o CP representativo escolhido para concentrar e configurar todos os end devices do Terminal 3, demonstrando o funcionamento das VLANs 774, 779, 780, 781 e 782. Os restantes CPs representativos mantêm apenas a configuração de trunk e gestão.
---

## 13. Integração em campus.pkt

Como tarefa adicional do Terminal 3, foi criada a simulação `campus.pkt`, integrando os ficheiros de todos os elementos do grupo num único campus.

Nesta integração:

- foi mantido um único MC central
- cada terminal ficou ligado ao MC através do respetivo IC
- os routers ficaram ligados aos ICs dos respetivos terminais
- o `campus.pkt` foi testado separadamente para validar a integração final

---

*RCOMP 2025-2026 | Projeto 1 Sprint 2 | Terminal 3 | 1241138*