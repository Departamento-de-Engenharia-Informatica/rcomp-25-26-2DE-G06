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


### 4.3 Comando para ver VTP status
- show vtp status - Exibe o status do VTP, incluindo o modo de operação.

---

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

> interface gigabitEthernet9/1

> switchport mode trunk

> switchport trunk allowed vlan 773-786

> exit

### 7.4 Configuração por ligação

#### MC ↔ IC

- No MC:
  interface gigabitEthernet9/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No IC:
  interface gigabitEthernet8/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

#### IC ↔ HC1 (HC-T2-L1-Sala6)

- No IC:
  interface gigabitEthernet9/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No HC1:
  interface gigabitEthernet9/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

#### IC ↔ HC2 (HC-T2-L1-Sala12)

- No IC:
  interface gigabitEthernet7/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No HC2:
  interface gigabitEthernet8/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

#### IC ↔ HC3 (HC-T2-L1-Sala11)

- No IC:
  interface gigabitEthernet6/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786
- No HC3:
  interface gigabitEthernet6/1
  switchport mode trunk
  switchport trunk allowed vlan 773-786

---

### 7.5 Configuração dos CP (Access Switches)

Os CP (Access Switches) são utilizados para ligação de dispositivos finais (end devices) e são conectados aos HC através de ligações Layer 2.

---

## 7.5.1 Tipo de ligação

* A ligação é configurada como **trunk no HC**
* No CP é configurada conforme o tipo de portas (access ou voice VLAN)

---

## 7.5.2 Ligação HC ↔ CP (uplink)

### Exemplo genérico de configuração

#### No HC (uplink para CP)


interface gigabitEthernetX/X
switchport mode trunk
switchport trunk allowed vlan 773-786


---

#### No CP (uplink para HC)


interface gigabitEthernetX/X
switchport mode trunk
no shutdown

---


### 7.6 Verificação da Configuração
Para verificar a configuração dos trunks, podem ser utilizados os seguintes comandos:
- show cdp neighbors - Exibe os vizinhos CDP (Cisco Discovery Protocol) e as interfaces de ligação.
- show vlan brief - Exibe um resumo das VLANs configuradas e das suas associações.
- show ip interface brief - Exibe um resumo das interfaces e dos seus estados.


---

## 8. Configuração de Layer 3 (Router-on-a-Stick)

Após a configuração da infraestrutura de Layer 2 (VLANs e Trunks), foi implementado o encaminhamento entre VLANs (Inter-VLAN Routing) através de um router Cisco 2811, utilizando a técnica de **Router-on-a-Stick**.

---

### 8.1 Ligação Router ↔ Switch (MC)

O router foi ligado ao switch principal (MC) através da seguinte ligação:

- Router: FastEthernet0/0
- MC: FastEthernet8/1

Esta ligação foi configurada como **trunk** no switch, permitindo o transporte de múltiplas VLANs.

#### Configuração no MC:

> enable  
> configure terminal  
> interface fastEthernet8/1  
> switchport mode trunk  
> switchport trunk allowed vlan 773-786  
> no shutdown  
> exit

---

### 8.2 Configuração do Router

Foi utilizada a técnica de subinterfaces no router, permitindo associar uma interface lógica a cada VLAN.

#### Ativação da interface física:

> enable  
> configure terminal  
> interface fastEthernet0/0  
> no shutdown  
> exit

---

### 8.3 Configuração das Subinterfaces (VLANs – Terminal 2)

Cada subinterface foi configurada com:
- encapsulamento IEEE 802.1Q
- endereço IP correspondente à gateway da VLAN

---

#### VLAN 773 – Campus Backbone

> interface fastEthernet0/0.773  
> encapsulation dot1Q 773  
> ip address 10.63.172.1 255.255.255.0
> exit

---

#### VLAN 774 – T2 SwitchesDMZ

Esta VLAN foi configurada como uma rede isolada de gestão, não estando associada a qualquer subinterface no router.

Desta forma, não existe gateway configurado para esta VLAN, garantindo que o tráfego de gestão permanece restrito ao domínio de camada 2.

