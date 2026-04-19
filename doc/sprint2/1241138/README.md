# RCOMP 2025-2026 - Projeto 1 - Sprint 2

**Terminal 3 - 1241138**

---

## 1. Preparação

| **Parâmetro**      | **Valor**                    |
|--------------------|------------------------------|
| Software utilizado | Cisco Packet Tracer 9.0.0810 |
| Terminal           | T3                           |
| VTP Domain         | rc2526deg6                   |
| Bloco IPv4         | 10.63.128.0/17               |
| Estudante          | 1241138                      |

---

## 2. Convenção de Nomes

### 2.1 Switches

| **Tipo**       | **Exemplo**                                                                                                |
|----------------|------------------------------------------------------------------------------------------------------------|
| MC             | MC-T3-L1-Sala1                                                                                             |
| IC             | IC-T3-L1-Sala1                                                                                             |
| HC             | HC-T3-L1-Sala1 / HC-T3-L1-Sala11 / HC-T3-L1-Sala15 / HC-T3-L2-Sala1 / HC-T3-L2-Sala13 / HC-T3-L2-Sala20    |
| CP             | CP-T3-L1-ParedeSup / CP-T3-L1-Sala13 / CP-T3-L1-Sala5 / CP-T3-L2-Sala4 / CP-T3-L2-Sala13 / CP-T3-L2-Sala20 |
| Stubs backbone | MC-T2-stub / MC-T4-stub                                                                                    |

### 2.2 Routers

| **Hostname**    | **Função**                                 |
|-----------------|--------------------------------------------|
| Rtr-T3-L1-Sala1 | Router principal do Terminal 3             |
| Rtr-T2          | Router representativo do Terminal 2 (stub) |
| Rtr-T4          | Router representativo do Terminal 4 (stub) |

> Nota: Os hostnames coincidem com o Display Name, sem espaços.

---

## 3. Construção da Topologia

### 3.1 Equipamentos utilizados

- 1 Router Cisco 2811 principal (`Rtr-T3-L1-Sala1`)
- 2 Routers Cisco 2811 representativos dos terminais externos (`Rtr-T2` e `Rtr-T4`)
- Switches modelo PT-Empty:
    - 1 MC (`MC-T3-L1-Sala1`)
    - 1 IC (`IC-T3-L1-Sala1`)
    - 3 HC no Level 1 (`HC-T3-L1-Sala1`, `HC-T3-L1-Sala11`, `HC-T3-L1-Sala15`)
    - 3 HC no Level 2 (`HC-T3-L2-Sala1`, `HC-T3-L2-Sala13`, `HC-T3-L2-Sala20`)
    - 3 CP representativos no Level 1 (`CP-T3-L1-ParedeSup`, `CP-T3-L1-Sala13`, `CP-T3-L1-Sala5`)
    - 3 CP representativos no Level 2 (`CP-T3-L2-Sala4`, `CP-T3-L2-Sala13`, `CP-T3-L2-Sala20`)
    - 2 stubs de backbone (`MC-T2-stub`, `MC-T4-stub`)

> Nota: Na simulação foram utilizados **CPs representativos por grupo/zona**, em vez de todos os CPs físicos do Sprint 1. Esta simplificação mantém a hierarquia MC → IC → HC → CP e permite demonstrar corretamente trunking, VTP, gestão de switches, backbone e ligação de end devices.
>
> O switch **CP-T3-L2-Sala13** foi o CP representativo configurado com os end devices para demonstrar o funcionamento de todas as VLANs do Terminal 3.

### 3.2 Módulos adicionados

Em cada switch PT-Empty:

1. Aba **Physical**
2. Desligar o switch (botão verde)
3. Adicionar módulos:
    - **PT-SWITCH-NM-1FGE-SM** — para ligações de fibra monomodo (backbone)
    - **PT-SWITCH-NM-1CGE** — para ligações de cobre (horizontal)
4. Ligar novamente

### 3.3 Ligações

- Backbone interno (MC → IC e IC → HCs) → **Fibra ótica monomodo**
- Ligações horizontais (HC → CP e CP → end devices) → **Cobre**
- MC-T3-L1-Sala1 ligado a MC-T2-stub e MC-T4-stub via fibra (campus backbone)
- Rtr-T3-L1-Sala1 ligado ao MC-T3-L1-Sala1 via cobre (único link trunk — router-on-a-stick)
- Rtr-T2 ligado ao MC-T2-stub via cobre
- Rtr-T4 ligado ao MC-T4-stub via cobre

