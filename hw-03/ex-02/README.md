# Questions

## 1. [Ingress Controller / Secrets] Crea los siguientes objetos de forma declarativa.

```console
kubectl create -f svc.yaml
kubectl create -f statefulset.yaml
```

## 2. Entrar en el primer pod y mostrar su hostname.
```console
kubectl exec -it mongod-0 bash 

hostname -f
> mongod-0.mongodb-service.default.svc.cluster.local
```

## 3. Entrar en mongo y crear un ReplicaSet.
```console
mongo
```

```console
> rs.initiate({ _id: "MainRepSet", version: 1, 
members: [ 
 { _id: 0, host: "mongod-0.mongodb-service.default.svc.cluster.local:27017" }, 
 { _id: 1, host: "mongod-1.mongodb-service.default.svc.cluster.local:27017" }, 
 { _id: 2, host: "mongod-2.mongodb-service.default.svc.cluster.local:27017" } 
]});

> rs.status();
```

## 4. Crear el usuario Administrador de mongo.

```console
> db.getSiblingDB("admin").createUser({
    user : "sergi",
    pwd  : "sergi12345",
    roles: [ { role: "root", db: "admin" } ]
});
```

## 5. Realizar una modificación dentro del primer pod y verificar el cambio en el resto.

mongod-0
```console
> use db-hw-03
> db.person.insert({"name":"Sergi"})
```
* (NOTA: Mirar imagen "ex2.1.png")

mongod-1
```console
kubectl exec -it mongod-1 bash

mongo

> rs.secondaryOk()
> use db-hw-03
> db.person.find()
```
* (NOTA: Mirar imagen "ex2.2.png")


## Diferencias que existiría si el montaje se hubiera realizado con el objeto de ReplicaSet

*  Con StatefulSet permite tener estado (ideal para bases de datos), es decir, persistencia. Con ReplicaSet no.

*  Con StatefulSet los nombres de los pods tienen un valor númerico en orden, cada vez que se incrementa el número de pods se aumenta en 1 el valor númerico del nombre (mongod-0 > mongod-1) y cuando se decrementa, se eliminan por orden de Mayor a Menor, esto permite una estrategia de Master-Slave, además de que permite identificar y mapear los pods debido a que siempre tendrán el mismo patron como nombre, a diferencia del objeto ReplicaSet que añade un sufijo de manera random.