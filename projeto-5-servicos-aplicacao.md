# Projeto 5: Configuração de Serviços de Aplicação (HTTP/DNS) e Análise de Camadas

## 1. Cenário de Negócio e Gestão de Risco
Em ambientes corporativos, a infraestrutura de rede depende da disponibilidade e integridade de serviços essenciais na camada de aplicação, como o DNS (Domain Name System) e o HTTP/HTTPS (Serviços Web). Falhas na resolução de nomes ou em serviços de aplicação impedem o acesso a sistemas internos e externos, resultando em indisponibilidade operacional.

O objetivo deste laboratório foi implementar um ambiente local contendo um Servidor Central configurado com os serviços Web (HTTP) e de Resolução de Nomes (DNS), validando a comunicação fim-a-fim, a resolução de nomes de domínio e a inspeção do tráfego das Camadas 4 (Transporte) e 7 (Aplicação).

---

## 2. Topologia do Laboratório

### Estrutura da Rede no Cisco Packet Tracer
<img width="226" height="402" alt="image" src="https://github.com/user-attachments/assets/5b0e1782-5c04-49ca-8c18-83bfd9ce6959" />


---

## 3. Matriz de Endereçamento e Serviços

| Dispositivo / Ativo | Interface | Endereço IP | Máscara de Rede | Gateway Padrão | Servidor DNS | Serviços Habilitados |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Servidor-Central | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | N/A | Local (192.168.1.10) | HTTP, HTTPS, DNS |
| PC-Cliente | FastEthernet0 | 192.168.1.5 | 255.255.255.0 | N/A | 192.168.1.10 | Cliente Web / CLI |
| Switch-2960 | Fa0/1 - Fa0/2 | Camada 2 | N/A | N/A | N/A | Comutação de Pacotes |

---

## 4. Implementação Técnica dos Serviços

### A. Configuração do Servidor Web (HTTP/HTTPS)
1. Ativação dos protocolos HTTP e HTTPS no Servidor-Central.
2. Customização do documento de entrada `index.html` para validar a entrega de conteúdo dinâmico ao cliente.

### B. Configuração do Servidor de Nomes (DNS)
1. Ativação do serviço de resolução de nomes.
2. Criação do registro de recursos do tipo A:
   - Domínio (Name): `www.aula.com`
   - Endereço de Destino (Address): `192.168.1.10`

---

## 5. Validação dos Testes e Troubleshooting

### A. Testes de Conectividade e Resolução de Nomes no Cliente

1. Teste de Conectividade ICMP (Ping):
   - Comando executado no PC-Cliente: `ping 192.168.1.10`
   - Resultado: 0% de perda de pacotes, confirmando acessibilidade na Camada 3.

2. Teste de Consulta DNS (Nslookup):
   - Comando executado no PC-Cliente: `nslookup www.aula.com`
   - Resultado: Retorno bem-sucedido mapeando o nome de domínio ao IP `192.168.1.10`.

<img width="808" height="608" alt="image" src="https://github.com/user-attachments/assets/56e125e3-b076-426f-a19a-fc357e599ba1" />


### B. Validação do Acesso Web e Inspeção de PDUs (Modo Simulação)

1. Acesso via Navegador:
   - Abertura do Web Browser no PC-Cliente e requisição da URL `http://www.aula.com`.
   - Resultado: Carregamento da página HTML customizada enviada pelo Servidor-Central.

<img width="766" height="307" alt="image" src="https://github.com/user-attachments/assets/445b69df-4b62-4757-82a4-4d5092f63c61" />


2. Inspeção de Protocolos nas Camadas OSI (PDU Details):
   - Análise de pacotes no Modo Simulação com filtros aplicados (DNS, TCP, HTTP, ICMP).
   - Inspeção de cabeçalhos confirmando o uso da Camada de Transporte (Porta 80 para HTTP e Porta 53 para DNS) e a encapsulação de dados na Camada de Aplicação.
