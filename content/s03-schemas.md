# 3. SCHEMAS AND RELATIONS: HOW TO STRUCTURE DOCUMENTS
(_1h53min_)

## 3.1 Reseting your database

> **Important**: We will regularly start with a clean database server (i.e. all data was purged) in this course.
> 
> 
> To get rid of your data, you can simply load the database you want to get rid of (`use databaseName`) and then execute `db.dropDatabase()`.
> 
> Similarly, you could get rid of a single collection in a database via `db.myCollection.drop()`.
>

## 3.2 Module introduction

- Understanding documents schemas & data types
- Modelling relations
- Schema validation

## 3.3 Why do you use schemas?

<img src="..\imgs\s3\s3-1.png" width=600 height=350 >

``` javascript
use shop

db.products.insertOne( { name: "A book", price: 12.99 } )

db.products.insertOne( { title: "T-shirt", seller: {name: "Max", age: 29} } )

```

## 3.4 Structuring documents

<img src="..\imgs\s3\s3-2.png" width=700 height=400 >

``` javascript
db.products.insertOne( { name: "A book", price: 12.99 } )

db.products.insertOne( { name: "A T-shirt", price: 20.99 } )

db.products.insertOne( { name: "A Computer", price: 1299.99, details: {cpu: "Intel i7 8770" } } )

db.products.insertOne( { name: "A Computer", price: 1299.99, details: null } )
```

## 3.5 Data types - An overview

- 16MB de tamanho máximo por documento

<img src="..\imgs\s3\s3-3.png" width=600 height=400 >

## 3.6 Data types in action

``` javascript
db.companies.insertOne( {
	name: "Fresh Apple Inc", 
	isStartup: true, 
	employees: 33, 
	funding: 12345678901234567890, 
	details: { 
		ceo: "Mark Super"}, 
			tags: [
				{title: "super"}, 
				{title: "perfect"}], 
			foundingDate: new Date(), 
			insertedAt: new Timestamp()
			}
)
```
<img src="..\imgs\s3\s3-4.png" width=600 height=400 >

- O valor em "_finding_" não é igual ao valor que foi inserido
- Isso ocorre porque o número do _insert_ era um número muito grande para o campo
- `db.stats():`

<img src="..\imgs\s3\s3-5.png" width=600 height=500 >

- De acordo com a aula, o número deveria ser menor por ser armazenado como um NumberInt32 (o que não ocorreu, talvez pela diferença de versão do Mongosh)

<img src="..\imgs\s3\s3-6.png" width=600 height=500 >

## 3.7 Data types and limits

> MongoDB has a couple of hard limits - most importantly, a single document in a collection (including all embedded documents it might have) must be <= **16mb**. Additionally, you may only have **100 levels of embedded documents**.
> 
> 
> You can find all limits (in great detail) here: https://docs.mongodb.com/manual/reference/limits/
> 
> For the data types, MongoDB supports, you find a **detailed overview** on this page: https://docs.mongodb.com/manual/reference/bson-types/
> 
> **Important data type limits are:**
> 
> - Normal integers (int32) can hold a maximum value of +-2,147,483,647
> - Long integers (int64) can hold a maximum value of +-9,223,372,036,854,775,807
> - Text can be as long as you want - the limit is the 16mb restriction for the overall document
> 
> It's also important to understand the difference between int32 (NumberInt), int64 (NumberLong) and a normal number as you can enter it in the shell. The same goes for a normal double and NumberDecimal.
> 
> **NumberInt** creates a **int32** value => `NumberInt(55)`
> 
> **NumberLong** creates a **int64** value => `NumberLong(7489729384792)`
> 
> If you just use a number (e.g. `insertOne({a: 1}`), this will get added as a **normal double** into the database. 
> 
> The reason for this is that the shell is based on JS which only knows float/ double values and doesn't differ between integers and floats.
> 
> **NumberDecimal** creates a high-precision double value => `NumberDecimal("12.99")` => This can be helpful for cases where you need (many) exact decimal places for calculations.
> 
> When not working with the shell but a MongoDB driver for your app programming language (e.g. PHP, .NET, Node.js, ...), you can use the driver to create these specific numbers.
> 
> Example for Node.js: http://mongodb.github.io/node-mongodb-native/3.1/api/Long.html
> 
> This will allow you to build a NumberLong value like this:
> `const Long = require('mongodb').Long;
> db.collection('wealth').insert( {
>     value: Long.fromString("121949898291")
> });`
> 
> By browsing the API docs for the driver you're using, you'll be able to identify the methods for building int32s, int64s etc.

## 3.8 How to derive your data structure - Requirements

