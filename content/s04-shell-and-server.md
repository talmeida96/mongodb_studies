# 4. EXPLORING THE SHELL AND THE SERVER

(_20min_)

## 4.1 Module introduction

- Start MongoDB Server as Process & Service
- Configuring database & Log Path (and Mode)
- Fixing issues

## 4.2 Finding available options

- Powershell → `mongosh`
- `mongosh --help`

## 4.3 Setting "dbpath" and "logpath"

``` markdown
mongod --dbpath C:\Program Files\MongoDB\Server\8.0\data

mongod --dbpath C:\Program Files\MongoDB\Server\8.0\log\mongod.log
```

## 4.4 Exploring the MongoDB options

- mongosh [help](https://www.mongodb.com/docs/mongodb-shell/reference/access-mdb-shell-help/)

## 4.5 MongoDB as a background service

- `fork` só funciona no MAC e no Linux
- o `fork` inicia o MongoDB como um "child process" (como um serviço no background)
- `sudo mongod —fork —logpath /Users/thayna/dev/mongodb/logs/log.log`
- para parar este serviço no MAC/Linux é necessário:
    - mudar para o admin db → `use admin`
    - desligar o servidor → `db.shutdownServer()`
---
- para fazer isso no Windows é necessário no momento da instalação, deixar o flag "_install as a service_" ativo
- em um cmd (run as admin) → `net start MongoDB`
- para parar o serviço → `net stop MongoDB`

## 4.6 Using a Config File

- [Configure Shell Settings Global](https://www.mongodb.com/docs/mongodb-shell/reference/configure-shell-settings-global/)
- [Configuration Options](https://www.mongodb.com/docs/manual/reference/configuration-options/)

``` markdown
# in mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: C:\Program Files\MongoDB\Server\8.0\data

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path:  C:\Program Files\MongoDB\Server\8.0\log\mongod.log

# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1

#processManagement:

#security:

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options:

#auditLog:
```

## 4.7 Shell options and help

<img src="..\imgs\s4\s4-1.png" width=700 height=500 >

- `db.help()` → mostra mais opções de comandos

## 4.8 Useful resources and links

> Helpful Articles/ Docs:
> 
> - More Details about Config Files: https://docs.mongodb.com/manual/reference/configuration-options/
> - More Details about the Shell (`mongo`) Options: https://www.mongodb.com/docs/manual/reference/method/
> - More Details about the Server (`mongod`) Options: https://docs.mongodb.com/manual/reference/program/mongod/