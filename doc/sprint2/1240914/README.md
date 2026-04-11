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

## 3. Construção da Topologia (Layer 1)

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
> vtp domain rc2526deg6

> vtp mode server

## 5. Configuração dos restantes switches
Para cada switch (IC, HC, CP):

### 5.1 Definir hostname
> enable

> configure terminal

> hostname [nome do switch]


### 5.2 Configuração VTP
> vtp domain rc2526deg6
 
> vtp mode client

## 6. Criação de VLANs (no MC)

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