<img src="..\imgs\s3\s3-7.png" width=800 height=500 >

## 3.9 Understanding relations

<img src="..\imgs\s3\s3-8.png" width=800 height=500 >

## 3.10 One to One relations - Embedded

<img src="..\imgs\s3\s3-9.png" width=700 height=450 >

``` javascript
use hospital

db.patients.insertOne( { name: "Max", age: 29, diseaseSummary: "summary-max-1" } )

db.diseaseSummaries.insertOne( { _id: "summary-max-1", diseases: [ "cold", "broken leg" ] } )

db.patients.findOne() // returns only Max

db.patients.findOne().diseaseSummary // retorna somente o o valor do campo "diseaseSummary"

var dsid = db.patients.findOne().diseaseSummary

db.diseaseSummaries.findOne({_id: dsid}) // encontra o primero resultado da coleção "diseaseSummaries" que tem o valor da variável "dsid"

db.patients.deleteMany({}) // deleta toda a coleção

db.patients.insertOne( { name: "Max", age: 29, diseaseSummary: { diseases: ["cold", "broken leg"] } } ) // nested embedded document -> melhor quando se tem uma FORTE relação 1:1
```

## 3.11 One to One - Using References

<img src="..\imgs\s3\s3-10.png" width=700 height=450 >

``` javascript
use carData

db.persons.insertOne({name: "Max", car: {model: "BMW", price: 40000} })

db.persons.deleteMany({})

db.persons.insertOne({name: "Max", age: 29, salary: 3000})

db.persons.insertOne({ model: "BMW", price: 40000, owner: ObjectId('67e5865b41c606bb19b71260') })
```

## 3.12 One to Many - Embedded

- **1:N**

    <img src="..\imgs\s3\s3-11.png" width=700 height=450 >

``` javascript
use support

db.questionThreads.insertOne({
	creator: "Max", 
	question: "How does that all work?", 
	answers: [
		"q1a1",
		"q1a2"
		]
})

db.answers.insertMany( [ 
	{	_id: "q1a1",	text: "It works like that." },
	{ _id: "q1a2", text: "Thanks!" }
] )

db.questionThreads.deleteMany({})

db.questionThreads.insertOne({
	creator: "Max", 
	question: "How does that all work?", 
	answers: [
		{ text: "Like that" },
		{ text: "Thanks" }
	]
})
```

## 3.13 One to Many - Using References

<img src="..\imgs\s3\s3-12.png" width=700 height=450 >

- Spliting in collections make sense:

    ``` javascript
    use cityData

    db.cities.insertOne( {name: "New York", coordinates: {lat: 21, lgn: 55} } )

    db.citizens.insertMany( [
        { 
            name: "Thayná Almeida", 
            cityId: ObjectId('67e58c0941c606bb19b71264')
        },
        {
            name: "Flavio Lopes",
            cityId: ObjectId('67e58c0941c606bb19b71264')
        }
    ] )
    ```

## 3.14 Many to Many - Embedded

<img src="..\imgs\s3\s3-13.png" width=700 height=450 >

``` javascript
use shop

db.products.insertOne( {title: "A Book", price: 12.99} )

db.customers.insertOne( {name: "Max", age: 29} )

db.orders.insertOne( {productId: ObjectId('67e58d2a41c606bb19b71267'), customerId: ObjectId('67e58d2f41c606bb19b71268')} ) // joined table

db.orders.drop() // delete this collection

// referenced:
db.customers.updateOne(
	{}, 
	{ $set: 
		{orders: 
		[ 
		{ productId: ObjectId('67e58d2a41c606bb19b71267'), 
			qty: 2 } 
		]} 
	} )
	
// embedded:
db.customers.updateOne(
	{}, 
	{ $set: 
		{ orders: 
		[ 
		{ title: "A Book", 
			price: 12.99,
			qty: 2 } 
		]} 
	} )
	
	// desvantagens: dados duplicados, se os dados do produto forem alterados, todos os dados de clientes que compraram devem ser alterados tbm
	// obs: o preço não deve mudar dentro de uma ordem, pois o cliente não deve pagar mais caro do que qnd comprou
```

- COISAS A SE CONSIDERAR:
    - Pensar em como serão "buscados" os dados
    - Com qual frequência os dados podem ser atualizados (por exemplo, alteração de valores)
    - E se forem atualizados, é necessário que seja atualizado tbm em todos outros locais onde ele aparece?
    - Duplicados podem ser considerados "OK"?

## 3.15 Many to Many - Using References 

<img src="..\imgs\s3\s3-14.png" width=700 height=450 >

