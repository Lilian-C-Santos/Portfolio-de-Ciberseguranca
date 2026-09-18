# Projeto 2: Bloqueando Acessos Físicos Indevidos (Port Security)

## O que eu fiz aqui
Neste laboratório, o foco foi a segurança na Camada 2 (Acesso à Rede). O objetivo foi evitar que pessoas mal-intencionadas conectem computadores ou dispositivos desconhecidos diretamente nas tomadas de rede das salas de reunião ou escritórios para tentar acessar a rede interna.

## Equipamentos usados na simulação
* 1 Switch Cisco (camada de acesso)
* 2 PCs autorizados (máquinas da empresa)
* 1 PC não autorizado (simulando um invasor ou dispositivo estranho)

## Como configurei a segurança no Switch (Passo a passo no terminal)

### 1. Limitando o número de dispositivos por porta
Acesse a interface onde o computador do funcionário fica conectado e ativei o recurso de *Port Security*, definindo que apenas um único endereço MAC (endereço físico da placa de rede) pode trafegar por ali.

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
