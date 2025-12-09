
# Documentação para converter arquivos Json para Collections **MongoDB**

## Pré-requisitos de Sistema

### 1. Docker
- **Download**: [DOCKER DESKTOP](https://www.docker.com/products/docker-desktop)

- **Tutorial do docker:** [DOCKER TUTORIAL](https://www.youtube.com/watch?v=ZyBBv1JmnWQ)
- **Verificar instalação**:
```bash
docker --version
```

## PASSOS PARA CRIAR CONTAINER MONGO: 

- ```docker pull mongo``` (baixar a imagem do Docker hub)

 - ```docker run -d --name mongo8 -p 27017:27017 mongo``` (criar o container da imagem que você fez o pull)

 ## PASSOS IMPORTAÇÃO DO JSON PARA O MONGO:

 - Baixar o arquivo json no seu computador

 - ```docker cp "local-do-arquivo/arquivo.json" nomedocontainer:/tmp/arquivo.json ``` (Isso irá copiar o arquivo do seu computador, para dentro da pasta **tmp** do seu container)

 - ```docker exec -it 'nome-do-container' mongoimport --db=nome-do-banco --collection=nome-da-colletcion --file=/tmp/arquivo.json``` (Isso irá realizar a conversão do seu arquivo Json para uma collection no MongoDB)

 ## PASSOS REALIZADOS NO TRABALHO:
- Feito o download do zip, com o csv. 
 [DATA_SET_LARGEST_COMPANIES](https://www.kaggle.com/datasets/harshgupta4444/list-of-largest-companies-in-world?resource=download)

 - Convertidos os arquivos do **Data Set** o arquivo **.csv** para **.NDJson** (Newline-Delimited JSON), utilizando o site: [CONVERSON_CSV_NDJSON](https://table.studio/convert/csv/to/ndjson)

 - Realizado o Download desses arquivo para nossa máquina local.

 - Copiado os arquivos locais para dentro da pasta **tmp** do container:
```docker cp C:\Users\lucas\Documents\ciencia-computacao\MESTRADO\modulo1\big-data\archive\Europe.ndjson mongo8:/tmp/Europe.ndjson``` Obs: fazer isso para cada arquivo

- Realizado a importação dos arquivos para tranforma-los em **Collections**: 
```docker exec -it mongo8 mongoimport --db=big_data_db --collection=europe --file=/tmp/Europe.ndjson ```

## Passos para acessar o **MongoDB**

- Entrar no banco: ```mongosh --port 27017``` 

- Entrar na base de dados: ``use big_data_db``

- Ver as collections: ``show collections``

    ![alt text](image.png)

## OPERAÇÕES NO **MONGODB** REALIZADAS TRABALHO:
 Foi seguida a documentação do mongodb para realizarmos as operações: [https://www.mongodb.com/docs/mongodb-shell/crud/](https://www.mongodb.com/docs/mongodb-shell/crud/)

### Inserts

#### Collection europe

```javascript
db.europe.insertMany([{
  "Rank": 200,
  "Company": "FakeEnergyCorp",
  "Industry": "Oil and gas",
  "Revenue(US$ billions)": 154.73,
  "Headquarters": "Norway"
},
{
  "Rank": 201,
  "Company": "FakePetroGlobal",
  "Industry": "Oil and gas",
  "Revenue(US$ billions)": 98.22,
  "Headquarters": "Canada"
},
{
  "Rank": 202,
  "Company": "FakeFuelSolutions",
  "Industry": "Oil and gas",
  "Revenue(US$ billions)": 62.51,
  "Headquarters": "Brazil"
}])

```

#### Collection india

```javascript
db.india.insertOne({
  "Rank": 87,
  "Forbes 2000 rank": 921,
  "Name": "Fake Global Nexa Systems",
  "Headquarters": "Zurich",
  "Revenue(billions US$)": 18.4,
  "Profit(billions US$)": "2.1",
  "Assets(billions US$)": 34.9,
  "Value(billions US$)": 59.3,
  "Industry": "Technology Services"
})
```

#### Collection philippines

``` javascript
db.philippines.insertOne(
{
  "Rank": 57,
  "Name": "Fake Capital Trust Bank",
  "Industry": "Banking",
  "Revenue(USD millions)": 1324,
  "Profits(USD millions)": "187",
  "Employees": 5120,
  "Headquarters": "Oslo"
})
```

#### Collection united_states

``` javascript
db.united_states.insertMany([{
  "Rank": 200,
  "Name": "FakeMotion Motors",
  "Industry": "Automotive industry",
  "Revenue (USD millions)": 95320,
  "Revenue growth": "6.4%",
  "Employees": 48100,
  "Headquarters": "Munich, Germany"
},
{
  "Rank": 201,
  "Name": "FakeDrive Automotive Group",
  "Industry": "Automotive industry",
  "Revenue (USD millions)": 121580,
  "Revenue growth": "3.2%",
  "Employees": 72000,
  "Headquarters": "Nagoya, Japan"
},
{
  "Rank": 202,
  "Name": "FakeTorque Engineering",
  "Industry": "Automotive industry",
  "Revenue (USD millions)": 68440,
  "Revenue growth": "4.8%",
  "Employees": 35200,
  "Headquarters": "Turin, Italy"
}])
```
### Updates 

#### Collection europe

```javascript
db.europe.updateMany(
  { Rank: { $gte: 200 } },
  {
    $set: { 'Revenue(US$ billions)': 160.75 }
  }
)
```

#### Collection india

```javascript 
db.india.replaceOne(
  { Rank: 87 },
  {
  Rank: 807,
  "Forbes 2000 rank": 100,
  Name: "Fake Replaced Global Nexa Systems",
  Headquarters: "LAGES-SC",
  "Revenue(billions US$)": 18.4,
  "Profit(billions US$)": "2.1",
  "Assets(billions US$)": 34.9,
  "Value(billions US$)": 59.3,
  Industry: "Technology 123 Services"
}
)
```

#### Collection philippines

``` javascript 
db.philippines.updateOne( { Rank: 57 },
{
  $set: {
    Name: 'Fake edited Motors'
  },
  $currentDate: { lastUpdated: true }
})
```

#### Collection united_states

``` javascript 
db.united_states.updateOne( { Rank: 200 },
{
  $set: {
    Headquarters: 'BEJA CITY'
  },
  $currentDate: { lastUpdated: true }
})
```

### Deletes 

#### Collection europe

```javascript 
db.europe.deleteMany({
  Rank: { $gte: 200 }
})
```

#### Collection india

```javascript 
db.india.remove({Rank: 807})
```

#### Collection philippines

```javascript 
db.philippines.deleteOne({Rank: 57})
```

#### Collection united_states

```javascript
db.united_states.deleteMany({
  Rank: { $gte: 200 }
})
 ```

### Queries
Obs: para as Queries, realizamos perguntas, que serão respondidas atrávés de operações de busca  no **MONGODB**

1. Qual a maior receita de cada collection?

    ```javascript
    db.europe.find().sort({ 'Revenue(US$ billions)': -1 }).limit(1)
    ```

    ```javascript
    db.india.find().sort({ 'Revenue(billions US$)': -1 }).limit(1)
    ```
    ```javascript
    db.philippines.find().sort({ 'Revenue(USD millions)': -1 }).limit(1)
    ```
    ```javascript
    db.united_states.find().sort({ 'Revenue (USD millions)': -1 }).limit(1)
    ```

 2. Entre as collections ‘india’ & ‘europe’, qual tem a maior receita e qual seu tipo de industria?
    ```javascript
    db.india.aggregate([
    {
        $project: {
        Industry: 1,
        Revenue: '$Revenue(billions US$)',
        _id: 0,
        source: { $literal: "india" },
        }
    },
    {
        $unionWith: {
        coll: "europe",
        pipeline: [
            {
            $project: {
                Industry: 1,
            Revenue: '$Revenue(US$ billions)',
                _id: 0,
                source: { $literal: "europe" }
            }
            }
        ]
        }
    },
    { $sort: { Revenue: -1 } },
    { $limit: 1 }
    ])
    ```

3. Qual cidade dos EUA fica situado a sede da companhia com maior quantidade de funcionários e maior crescimento de receita?

    ```javascript
    db.united_states.find().sort({ Employees: -1}).limit(1).projection({ Headquarters: 1, _id: 0 })
    ```

4. Na collection ‘philippines’, quais empresas do top 15 ao top 5 do rank, tem uma receita maior que 5.5 milhões de dolares? 


    ```javascript
    db.philippines.find({ Rank: {$gte: 5, $lte: 15}, 'Revenue(USD millions)': {$gte: 5500} })
    ```


