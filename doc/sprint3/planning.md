RCOMP 2025-2026 Project 1 - Sprint 3 planning
===========================================
### Sprint master: 1241138 ###

# 1. Sprint's backlog #
O objetivo deste sprint é a transição para routing dinâmico e a implementação de serviços de rede (DHCP, DNS, VoIP, NAT) e políticas de segurança (ACLs) sobre a infraestrutura de Layer 2 e 3 do sprint anterior.
T.3.1 - Atualização da simulação do Terminal 2: routing OSPF, DHCPv4, VoIP, DNS Root, NAT e ACLs. 
T.3.2 - Atualização da simulação do Terminal 3: routing OSPF, DHCPv4, VoIP, DNS Subdomain, NAT e ACLs. Integração final de todos os terminais no ficheiro campus.pkt. 
T.3.4 - Atualização da simulação do Terminal 4: routing OSPF, DHCPv4, VoIP, DNS Subdomain, NAT e ACLs. 
(A tarefa T.3.3 Terminal 5 é ignorada).

# 2. Technical decisions and coordination #
Nesta secção estão registadas as decisões que garantem a interoperabilidade entre os terminais e a consistência dos serviços globais do campus.

**Routing Dinâmico e Conetividade:**
- Protocolo: OSPF substitui o routing estático (tabelas anteriores devem ser apagadas, exceto a rota por defeito no T2). 
- OSPF Area 0: VLAN 773 (Campus Backbone). 
- Internet (T2): O router T2 injeta a rota para o ISP no OSPF via comando default-information originate. 
- Packet Tracer: Mantida a versão 9.0.0810.
  
**Serviços de Rede:**
- DHCPv4: Configurado nos routers para as VLANs locais. Excluídos: Campus Backbone e DMZs (IPs estáticos). 
- VoIP (Option 150): O pool DHCP da rede VoIP deve incluir a opção 150 com o IP do router (servidor TFTP). 
- NAT: Redirecionamento estático das portas TCP 80 (HTTP) e 443 (HTTPS) vindas do backbone para o servidor DNS local (que terá serviço Web ativo).
- DNS: O domínio raiz será rcomp-25-26-2de-g6
   - Os servidores DNS serão os mesmos do Sprint 2 (já existentes nas DMZs).
   - O servidor DNS do T2 (Root) terá Glue Records (registos A) para apontar para os servidores ns dos subdomínios (T3 e T4).
   - Todos os nós finais usarão o servidor DNS local, configurado via DHCP (PCs) ou manualmente (servidores).

## 2.1 OSPF Area IDs, VoIP e DNS Schema
| Terminal | OSPF Area ID | Prefixo VoIP | Gama de números | Domínio DNS Local             | Router (backbone IP) |
|----------|--------------|--------------|-----------------|-------------------------------|----------------------|
| T2       | Area 2       | 2            | 2000 - 2999     | rcomp-25-26-2de-g6            | 10.63.172.1          |          
| T3       | Area 3       | 3            | 3000 - 3999     | terminal-3.rcomp-25-26-2de-g6 | 10.63.172.2          |  
| T4       | Area 4       | 4            | 4000 - 4999     | terminal-4.rcomp-25-26-2de-g6 | 10.63.172.3          |

- **VoIP Call Forwarding:** Cada router terá dial-peers configurados para encaminhar chamadas com prefixos de outros terminais para o IP do router correspondente no backbone.
- **Configuração de Switch:** As portas para os telefones 7960 terão switchport voice vlan ativa e no switchport access vlan.

## 2.2 Endereçamento de Servidores (DMZ)
Conforme o plano VLSM do Sprint 2, cada terminal utilizará o primeiro servidor existente para DNS e o novo servidor adicionado para HTTP.

| Terminal | Servidor DNS (ns) IP | Servidor HTTP (server1) IP |
|----------|----------------------|----------------------------|
| T2       | 10.63.174.130        | 10.63.174.131              |          
| T3       | 10.63.173.2          | 10.63.173.3                |  
| T4       | 10.63.174.2          | 10.63.174.3                |

- **Registos DNS:** Todos os servidores HTTP terão aliases www e web (CNAME para server1). O alias dns apontará para ns. 
- **Nota NAT:** O serviço HTTP/HTTPS deve ser ativado também no servidor DNS para suportar o redirecionamento NAT vindo do backbone.

## 2.3 Políticas de Firewall (ACLs)
As ACLs serão aplicadas nos routers com a seguinte ordem de precedência:
1. Anti-spoofing: Bloqueio de IPs forjados internos e externos.
2. ICMP: Permitir sempre echo requests/replies.
3. DMZ: Tráfego para a DMZ limitado a DNS, HTTP e HTTPS para os respetivos servidores. Todo o tráfego originado na DMZ é permitido.
4. Router Protection: Bloquear tráfego destinado ao IP do router, exceto: DHCPv4, TFTP, ITS, OSPF e NAT.
5. Transit: Permitir o restante tráfego de passagem.

# 3. Subtasks assignment #

#### Distribution: ####
* 1240914 - T.3.1 - Atualização da simulação do Terminal 2: routing OSPF, DHCPv4, VoIP, DNS Root, NAT e ACLs.
* 1241138 - T.3.2 - Atualização da simulação do Terminal 3: routing OSPF, DHCPv4, VoIP, DNS Subdomain, NAT e ACLs. Integração final de todos os terminais no ficheiro campus.pkt.
* 1240895 - Atualização da simulação do Terminal 4: routing OSPF, DHCPv4, VoIP, DNS Subdomain, NAT e ACLs. 
* (A tarefa T.3.3 Terminal 5 é ignorada).

# 4. Do SPRINT 2...

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
