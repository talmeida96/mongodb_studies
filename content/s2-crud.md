# 2. UNDERSTANDING THE BASICS & CRUD OPERATIONS
(_1h9min_)

1. [MongoDB CRUD Operations](https://www.mongodb.com/docs/manual/crud/)

## 2.1. Module Introduction

- **CRUD** → Create, Read, Update, Delete

- **CONTEÚDO** DO MÓDULO:
    1. Basics about Collections and Documents
    2. Basic data types
    3. Performing CRUD Operations

## 2.2. Understanding databases, collections & documents

- É possível ter 1 ou mais _databases_ dentro de um servidor de banco de dados
- Cada _database_ pode ter 1 ou mais "`collections`" → equivalente às tabelas em SQL DBs
- Dentro das "`collections`" temos múltiplos "`documents`" → são os dados que são armazenados no banco

    <img src="imgs\s2\s2-1.png" width=600 height=300 >

- Todos esses conteúdos são criados automaticamente/implicitamente quando se começa a criar os dados para armazenar
- É possível criar explicitamente os conteúdos (o que permite configurá-los mais profundamente)

## 2.3. The Shell & MongoDB drivers for different languages

1. [Start Developing with MongoDB](https://www.mongodb.com/docs/drivers/#start-developing-with-mongodb)
2. [`mongosh` Methods](https://www.mongodb.com/docs/manual/reference/method/#mongosh-methods)

- O MongoDB possui drivers para diferentes tipos de linguagens de programação, cada uma com suas adaptações de sintaxe para uso em construção de aplicações.
- O uso do Shell é uma forma de compreender o funcionamento da linguagem do MongoDB em um ambiente genérico, onde depois é possível realizar a adaptação para a linguagem do Driver necessário.
- A documentação do MongoDB para operações CRUD está escrita utilizando da sintaxe de comandos através do Shell.

## 2.4. Creating databases & collections

1. [Databases and Collections in MongoDB](https://www.mongodb.com/docs/manual/core/databases-and-collections/#databases-and-collections-in-mongodb)

- O comando `show dbs` exibe todos os databases existentes dentro do MongoDB Server conectado
- O comando `use <database-name>` altera para um database existente ou cria o database com o nome utilizado

    <img src="imgs\s2\s2-2.png" width=300 height=200 >

- Apesar de navegarmos para o database "**flights**" do exemplo acima utilizando o `use flights`, ao usar o `use dbs` ele não aparece. Isso ocorre porque ele só será definitivamente criado quando iniciarmos a inserção de dados nele.
- `db.collection.insertOne({})` → `db.flightData.insertOne({})`
    - `db` → o database onde se está referenciado → neste caso, o "flights"
    - `flightData` → a "collection" de referência. neste caso, ainda não existe e será criada implicitamente
    - `insertOne` → sintaxe  de criação de dados
    - `{ }` → JSON com os dados que serão criados

## 2.5. Understanding JSON data

```json
[
  {
    "name": "string values",
    "name": 30, // number values don't need quotation marks
    "departureAirport": "MUC",
    "arrivalAirport": "SFO",
    "aircraft": "Airbus A380",
    "distance": 12000,
    "intercontinental": true
  },
  {
    "departureAirport": "LHR",
    "arrivalAirport": "TXL",
    "aircraft": "Airbus A320",
    "distance": 950,
    "intercontinental": false
  }
]
```

<img src="imgs\s2\s2-3.png" width=300 height=200 >

- Todo documento criado recebe um **ID ÚNICO**

## 2.6 Comparing JSON e BSON

- BSON → Binary Json

<img src="imgs\s2\s2-4.png" width=500 height=200 >

- O `_id` pode ser inserido manualmente, desde que se garanta que ele será ÚNICO

<img src="imgs\s2\s2-5.png" width=550 height=300 >

## 2.7 Create, Read, Update, Delete (CRUD) & MongoDB

<img src="imgs\s2\s2-6.png" width=600 height=300 >

<img src="imgs\s2\s2-7.png" width=600 height=250 >

### Exemplo 1:
<img src="imgs\s2\s2-8.png" width=600 height=300 >

## 2.8. Finding, Inserting, Deleting and Updating elements

1. [$set](https://www.mongodb.com/docs/manual/reference/operator/update/set/#-set)

- `db.<collection-name>.deleteOne({})` → `db.flightData.deleteOne({departureAirport: “TXL”})` → deleta a primeira aparição de documento cujo campo "departureAirport" possui o valor "TXL"
- É possível realizar deletes à partir do filtro de qualquer campo existente no documento

    <img src="imgs\s2\s2-9.png" width=600 height=450 >

``` javascript
db.<collection-name>.updateOne({ "filtro-para-documento": "valor do filtro" }, { $set: { "como-será-alterado" } })

db.flightData.updateOne({ distance: 12000 }, { `$set`: { marker: “delete” } })
```
- `$set` → reservado pelo MongoDB para indicar operadores que descrevem a alteração que se deseja realizar

    <img src="imgs\s2\s2-10.png" width=600 height=450 >

``` javascript
db.<collection-name>.updateMany({ "filtro-para-documento": "valor do filtro" }, { $set: { "como-será-alterado" } })

db.flightData.updateMany({}, { $set: { marker: "toDelete" } })
``` 

- É possível deixar o campo de `"filtro-para-documento"` vazio, e desta forma as alterações abrangeram todos os documentos desta coleção. O mesmo vale para o `deleteMany()`

    <img src="imgs\s2\s2-11.png" width=600 height=450 >

``` javascript
db.<collection-name>.deleteMany( { "filtro-para-documento": "valor do filtro" })

db.flightData.deleteMany( {marker: "toDelete"} )
``` 

<img src="imgs\s2\s2-12.png" width=300 height=120 >

## 2.9. Understanding "insertMany()"

1. [Insert Documents](https://www.mongodb.com/docs/mongodb-shell/crud/insert/#insert-documents)

``` javascript
[
  {
    "name": "string values",
    "name": 30, // number values don't need quotation marks
    "departureAirport": "MUC",
    "arrivalAirport": "SFO",
    "aircraft": "Airbus A380",
    "distance": 12000,
    "intercontinental": true
  },
  {
    "departureAirport": "LHR",
    "arrivalAirport": "TXL",
    "aircraft": "Airbus A320",
    "distance": 950,
    "intercontinental": false
  }
]
```

<img src="imgs\s2\s2-13.png" width=450 height=300 >

## 2.10. Diving deeper into finding data

1. [Comparison Query Operators](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/)
2. [Comparison/Sort Order](https://www.mongodb.com/docs/manual/reference/bson-type-comparison-order/#comparison-sort-order)

## 2.11. "update()" vs. "updateMany()"

1. [Update Documents](https://www.mongodb.com/docs/mongodb-shell/crud/update/#update-documents)

- desta forma, o documento **TODO** será alterado, mantendo somente o `ObjectId`:

    ``` javascript
    db.flightData.update({})

    db.flightData.update({_id: ObjectId('67e2ebea41c606bb19b71239')}, {delayed: true})
    ```

> - ESTE MÉTODO FOI **REMOVIDO** NO _mongosh_
>
>   - Compatibility Changes: ****https://www.mongodb.com/docs/mongodb-shell/reference/compatibility/#deprecated-methods
>   - https://www.mongodb.com/docs/manual/reference/method/db.collection.update/#db.collection.update--

- `db.<collection-name>.replaceOne({})` → substitui um documento por OUTRO documento

``` javascript
db.flightData.replaceOne({
    "departureAirport": "MUC",
    "arrivalAirport": "SFO",
    "aircraft": "Airbus A380",
    "distance": 12000,
    "intercontinental": true
  })
```
<img src="imgs\s2\s2-14.png" width=550 height=600 >

## 2.12. Understanding "find()" and the cursor object

1. [find](https://www.mongodb.com/docs/manual/reference/command/find/#find)
2. [cursor.toArray()](https://www.mongodb.com/docs/manual/reference/method/cursor.toArray/#cursor.toarray--)
3. [cursor.forEach()](https://www.mongodb.com/docs/manual/reference/method/cursor.forEach/#cursor.foreach--)

### Inserção de 20 documentos em uma nova coleção (tabela) chamada "passengers"

``` javascript
db.passengers.insertMany(  [
    {
      "name": "Max Schwarzmueller",
      "age": 29
    },
    {
      "name": "Manu Lorenz",
      "age": 30
    },
    {
      "name": "Chris Hayton",
      "age": 35
    },
    {
      "name": "Sandeep Kumar",
      "age": 28
    },
    {
      "name": "Maria Jones",
      "age": 30
    },
    {
      "name": "Alexandra Maier",
      "age": 27
    },
    {
      "name": "Dr. Phil Evans",
      "age": 47
    },
    {
      "name": "Sandra Brugge",
      "age": 33
    },
    {
      "name": "Elisabeth Mayr",
      "age": 29
    },
    {
      "name": "Frank Cube",
      "age": 41
    },
    {
      "name": "Karandeep Alun",
      "age": 48
    },
    {
      "name": "Michaela Drayer",
      "age": 39
    },
    {
      "name": "Bernd Hoftstadt",
      "age": 22
    },
    {
      "name": "Scott Tolib",
      "age": 44
    },
    {
      "name": "Freddy Melver",
      "age": 41
    },
    {
      "name": "Alexis Bohed",
      "age": 35
    },
    {
      "name": "Melanie Palace",
      "age": 27
    },
    {
      "name": "Armin Glutch",
      "age": 35
    },
    {
      "name": "Klaus Arber",
      "age": 53
    },
    {
      "name": "Albert Twostone",
      "age": 68
    },
    {
      "name": "Gordon Black",
      "age": 38
    }
  ])
```

- O `.find()` não nos retorna todos os documentos de uma coleção, mas sim um "_cursor object_"

    <img src="imgs\s2\s2-15.png" width=400 height=200 >

---

- [**`cursor.toArray()`**](https://www.mongodb.com/docs/manual/reference/method/cursor.toArray/#cursor.toarray--)

> The `toArray()` method returns an array that contains all the documents from a cursor.
> 
> The method iterates completely the cursor, loading all the documents into RAM and exhausting the cursor.

- [**`cursor.forEach()`**](https://www.mongodb.com/docs/manual/reference/method/cursor.forEach/#cursor.foreach--)

> Iterates the cursor to apply a JavaScript `function` to each document from the cursor.

## 2.13. Understanding Projection

1. [Projection](https://www.mongodb.com/docs/ruby-driver/current/reference/projection/#projection)

> By default, queries in MongoDB return all fields in matching documents. To limit the amount of data that MongoDB sends to applications, you can include a [projection](https://www.mongodb.com/docs/manual/tutorial/project-fields-from-query-results/) document in the query operation.
> 
> 
> The projection document limits the fields to return for all matching documents. The projection document can specify the inclusion of fields or the exclusion of field and has the following form:
> 
> ```jsx
> {'projection': {field1: <value>,field2: <value> ... } }
> ```
> 
> `<value>` may be `0` (or `false`) to exclude the field, or `1` (or `true`) to include it. With the exception of the `_id` field, you may not have both inclusions and exclusions in the same projection document.

<img src="imgs\s2\s2-16.png" width=600 height=400 >

- `db.passengers.find({}, {name: 1})`
    - o primeiro campo fica vazio pq se quer todos os resultados, sem filtros. 
    - o segundo campo é quais informações do documento se quer ver.

<img src="imgs\s2\s2-17.png" width=550 height=600 >

<img src="imgs\s2\s2-18.png" width=500 height=400 >

## 2.14. Embedded documents and arrays - The Theory

- Embedded documents:

<img src="imgs\s2\s2-19.png" width=500 height=300 >

- Arrays:

<img src="imgs\s2\s2-20.png" width=500 height=300 >

## 2.15 Working with Embedded documents

``` javascript
db.flightData.updateMany({}, {$set: { status: {desc: "on-time", lastUpdt: "1h ago"} }})
```

<img src="imgs\s2\s2-21.png" width=600 height=400 >

``` javascript
db.passengers.updateOne({name: "Albert Twostone"}, {$set: {hobbies: ["sports", "cooking"]}})
```

## 2.16 Accessing Structured data

``` javascript
db.passengers.findOne({name: "Albert Twostone"}).hobbies

db.passengers.find({hobbies: "sports"})

db.flightData.find({"status.desc": "on-time"})
```

## 2.17 Assignment 1: Time to Practice - The basics and CRUD operations

1. Insert 3 patient records with at least 1 history entry per patient
2. Update patient data of 1 patient with new age, name and history entry
3. Find all patients who are older than 30 (or a value of your choise)
4. Delete all patients who got a cold as a disease

### 1. Insert 3 patient records with at least 1 history entry per patient

``` javascript
use hospital

db.patients.insertMany(
[
  {
    "firstName": "Eric",
    "lastName": "Almeida",
    "age": 48,
    "history": [
	    { "disease": "cold", "treatment": "pills" },
	    { "disease": "stomach ache", "treatment": "pills" }
	    ]
  },
  {
    "firstName": "Flávio",
    "lastName": "Lopes",
    "age": 38,
    "history": [
	    { "disease": "cold", "treatment": "pills", "symptom": "fever" },
	    { "disease": "conjunctivitis", "treatment": "eye drops" }
	    ]
  },
  {
    "firstName": "Thayná",
    "lastName": "Almeida",
    "age": 29,
    "history": [
	    { "disease": "dehydration", "treatment": "hydration" },
	    { "disease": "conjunctivitis", "treatment": "eye drops" }
	    ]
  }
])
```

### 2. Update patient data of 1 patient with new age, name and history entry

``` javascript
db.patients.updateOne( {firstName: "Eric"}, { $set: {age: 50 , firstName: "Erick"} } )

db.patients.updateOne( {firstName: "Erick"}, { $set: {"history": [ { "disease": "cold and fever", "treatment": "pills" } ] } })
```

### 3. Find all patients who are older than 30 (or a value of your choise)

1. [Comparison Query Operators](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/#comparison-query-operators)

    > Comparison operators return data based on value comparisons.
    > 
    > 
    > 
    > | **Name** | **Description** |
    > | --- | --- |
    > | [`$eq`](https://www.mongodb.com/docs/manual/reference/operator/query/eq/#mongodb-query-op.-eq) | Matches values that are equal to a specified value. |
    > | [`$gt`](https://www.mongodb.com/docs/manual/reference/operator/query/gt/#mongodb-query-op.-gt) | Matches values that are greater than a specified value. |
    > | [`$gte`](https://www.mongodb.com/docs/manual/reference/operator/query/gte/#mongodb-query-op.-gte) | Matches values that are greater than or equal to a specified value. |
    > | [`$in`](https://www.mongodb.com/docs/manual/reference/operator/query/in/#mongodb-query-op.-in) | Matches any of the values specified in an array. |
    > | [`$lt`](https://www.mongodb.com/docs/manual/reference/operator/query/lt/#mongodb-query-op.-lt) | Matches values that are less than a specified value. |
    > | [`$lte`](https://www.mongodb.com/docs/manual/reference/operator/query/lte/#mongodb-query-op.-lte) | Matches values that are less than or equal to a specified value. |
    > | [`$ne`](https://www.mongodb.com/docs/manual/reference/operator/query/ne/#mongodb-query-op.-ne) | Matches all values that are not equal to a specified value. |
    > | [`$nin`](https://www.mongodb.com/docs/manual/reference/operator/query/nin/#mongodb-query-op.-nin) | Matches none of the values specified in an array. |

``` javascript
db.patients.find({ age: { $gt: 30 } } )
```

### 4. Delete all patients who got a cold as a disease

1. [`$elemMatch` (projection)](https://www.mongodb.com/docs/manual/reference/operator/projection/elemMatch/)

``` javascript
db.patients.deleteMany( { history: { $elemMatch: { disease: "cold" } } } )

db.patients.deleteMany( { "history.disease": "cold and fever" } ) // another way
```

## 2.18 Wrap up

<img src="imgs\s2\s2-22.png" width=600 height=300 >

## 2.19 Useful resources and links

> Useful Articles/ Docs:
> 
> - Learn more about the MongoDB Drivers: https://docs.mongodb.com/ecosystem/drivers/
> - Dive into the official Getting Started Docs: https://docs.mongodb.com/manual/tutorial/getting-started/