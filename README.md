# Infraestrutura Segura com Terraform e AWS

Este projeto provisiona uma infraestrutura segura na AWS usando **Terraform**, com foco em alta segurança e automação. A configuração cria uma instância EC2 Debian 12, protege os acessos com grupos de segurança dedicados e instala automaticamente o servidor **Nginx**.

## 🏗️ **Recursos Criados**

- **VPC** com suporte a DNS e IPs públicos.
- **Subrede pública** para hospedar a instância EC2.
- **Internet Gateway** para acesso externo.
- **Tabela de Rotas** para direcionar tráfego externo à subrede.
- **Instância EC2** com Debian 12, configurada via **User Data** para instalar o Nginx e ajustar regras de segurança.
- **Grupos de Segurança (SGs)** segmentados:
  - **SSH SG:** Acesso restrito à porta 22 para o IP definido (padrão `0.0.0.0/0`, mas pode ser ajustado).
  - **Web SG:** Permite tráfego HTTP (80) e HTTPS (443) de qualquer IP.
- **Par de chaves RSA** para acesso seguro via SSH.

## 🔒 **Melhorias de Segurança**

- **Acesso SSH restrito:** Agora o SSH só é permitido para o IP especificado na variável `ssh_allowed_ip`.
- **Divisão de grupos de segurança:** Isolamento das regras de SSH e Web para reduzir a superfície de ataque.
- **Login root desabilitado:** Acesso root via SSH é bloqueado para maior segurança.
- **Firewall UFW ativado:** Regras adicionais no sistema operacional para reforçar a segurança.

## 🚀 **Automação com Nginx**

A instância EC2 é provisionada para:
- Instalar e iniciar o **Nginx** automaticamente.
- Configurar o **UFW** para liberar portas 22, 80 e 443.
- Reiniciar o serviço SSH com a restrição de login root.

Após a criação, o Nginx estará rodando e acessível via IP público da instância.

## 📂 **Arquivos principais**
- `main.tf`: Código principal Terraform para provisionar a infraestrutura.
- `variables.tf`: Definição das variáveis para personalizar a configuração.
- `outputs.tf`: Saídas com IP público e chave privada.

## 📘 **Como usar**

1. **Configurar Terraform:**
```bash
terraform init
```

2. **Planejar a infraestrutura:**
```bash
terraform plan -var="ssh_allowed_ip=<SEU_IP_CIDR>"
```

3. **Aplicar as mudanças:**
```bash
terraform apply -var="ssh_allowed_ip=<SEU_IP_CIDR>"
```

4. **Acessar a instância:**
```bash
ssh -i <CAMINHO_PARA_CHAVE> admin@<IP_PUBLICO_EC2>
```

5. **Testar o Nginx:**
Abra o navegador e acesse:
```
http://<IP_PUBLICO_EC2>
```
Deve aparecer a página padrão do Nginx! 🎉

## ✅ **Resultados Esperados**
- A instância EC2 deve estar acessível pelo IP público.
- O **Nginx** deve estar ativo e respondendo na porta 80.
- O acesso SSH deve ser possível apenas pelo IP autorizado.

Essa configuração oferece um ambiente seguro e pronto para hospedar aplicações web básicas, com práticas recomendadas de segurança e automação. Se quiser aprimorar ainda mais, estou aqui para ajudar! 🚀