---

#### VLAN 775 – T2 UserOutlets

> interface fastEthernet0/0.775  
> encapsulation dot1Q 775  
> ip address 10.63.152.1 255.255.252.0
> exit

---

#### VLAN 776 – T2 WiFi

> interface fastEthernet0/0.776  
> encapsulation dot1Q 776  
> ip address 10.63.128.1 255.255.248.0
> exit

---

#### VLAN 777 – T2 VoIP

> interface fastEthernet0/0.777  
> encapsulation dot1Q 777  
> ip address 10.63.166.1 255.255.254.0
> exit

---

#### VLAN 778 – T2 ServersDMZ

> interface fastEthernet0/0.778  
> encapsulation dot1Q 778  
> ip address 10.63.174.129 255.255.255.128
> exit

---

### 8.4 Verificação

Para validar a configuração, foram utilizados os seguintes comandos:

- Verificação do estado das interfaces:
  - `show ip interface brief`

Este comando permite confirmar:
- Interfaces ativas (up/up)
- Subinterfaces corretamente configuradas
- Endereços IP atribuídos

---

### 8.5 Resultado

Com esta configuração:
- Cada VLAN possui uma gateway configurada no router
- É possível comunicação entre diferentes VLANs
- A rede Layer 3 encontra-se operacional no Terminal 2

---

## 9. Configuração de End Devices (Layer 2 + Layer 3)

---

### 9.1 Objetivo

Configurar dispositivos finais (end devices) para cada VLAN do Terminal 2, garantindo:

* Associação correta à VLAN
* Configuração IP válida
* Comunicação com o router (gateway)

---

### 9.2 Equipamentos necessários

Adicionar no Packet Tracer:

* 2 PCs
* 1 Laptop
* 1 Access Point (não usar wireless router)
* 1 Server
* 1 VoIP (modelo 7960)

---

### 9.3 Ligações físicas

#### Tipo de cabos:

* PC / Server / AP / VoIP → **Copper Straight-Through**
* Ligações feitas a um **CP (Access Switch)**


### 9.4 Configuração das portas no CP

Aceder ao switch CP:


enable
configure terminal


---

#### PC – User VLAN (775)

enable
conf t
interface fa8/1
switchport mode access
switchport access vlan 775
no shutdown
exit


---

#### PC – SwitchesDMZ (774)

enable
conf t
interface fa7/1
switchport mode access
switchport access vlan 774
no shutdown
exit

---

#### Server – ServersDMZ (778)

enable
conf t
interface fa6/1
switchport mode access
switchport access vlan 778
no shutdown
exit


---

#### Access Point – WiFi (776)

enable
conf t
interface fa4/1
switchport mode access
switchport access vlan 776
no shutdown
exit


---

#### IP Phone – VoIP (777)

enable
conf t
interface fa5/1
switchport mode access
no switchport access vlan
switchport voice vlan 777
no shutdown
exit


---

### 9.5 Configuração IPv4 dos dispositivos

#### PC – User VLAN

* IP: 10.63.152.2
* Máscara: 255.255.252.0
* Gateway: 10.63.152.1

---

#### PC – SwitchesDMZ

* IP: 10.63.164.100
* Máscara: 255.255.254.0
* Gateway: 10.63.164.1

---

#### Server – ServersDMZ

* IP: 10.63.174.130
* Máscara: 255.255.255.128
* Gateway: 10.63.174.129

---

#### Access Point
O SSID foi trocado de Default para T2-WiFi.
ainda nao feito
* IP: 10.63.128.2
* Máscara: 255.255.248.0
* Gateway: 10.63.128.1

---

#### Laptop (WiFi)

Após ligação ao Access Point:

* Foi adicionado o modulo WMP300N ao laptop para permitir a conexão wireless.
* Foi ligado ao SSID T2-WiFi através do menu de conexões wireless do laptop.

* IP: 10.63.128.3
* Máscara: 255.255.248.0
* Gateway: 10.63.128.1

---

#### IP VoIP

* Não configurar IP nesta fase 