---

## 4. Configuração do Switch Principal (MC)

O switch `MC-T3-L1-Sala1` funciona como **VTP Server**, sendo responsável pela criação e propagação da base de dados global de VLANs.

### 4.1 Hostname e VTP

```
enable
configure terminal
hostname MC-T3-L1-Sala1
vtp domain rc2526deg6
vtp mode server
```

### 4.2 Criação das VLANs

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

## 5. Configuração dos Restantes Switches

Todos os switches exceto o MC foram configurados como **VTP Client**, recebendo automaticamente a base de dados de VLANs propagada pelo MC.

```
enable
configure terminal
hostname [nome do switch]
vtp domain rc2526deg6
vtp mode client
```

---

## 6. Configuração de Trunks

Todas as ligações entre switches foram configuradas em trunk, transportando as VLANs 773-786.

### 6.1 Portas trunk por dispositivo

#### MC-T3-L1-Sala1

| **Porta**          | **Ligado a**    |
|--------------------|-----------------|
| GigabitEthernet0/1 | MC-T2-stub      |
| GigabitEthernet1/1 | MC-T4-stub      |
| GigabitEthernet2/1 | IC-T3-L1-Sala1  |
| GigabitEthernet4/1 | Rtr-T3-L1-Sala1 |

#### IC-T3-L1-Sala1

| **Porta** | **Ligado a** |
| --- | --- |
| GigabitEthernet0/1 | MC-T3-L1-Sala1 |
| GigabitEthernet1/1 | HC-T3-L1-Sala1 |
| GigabitEthernet2/1 | HC-T3-L1-Sala11 |
| GigabitEthernet3/1 | HC-T3-L1-Sala15 |
| GigabitEthernet4/1 | HC-T3-L2-Sala1 |
| GigabitEthernet5/1 | HC-T3-L2-Sala13 |
| GigabitEthernet6/1 | HC-T3-L2-Sala20 |

#### HCs Level 1

| **Switch** | **Porta IC (uplink)** | **Porta CP (downlink)** |
| --- | --- | --- |
| HC-T3-L1-Sala1 | GigabitEthernet0/1 | GigabitEthernet1/1 → CP-T3-L1-ParedeSup |
| HC-T3-L1-Sala11 | GigabitEthernet0/1 | GigabitEthernet1/1 → CP-T3-L1-Sala13 |
| HC-T3-L1-Sala15 | GigabitEthernet0/1 | GigabitEthernet1/1 → CP-T3-L1-Sala5 |

#### HCs Level 2

| **Switch** | **Porta IC (uplink)** | **Porta CP (downlink)** |
| --- | --- | --- |
| HC-T3-L2-Sala1 | GigabitEthernet0/1 | GigabitEthernet1/1 → CP-T3-L2-Sala4 |
| HC-T3-L2-Sala13 | GigabitEthernet0/1 | GigabitEthernet1/1 → CP-T3-L2-Sala13 |
| HC-T3-L2-Sala20 | GigabitEthernet0/1 | GigabitEthernet1/1 → CP-T3-L2-Sala20 |

### 6.2 Configuração típica de trunk

```
interface GigabitEthernetX/1
 switchport mode trunk
 switchport trunk allowed vlan 773-786
 no shutdown
exit
```

### 6.3 Verificação

```
show interfaces trunk
show vlan brief
show vtp status
```

Resultado esperado: todas as portas de interligação em estado `trunking`, encapsulamento `802.1q`, VLANs 773-786 permitidas e ativas.

---

## 7. Gestão dos Switches (SwitchesDMZ - VLAN 774)

A rede de gestão foi implementada com uma SVI (`interface vlan 774`) em todos os switches, permitindo acesso remoto e monitorização. A VLAN 774 é uma rede isolada — não possui sub-interface no router, garantindo que o tráfego de gestão permanece restrito à camada 2.

### 7.1 Endereçamento de gestão

