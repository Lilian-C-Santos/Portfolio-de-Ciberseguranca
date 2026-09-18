# Projeto 3: Criando Regras de Filtro de Tráfego com ACL (Access Control List)

## O que eu fiz aqui
Neste projeto, apliquei o conceito de controle de acesso de tráfego usando Listas de Controle de Acesso (ACLs) no roteador. O objetivo foi criar regras de firewall simples para isolar o setor financeiro e garantir que apenas conexões autorizadas cheguem até o servidor de banco de dados.

## Equipamentos usados na simulação
* 1 Roteador (executando a filtragem de pacotes)
* 1 Servidor Web / Banco de Dados (Zona de Servidores)
* 2 Redes distintas: Rede Vendas (192.168.10.0/24) e Rede Financeiro (192.168.20.0/24)

## Como configurei as regras no Roteador (Passo a passo no terminal)

### 1. Criando uma ACL Estendida
Optei por uma ACL estendida (Extended ACL) porque ela permite especificar a origem, o destino e o protocolo exato (ex: HTTP, HTTPS, Ping) que queremos liberar ou bloquear.

```cisco
R-GW-CORP> enable
R-GW-CORP# configure terminal
R-GW-CORP(config)# ip access-list extended REGRAS-DEFESA-SERVIDORES
