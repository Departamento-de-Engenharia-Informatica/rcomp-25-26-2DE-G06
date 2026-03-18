RCOMP 2025-2026 Project 1 - Sprint 1 planning
===========================================
### Sprint master: 1240914 ###

# 1. Sprint's backlog #

>  T.1.1 - Development of a structured cabling project for Terminal 2 encompassed floors, including the campus backbone.

>  T.1.2 - Development of a structured cabling project for Terminal 3 encompassed floors.

>  T.1.3 - Development of a structured cabling project for Terminal 5 encompassed floors.

>  T.1.4 - Development of a structured cabling project for Terminal 4 encompassed floors.

### Nota sobre a tarefa T.1.4
Inicialmente, a equipa era composta por quatro elementos e a tarefa T.1.4 (Terminal 4) foi atribuída ao estudante 1240895.
Posteriormente, devido a uma reorganização dos grupos, a equipa passou a ter apenas três elementos. Apesar disso, após contacto com a docente da unidade curricular, foi autorizada a continuação do desenvolvimento da tarefa T.1.4 pelo estudante 1240895.

# 2. Technical decisions and coordination #

#### Decisions: ####
* Backbone cable types used are Campus Backbone and Vertical Backbones
* Copper cable type to be adopted is CAT7
* Copper cable wiring standard adopted is 568A
* Fibre optic cable type to be adopted is monomode 
* Redundant links between ICs and between HCs

# 3. Subtasks assignment #

#### Distribution: ####
* 1240914- Structured cable design for Terminal 2
* 1241138 - Structured cable design for Terminal 3
* 1240895 - Structured cable design for Terminal 4



RCOMP 2025-2026 Project 1 - Sprint 2 planning
===========================================
### Sprint master:  ###

# 1. Sprint's backlog #

>  T.2.1 - Development of a layer two and layer three Packet Tracer simulation for Terminal 2 encompassed floors, the campus backbone, and including the internet connection.

>  T.2.2 - Development of a layer two and layer three Packet Tracer simulation for Terminal 3 encompassed floors and the campus backbone. Integration of every member’s Packet Tracer simulation into a single simulation.

>  T.2.3 - Development of a layer two and layer three Packet Tracer simulation for Terminal 5 encompassed floors and the campus backbone.

>  T.2.4 - Development of a layer two and layer three Packet Tracer simulation for Terminal 4 encompassed floors and the campus backbone.

### Nota sobre a tarefa T.1.4
A tarefa T.2.3 (Terminal 5) é ignorada por a equipa possuir apenas 3 elementos, e ter sido acordado anteriormente com a docente da unidade curricular que a tarefa T.1.4 (Terminal 4) seria continuada pelo estudante 1240895, mesmo após a reorganização dos grupos.

# 2. Technical decisions and coordination #

#### Decisions: ####
* Packet Tracer Version: 8.2.1 para garantir compatibilidade na integração final.
* VTP Domain: rc2526deg6
* VTP Mode: Pelo menos um switch em modo Server e os restantes em modo Client.
* VLAN IDs Range: 773 a 793.
* Spanning Tree Protocol (STP): Mantido ativo por defeito em todos os switches para gerir links redundantes.
* Routing: Static routing em todos os routers (modelo 2811 obrigatoriamente).
* ISP router’s IPv4 node address: 89.73.67.134/30
* IPv4 address space to use (Block of IPv4 addresses): 10.63.128.0/17 

### Device naming convention:
* Switches: Sw-[Terminal]-[Localização/Sala] (ex: Sw-T2-Sala6).
* Routers: Rtr-[Terminal] (ex: Rtr-T2).
* Hostnames: Devem coincidir com o Display Name mas sem espaços.

# 3. Subtasks assignment #

#### Distribution: ####
* 1240914- Structured cable design for Terminal 2
* 1241138 - Structured cable design for Terminal 3
* 1240895 - Structured cable design for Terminal 4