| **Switch** | **IP de Gestão** | **Máscara** |
| --- | --- | --- |
| MC-T3-L1-Sala1 | 10.63.164.101 | 255.255.254.0 |
| IC-T3-L1-Sala1 | 10.63.164.102 | 255.255.254.0 |
| HC-T3-L1-Sala1 | 10.63.164.103 | 255.255.254.0 |
| HC-T3-L1-Sala11 | 10.63.164.104 | 255.255.254.0 |
| HC-T3-L1-Sala15 | 10.63.164.105 | 255.255.254.0 |
| HC-T3-L2-Sala1 | 10.63.164.106 | 255.255.254.0 |
| HC-T3-L2-Sala13 | 10.63.164.107 | 255.255.254.0 |
| HC-T3-L2-Sala20 | 10.63.164.108 | 255.255.254.0 |
| CP-T3-L1-ParedeSup | 10.63.164.110 | 255.255.254.0 |
| CP-T3-L1-Sala13 | 10.63.164.111 | 255.255.254.0 |
| CP-T3-L1-Sala5 | 10.63.164.112 | 255.255.254.0 |
| CP-T3-L2-Sala4 | 10.63.164.120 | 255.255.254.0 |
| CP-T3-L2-Sala13 | 10.63.164.121 | 255.255.254.0 |
| CP-T3-L2-Sala20 | 10.63.164.122 | 255.255.254.0 |

### 7.2 Configuração aplicada em cada switch

```
interface vlan 774
 ip address 10.63.164.X 255.255.254.0
 no shutdown
exit
```

---

## 8. Configuração de Layer 3 (Router-on-a-Stick)

O encaminhamento entre VLANs é feito pelo router `Rtr-T3-L1-Sala1` (modelo Cisco 2811), ligado ao MC-T3-L1-Sala1 por um único link trunk. Cada VLAN aparece no router como uma sub-interface lógica com encapsulamento IEEE 802.1Q.

### 8.1 Ativação da interface física

```
enable
configure terminal
interface FastEthernet0/0
 no ip address
 no shutdown
exit
```

### 8.2 Sub-interfaces do Terminal 3

| **Sub-interface** | **VLAN** | **IP Gateway** | **Máscara** |
| --- | --- | --- | --- |
| Fa0/0.773 | 773 — CampusBackbone | 10.63.172.2 | 255.255.255.0 |
| Fa0/0.779 | 779 — T3-UserOutlets | 10.63.156.1 | 255.255.252.0 |
| Fa0/0.780 | 780 — T3-WiFi | 10.63.144.1 | 255.255.248.0 |
| Fa0/0.781 | 781 — T3-VoIP | 10.63.170.1 | 255.255.254.0 |
| Fa0/0.782 | 782 — T3-ServersDMZ | 10.63.173.1 | 255.255.255.0 |

```
interface FastEthernet0/0.773
 encapsulation dot1Q 773
 ip address 10.63.172.2 255.255.255.0
 no shutdown
exit
interface FastEthernet0/0.779
 encapsulation dot1Q 779
 ip address 10.63.156.1 255.255.252.0
 no shutdown
exit
interface FastEthernet0/0.780
 encapsulation dot1Q 780
 ip address 10.63.144.1 255.255.248.0
 no shutdown
exit
interface FastEthernet0/0.781
 encapsulation dot1Q 781
 ip address 10.63.170.1 255.255.254.0
 no shutdown
exit
interface FastEthernet0/0.782
 encapsulation dot1Q 782
 ip address 10.63.173.1 255.255.255.0
 no shutdown
exit
```

> Nota: A VLAN 774 (SwitchesDMZ) não possui sub-interface no router, mantendo-se isolada a Layer 2.

---

## 9. Routers Externos Representativos (Campus Backbone)

Para tornar o campus backbone coerente com as rotas estáticas configuradas, foram adicionados dois routers representativos dos terminais T2 e T4. Conforme o enunciado, estes routers externos apenas necessitam da ligação à backbone e respetivo IP.

### Rtr-T2

```
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

```
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

### Campus Backbone — endereços dos routers

| **Router**      | **IP no Backbone** |
|-----------------|--------------------|
| Rtr-T2          | 10.63.172.1        |
| Rtr-T3-L1-Sala1 | 10.63.172.2        |
| Rtr-T4          | 10.63.172.3        |

