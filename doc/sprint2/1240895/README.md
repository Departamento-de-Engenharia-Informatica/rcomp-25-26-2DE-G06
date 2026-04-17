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
  - 14 CP (9 no Level 0, 14 no Level 2)

> Nota: O switch PT-Empty suporta apenas 10 portas. Como o Level 2 possui 14 CPs, foi necessário adicionar um segundo HC no Level 2 (HC-T4-L2-Sala3(2)) para suportar todas as ligações.

### 3.2 Módulos adicionados

Em cada switch PT-Empty:

- Aba Physical
- Desligar o switch
- Adicionar módulo PT-SWITCH-NM-1FGE (fibra)
- Ligar novamente

### 3.3 Ligações

- Backbone (IC -> HC) → Fibra Ótica OM4
- Ligações horizontais (CP -> end devices) → Cobre CAT7

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

Os trunks permitem transportar tráfego de múltiplas VLANs entre switches. São configurados em todas as ligações entre switches da hierarquia: MC -> IC -> HC -> CP.

### 6.1 MC <-> IC-T4

No MC (porta para IC-T4):

```
interface GigabitEthernet1/1
switchport mode trunk
switchport trunk native vlan 1
no shutdown
```

No IC-T4 (porta para MC):

```
interface GigabitEthernet8/1
switchport mode trunk
switchport trunk native vlan 1
no shutdown
```

### 6.2 IC-T4 <-> HC-T4-L0-Sala4

No IC-T4:

```
interface GigabitEthernet9/1
switchport mode trunk
switchport trunk native vlan 1
no shutdown
```

No HC-T4-L0-Sala4:

```
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk native vlan 1
no shutdown
```

### 6.3 IC-T4 <-> HC-T4-L2-Sala3 e HC-T4-L2-Sala3(2)

Mesma lógica de trunk em ambos os lados de cada ligação.

### 6.4 HC -> CP (uplinks)

Todas as ligações HC -> CP são configuradas como trunk em ambos os lados:

```
interface GigabitEthernetX/X
switchport mode trunk
switchport trunk native vlan 1
no shutdown
```

### 6.5 Porta do IC-T4 para o Rtr-T4 (CampusBackbone)

A porta do IC-T4 ligada ao Rtr-T4 é configurada como trunk para suportar o router-on-a-stick:

```
interface FastEthernet7/1
switchport mode trunk
switchport trunk native vlan 1
no shutdown
```

---

## 7. Gestão dos Switches (SwitchesDMZ - VLAN 774)

Cada switch possui uma interface virtual (SVI) associada à VLAN 774, permitindo gestão remota.

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

- 2 PCs (utilizadores - VLAN 783)
- 2 PCs (DMZ - VLAN 786)
- 1 Laptop (WiFi - VLAN 784) por piso
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

### 9.3 Endereçamento IPv4 - Level 0

| **Dispositivo** | **VLAN** | **IP**      | **Máscara** | **Gateway** |
| --- | --- |-------------| --- |-------------|
| PC-T4-Users-1 | 783 | 10.63.160.2 | 255.255.252.0 | 10.63.160.1 |
| PC-T4-DMZ-1 | 786 | 10.63.165.2 | 255.255.255.128 | 10.63.165.1 |
| T4-Server-1 | 786 | 10.63.174.2 | 255.255.255.128 | 10.63.174.1 |
| T4-F0-AP1 | 784 | 10.63.136.2 | 255.255.248.0 | 10.63.136.1 |
| T4-Laptop-WiFi-1 | 784 | 10.63.136.3 | 255.255.248.0 | 10.63.136.1 |

### 9.4 Endereçamento IPv4 - Level 2

| **Dispositivo** | **VLAN** | **IP**       | **Máscara** | **Gateway** |
| --- | --- |--------------| --- |-------------|
| PC-T4-Users-2 | 783 | 10.63.160.3  | 255.255.252.0 | 10.63.160.1 |
| PC-T4-DMZ-2 | 786 | 10.63.165.20 | 255.255.255.128 | 10.63.165.1  |
| T4-Server-2 | 786 | 10.63.174.3  | 255.255.255.128 | 10.63.174.1 |
| T4-F2-AP1 | 784 | 10.63.136.4  | 255.255.248.0 | 10.63.136.1 |
| T4-Laptop-WiFi-2 | 784 | 10.63.136.5  | 255.255.248.0 | 10.63.136.1 |

### 9.5 Configuração do Laptop (WiFi)

Para ligar o laptop ao Access Point no Packet Tracer:

1. Abrir o laptop -> separador Physical
2. Desligar o laptop
3. Remover a placa de rede com fio
4. Inserir a placa wireless WPC300N
5. Ligar o laptop
6. Aceder a Desktop -> PC Wireless e ligar ao SSID do AP
7. Configurar o IP manualmente em Desktop -> IP Configuration

---
