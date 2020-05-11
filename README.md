# redis-replication-kubernetes-microservice
In some use cases, a redis cluster or redis sentinel is considered as over enginering, over resource consumption, 
and a redis replication can perfectly fit.
Espesially in a microservice context, where one should be 100% independent from others, having its life cycle 
and requirments and one of those is redis. 

# Getting started
## Prerequisites
* [helm](https://helm.sh/) Reffer to [official documentation](https://helm.sh/docs/intro/install/) for intructions.

## Deployment
```shell
helm  install ./ -f values.yaml --namespace microservice-redis --name microservice-redis
```
# Anatomy

## Redis-slave 
* Redis slave will be deployed as a side car (Read Only) to your main contianer, and will automaticly connects to redis master.
* Redis slave will not have persistence since it will serve as a replicas to master, if you wish to enable it you can edit [redis-slave-configmap]()
* you can edit Redis slave configuration in [redis-slave-configmap]() or in [values.yaml]()

## Redis-master
* Redis master will be deployed as an independent deployment (Write Only), You should write to it from you main miscroservice container.
* Redis master will be persisted through a pvc. AOF and RDB are enables. More details in [redis-master-deployment.yaml]()
* you can edit Redis master configuration in [redis-master-configmap]() or in [values.yaml]()


## Main Microservice Conatiner
* Microservice Conatiner will have Read only access to Redis slave.
* Microservice Conatiner will have write only access to Redis Master.
* Microservice will have the follwoing Environment variables to facilitate access to Redis

| Env Variable   | Function |
| ------------- | ------------- |
| MASTER_REDIS_DB  | Redis master Data Base  |
| MASTER_REDIS_HOST  | Redis master host  |
| MASTER_REDIS_PORT  | Redis master port  |
| MASTER_REDIS_PASSWORD  | Redis master password  |
| MASTER_REDIS_CONNECTION_NAME  | Redis master connection name  |
| ------------- | ------------- |
| SLAVE_REDIS_HOST  | Redis slave host |
| SLAVE_REDIS_PORT  | Redis slave port  |
| SLAVE_REDIS_DB  | Redis slave Data Base  |
| SLAVE_REDIS_PASSWORD  | Redis slave password  |
| SLAVE_REDIS_CONNECTION_NAME  | Redis slave connection name  |


