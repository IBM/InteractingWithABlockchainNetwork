# Interacting with a Blockchain Network

## Instructions for setting the blockchainNetwork

Welcome to Part 2 of building a Blockchain Application.  Now that you have created a blockchain network, you can learn how to interact with it to add participants, perform transactions and query the network.

## Included Components
* Hyperledger Fabric
* Docker
* Hyperledger Fabric SDK for node.js


## Application Workflow Diagram
![Application Workflow](images/Pattern1-Build-a-network.png)

## Prerequisites
* [Docker](https://www.docker.com/products/overview) - v1.13 or higher
* [Docker Compose](https://docs.docker.com/compose/overview/) - v1.8 or higher

## Steps
1. [Run Build.sh Script to build network](#1-run-the-build.sh-script)
2. [Check the logs](#2-check-the-logs)
3. [Test commands on the network](#3-Test-commands-on-the-network)


## 1. Run the Build.sh Script

### Open a new terminal and run the following command:

This step will do the following:

* Remove docker images and containers

* Remove old certificates

* Create and instantiate certificates on the peers in the network

* Start the blockchain network

**Note: the `build.sh` command will run a long time; perhaps 3-4 mins

```bash
export FABRIC_CFG_PATH=$(pwd)
chmod +x cryptogen
chmod +x configtxgen
chmod +x ./rabbitCluster/cluster-entrypoint.sh
chmod +x generate-certs.sh
chmod +x generate-cfgtx.sh
chmod +x docker-images.sh
chmod +x build.sh
chmod +x clean.sh
./build.sh
```


## 2. Check the logs

This step will check the log results from runnng the `./build.sh` command.

**Command**
```bash
docker logs blockchain-setup
```
**Output:**
```bash
CA registration complete 
CA registration complete 
Default channel not found, attempting creation...
Successfully created a new default channel.
Joining peers to the default channel.
Chaincode is not installed, attempting installation...
Base container image present.
info: [packager/Golang.js]: packaging GOLANG from bcfit
info: [packager/Golang.js]: packaging GOLANG from bcfit
Successfully installed chaincode on the default channel.
Successfully instantiated chaincode on all peers.
Blockchain newtork setup complete.
```

**Command**
```bash
docker ps
```
**Output:**
```bash
80de48d4f372        dev-fitcoin-peer-bcfit-1-7df93f2b75a05e9e7896ce92dcb057539e271722e43eba5ff9c75aae902fdcce   "chaincode -peer.add…"   3 hours ago         Up 3 hours                                                                dev-fitcoin-peer-bcfit-1
0bb401fe44b9        backend                                                                                     "node index.js"          3 hours ago         Up 3 hours                                                                fitcoin_fitcoin-backend_1
f873f201e99c        backend                                                                                     "node index.js"          3 hours ago         Up 3 hours                                                                fitcoin_shop-backend_1
c7fdf4341ee9        rabbit-client                                                                               "node index.js"          3 hours ago         Up 3 hours          0.0.0.0:3000->3000/tcp                                rabbit-client
499cf8d837ea        redis-server                                                                                "/docker-entrypoint.…"   3 hours ago         Up 3 hours          6379/tcp, 0.0.0.0:7000-7007->7000-7007/tcp            fitcoin_redis-server_1
febba02cc941        dev-shop-peer-bcfit-1-0e0d4e71de9ac7df4d0d20dfcf583e3e63227edda600fe338485053387e09c50      "chaincode -peer.add…"   3 hours ago         Up 3 hours                                                                dev-shop-peer-bcfit-1
03141f47f646        haproxy:1.7                                                                                 "/docker-entrypoint.…"   3 hours ago         Up 3 hours          0.0.0.0:5672->5672/tcp, 0.0.0.0:15672->15672/tcp      rabbitmq
5ada66d99be3        rabbitmq:3-management                                                                       "/usr/local/bin/clus…"   3 hours ago         Up 3 hours          4369/tcp, 5671-5672/tcp, 15671-15672/tcp, 25672/tcp   rabbitmq3
f51d8f5d7fc7        rabbitmq:3-management                                                                       "/usr/local/bin/clus…"   3 hours ago         Up 3 hours          4369/tcp, 5671-5672/tcp, 15671-15672/tcp, 25672/tcp   rabbitmq2
172cabad39b4        rabbitmq:3-management                                                                       "docker-entrypoint.s…"   3 hours ago         Up 3 hours          4369/tcp, 5671-5672/tcp, 15671-15672/tcp, 25672/tcp   rabbitmq1
5ffcf480fbb6        blockchain-setup                                                                            "node index.js"          3 hours ago         Up 3 hours          3000/tcp                                              blockchain-setup
f58e488b23da        fitcoin-peer                                                                                "peer node start"        3 hours ago         Up 3 hours          0.0.0.0:8051->7051/tcp, 0.0.0.0:8053->7053/tcp        fitcoin-peer
8e4facc6fd4a        shop-peer                                                                                   "peer node start"        3 hours ago         Up 3 hours          0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp        shop-peer
4297f9e1f45c        hyperledger/fabric-couchdb:x86_64-1.0.2                                                     "tini -- /docker-ent…"   3 hours ago         Up 3 hours          4369/tcp, 9100/tcp, 0.0.0.0:6984->5984/tcp            couchdb1
e0fdc5312585        orderer-peer                                                                                "orderer"                3 hours ago         Up 3 hours          0.0.0.0:7050->7050/tcp                                orderer0
56c209a37bed        shop-ca                                                                                     "fabric-ca-server st…"   3 hours ago         Up 3 hours          0.0.0.0:7054->7054/tcp                                shop-ca
1db6a6ca3a6f        fitcoin-ca                                                                                  "fabric-ca-server st…"   3 hours ago         Up 3 hours          0.0.0.0:8054->7054/tcp                                fitcoin-ca
c90b289a10e6        hyperledger/fabric-couchdb:x86_64-1.0.2                                                     "tini -- /docker-ent…"   3 hours ago         Up 3 hours          4369/tcp, 9100/tcp, 0.0.0.0:5984->5984/tcp            couchdb0
```

**Command**
```bash
docker logs fitcoin_fitcoin-backend_1
```
**Output:**
```
CA registration complete 
CA registration complete 
CA registration complete 
[x] Awaiting RPC requests on clientClient2
[x] Awaiting RPC requests on clientClient0
[x] Awaiting RPC requests on clientClient1
```

**Command**
```bash
docker logs fitcoin_shop-backend_1
```
**Output:**
```
CA registration complete 
CA registration complete 
Starting socker server
[x] Awaiting RPC requests on clientClient0
```

## 3.  Test commands on the network


**To view the Blockchain Events**

In a separate terminal navigate to testApplication folder and run the following command:
```
cd testApplication
npm install
node index.js
```
Navigate to url to view the blockchain blocks: **http://localhost:8000/history.html**
Now navigate to url to perform operations on network : **http://localhost:8000/test.html**

**Sample  values for request**



**Enroll Operation**
```
type = enroll
userId = <leave blank>
fcn = <leave blank>
args = <leave blank>
```

From this you will see a return message for a User ID:  (`e0165a07-9358-470e-b29d-9412b7967000` is the id that is dynamically created)

```
{"message":"success","result":{"user":"e0165a07-9358-470e-b29d-9412b7967000","txId":"dfc8b4849a2fe4352ff1213c7445fbe2ecdb649f444580c6d010a1fca3fb990d"}}
```


**Invoke Operation** (This will create a user with 500 fitcoins)
```
type = invoke
userId = e0165a07-9358-470e-b29d-9412b7967000
fcn = createMember
args = <userID>,<Number as String> i.e. e0165a07-9358-470e-b29d-9412b7967000,500 
```

From this you will see a return message: (500 fitcoins were created)

```
{"message":"success","result":{"txId":"82700302bd916df4aecc9685150d0e5e9ba8a385407fbd1d80b7f03c5c474255","results":{"status":200,"message":"","payload":"{\"id\":\"e0165a07-9358-470e-b29d-9412b7967000\",\"memberType\":\"user\",\"fitcoinsBalance\":5,\"totalSteps\":500,\"stepsUsedForConversion\":500,\"contractIds\":null,\"generatedFitcoins\":5}"}}}
```

**Invoke Operation** (Alternative way to generate 500 fitcoins)
```
type = invoke
userId = e0165a07-9358-470e-b29d-9412b7967000
fcn = generateFitcoins
args = <userID>,<Number as String> i.e. e0165a07-9358-470e-b29d-9412b7967000,500
```


**Query Operation**
```
type = query
userId = <userID> i.e. e0165a07-9358-470e-b29d-9412b7967000
fcn = getState
args = <userID> i.e. e0165a07-9358-470e-b29d-9412b7967000
```

From this you will see a return message: (It shows that this userid has 500 fitcoins)
```
{"message":"success","result":"{\"contractIds\":null,\"fitcoinsBalance\":5,\"id\":\"e0165a07-9358-470e-b29d-9412b7967000\",\"memberType\":\"user\",\"stepsUsedForConversion\":500,\"totalSteps\":500}"}
```


## Additional Resources
* [Hyperledger Fabric Docs](http://hyperledger-fabric.readthedocs.io/en/latest/)
* [Hyperledger Composer Docs](https://hyperledger.github.io/composer/introduction/introduction.html)

## License
[Apache 2.0](LICENSE)