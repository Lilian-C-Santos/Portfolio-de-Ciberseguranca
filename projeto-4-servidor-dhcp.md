# Projeto 4: Automação e Distribuição de Endereçamento IP com Servidor DHCP no Roteador

## 1. Cenário de Negócio e Gestão de Risco
Em redes corporativas em expansão, a configuração manual de endereços IP (estática) em cada estação de trabalho é ineficiente, sujeita a erros humanos (como duplicidade de IPs) e gera um alto custo operacional para a equipe de TI. Além disso, a falta de controle centralizado dificulta a aplicação de políticas de rede e navegação.

O objetivo deste laboratório foi configurar o roteador principal da infraestrutura como Servidor DHCP (Dynamic Host Configuration Protocol), garantindo a entrega automática e centralizada de IP, Máscara de Rede, Gateway Padrão e Servidor DNS para os computadores da rede local.

---

## 2. Topologia do Laboratório

### Estrutura da Rede no Cisco Packet Tracer
<img width="693" height="537" alt="image" src="https://github.com/user-attachments/assets/3316cc21-e225-4c8a-bbff-671dca8aaf7c" />


---

## 3. Matriz de Endereçamento e Escopo DHCP

| Dispositivo / Ativo | Interface | Tipo de Endereçamento | Endereço IP / Sub-rede | Gateway Padrão | Servidor DNS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R-GW-2911 | GigabitEthernet0/0 | Estático (Gateway) | 192.168.1.1 /24 | N/A | N/A |
| Escopo DHCP | Pool: MULHER_DIGITAL2026 | Dinâmico (Automático) | 192.168.1.0 /24 | 192.168.1.1 | 8.8.8.8 |
| PC0 | FastEthernet0 | Dinâmico (DHCP) | 192.168.1.2 (Atribuído) | 192.168.1.1 | 8.8.8.8 |
| PC1 | FastEthernet0 | Dinâmico (DHCP) | 192.168.1.3 (Atribuído) | 192.168.1.1 | 8.8.8.8 |

---

## 4. Implementação Técnica (Linha de Comando - Cisco IOS)

### A. Ativação da Interface da Rede Local (Gateway)
Definição do endereço IP fixo na interface do roteador conectada ao switch e ativação da porta.

Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

### B. Criação e Configuração do Pool DHCP
Criação do escopo de endereçamento dinâmico, definindo a sub-rede, a rota padrão e o servidor DNS corporativo.

Router(config)# ip dhcp pool MULHER_DIGITAL2026
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

### C. Persistência de Dados
Salvamento das configurações da memória RAM para a memória não-volátil (NVRAM).

Router# copy running-config startup-config

---

## 5. Validação dos Testes e Troubleshooting

### Validação do Recebimento de IP via DHCP no Cliente (PC0)
<img width="527" height="188" alt="image" src="https://github.com/user-attachments/assets/1cb8f080-affe-4fcc-946d-04880d5c03ab" />

Para validar se o serviço DHCP está entregando as configurações corretamente aos clientes, foram executados os seguintes passos de verificação:

1. Solicitando IP via DHCP no Cliente (PC0 e PC1):
   - Ação: Alteração do modo de configuração IP de "Static" para "DHCP" nas propriedades do adaptador de rede dos computadores.
   - Resultado Esperado: Confirmação de recebimento do IP dinâmico dentro da faixa 192.168.1.0/24, acompanhado de Gateway (192.168.1.1) e DNS (8.8.8.8).

2. Comandos de Verificação e Inspeção no Roteador:
   - Comando para listar os IPs distribuídos: show ip dhcp binding
   - Comando para verificar métricas e conflitos: show ip dhcp server statistics
