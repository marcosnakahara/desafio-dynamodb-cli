# desafio-dynamodb-cli

- Install AWS CLI
[download](https://aws.amazon.com/pt/cli/)
- aws configure
  - Create a acess key: AWS Login > Security Credentials > Access Keys
  - Input:
    - AWS Access Key ID
    - AWS Secret Access Key
    - Default region name
    - Default output format
  
  
## Commands
Note: commands below are formatted to be executed on Windows. For another OS replace the caret (^) with backslash (\).

### Create a table
```
aws dynamodb create-table ^
    --table-name Game ^
    --attribute-definitions ^
        AttributeName=Producer,AttributeType=S ^
        AttributeName=GameTitle,AttributeType=S ^
    --key-schema ^
        AttributeName=Producer,KeyType=HASH ^
        AttributeName=GameTitle,KeyType=RANGE ^
    --provisioned-throughput ^
        ReadCapacityUnits=10,WriteCapacityUnits=5
```
