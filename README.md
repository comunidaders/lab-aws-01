# Desafio AWS: Implementação de uma Aplicação Web Escalável

## Objetivo
O objetivo deste desafio é implementar uma aplicação web escalável utilizando vários serviços da AWS. Os participantes devem demonstrar habilidades em provisionamento de infraestrutura, configuração de serviços e boas práticas de segurança.

## Cenário
Você foi contratado para construir uma infraestrutura na AWS para uma aplicação web que deve ser altamente disponível, escalável e segura. A aplicação consiste em um front-end estático hospedado em um bucket S3 e um back-end que consiste em uma API rodando em instâncias EC2. O tráfego deve ser distribuído usando um Load Balancer, e os dados devem ser armazenados em um banco de dados RDS.

## Requisitos

### 1. Bucket S3 para o Front-end
- Crie um bucket S3 para hospedar o conteúdo estático da aplicação web.
- Habilite a hospedagem de site estático no bucket S3.
- Configure políticas de bucket para tornar o conteúdo acessível publicamente.

### 2. Instâncias EC2 para o Back-end
- Crie uma VPC com subnets públicas e privadas.
- Lance instâncias EC2 em uma subnet privada para rodar a API.
- Configure um Security Group para permitir tráfego HTTP e HTTPS para as instâncias EC2 apenas através do Load Balancer.
- Utilize um script de user data para instalar e iniciar o servidor da API nas instâncias EC2.

### 3. Elastic Load Balancer (ELB)
- Crie um Load Balancer para distribuir o tráfego entre as instâncias EC2.
- Configure o Load Balancer para escutar nas portas HTTP (80) e HTTPS (443).
- Associe o Load Balancer com o Security Group correto.

### 4. Banco de Dados RDS
- Crie uma instância RDS para o banco de dados da aplicação.
- Configure o RDS em uma subnet privada e configure as regras de segurança apropriadas.
- Assegure-se de que a instância RDS seja acessível apenas pelas instâncias EC2.

### 5. Boas Práticas de Segurança
- Habilite o versionamento no bucket S3.
- Configure o Multi-AZ no RDS para alta disponibilidade.
- Implemente o IAM com políticas adequadas para acessar os recursos necessários.
- Configure o CloudWatch para monitorar a saúde das instâncias EC2 e do Load Balancer.

### 6. Entrega e Documentação
- Documente o processo de configuração e as decisões tomadas.
- Forneça instruções claras de como a infraestrutura foi configurada através do console da AWS.
- Inclua capturas de tela ou links para recursos no console da AWS que demonstram que a infraestrutura está funcionando conforme esperado.

## Critérios de Avaliação

1. **Completeness**: Todos os requisitos foram atendidos e a infraestrutura está completa e funcional.
2. **Segurança**: As boas práticas de segurança foram implementadas e documentadas.
3. **Documentação**: A documentação é clara, detalhada e fácil de seguir.
4. **Funcionamento**: A aplicação web é acessível e funciona conforme esperado.

## Passo a Passo de Configuração

### 1. Configurar o Bucket S3

1. **Criar Bucket S3**:
   - Acesse o console da AWS S3.
   - Clique em "Create bucket".
   - Defina o nome do bucket (ex.: `my-web-app-bucket`) e a região.
   - Complete as configurações restantes e crie o bucket.

2. **Habilitar Hospedagem de Site Estático**:
   - Acesse o bucket criado.
   - Vá para a aba "Properties".
   - Selecione "Static website hosting" e configure o index e error document (ex.: `index.html`, `error.html`).

3. **Configurar Políticas de Bucket**:
   - Vá para a aba "Permissions".
   - Em "Bucket Policy", adicione a política para permitir acesso público:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "PublicReadGetObject",
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::my-web-app-bucket/*"
         }
       ]
     }
     ```

### 2. Configurar a VPC e Instâncias EC2

1. **Criar VPC e Subnets**:
   - Acesse o console da AWS VPC.
   - Crie uma VPC com um CIDR block adequado (ex.: `10.0.0.0/16`).
   - Crie subnets públicas e privadas dentro da VPC.

2. **Lançar Instâncias EC2**:
   - Acesse o console da AWS EC2.
   - Lance instâncias EC2 dentro da subnet privada.
   - Configure o Security Group para permitir tráfego HTTP e HTTPS apenas através do Load Balancer.

3. **Configurar User Data**:
   - Durante o lançamento da instância EC2, adicione o script de user data para configurar o servidor da API:
     ```sh
     #!/bin/bash
     yum update -y
     yum install -y httpd
     systemctl start httpd
     systemctl enable httpd
     ```

### 3. Configurar o Elastic Load Balancer

1. **Criar Load Balancer**:
   - Acesse o console da AWS ELB.
   - Crie um Load Balancer e configure-o para escutar nas portas HTTP (80) e HTTPS (443).
   - Adicione as instâncias EC2 ao Load Balancer.

2. **Associar Security Groups**:
   - Certifique-se de que o Load Balancer está associado ao Security Group correto.

### 4. Configurar o Banco de Dados RDS

1. **Criar Instância RDS**:
   - Acesse o console da AWS RDS.
   - Crie uma instância RDS com Multi-AZ habilitado para alta disponibilidade.
   - Configure o Security Group para permitir acesso apenas das instâncias EC2.

### 5. Boas Práticas de Segurança

1. **Habilitar Versionamento no S3**:
   - Na aba "Properties" do bucket S3, habilite o versionamento.

2. **Configurar IAM**:
   - Crie políticas e roles IAM apropriadas para acessar os recursos.

3. **Configurar CloudWatch**:
   - Configure alarmes e dashboards no CloudWatch para monitorar a saúde das instâncias EC2 e do Load Balancer.

### 6. Documentação

- Documente cada etapa do processo com capturas de tela e descrições detalhadas.
- Inclua links para os recursos no console da AWS que demonstram a funcionalidade da infraestrutura.

---

Espero que isso ajude! Se precisar de ajustes ou informações adicionais, estou à disposição.