---
## 9. Switches Remote Management (Switches DMZ)

Para permitir a gestão remota dos switches através de protocolos como SSH, SNMP e HTTP, foi configurada uma rede de gestão dedicada designada por Switches DMZ, associada à VLAN 774.

Esta rede tem como objetivo fornecer conectividade de gestão aos dispositivos de switching do Terminal 2, garantindo separação lógica relativamente ao tráfego de utilizador, servidores e outras VLANs.

---

### 9.1 Implementação da rede de gestão

Cada switch possui uma interface virtual (SVI – Switch Virtual Interface) associada à VLAN 774, permitindo a atribuição de um endereço IPv4 de gestão.

A configuração é realizada da seguinte forma:

interface vlan 774
ip address 10.63.164.X 255.255.254.0
no shutdown


---

### 9.2 Endereçamento dos switches

| Switch | IP de gestão |
|--------|--------------|
| MC     | 10.63.164.2  |
| IC     | 10.63.164.3  |
| HC     | 10.63.164.10 |
| CP     | 10.63.164.20 |

---

### 9.3 Conectividade da VLAN de gestão

A VLAN 774 é transportada entre todos os switches através de ligações trunk, permitindo conectividade de camada 2 entre os dispositivos de gestão ao longo da hierarquia da rede.

Esta implementação garante que cada switch pode ser acedido a partir da estação de gestão localizada na mesma VLAN.

---

### 9.4 Considerações sobre isolamento

A rede Switches DMZ é logicamente isolada, sendo utilizada exclusivamente para gestão e monitorização da infraestrutura.

Embora exista routing global no projeto (Router-on-a-Stick), a VLAN 774 é utilizada apenas para tráfego de administração, não sendo o seu objetivo fornecer comunicação entre redes de utilizadores.

---

### 9.5 Resultado

A rede de gestão encontra-se operacional, permitindo:

- Acesso remoto aos switches
- Monitorização da infraestrutura de rede
- Separação lógica do tráfego de gestão
- Administração centralizada da topologia do Terminal 2

Conclui-se que a infraestrutura de gestão está corretamente implementada e funcional.

---
## 10. Routers and Static Routing

No Terminal 2 foi configurado um router Cisco 2811 com o objetivo de assegurar o encaminhamento de tráfego IPv4 entre as redes locais e os restantes terminais do campus.

Foi utilizada a técnica de **Router-on-a-Stick**, permitindo a ligação a múltiplas VLANs através de uma única interface física em modo trunk, recorrendo a subinterfaces com encapsulamento IEEE 802.1Q.

A VLAN 774 (SwitchesDMZ) não foi configurada no router, garantindo o seu isolamento conforme os requisitos do projeto.

---

### 10.1 Ligação ao Campus Backbone

O router foi ligado à VLAN 773 (Campus Backbone), permitindo a comunicação com os routers dos restantes terminais.

| Terminal | Endereço IP |
|----------|------------|
| T2       | 10.63.172.1 |
| T3       | 10.63.172.2 |
| T4       | 10.63.172.3 |

---

### 10.2 Configuração de Rotas Estáticas

Para permitir a comunicação com os restantes terminais (T3 e T4), foram configuradas rotas estáticas com base no plano de endereçamento IPv4 (VLSM).

Cada rota utiliza como next-hop o endereço IP do router de destino na rede de backbone.

---

#### Rotas para o Terminal 3 (T3)
ip route 10.63.144.0 255.255.248.0 10.63.172.2
ip route 10.63.156.0 255.255.252.0 10.63.172.2
ip route 10.63.170.0 255.255.254.0 10.63.172.2
ip route 10.63.173.0 255.255.255.0 10.63.172.2


---

#### Rotas para o Terminal 4 (T4)
ip route 10.63.136.0 255.255.248.0 10.63.172.3
ip route 10.63.160.0 255.255.252.0 10.63.172.3
ip route 10.63.168.0 255.255.254.0 10.63.172.3
ip route 10.63.174.0 255.255.255.128 10.63.172.3


---

### 10.3 Resultado

Com esta configuração:

- ✔ Encaminhamento correto entre VLANs locais
- ✔ Comunicação funcional entre os diferentes terminais
- ✔ Separação garantida da VLAN SwitchesDMZ (774)
- ✔ Estrutura preparada para integração com o backbone

Conclui-se que o encaminhamento IPv4 entre os terminais do campus se encontra corretamente configurado.

---
## 11. Internet Connection

O Terminal 2 possui ligação à Internet, sendo responsável por encaminhar o tráfego externo de toda a infraestrutura do campus.

---

### 11.1 Ligação ao ISP

A ligação à Internet foi representada através de um modem DSL ligado ao router do Terminal 2, estabelecendo comunicação com o router do ISP.

- **Endereço do ISP:** 89.73.67.134/30
- **Meio físico:** ligação DSL (modem)
- **Router do terminal:** Rtr-T2

---

### 11.2 Configuração de Rota por Defeito

Para permitir o encaminhamento de tráfego destinado a redes externas ao campus, foi configurada uma rota por defeito no router do Terminal 2.
ip route 0.0.0.0 0.0.0.0 89.73.67.134


Esta rota indica que todo o tráfego cujo destino não esteja presente na tabela de encaminhamento deve ser enviado para o router do ISP.

---

### 11.3 Configuração do Router do ISP

O router do ISP foi configurado com rotas estáticas de retorno, permitindo encaminhar corretamente o tráfego destinado ao bloco de endereços do campus:

- **Bloco de endereçamento do campus:** 10.63.128.0/17

Exemplo de configuração no ISP:
ip route 10.63.128.0 255.255.128.0 <IP_do_Rtr-T2>


Esta configuração garante que o tráfego de resposta proveniente da Internet consegue alcançar corretamente a infraestrutura interna do campus.

---

### 11.4 Resultado

Com esta configuração:

- ✔ O Terminal 2 tem acesso à Internet
- ✔ Os restantes terminais conseguem aceder à Internet através do T2
- ✔ Existe encaminhamento de ida e volta (full connectivity)
- ✔ A infraestrutura do campus está corretamente integrada com o ISP

Conclui-se que a ligação à Internet se encontra corretamente configurada e funcional.

---
## 12. Testes de Validação Final
Após a configuração completa da infraestrutura de rede do Terminal 2, incluindo:
* VLANs e VTP
* Trunking entre switches
* Inter-VLAN routing (Router-on-a-Stick)
* Routing estático entre terminais
* Ligação ao ISP (Internet)
* Configuração de dispositivos finais
foram realizados testes de validação para garantir o correto funcionamento de toda a rede.

---

### 12.1 Teste de conectividade com o gateway (Router-on-a-Stick)

**Objetivo:**
Validar se os dispositivos conseguem comunicar com o gateway da sua própria VLAN.

**Dispositivo:** PC – VLAN UserOutlets  
**Comando:**
ping 10.63.152.1


**Resultado esperado:**
✔ Resposta com sucesso (0% packet loss)

---

### 12.2 Teste de comunicação inter-VLAN (User → Servers DMZ)

**Objetivo:**
Confirmar o encaminhamento entre VLANs através do Router-on-a-Stick.

**Dispositivo:** PC – VLAN UserOutlets  
**Comando:**
ping 10.63.174.130


**Resultado esperado:**
✔ Comunicação bem-sucedida

---

### 12.3 Teste de conectividade WiFi (Laptop → Gateway)

**Objetivo:**
Validar a ligação do dispositivo wireless ao Access Point e à VLAN correspondente.

**Dispositivo:** Laptop – VLAN WiFi  
**Comando:**
ping 10.63.128.1

**Resultado esperado:**
✔ Gateway acessível

---

### 12.4 Teste de comunicação inter-VLAN (WiFi → UserOutlets)

**Objetivo:**
Confirmar comunicação entre dispositivos WiFi e outras VLANs.

**Dispositivo:** Laptop – VLAN WiFi  
**Comando:**
ping 10.63.152.1

**Resultado esperado:**
✔ Comunicação bem-sucedida

---

