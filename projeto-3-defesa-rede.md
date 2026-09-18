# Projeto 3: Criando Regras de Filtro de Tráfego com ACL (Access Control List)

## 1. Cenário de Negócio e Gestão de Risco
Em uma arquitetura de rede corporativa, diferentes departamentos possuem necessidades distintas de acesso a dados. Permitir que o setor de vendas acesse livremente os servidores de banco de dados do setor financeiro viola o princípio do menor privilégio e expõe a empresa a vazamentos de dados confidenciais.

O objetivo deste laboratório foi implementar Listas de Controle de Acesso (ACLs Estendidas) no roteador principal para realizar a filtragem granular de pacotes de Camada 3 e 4, permitindo apenas serviços estritamente necessários entre sub-redes.

---

## 2. Matriz de Endereçamento IP e Políticas de Segurança

| Rede / Origem | Sub-rede / IP | Destino | Serviço / Porta | Ação Política |
| :--- | :--- | :--- | :--- | :--- |
| Rede Vendas | 192.168.10.0/24 | Servidor Web (192.168.100.10) | HTTP / HTTPS (80/443) | Permitir |
| Rede Vendas | 192.168.10.0/24 | Servidor DB (192.168.100.10) | ICMP (Ping) / DB | Bloquear |
| Rede Financeiro | 192.168.20.0/24 | Servidor (192.168.100.10) | Todos os Protocolos (IP) | Permitir |

---

## 3. Implementação Técnica (Linha de Comando - Cisco IOS)

### A. Criação da ACL Estendida
Criação de uma ACL nomeada do tipo estendida, permitindo especificar IP de origem, IP de destino e protocolo de transporte.

R-GW-CORP> enable
R-GW-CORP# configure terminal
R-GW-CORP(config)# ip access-list extended REGRAS-DEFESA-SERVIDORES

### B. Definição das Regras de Filtragem
Inclusão de regras com comentários (remarks) para documentar a intenção de cada filtro na política corporativa.

R-GW-CORP(config-ext-nacl)# remark Permite trafego Web da rede Vendas para o Servidor
R-GW-CORP(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 host 192.168.100.10 eq 80
R-GW-CORP(config-ext-nacl)# remark Permite acesso total do Financeiro ao Servidor
R-GW-CORP(config-ext-nacl)# permit ip 192.168.20.0 0.0.0.255 host 192.168.100.10
R-GW-CORP(config-ext-nacl)# remark Bloqueia qualquer outro acesso nao especificado
R-GW-CORP(config-ext-nacl)# deny ip any any
R-GW-CORP(config-ext-nacl)# exit

### C. Aplicação da ACL na Interface do Roteador
Vínculo do grupo de acesso na interface de entrada que recebe o tráfego dos usuários.

R-GW-CORP(config)# interface gigabitEthernet 0/0/0
R-GW-CORP(config-if)# ip access-group REGRAS-DEFESA-SERVIDORES in
R-GW-CORP(config-if)# exit

---

## 4. Validação dos Testes e Troubleshooting

Para comprovar que a filtragem seletiva por serviço está funcionando adequadamente, realizei a validação prática:

1. Teste de Acesso Web (Navegador):
   - Ação: Acesso do PC de Vendas ao endereço do Servidor no navegador (http://192.168.100.10).
   - Resultado Esperado: Página inicial carregada com sucesso (Porta 80 liberada).

2. Teste de Conectividade Direta (Ping):
   - Ação: Comando ping 192.168.100.10 executado no Prompt do PC de Vendas.
   - Resultado Esperado: Mensagem Destination Host Unreachable (Bloqueado pela regra implícita/deny).

3. Comandos de Inspeção e Métricas:
   - Comando para verificar contadores de pacotes filtrados: show access-lists REGRAS-DEFESA-SERVIDORES
   - Resultado Esperado: Exibição dos pacotes aceitos e negados por regra (match counters).
