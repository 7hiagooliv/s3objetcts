# Desafio: Tarefas Automatizadas com Amazon S3, Lambda e DynamoDB usando LocalStack

Este repositório consolida as anotações, insights e códigos práticos desenvolvidos durante o laboratório de automação de infraestrutura AWS. O objetivo principal é simular um ambiente em nuvem localmente para processar uploads de arquivos e registrar metadados de forma automatizada.

## 🛠️ Arquitetura do Fluxo
1. **Upload**: Um arquivo é enviado para o bucket do **Amazon S3**.
2. **Gatilho (Trigger)**: O S3 detecta o novo objeto e dispara um evento para o **AWS Lambda**.
3. **Processamento**: A função Lambda extrai os dados do evento (Nome do bucket, Nome do arquivo, Tamanho e Data).
4. **Persistência**: A Lambda salva esses metadados em uma tabela do **Amazon DynamoDB**.

---

## 💻 Configuração do Ambiente Local (LocalStack)

Para emular os serviços da AWS localmente sem gerar custos, utilizamos o **LocalStack** via Docker.

### Passo 1: Subir o LocalStack
Crie um arquivo `docker-compose.yml` na raiz do projeto:

```yaml
version: '3.8'

services:
  localstack:
    container_name: localstack_main
    image: localstack/localstack
    ports:
      - "127.0.0.1:4566:4566"            # Edge port para todos os serviços
    environment:
      - SERVICES=s3,lambda,dynamodb      # Serviços ativados
      - AWS_DEFAULT_REGION=us-east-1
    volumes:
      - "${LOCALSTACK_VOLUME_DIR:-./volume}:/var/lib/localstack"
