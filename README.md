# README: Infraestrutura AWS Segura com Terraform

Este projeto define a criação de uma infraestrutura AWS segura utilizando Terraform. A configuração inclui uma instância EC2 com Nginx, protegida por grupos de segurança dedicados para SSH e tráfego web.

## 📂 Estrutura da Infraestrutura

### 🔧 **Provider AWS**
- Configuração do provedor AWS na região `us-east-1`.

### 📊 **Variáveis**
- `projeto`: Nome do projeto (padrão: `VExpenses`).
- `candidato`: Nome do candidato (padrão: `SeuNome`).
- `ssh_allowed_ip`: IP permitido para acesso SSH (padrão: `0.0.0.0/0`).

### 🔑 **Chave SSH**
- `tls_private_key`: Gera uma chave privada RSA de 2048 bits.
- `aws_key_pair`: Cria um par de chaves AWS com o nome `{projeto}-{candidato}-key`.

### 🔐 **Grupos de Segurança**
- `ssh_sg`: Permite acesso SSH da origem definida em `ssh_allowed_ip`.
- `web_sg`: Permite acesso HTTP (porta 80) e HTTPS (porta 443) para qualquer origem.

### 🖥️ **Instância EC2**
- Cria uma instância Debian 12 `t2.micro` com:
  - 20 GB de volume EBS (`gp2`).
  - IP público associado.
  - Grupos de segurança para SSH e web.
  - Script de inicialização que:
    - Instala e configura o Nginx.
    - Configura o `ufw` para proteger a instância.
    - Desabilita login root via SSH.

### 📤 **Outputs**
- `ec2_public_ip`: Endereço IP público da instância EC2.
- `private_key`: Chave privada para acessar a instância EC2.

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

5. **Verificar a página padrão do Nginx:**
```bash
http://<ec2_public_ip>
```

---

Essa configuração oferece uma instância pronta para hospedar aplicações web com segurança reforçada. Se quiser ajustar ou adicionar recursos, estou aqui para ajudar! 🚀