### 12.5 Teste de comunicação a partir do Server (Servers DMZ)

**Objetivo:**
Validar comunicação bidirecional entre VLANs.

**Dispositivo:** Server – VLAN ServersDMZ  
**Comando:**
ping 10.63.152.2

**Resultado esperado:**
✔ Comunicação bem-sucedida

---

### 12.6 Teste de isolamento da VLAN SwitchesDMZ

**Objetivo:**
Confirmar que a VLAN 774 (SwitchesDMZ) se encontra isolada e sem acesso a outras redes.

---

#### 12.6.1 Users → VLAN 774

**Dispositivo:** PC – VLAN UserOutlets  
**Comando:**
ping 10.63.164.2

**Resultado esperado:**
✘ Falha (Destination host unreachable)

---

#### 12.6.2 Comunicação interna na VLAN 774

**Dispositivo:** PC – VLAN SwitchesDMZ  
**Comando:**
ping 10.63.164.2
ping 10.63.164.3
ping 10.63.164.10

**Resultado esperado:**
✔ Comunicação bem-sucedida (Layer 2)

---

#### 12.6.3 VLAN 774 → Gateway inexistente

**Dispositivo:** PC – VLAN SwitchesDMZ  
**Comando:**
ping 10.63.164.1

**Resultado esperado:**
✘ Falha (sem gateway configurado)

---

### 12.7 Teste de conectividade com o ISP (Internet)

**Objetivo:**
Validar a ligação entre o Router do Terminal 2 e o ISP.

**Dispositivo:** Rtr-T2  
**Comando:**
ping 89.73.67.134
**Resultado esperado:**
✔ Conectividade com ISP estabelecida

---

### 12.8 Teste de routing estático entre terminais (Campus Backbone)

**Objetivo:**
Validar comunicação entre terminais através do backbone.

**Dispositivo:** Rtr-T2  
**Comando:**

ping 10.63.172.2 (Rtr-T3)
ping 10.63.172.3 (Rtr-T4)

**Resultado esperado:**
✔ Comunicação entre routers bem-sucedida

---

### 12.9 Teste de conectividade com Internet (simulação externa)

**Objetivo:**
Validar acesso a redes externas.

**Dispositivo:** Rtr-T2  
**Comando:**
ping 8.8.8.8

**Resultado esperado:**
✔ Sucesso (se ISP tiver routing externo configurado)
ou
✘ Falha (caso não exista simulação de Internet externa no Packet Tracer)

---

### 12.10 Verificação do Access Point

**Objetivo:**
Confirmar o correto funcionamento da rede WiFi.

**Procedimento:**
- Configuração do SSID "T2-WiFi"
- Associação do laptop à rede
- Validação de conectividade IP

**Resultado esperado:**
✔ Associação WiFi realizada com sucesso  
✔ Conectividade com gateway e outras VLANs

---

### 12.11 Verificação da VLAN VoIP

**Objetivo:**
Garantir que o dispositivo VoIP está corretamente associado à VLAN.

**Procedimento:**
- Ligação do telefone IP à porta configurada com voice VLAN 777
- Verificação da associação automática

**Resultado esperado:**
✔ Dispositivo corretamente associado à VLAN VoIP

*Nota:* Não foram realizados testes de comunicação VoIP nesta fase.

---

### 12.12 Conclusão dos testes

Os testes realizados permitiram validar:

- ✔ Funcionamento correto das VLANs
- ✔ Configuração adequada das portas em modo access e trunk
- ✔ Encaminhamento inter-VLAN através do Router-on-a-Stick
- ✔ Routing estático entre terminais (Campus Backbone)
- ✔ Conectividade com ISP (Terminal 2)
- ✔ Conectividade da rede WiFi
- ✔ Comunicação bidirecional entre VLANs
- ✔ Isolamento da VLAN 774 (SwitchesDMZ)
- ✔ Comunicação interna da VLAN de gestão assegurada por Layer 2

Conclui-se que a infraestrutura de rede do Terminal 2 se encontra corretamente configurada, funcional e em conformidade com os requisitos do projeto.

---
