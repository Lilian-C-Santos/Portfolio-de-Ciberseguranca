# Projeto 1: Montando e Protegendo uma Rede em Ambiente de Simulação

## O que eu fiz aqui
Neste laboratório, montei a estrutura inicial de uma rede corporativa em um ambiente de simulação. O objetivo principal não foi só colocar os aparelhos para conversarem entre si, mas garantir que a parte de gerência do roteador e do switch estivesse protegida logo no primeiro dia, evitando acesso indevido na rede.

## Equipamentos usados na simulação
* 1 Roteador (servindo como o gateway principal da rede)
* 1 Switch (conectando as máquinas locais do escritório)
* 2 PCs (simulando as estações de trabalho dos funcionários)

## Como configurei os equipamentos (Passo a passo no terminal)

### 1. Nomeando o aparelho e escondendo as senhas
A primeira coisa que fiz foi mudar o nome padrão do roteador para facilitar a identificação na rede e ativar a criptografia de senhas no sistema, garantindo que as credenciais não fiquem visíveis na tela.

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R-GW-CORP
R-GW-CORP(config)# enable secret SenhaForteEnable2026!
R-GW-CORP(config)# service password-encryption
