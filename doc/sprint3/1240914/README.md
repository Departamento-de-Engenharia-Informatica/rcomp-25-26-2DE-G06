# RCOMP – Projeto 1 – Sprint 2 - 1240914
## Terminal 2

---

# FASE 1 — Limpeza + base do OSPF

## 1.1 Remover routing estático (EXCETO default route)

No router T2:
enable
conf t
no ip route 10.63.144.0 255.255.248.0 10.63.172.2
no ip route 10.63.156.0 255.255.252.0 10.63.172.2
no ip route 10.63.170.0 255.255.254.0 10.63.172.2
no ip route 10.63.173.0 255.255.255.0 10.63.172.2

no ip route 10.63.136.0 255.255.248.0 10.63.172.3
no ip route 10.63.160.0 255.255.252.0 10.63.172.3
no ip route 10.63.168.0 255.255.254.0 10.63.172.3
no ip route 10.63.174.0 255.255.255.128 10.63.172.3

## 1.2 Ativar OSPF no router T2

router ospf 1
router-id 2.2.2.2

## 1.3 Anunciar redes do T2 no OSPF

### BackBone (area 0)
network 10.63.172.0 0.0.0.255 area 0

### T2-WiFi (/21 → 10.63.128.0)
network 10.63.128.0 0.0.7.255 area 2

### T2-UserOutlets (/22 → 10.63.152.0)
network 10.63.152.0 0.0.3.255 area 2

### T2-VoIP (/23 → 10.63.166.0)
network 10.63.166.0 0.0.1.255 area 2

### T2-ServersDMZ (/25 → 10.63.174.128)
network 10.63.174.128 0.0.0.127 area 2

**Nota:**
- VLAN 773 (backbone) → area 0
- resto do T2 → area 2

## 1.4 Default route no OSPF

default-information originate

## 1.5 Verificações

Ver se o OSPF está ativo: show ip protocols
Ver vizinhos OSPF: show ip ospf neighbor (depois de t3 e t4 terem ospf ativo)
Ver base de dados OSPF: show ip ospf database
Ver rotas aprendidas via OSPF: show ip route ospf
Ver tabela completa de routing: show ip route
Ver interfaces OSPF: show ip ospf interface brief


# FASE 2 — Preparar SERVIDORES (antes de DHCP e DNS)

## 2.1 Adicionar segundo servidor na DMZ (T2)

**Na VLAN 778:**
- server1 → já existe (DNS root + web depois) (deve ser tratado como ns)
- server2 → NOVO (HTTP/HTTPS) (deve ser nomeado como server1)

**Configurar:**
- IP fixo: 10.63.174.131
- gateway: 10.63.174.129

## 2.2 Ativar HTTP no server2
No Packet Tracer:
- Services → HTTP → ON
- Services → HTTPS → ON
- Criar página simples: “Terminal 2 - HTTP Server (Sprint 3)”

# FASE 3 — DHCP

## 3.1 Criar pools no router T2

Exemplo VLAN 775 (UserOutlets):

ip dhcp excluded-address 10.63.152.1 10.63.152.10

ip dhcp pool T2-UserOutlets
network 10.63.152.0 255.255.252.0
default-router 10.63.152.1
dns-server 10.63.174.130
domain-name rcomp-25-26-cc-gn

## 3.2 WiFi VLAN 776
ip dhcp excluded-address 10.63.128.1 10.63.128.10

ip dhcp pool T2-WiFi
network 10.63.128.0 255.255.248.0
default-router 10.63.128.1
dns-server 10.63.174.130
domain-name rcomp-25-26-cc-gn

## 3.3 VoIP VLAN 777 
ip dhcp excluded-address 10.63.166.1 10.63.166.10

ip dhcp pool T2-VoIP
network 10.63.166.0 255.255.254.0
default-router 10.63.166.1
option 150 ip 10.63.166.1

# FASE 4 — VoIP (Call Manager no router)
No router T2:

telephony-service
max-ephones 10
max-dn 10
ip source-address 10.63.166.1 port 2000
auto assign 1 to 2

Depois criar telefones (mínimo 2):
ephone-dn 1
number 2001

ephone-dn 2
number 2002

# FASE 5 — DNS (ROOT no T2)
No server DNS (10.63.174.130):

## 5.1 Nome:
ns.rcomp-25-26-2de-g6

## 5.2 Registos obrigatórios:

**A record:**
- server1 → 10.63.174.130
- server2 → 10.63.174.131

**CNAME:**
- www → server1
- web → server1
- dns → ns

## 5.3 Glue records 

Adicionar:
- NS de T3
- NS de T4

# FASE 6 — NAT (só depois de DNS funcionar)
No router T2:

## 6.1 Definir interfaces:
interface fa0/0
ip nat inside

interface serial/...
ip nat outside

## 6.2 NAT static (HTTP/HTTPS → server DNS)
ip nat inside source static tcp 10.63.174.130 80 89.73.67.134 80
ip nat inside source static tcp 10.63.174.130 443 89.73.67.134 443

# FASE 7 — ACLs
Aqui vai-se aplicar regras em ordem:

## 7.1 Anti spoofing

## 7.2 ICMP allow

## 7.3 DMZ rules

## 7.4 Router protection

## 7.5 Allow rest


