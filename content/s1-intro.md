# 1. INTRODUCTION
(_1h6min_)

1. [Introduction to MongoDB](https://www.mongodb.com/docs/manual/introduction/#introduction-to-mongodb)
2. [MongoDB Community Server Download](https://www.mongodb.com/try/download/community)
3. [MongoDB Command Line Database Tools Download](https://www.mongodb.com/try/download/database-tools)
4. [MongoDB Shell Download](https://www.mongodb.com/try/download/shell)
5. [Welcome to MongoDB Shell (mongosh)](https://www.mongodb.com/docs/mongodb-shell/#welcome-to-mongodb-shell--mongosh-)


## 1.1. Module Introduction

### Ecossistema do MongoDB

<img src="..\imgs\s1\s1-1.png" width=600 height=300 >
<img src="..\imgs\s1\s1-2.png" width=600 height=300 >


## 1.2. What is MongoDB?
O nome MongoDB tem origem na palavra "homongous" ("enorme", em inglês), pois foi criado para armazenagem de grandes quantidades de dados.

### 1.2.1. How it works?

- O armazenamento das mensagens nos documentos são no formato JSON (BSON).
- BSON é uma versão binária das informações (o mongoDB converte as informações JSON para BSON de forma transparente ao usuário).

<img src="..\imgs\s1\s1-3.png" width=600 height=300 >
<img src="..\imgs\s1\s1-4.png" width=600 height=300 >


## 1.3. The key MongoDB characteristics (and how it differs from SQL databases)

Ao invés de seguir a linha de raciocínio de databases SQL onde todas as informações devem ser normalizadas e distribuídas em tabelas com esquemas e estruturas bem definidos, o NoSQL simplesmente armazena todas as informações em um único **documento**, sem "_schemas_".

Isso significa que um **documento** pode possuir uma quantidade de informações bastante diferente de outros.

<img src="..\imgs\s1\s1-5.png" width=600 height=200 >

### 1.3.1. Relations

Podem não haver relações, ou pode-se ter poucas relações…

<img src="..\imgs\s1\s1-6.png" width=500 height=150 >

- Você tem menos tabelas, menos coisas para conectar, e ao invés disto você armazena os dados já juntos, e é isso que traz **eficiência** para algumas aplicações.
- Quando o MongoDB está buscando os dados, ele não precisa ir até a "_collection_ 1", mergear com a "_collection_ 2" e com a "_collection_ 3". Ao invés disto, ele navega até a "_collection_ A" e de forma rápida e eficaz, encontra o documento necessário e já tem todas as informações necessárias ali, sem precisar realizar junções de tabelas.
- Isso traz velocidade, desempenho e flexibilidade!
- O que torna o MongoDB popular para aplicações com ações de "leitura e escrita" pesadas.

## 1.4. Understanding the MongoDB ecosystem

- Compass → GUI
- BI Connectors & MongoDB Charts → Tools for Data Scientists

<img src="..\imgs\s1\s1-7.png" width=600 height=300 >

## 1.5. General setup instructions & Windows Installation

### 1.5.1. Follow below steps:

- [Site MongoDB](mongodb.com)

1. Navegar para a aba Products → Try Community Edition (Download) → Selecionar a versão corrente do Windows com pacote '_msi_'

    <img src="..\imgs\s1\s1-8.png" width=350 height=200 >

2. Após download, procurar o arquivo na máquina local e iniciar a instalação
3. Aceitar os "Termos de Instalação"
4. Escolher a opção **Custom Setup**

    <img src="..\imgs\s1\s1-9.png" width=400 height=300 >

5. "**Install MongoD as a Service**" faz com que a aplicação rode em segundo plano toda vez que a máquina é inicializada. É possível pausar, parar e reiniciar manualmente, mas esta opção automaticamente inicia o serviço ao início da máquina de instalação.
6. **Data Directory**: é onde os arquivos de dados serão armazenados.
7. **Log Directory**: é onde os metadados de logs serão armazenados.

    <img src="..\imgs\s1\s1-10.png" width=400 height=300 >

8. Manter o _flag_ "**Install MongoDB Compass**"
9. Após a instalação, ao navegar para "Services", é possível ver o programa MongoDB em execução por padrão.
    - Se o serviço estiver "pausado" ou "parado", não é possível interagir ou enviar comandos para o MongoDB.

    <img src="..\imgs\s1\s1-11.png" width=1500 height=200 >

- Uma forma alternativa de "iniciar" e "parar" o serviço é através do _prompt_ de comando:

    ```
    net start MongoDB
    net stop MongoDB
     ```

- Na pasta "_bin_" deve haver o executável "**mongo.exe**" que é utilizado para iniciar o "*client*" (o shell interativo que nos permite interagir com o servidor)

    <img src="..\imgs\s1\s1-13.png" width=500 height=200 >

### 1.5.2. Error Diagnosis

> ❌ O executável "mongo.exe" não está mais incluso à partir da versão "MongoDB 6.0".
>
> As etapas descritas abaixo não fazem parte do curso da UDEMY


- o "mongo.exe" não está incluído na pasta _bin_ das versões acima da 6.0 do MongoDB:
    - Solução no fórum do provedor: [Mongo .exe file is missing in bin folder](https://www.mongodb.com/community/forums/t/mongo-exe-file-is-missing-in-bin-folder/180140/4)
    - Link direto para o download da ferramenta `mongosh`: [Download `mongosh`](https://www.mongodb.com/try/download/shell)
    - Instruções para conexão local: [Connect to a local deployment on the default port](https://www.mongodb.com/docs/mongodb-shell/connect/#connect-to-a-local-deployment-on-the-default-port)
    
    ---
    
    - O _shell_ aberto é onde é possível interagir com o _database_, fazendo inserções, buscas, etc.
    - o comando `mongosh` executa uma conexão local.
    - o comando `show dbs` exibe os databases _default_.

    <img src="..\imgs\s1\s1-14.png" width=600 height=350 >

## 1.6 Installing MongoDB Shell

- [Link para instalação no Windows (_msi_)](https://www.mongodb.com/try/download/shell)

> 💡Os comandos usados no _shell_ antigo “mongo.exe” e no novo “mongosh.exe” são equivalentes.
> - O novo _shell_ apresenta as informações de uma forma mais conveniente e mais simples de se entender.

## 1.7. Installing _mongoimport_

> In this course, we'll also use another tool called `mongoimport`. 
It's a tool / local command which we will occasionally use in this course to import some prepared data into a MongoDB database.
> 
> 
> Since it wasn't installed together with the MongoDB server + client, you need to install it as a separate tool, following the installation instructions you find [HERE](https://docs.mongodb.com/database-tools/installation/installation/)
> 
> (these "Database tools" contain the `mongoimport` tool / command).
> 
> - [Windows Installation](https://docs.mongodb.com/database-tools/installation/installation-windows/#installation)
> - [macOS Installation](https://docs.mongodb.com/database-tools/installation/installation-macos/#installation)
> - [Linux Installation](https://docs.mongodb.com/database-tools/installation/installation-linux/#installation)

## 1.8. Let's get started!

- Para navegar para a instância de um _database_, use o comando `use <database-name>`, por exemplo: `use shop`
- Se o database ainda não existir, ele será criado assim que os dados começarem a ser inseridos
- Para criar uma nova coleção em um _database_, use o comando `db.<collection>.insertOne({})` → `db.products.insertOne( {name: “A Book”, price: 12.99} )`
- O _prompt_ retornará uma mensagem, seja para informar um erro ou o sucesso da criação (com o ID único):

    <img src="..\imgs\s1\s1-15.png" width=600 height=400 >

- Retorna todos os arquivos presentes na coleção → `db.products.find()`

## 1.9 Shell vs. Drivers

- [MongoDB Drivers](https://www.mongodb.com/docs/drivers/)
- [Python (pymongo)](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/crud/insert/)

## 1.10 MongoDB + Clients: The big picture

<img src="..\imgs\s1\s1-16.png" width=600 height=300 >

<img src="..\imgs\s1\s1-17.png" width=600 height=300 >

## 1.11 Course outline

<img src="..\imgs\s1\s1-18.png" width=600 height=300 >

## 1.12 How to get most of the course

<img src="..\imgs\s1\s1-19.png" width=600 height=300 >