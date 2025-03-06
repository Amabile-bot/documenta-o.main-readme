# documenta-o.main-readme
Aqui está uma explicação detalhada sobre as alterações feitas nesse código Terraform e o que você esperaria que funcionasse ao aplicá-lo:

### **Alterações e Explicações:**

1. **Provider AWS:**
   - **O que foi feito:**
     - Definido o provider da AWS, especificando a região `us-east-1` para que os recursos sejam criados nessa região.
   - **O que deveria funcionar:**
     - A conexão com a AWS seria feita na região `us-east-1` para criar todos os recursos subsequentes.

2. **Variáveis de Projeto e Candidato:**
   - **O que foi feito:**
     - Variáveis foram criadas para armazenar o nome do projeto (`projeto`) e o nome do candidato (`candidato`), com valores padrão "VExpenses" e "SeuNome", respectivamente.
   - **O que deveria funcionar:**
     - Essas variáveis são usadas ao longo do código para nomear os recursos de forma dinâmica.

3. **Geração de Chave SSH:**
   - **O que foi feito:**
     - Um recurso `tls_private_key` foi adicionado para gerar uma chave RSA privada, que será usada para acessar a instância EC2.
   - **O que deveria funcionar:**
     - A chave privada é criada para ser usada na instância EC2.

4. **Key Pair no AWS:**
   - **O que foi feito:**
     - Um recurso `aws_key_pair` foi criado para gerar uma chave pública que será usada no EC2, baseada na chave privada criada anteriormente.
   - **O que deveria funcionar:**
     - O recurso `aws_key_pair` usa a chave pública gerada para criar um par de chaves na AWS, permitindo que a instância EC2 seja acessada via SSH.

5. **Criação da VPC:**
   - **O que foi feito:**
     - Um recurso `aws_vpc` foi adicionado para criar uma VPC com o bloco CIDR `10.0.0.0/16`, com suporte a DNS e a atribuição de nomes de host.
   - **O que deveria funcionar:**
     - A VPC será criada com a faixa CIDR `10.0.0.0/16`, permitindo a criação de sub-redes dentro dessa faixa.

6. **Criação da Subnet:**
   - **O que foi feito:**
     - Um recurso `aws_subnet` foi adicionado para criar uma subnet dentro da VPC, com o bloco CIDR `10.0.1.0/24` na zona de disponibilidade `us-east-1a`.
   - **O que deveria funcionar:**
     - A subnet será criada dentro da VPC, permitindo que as instâncias EC2 sejam colocadas nela.

7. **Internet Gateway:**
   - **O que foi feito:**
     - Um recurso `aws_internet_gateway` foi criado para permitir que a VPC tenha conectividade com a internet.
   - **O que deveria funcionar:**
     - A instância EC2 na VPC poderá acessar a internet, uma vez que o gateway foi configurado.

8. Tabela de Roteamento:
   - **O que foi feito:**
     - Foi criada uma tabela de roteamento (`aws_route_table`) associando a rota `0.0.0.0/0` (internet) ao gateway de internet.
   - **O que deveria funcionar:**
     - As instâncias dentro da VPC poderão se comunicar com a internet através da tabela de roteamento.

9. Associação de Tabela de Roteamento:
   - **O que foi feito:**
     - O recurso `aws_route_table_association` foi usado para associar a tabela de roteamento à subnet criada.
   - **O que deveria funcionar:**
     - A subnet configurada terá acesso à internet via a tabela de roteamento.

10. Grupo de Segurança:
    - O que foi feito:
      - Um grupo de segurança (`aws_security_group`) foi criado com regras de acesso:
        - Permitir SSH na porta 22 apenas para o IP específico (`YOUR_IP/32`).
        - Permitir HTTP na porta 80 de qualquer origem (`0.0.0.0/0`).
        - Permitir tráfego de saída para qualquer destino.
    - **O que deveria funcionar:**
      - A instância EC2 estará protegida, permitindo apenas acessos SSH do IP específico e HTTP de qualquer origem.

11. **AMI Debian 12:**
    - **O que foi feito:**
      - Foi configurado o filtro `aws_ami` para buscar a imagem mais recente do Debian 12.
    - **O que deveria funcionar:**
      - A instância EC2 será provisionada com a imagem mais recente do Debian 12.

12. Instância EC2:
    - O que foi feito:
      - Foi configurada uma instância EC2 usando a AMI Debian 12, tipo de instância `t2.micro`, na subnet criada, associada ao grupo de segurança, com a chave SSH e a configuração de IP público.
      - Também foi configurado o `user_data` para atualizar o sistema e instalar o Nginx, além de configurá-lo para iniciar automaticamente.
    - **O que deveria funcionar:**
      - A instância EC2 será provisionada com Debian 12 e Nginx instalado, pronto para ser acessado via SSH e HTTP.

13. Saídas:
    - **O que foi feito:**
      - Foram adicionadas duas saídas:
        - A chave privada para acesso SSH (`private_key`).
        - O IP público da instância EC2 (`ec2_public_ip`).
    - **O que deveria funcionar:**
      - A chave privada será exibida para acesso SSH, e o IP público será fornecido para acessar o Nginx na instância EC2.

### Resumo do Funcionamento Esperado:
- Infraestrutura criada:
  - Uma VPC com uma subnet.
  - Um gateway de internet para conectar a VPC à internet.
  - Uma instância EC2 com Debian 12 e Nginx instalada.
  - O acesso SSH à instância é limitado a um IP específico, enquanto o HTTP é liberado para qualquer origem.
  
  O que deve funcionar 
  - Você deve ser capaz de acessar a instância EC2 via SSH (com a chave gerada) e via HTTP, após provisionamento da instância.
  - A instância deve ter o Nginx funcionando, acessível através do IP público.

