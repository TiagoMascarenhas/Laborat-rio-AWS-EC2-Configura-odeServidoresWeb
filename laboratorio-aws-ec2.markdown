# Laboratório AWS EC2: Configuração de Servidores Web

## Resumo

Este projeto demonstra minha capacidade de trabalhar com o Amazon Web Services (AWS) Elastic Compute Cloud (EC2) para lançar e configurar servidores virtuais. Neste laboratório, fornecido pela Escola da Nuvem, lancei uma instância EC2 pelo Console de Gerenciamento da AWS, configurei um servidor web, criei um grupo de segurança e usei o AWS CloudShell para automatizar a criação de outra instância. O laboratório destaca habilidades em computação em nuvem, gerenciamento de servidores Linux e configuração de infraestrutura na AWS.

## Objetivos

- Lançar um servidor web de teste usando o Console de Gerenciamento da AWS.
- Configurar o servidor web para permitir conexões HTTP.
- Criar uma instância EC2 usando comandos no AWS CloudShell.
- Encerrar instâncias EC2 para gerenciar recursos de forma eficiente.

## Habilidades Demonstradas

- Gerenciamento de instâncias EC2 na AWS
- Configuração de servidores Linux (Amazon Linux 2)
- Configuração de grupos de segurança para acesso à rede
- Uso do AWS CloudShell para automação via linha de comando
- Scripting em Bash para inicialização de servidores
- Resolução de problemas no Console AWS e credenciais

## Implementação

### Passo 1: Acesso ao Console de Gerenciamento da AWS

Acessei o Console de Gerenciamento da AWS com as credenciais fornecidas pela Escola da Nuvem. Verifiquei a conta no canto superior direito para garantir que estava usando as credenciais corretas, evitando cobranças desnecessárias. Em caso de erro de "credenciais inválidas", cliquei no link "To logout, click here" para resolver.

### Passo 2: Lançamento de uma Instância EC2 pelo Console

1. Selecionei a **Amazon Linux 2 AMI (HVM)** como imagem de máquina.
2. Escolhi o tipo de instância **t2.micro** para eficiência de custo.
3. Criei um par de chaves chamado `parchave-TiagoMascarenhas` usando o formato RSA e `.pem`, salvando o arquivo em local seguro.
4. Configurei um grupo de segurança chamado `Tiagomascarnhas-grupo` para permitir tráfego HTTP (porta 80).
5. Adicionei um script de dados do usuário para instalar e iniciar um servidor HTTP:

   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl enable httpd
   systemctl start httpd
   echo "<html><h1>Bem-vindo ao meu servidor web!</h1></html>" > /var/www/html/index.html
   ```
6. Lancei a instância e copiei o endereço IPv4 público (`54.86.73.5`) para acessar o servidor web.

**Captura de Tela**: \[Inserir captura de tela do Console AWS mostrando a instância lançada\]

### Passo 3: Configuração do Servidor Web

Acessei o IP público (`http://54.86.73.5`) no navegador e confirmei a mensagem "Bem-vindo ao meu servidor web!", verificando que o servidor HTTP estava funcionando.

**Captura de Tela**: \[Inserir captura de tela da saída do servidor web no navegador\]

### Passo 4: Lançamento de uma Instância EC2 via CloudShell

1. Abri o AWS CloudShell e criei um grupo de segurança com o nome `Tiagomascarnhas-grupo`:

   ```bash
   SECURITY_GROUP_ID=$(aws ec2 create-security-group --group-name Tiagomascarnhas-grupo --description "Permitir HTTP" --query "GroupId" --output text)
   ```
2. Autorizei o tráfego HTTP:

   ```bash
   aws ec2 authorize-security-group-ingress --group-id $SECURITY_GROUP_ID --protocol tcp --port 80 --cidr 0.0.0.0/0
   ```
3. Lancei uma nova instância EC2 com o ID `i-0d36d8dc45648f9b2`:

   ```bash
   aws ec2 run-instances --instance-type t2.micro --image-id ami-12345678 --key-name parchave-TiagoMascarenhas --security-group-ids $SECURITY_GROUP_ID
   ```

   Nota: Usei o ID da AMI correto para o Amazon Linux 2.

**Captura de Tela**:


![image](https://github.com/user-attachments/assets/9c6d6498-7709-4b55-a73c-cb14aed1a921)


### Passo 5: Encerramento das Instâncias

Para evitar cobranças, encerrei a instância com ID `i-0d36d8dc45648f9b2` e excluí o grupo de segurança `Tiagomascarnhas-grupo` pelo Console AWS.

## Desafios e Soluções

- **Erro de Credenciais**: Enfrentei um erro de "credenciais inválidas" ao fazer login. Resolvi saindo e autenticando novamente com as credenciais fornecidas.

## Resultados

- Lancei com sucesso um servidor web funcional usando o Console AWS no endereço `54.86.73.5`.
- Automatizei a criação da instância EC2 com ID `i-0d36d8dc45648f9b2` via CloudShell, demonstrando proficiência em automação.
- Configurei o grupo de segurança `Tiagomascarnhas-grupo` e acessei o servidor via HTTP.
- Encerrei os recursos para evitar custos adicionais.

## Aprendizados

- Compreendi como provisionar e gerenciar instâncias EC2 na AWS.
- Aprendi a configurar servidores Linux e grupos de segurança para acesso à rede.
- Ganhei experiência com o AWS CloudShell para automação de infraestrutura.
- Desenvolvi habilidades de resolução de problemas ao corrigir erros no laboratório.

## Recursos Adicionais

- Iniciar uma instância do Amazon EC2
- Tipos de Instâncias
- Amazon Machine Images (AMI)
- Pares de Chaves do Amazon EC2
