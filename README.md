# 🚀 Automação de Infraestrutura na AWS com CloudFormation

Análise prática, arquitetura e documentação do laboratório de Infraestrutura como Código (IaC) desenvolvido para o desafio de projeto da **Digital Innovation One (DIO)**. O principal objetivo deste repositório é consolidar os fundamentos do **AWS CloudFormation** para o provisionamento automatizado, consistente e seguro de recursos computacionais na nuvem.

---

## 🎯 Cenário de Negócio Proposto
Para aplicar os conceitos abordados em aula, foi desenvolvido um template declarativo em **YAML** focado no deploy automatizado de um ambiente de Servidor Web corporativo. 

A Stack desenvolvida mitiga erros humanos de configuração manual ao provisionar uma instância computacional robusta, configurando automaticamente as dependências do servidor HTTP no momento da inicialização (*boot*) e blindando a rede com regras restritivas de acesso.

---

## 🏗️ Arquitetura e Recursos Provisionados

O CloudFormation gerencia de forma centralizada o ciclo de vida dos seguintes componentes integrados:

1. **AWS::EC2::Instance (Servidor Web):** Instância virtual EC2 inicializada com um script de automação (`UserData`) que atualiza o sistema, instala e inicializa o servidor de páginas Apache (HTTPD).
2. **AWS::EC2::SecurityGroup (Firewall de Rede):** Grupo de segurança configurado de forma granular, liberando estritamente as portas **80** (HTTP para tráfego web) e **22** (SSH para administração remota).
3. **Mapeamento Multi-Região (Mappings):** Arquitetura dinâmica preparada para deploys globais, selecionando a AMI (Amazon Linux 2) correta de forma automática dependendo da região AWS escolhida para a Stack.

### 🗺️ Fluxo Lógico do Provisionamento (IaC)

```mermaid
graph TD
    Template[📝 Template YAML/JSON] -->|Upload via Console ou CLI| CF[🔥 AWS CloudFormation Engine]
    
    subgraph AWS Cloud [Infraestrutura AWS]
        CF -->|1. Cria Primeiro| SG{🛡️ Security Group}
        CF -->|2. Cria com Dependência| EC2[💻 Instância EC2]
        
        SG -->|Aplica Regras Ingress: Portas 80 e 22| EC2
        EC2 -->|Executa no Boot: UserData| Apache[⚙️ Instalação Apache HTTPD]
    end
    
    Apache --> StackOut([🏁 Stack Criada: CREATE_COMPLETE])