---

## 10. Routing Estático

O router `Rtr-T3-L1-Sala1` foi configurado com rotas estáticas para os terminais T2 e T4, utilizando como next-hop o IP do router de destino na rede de backbone (VLAN 773).

### 10.1 Rotas para T2 (via 10.63.172.1)

```
ip route 10.63.128.0 255.255.248.0 10.63.172.1
ip route 10.63.152.0 255.255.252.0 10.63.172.1
ip route 10.63.166.0 255.255.254.0 10.63.172.1
ip route 10.63.174.128 255.255.255.128 10.63.172.1
```

### 10.2 Rotas para T4 (via 10.63.172.3)

```
ip route 10.63.136.0 255.255.248.0 10.63.172.3
ip route 10.63.160.0 255.255.252.0 10.63.172.3
ip route 10.63.168.0 255.255.254.0 10.63.172.3
ip route 10.63.174.0 255.255.255.128 10.63.172.3
```

### 10.3 Default route (para internet via T2)

```
ip route 0.0.0.0 0.0.0.0 10.63.172.1
```

---

## 11. End Devices

O enunciado exige pelo menos um end device por VLAN em uso no terminal. Os end devices foram concentrados no CP representativo `CP-T3-L2-Sala13`.

### 11.1 Equipamentos adicionados

| **Dispositivo** | **Modelo PT** |
| --- | --- |
| PC utilizadores | PC-PT |
| PC gestão DMZ | PC-PT |
| Servidor | Server-PT |
| Access Point | AccessPoint-PT |
| Laptop WiFi | Laptop-PT com WPC300N |
| Telefone VoIP | Cisco 7960 |

### 11.2 Configuração das portas no CP-T3-L2-Sala13

| **Porta** | **Função** | **VLAN** |
| --- | --- | --- |
| GigabitEthernet0/1 | Uplink para HC-T3-L2-Sala13 | trunk 773-786 |
| GigabitEthernet1/1 | PC-T3-Users-1 | access 779 |
| GigabitEthernet2/1 | T3-Server-1 | access 782 |
| GigabitEthernet3/1 | Access Point T3 | access 780 |
| GigabitEthernet4/1 | VoIP 7960 | voice vlan 781 |
| GigabitEthernet5/1 | PC-T3-DMZ-1 | access 774 |

```
interface GigabitEthernet1/1
 switchport mode access
 switchport access vlan 779
 no shutdown
exit
interface GigabitEthernet2/1
 switchport mode access
 switchport access vlan 782
 no shutdown
exit
interface GigabitEthernet3/1
 switchport mode access
 switchport access vlan 780
 no shutdown
exit
interface GigabitEthernet4/1
 switchport mode access
 switchport voice vlan 781
 no shutdown
exit
interface GigabitEthernet5/1
 switchport mode access
 switchport access vlan 774
 no shutdown
exit
```

### 11.3 Endereçamento IPv4 dos end devices

| **Dispositivo** | **VLAN** | **IP** | **Máscara** | **Gateway** |
| --- | --- | --- | --- | --- |
| PC-T3-Users-1 | 779 — T3-UserOutlets | 10.63.156.10 | 255.255.252.0 | 10.63.156.1 |
| T3-Server-1 | 782 — T3-ServersDMZ | 10.63.173.10 | 255.255.255.0 | 10.63.173.1 |
| T3-AP1 | 780 — T3-WiFi | SSID: T3-WIFI | — | — |
| T3-Laptop-WiFi-1 | 780 — T3-WiFi | 10.63.144.10 | 255.255.248.0 | 10.63.144.1 |
| PC-T3-DMZ-1 | 774 — SwitchesDMZ | 10.63.164.130 | 255.255.254.0 | — |
| VoIP 7960 | 781 — T3-VoIP | — | — | Sprint 3 |

> Nota: O PC da SwitchesDMZ não possui gateway — a VLAN 774 é uma rede isolada sem encaminhamento Layer 3.

### 11.4 Configuração do Laptop WiFi

1. Laptop → aba **Physical**
2. Desligar o dispositivo
3. Remover a placa de rede cablada
4. Inserir a placa wireless **WPC300N**
5. Ligar o laptop
6. Ir a **Desktop → PC Wireless** e associar ao SSID `T3-WIFI`
7. Configurar IP manualmente em **Desktop → IP Configuration**

