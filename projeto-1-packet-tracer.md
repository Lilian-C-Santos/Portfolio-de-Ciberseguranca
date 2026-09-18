# Projeto 1: Hardening de Equipamentos e Acesso Remoto Seguro (SSH)

## 1. Cenário de Negócio e Gestão de Risco
Em muitas redes corporativas, equipamentos de infraestrutura novos vêm configurados de fábrica com credenciais padrão e serviços de gerenciamento inseguros ativos (como o Telnet). Isso expõe a empresa a riscos graves de interceptação de dados (Man-in-the-Middle) e acessos não autorizados à gerência da rede.

O objetivo deste laboratório foi aplicar o processo de Hardening (fortalecimento de ativos) no roteador principal da empresa, garantindo confidencialidade na administração remota e conformidade com boas práticas de segurança da informação.

---

## 2. Matriz de Endereçamento IP

| Dispositivo | Interface | Endereço IP | Máscara de Rede | Gateway Padrão | Função |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R-GW-CORP | GigabitEthernet0/0/0 | 192.168.1.1 | 255.255.255.0 | N/A | Gateway Principal |
| PC-ADMIN-01 | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | Estação de Gerência |
| PC-USER-02 | FastEthernet0 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 | Estação de Trabalho |

---

## 3. Implementação Técnica (Linha de Comando - Cisco IOS)

### A. Configuração de Identidade e Criptografia
Alteração do nome do equipamento, definição de senha secreta encriptada para modo privileged e criptografia global das senhas salvas.

Router> enable
Router# configure terminal
Router(config)# hostname R-GW-CORP
R-GW-CORP(config)# enable secret SenhaForteEnable2026!
R-GW-CORP(config)# service password-encryption

### B. Notificação de Advertência (Banner MOTD)
Inclusão de aviso legal para respaldar a empresa juridicamente contra acessos não autorizados.

R-GW-CORP(config)# banner motd # ATENCAO: Acesso restrito a pessoas autorizadas. Conexoes monitoradas. #

### C. Habilitação de SSH v2 e Desativação do Telnet
Geração de chave RSA de 2048 bits para estabelecer tunelamento criptografado e restrição das linhas VTY apenas para tráfego SSH.

R-GW-CORP(config)# ip domain-name empresa.local
R-GW-CORP(config)# crypto key generate rsa general-keys modulus 2048
R-GW-CORP(config)# username admin privilege 15 secret SenhaAdminSSH2026!
R-GW-CORP(config)# line vty 0 4
R-GW-CORP(config-line)# transport input ssh
R-GW-CORP(config-line)# login local
R-GW-CORP(config-line)# exit

---

## 4. Validação dos Testes e Troubleshooting

Para validar se as políticas de segurança foram aplicadas com sucesso, executei os seguintes comandos de verificação no terminal:

1. Teste de Bloqueio do Telnet:
   - Comando: telnet 192.168.1.1
   - Resultado Esperado: Conexão recusada imediatamente pelo roteador.

2. Validação do Acesso SSH:
   - Comando no Prompt do PC: ssh -l admin 192.168.1.1
   - Resultado Esperado: Solicitação de credenciais e acesso concedido de forma criptografada.

3. Verificação de Criptografia de Senhas:
   - Comando: show running-config
   - Resultado Esperado: Todas as senhas exibidas em formato hash (massa ilegível de caracteres), sem expor texto limpo.
