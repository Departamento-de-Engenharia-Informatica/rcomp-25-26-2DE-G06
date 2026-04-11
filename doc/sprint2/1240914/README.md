# RCOMP – Projeto 1 – Sprint 2 - 1240914
## Terminal 2 

---

## 1. Preparação

- Software utilizado: Cisco Packet Tracer 9.0.0810
- Terminal: T2
- VTP Domain: rc2526deg6
- Bloco IPv4: 10.63.128.0/17

---

## 2. Convenção de Nomes

### Switches
- MC: MC-T2-L1-Sala6
- IC: IC-T2-L1-Sala6
- HC: HC-T2-L1-SalaX
- CP: CP-T2-L1-SalaX

### Router
- RT-T2-L1-Sala6

### Nota
- Hostnames iguais ao Display Name (sem espaços)

---

## 3. Construção da Topologia

### Equipamentos utilizados:
- 1 Router Cisco 2811
- Switches modelo PT-Empty:
    - 1 MC
    - 1 IC
    - 3 HC
    - 8 CP

---

### 3.1 Módulos adicionados

Em cada switch:

1. Aba **Physical**
2. Desligar o switch
3. Adicionar módulo(s):
    - **PT-SWITCH-NM-1FGE** (fibra)
4. Ligar novamente

---

### 3.2 Ligações

- Backbone → Fibra Ótica Monomodo (linha laranja com "M")
- Ligações horizontais → cobre (quando necessário)
- Redundância mantida conforme Sprint 1

---

## 4. Configuração de Switch Principal (MC)

### 4.1 Definir hostname
> enable 

> configure terminal

> hostname MC-T2-L1-Sala6

### 4.2 Configuração VTP
VTP (VLAN Trunking Protocol) é um protocolo de camada 2 que permite a propagação de informações de VLANs em uma rede. Ele é usado para gerenciar VLANs em uma rede com múltiplos switches, garantindo que as VLANs sejam consistentes em toda a rede.

> vtp domain rc2526deg6

> vtp mode server

## 5. Configuração dos restantes switches
Para cada switch (IC, HC, CP):

### 5.1 Definir hostname
> enable

> configure terminal

> hostname [nome do switch]


### 5.2 Configuração VTP
VTP (VLAN Trunking Protocol) é um protocolo de camada 2 que permite a propagação de informações de VLANs em uma rede. Ele é usado para gerenciar VLANs em uma rede com múltiplos switches, garantindo que as VLANs sejam consistentes em toda a rede.

> vtp domain rc2526deg6
 
> vtp mode client

## 6. Criação de VLANs (no MC)
VLANs são usadas para segmentar uma rede em domínios de broadcast menores, melhorando a segurança e o desempenho. Cada VLAN é identificada por um número (ID) e pode ser associada a um nome para facilitar a identificação.

vlan 773
name CampusBackbone
exit

vlan 774
name SwitchesDMZ
exit

vlan 775
name T2-UserOutlets
exit

vlan 776
name T2-WiFi
exit

vlan 777
name T2-VoIP
exit

vlan 778
name T2-ServersDMZ
exit

vlan 779
name T3-UserOutlets
exit

vlan 780
name T3-WiFi
exit

vlan 781
name T3-VoIP
exit

vlan 782
name T3-ServersDMZ
exit

vlan 783
name T4-UserOutlets
exit

vlan 784
name T4-WiFi
exit

vlan 785
name T4-VoIP
exit

vlan 786
name T4-ServersDMZ
exit

---

## 7. Configuração de Trunk

Um trunk é uma ligação entre switches que permite transportar tráfego de múltiplas VLANs através da mesma ligação física.  
Este tipo de ligação é essencial para o funcionamento do VTP e para garantir a comunicação entre todas as VLANs distribuídas pela rede.

---

### 7.1 Objetivo dos Trunks

Os trunks permitem:
- Transporte de várias VLANs simultaneamente entre switches
- Propagação correta das VLANs criadas no VTP Server
- Interligação de toda a infraestrutura de switching do campus
- Suporte à arquitetura hierárquica da rede (MC → IC → HC → CP)

---

### 7.2 Topologia onde são aplicados trunks

Os trunks são configurados apenas entre switches, mais concretamente:

- Entre o **MC (Main Core switch)** e os **IC (Intermediate switches)**
- Entre o **IC e os HCs (Horizontal switches)**

---

### 7.3 Configuração dos Trunks

A configuração de trunk deve ser aplicada em ambos os lados de cada ligação entre switches.

#### Exemplo de configuração (modo trunk)

Em cada interface de ligação entre switches:

> enable

> configure terminal

> interface gig0/1

> switchport mode trunk

> switchport trunk allowed vlan 773-786

> exit

### 7.4 Configuração por ligação

#### MC ↔ IC

- No MC:
  interface g0/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No IC:
  interface g0/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

#### IC ↔ HC1 (HC-T2-L1-Sala6)

- No IC:
  interface g0/2
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No HC1:
  interface g0/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

#### IC ↔ HC2 (HC-T2-L1-Sala12)

- No IC:
  interface g0/3
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No HC2:
  interface g0/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

#### IC ↔ HC3 (HC-T2-L1-Sala11)

- No IC:
  interface g0/4
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No HC3:
  interface g0/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

### 7.5 Verificação da Configuração
Para verificar a configuração dos trunks, podem ser utilizados os seguintes comandos:
- `show interfaces trunk` → Exibe as interfaces configuradas como trunk e as VLANs permitidas
- `show vlan` → Exibe as VLANs configuradas e as interfaces associadas
- `show vtp status` → Exibe o status do VTP, incluindo o modo e o domínio configurados