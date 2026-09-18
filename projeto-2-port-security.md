# Projeto 2: Segurança de Camada 2 - Bloqueio de Acessos Físicos Indevidos com Port Security

## 1. Cenário de Negócio e Gestão de Risco
Pontas de rede não protegidas em ambientes corporativos (como tomadas RJ45 em salas de reunião ou recepção) representam um ponto crítico de vulnerabilidade. Um atacante ou usuário não autorizado pode conectar um dispositivo estranho (como um notebook pessoal ou um roteador não homologado) para capturar pacotes ou tentar invasões internas.

O objetivo deste laboratório foi implementar a funcionalidade de Port Security nos switches de acesso da empresa, garantindo que apenas dispositivos com endereços MAC autorizados consigam trafegar dados na rede local.

---

## 2. Matriz de Endereçamento IP e Mapeamento de Portas

| Dispositivo | Interface Switch | Endereço IP | Endereço MAC | Status Esperado |
| :--- | :--- | :--- | :--- | :--- |
| SW-ACESSO-01 | FastEthernet0/1 | N/A | N/A | Ativo (Trunk / Uplink) |
| PC-CORP-01 | FastEthernet0/2 | 192.168.10.10 | 0060.2F4A.1111 | Autorizado (Conectado) |
| PC-CORP-02 | FastEthernet0/3 | 192.168.10.11 | 0060.2F4A.2222 | Autorizado (Conectado) |
| PC-INTRUSO | FastEthernet0/2 | 192.168.10.99 | 00D0.BA99.9999 | Bloqueado (Violação) |

---

## 3. Implementação Técnica (Linha de Comando - Cisco IOS)

### A. Seleção das Interfaces e Ativação do Modo de Acesso
Definição das portas do switch que atenderão as estações de trabalho e garantia do modo de acesso estático.

Switch> enable
Switch# configure terminal
Switch(config)# hostname SW-ACESSO-01
SW-ACESSO-01(config)# interface range fastEthernet 0/2 - 24
SW-ACESSO-01(config-if-range)# switchport mode access

### B. Habilitação e Configuração do Port Security
Ativação do limite de dispositivos por porta, aprendizado dinâmico do endereço MAC (Sticky) e definição da ação em caso de violação de segurança.

SW-ACESSO-01(config-if-range)# switchport port-security
SW-ACESSO-01(config-if-range)# switchport port-security maximum 1
SW-ACESSO-01(config-if-range)# switchport port-security mac-address sticky
SW-ACESSO-01(config-if-range)# switchport port-security violation shutdown
SW-ACESSO-01(config-if-range)# exit

---

## 4. Validação dos Testes e Troubleshooting

Para validar a eficácia da política de contenção física, executei os seguintes testes de simulação:

1. Aprendizado Sticky do MAC Autorizado:
   - Ação: Conexão do PC-CORP-01 na porta FastEthernet0/2.
   - Resultado Esperado: O switch aprendeu o endereço MAC do computador corporativo e gravou automaticamente na configuração em execução.

2. Simulação de Ataque / Troca de Dispositivo (PC Intruso):
   - Ação: Desconexão do PC corporativo e conexão do PC-INTRUSO na mesma porta FastEthernet0/2.
   - Resultado Esperado: O switch detectou a divergência do endereço MAC, gerou um alerta e alterou imediatamente o estado da interface para err-disabled (desligando a porta).

3. Comandos de Verificação e Restabelecimento:
   - Comando para verificar status de segurança: show port-security interface fastEthernet 0/2
   - Comando para reativar porta após desectar o dispositivo não autorizado:
     SW-ACESSO-01(config-if)# shutdown
     SW-ACESSO-01(config-if)# no shutdown