``` javascript
use bookRegistry

db.books.insertOne( { name: "My Favorite Book", authors: [{name: "Marx", age: 29}, {name: "Luca", age: 30} ]})

db.authors.insertMany( [ 
	{ name: "Marx", age: 29, address: { stret: "Main" } },
	{ name: "Marx", age: 29, address: { stret: "Main" } },
] )

db.books.updateOne( {}, { $set: { authors: [ObjectId('67ea8e72e89c115337b71237'), ObjectId('67ea8e72e89c115337b71238')] } } )

db.authors.updateOne( {_id: ObjectId('67ea8e72e89c115337b71238')}, { $set: {name: "Manuel"} } )
```

- COISAS A CONSIDERAR:
    - Usar referencias em alguns casos pode ser ideal
    - Neste exemplo, se a informação dos autores for alterada, teríamos que alterar em cada documento de livro esta informação

## 3.16 Summarizing relations

- Como pensar sobre as relações?
- Quando usar cada approach?
- Sempre depende do que a aplicação precisa. Da frequência com que os dados são alterados. Qual o tamanho do seu banco.

    <img src="..\imgs\s3\s3-15.png" width=700 height=450 >

## 3.17 Using "lookUp()" for merging reference relations 

<img src="..\imgs\s3\s3-16.png" width=500 height=500 >

- Buscar todos os livros da coleção também exibindo os dados dos autores

``` javascript 
db.books.aggregate(
	[
		{ $lookup: { from: "authors", localField: "authors", foreignField: "_id", as: "creators" } } 
	]
)
```

## 3.18 Planning the example exercise

<img src="..\imgs\s3\s3-17.png" width=700 height=500 >
<img src="..\imgs\s3\s3-18.png" width=700 height=500 >

## 3.19 Implementing the example exercise 

``` javascript
use blog

db.users.insertMany(
	[
		{name: "Max", age: 30, email: "max@test.com"},
		{name: "Thay", age: 29, email: "Thay@test.com"}
	]
)

db.posts.insertOne(
	{ title: "My First Post",
		text: "This is my first post, hope you like", 
		tag: ["new", "tech"], 
		creator: ObjectId('67ea96bce89c115337b7123a'), 
		comments: [
			{ text: "I like the post", author: ObjectId('67ea96bce89c115337b71239') }
		]
	}
)
```

## 3.20 Understanding schema validation

<img src="..\imgs\s3\s3-19.png" width=700 height=500 >

<img src="..\imgs\s3\s3-20.png" width=700 height=500 >

## 3.21 Adding collection document validation

``` javascript
db.posts.drop()

db.createCollection('posts', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  }
});
```

``` javascript
db.posts.insertOne(
	{ title: "My First Post",
		text: "This is my first post, hope you like", 
		tag: ["new", "tech"], 
		creator: ObjectId('67ea96bce89c115337b7123a'), 
		comments: [
			{ text: "I like the post", author: ObjectId('67ea96bce89c115337b71239') }
		]
	}
)


// forçando um erro ao colocar a info fora do padrão do Schema
db.posts.insertOne(
	{ title: "My First Post",
		text: "This is my first post, hope you like", 
		tag: ["new", "tech"], 
		creator: ObjectId('67ea96bce89c115337b7123a'), 
		comments: [
			{ text: "I like the post", author: 12 }
		]
	}
)
```

<img src="..\imgs\s3\s3-21.png" width=650 height=800 >

## 3.22 Changing the validation action

``` javascript
db.runCommand({
  collMod: 'posts',
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  },
  validationAction: 'warn'
});
```

- Ao tentar inserir um documento com alguma info fora do padrão do schema (como o abaixo), agora o documento será inserido
- A única diferença é que um “warning” será inserido no arquivo de logs

``` javascript
// forçando um erro ao colocar a info fora do padrão do Schema
db.posts.insertOne(
	{ title: "My First Post",
		text: "This is my first post, hope you like", 
		tag: ["new", "tech"], 
		creator: ObjectId('67ea96bce89c115337b7123a'), 
		comments: [
			{ text: "I like the post", author: 12 }
		]
	}
)
```

## 3.23 Wrap up

<img src="..\imgs\s3\s3-22.png" width=700 height=500 >
<br>
<img src="..\imgs\s3\s3-23.png" width=700 height=500 >

## 3.24 Useful resources and links

> Helpful Articles/ Docs:
> 
> - The MongoDB Limits: https://docs.mongodb.com/manual/reference/limits/
>     - The MongoDB Data Types: https://docs.mongodb.com/manual/reference/bson-types/
>     - More on Schema Validation: https://docs.mongodb.com/manual/core/schema-validation/