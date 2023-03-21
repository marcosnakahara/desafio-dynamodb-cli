# desafio-dynamodb-cli

- Instalar AWS CLI
[download](https://aws.amazon.com/pt/cli/)
- aws configure
  - Criar acess key em: AWS Login > Security Credentials > Access Keys
  - Informar:
    - AWS Access Key ID
    - AWS Secret Access Key
    - Default region name
    - Default output format
  
  
## Comandos

### Criar uma tabela
```
aws dynamodb create-table \
    --table-name Game \
    --attribute-definitions \
        AttributeName=Producer,AttributeType=S \
        AttributeName=GameTitle,AttributeType=S \
    --key-schema \
        AttributeName=Producer,KeyType=HASH \
        AttributeName=GameTitle,KeyType=RANGE \
    --provisioned-throughput \
        ReadCapacityUnits=10,WriteCapacityUnits=5
```