### 11.5 VoIP

Foi utilizado o modelo **Cisco 7960**, conforme exigido pelo enunciado. O telefone foi ligado à porta `GigabitEthernet4/1` do CP-T3-L2-Sala13, configurada com `switchport voice vlan 781`.

> Nota: A configuração IPv4 do telefone VoIP está fora do âmbito do Sprint 2.

---

## 12. Testes de Validação Final

### 12.1 Teste Wi-Fi — laptop → gateway

```
ping 10.63.144.1
```
Resultado: ✔ sucesso (4/4 respostas)

### 12.2 Teste Wi-Fi — laptop → gateway UserOutlets

```
ping 10.63.156.1
```
Resultado: ✔ sucesso

### 12.3 Teste WiFi — laptop → gateway ServersDMZ

```
ping 10.63.173.1
```
Resultado: ✔ sucesso

### 12.4 Teste SwitchesDMZ — comunicação interna

```
ping 10.63.164.101
ping 10.63.164.102
```
Resultado: ✔ sucesso (Layer 2 dentro da VLAN 774)

### 12.5 Teste de isolamento da SwitchesDMZ

```
ping 10.63.144.1
```
(a partir do PC-T3-DMZ-1)

Resultado: ✘ falha — sem gateway configurado, como esperado

### 12.6 Teste backbone — router T3 → routers externos

```
ping 10.63.172.1
ping 10.63.172.3
```
Resultado: ✔ sucesso

### 12.7 Verificação da tabela de routing

```
show ip route
```

Resultado obtido: redes T3 diretamente ligadas, rotas estáticas para T2 via `10.63.172.1`, rotas estáticas para T4 via `10.63.172.3`, default route via `10.63.172.1`.

### 12.8 Verificação dos trunks

```
show interfaces trunk
```

Resultado obtido: todas as portas de interligação em estado `trunking`, encapsulamento `802.1q`, VLANs 773-786 permitidas e ativas em toda a hierarquia.

### 12.9 Conclusão dos testes

| | |
| --- | --- |
| ✔ | VLANs correctamente configuradas e propagadas via VTP |
| ✔ | Trunks operacionais em toda a hierarquia MC → IC → HC → CP |
| ✔ | Encaminhamento inter-VLAN via router-on-a-stick |
| ✔ | Rede WiFi funcional (SSID T3-WIFI, laptop associado) |
| ✔ | SwitchesDMZ isolada — sem gateway Layer 3 |
| ✔ | Comunicação backbone com T2 e T4 |
| ✔ | Routing estático consistente com o plano de endereçamento |
| ✔ | Default route para internet via T2 |

---

## 13. Nota Técnica — CPs Representativos

O Terminal 3 possui 22 CPs físicos no Sprint 1 (11 por piso). Na simulação Packet Tracer, foram utilizados **CPs representativos por grupo/zona** para manter a hierarquia lógica sem duplicar desnecessariamente a complexidade da topologia.

### Level 1

| **CP Representativo** | **Zona** | **HC ligado** |
| --- | --- | --- |
| CP-T3-L1-ParedeSup | Parede superior e canto | HC-T3-L1-Sala1 |
| CP-T3-L1-Sala13 | Zona esquerda (Salas 11-13) | HC-T3-L1-Sala11 |
| CP-T3-L1-Sala5 | Zona direita e central (Salas 5-15) | HC-T3-L1-Sala15 |

### Level 2

| **CP Representativo** | **Zona** | **HC ligado** |
| --- | --- | --- |
| CP-T3-L2-Sala4 | Zona superior direita | HC-T3-L2-Sala1 |
| CP-T3-L2-Sala13 | Zona central e inferior | HC-T3-L2-Sala13 |
| CP-T3-L2-Sala20 | Zona inferior direita | HC-T3-L2-Sala20 |

O `CP-T3-L2-Sala13` foi o CP representativo escolhido para concentrar todos os end devices, demonstrando o correto funcionamento das VLANs 779, 780, 781, 782 e 774.

---

*RCOMP 2025-2026 | Projeto 1 Sprint 2 | Terminal 3 | 1241138*