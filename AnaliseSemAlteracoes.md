# README: Infraestrutura AWS com Terraform

Este projeto define a criação de uma infraestrutura básica na AWS utilizando Terraform. A configuração provisiona recursos essenciais para rodar uma instância EC2 com uma chave SSH segura, conectada a uma rede VPC com acesso público.

## 📂 Estrutura da Infraestrutura

### 🔧 **Provider AWS**
- Configuração do provedor AWS na região `us-east-1`.

### 📊 **Variáveis**
- `projeto`: Nome do projeto (padrão: `VExpenses`).
- `candidato`: Nome do candidato (padrão: `SeuNome`).

### 🔑 **Chave SSH**
- `tls_private_key`: Gera uma chave privada RSA de 2048 bits.
- `aws_key_pair`: Cria um par de chaves AWS a partir da chave pública gerada, nomeada com o padrão: `{projeto}-{candidato}-key`.

### 🌐 **Rede (VPC e Subnet)**
- `aws_vpc`: Cria uma VPC com o CIDR `10.0.0.0/16`, habilitando DNS.
- `aws_subnet`: Sub-rede pública com o CIDR `10.0.1.0/24` na zona `us-east-1a`.

### 🌍 **Acesso à Internet**
- `aws_internet_gateway`: Cria um IGW para a VPC.
- `aws_route_table`: Define uma tabela de rotas permitindo tráfego externo (`0.0.0.0/0`) pelo IGW.
- `aws_route_table_association`: Associa a sub-rede pública à tabela de rotas.

### 🔐 **Grupo de Segurança (SG)**
- Regras de entrada (ingress):
  - Porta 22 (SSH) liberada para qualquer IP (`0.0.0.0/0`).
- Regras de saída (egress):
  - Todo o tráfego de saída permitido.

### 🖼️ **Imagem da Instância (AMI)**
- `data.aws_ami`: Busca a AMI mais recente do Debian 12 (64 bits, HVM).

### 🖥️ **Instância EC2**
- Cria uma instância `t2.micro` com as configurações:
  - 20 GB de volume EBS (`gp2`), deletado na rescisão.
  - Associação de IP público.
  - Script de inicialização (`user_data`) para atualizar e atualizar o sistema.

### 📤 **Outputs**
- `private_key`: Chave privada para acessar a instância EC2.
- `ec2_public_ip`: Endereço IP público da instância EC2.

## 🛠️ **Como Usar**

1. **Inicializar o Terraform:**
```bash
terraform init
```

2. **Visualizar o plano de execução:**
```bash
terraform plan
```

3. **Aplicar a configuração:**
```bash
terraform apply
```

4. **Acessar a instância via SSH:**
```bash
ssh -i <arquivo-chave>.pem admin@<ec2_public_ip>
```

---

Com esse código, você terá uma infraestrutura funcional e segura para iniciar seus projetos na AWS! Se quiser ajustes ou otimizações, estou aqui para ajudar. 🚀

