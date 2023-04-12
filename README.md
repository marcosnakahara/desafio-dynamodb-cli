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
Note: commands below are formatted to be executed on Windows. For another OS replace the caret ( ^ ) with backslash ( \\ ).

### Create a table
```
aws dynamodb create-table ^
    --table-name Game ^
    --attribute-definitions ^
        AttributeName=Publisher,AttributeType=S ^
        AttributeName=GameTitle,AttributeType=S ^
    --key-schema ^
        AttributeName=Publisher,KeyType=HASH ^
        AttributeName=GameTitle,KeyType=RANGE ^
    --provisioned-throughput ^
        ReadCapacityUnits=10,WriteCapacityUnits=5
```

### Insert an item
```
aws dynamodb put-item ^
    --table-name Game ^
    --item file://item_game.json
```

### Insert multiple items
```
aws dynamodb batch-write-item ^
    --request-items file://batch_games.json
```

### Create a global secondary index based on franchise
```
aws dynamodb update-table ^
    --table-name Game ^
    --attribute-definitions AttributeName=Franchise,AttributeType=S ^
    --global-secondary-index-updates ^
        "[{\"Create\":{\"IndexName\":\"Franchise-index\",\"KeySchema\":[{\"AttributeName\":\"Franchise\",\"KeyType\":\"HASH\"}],\"ProvisionedThroughput\":{\"ReadCapacityUnits\":10,\"WriteCapacityUnits\":5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

### Create a global secondary index based on publisher and franchise
```
aws dynamodb update-table ^
    --table-name Game ^
    --attribute-definitions^
        AttributeName=Publisher,AttributeType=S ^
        AttributeName=Franchise,AttributeType=S ^
    --global-secondary-index-updates ^
        "[{\"Create\":{\"IndexName\":\"PublisherFranchise-index\",\"KeySchema\":[{\"AttributeName\":\"Publisher\",\"KeyType\":\"HASH\"},{\"AttributeName\":\"Franchise\",\"KeyType\":\"RANGE\"}],\"ProvisionedThroughput\":{\"ReadCapacityUnits\":10,\"WriteCapacityUnits\":5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

### Create a global secondary index based on game title and release year
```
aws dynamodb update-table ^
    --table-name Game ^
    --attribute-definitions^
        AttributeName=GameTitle,AttributeType=S ^
        AttributeName=ReleaseYear,AttributeType=S ^
    --global-secondary-index-updates ^
        "[{\"Create\":{\"IndexName\":\"GameTitleReleaseYear-index\",\"KeySchema\":[{\"AttributeName\":\"GameTitle\",\"KeyType\":\"HASH\"},{\"AttributeName\":\"ReleaseYear\",\"KeyType\":\"RANGE\"}],\"ProvisionedThroughput\": {\"ReadCapacityUnits\":10,\"WriteCapacityUnits\":5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

### Query item by publisher
```
aws dynamodb query ^
    --table-name Game ^
    --key-condition-expression "Publisher = :publisher" ^
    --expression-attribute-values "{\":publisher\":{\"S\":\"Square Enix\"}}"
```

### Query by publisher and game title
```
aws dynamodb query ^
    --table-name Game ^
    --key-condition-expression "Publisher = :publisher and GameTitle = :title" ^
    --expression-attribute-values file://key_conditions.json
```

### Query by secondary index based on franchise
```
aws dynamodb query ^
    --table-name Game ^
    --index-name Franchise-index ^
    --key-condition-expression "Franchise = :name" ^
    --expression-attribute-values  "{\":name\":{\"S\":\"Metal Gear\"}}"
```

### Query by secondary index based on publisher and franchise
```
aws dynamodb query ^
    --table-name Game ^
    --index-name PublisherFranchise-index ^
    --key-condition-expression "Publisher = :v_publisher and Franchise = :v_franchise" ^
    --expression-attribute-values "{\":v_publisher\":{\"S\":\"Square Enix\"},\":v_franchise\":{\"S\":\"Final Fantasy\"}}"
```

### Query by secondary index based on game title and release year
```
aws dynamodb query ^
    --table-name Game ^
    --index-name GameTitleReleaseYear-index ^
    --key-condition-expression "GameTitle = :v_game and ReleaseYear = :v_year" ^
    --expression-attribute-values "{\":v_game\":{\"S\":\"Metal Gear Solid\"},\":v_year\":{\"S\":\"1998\"}}"
```