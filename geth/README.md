# Ethereum Public Network

[Geth](https://geth.ethereum.org) is official Go implementation of the Ethereum protocol

## Installing the Chart
To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release .
```

## Configuration

The following tables lists the configurable parameters of the geth chart and their default values.

  Parameter                | Description                                                                         | Default
---------------------------|-------------------------------------------------------------------------------------|--------
`image.repository`         | Image source repository name                                                        | `ethereum/client-go`
`image.tag`                | `geth` image tag.                                                                   | `stable`
`image.pullPolicy`         | Image pull policy                                                                   | `IfNotPresent`
`rpcPort`                  | HTTP-RPC server listening port                                                      | `8545`
`wsPort`                   | WS-RPC server listening port                                                        | `8546`
`rpcApi`                   | API's offered over the HTTP-RPC interface                                           | `net,eth,personal,web3`
`wsApi`                    | API's offered over the WS-RPC interface                                             | `net,eth,personal,web3`
`wsOrigins`                | Origins from which to accept websockets requests                                    | `*`
`networkId`                | Network identifier (integer, 1=Frontier, 2=Morden (disused), 3=Ropsten, 4=Rinkeby)  | `1`
`syncMode`                 | Blockchain sync mode ("fast", "full", or "light")                                   | `fast`
`testnet`                  | Ropsten network: pre-configured proof-of-work test network                          | `false`
`rinkeby`                  | Rinkeby network: pre-configured proof-of-authority test network                     | `false`
`customArgs`               | Custom geth arguments with values                                                   | `{}`
`persistence.enabled`      | Create a volume to store data                                                       | `true`
`persistence.accessMode`   | ReadWriteOnce or ReadOnly                                                           | `ReadWriteOnce`
`persistence.size`         | Size of persistent volume claim                                                     | `300Gi`
`resources`                | CPU/Memory resource requests/limits                                                 | `{}`
`serviceExternal.enabled`  | Enable or disable external access to RPC/WS                                         | `true`
`hpa.minReplicas`          | Minimal replicas count of TX nodes                                                  | `1`
`hpa.maxReplicas`          | Maximum replicas count of TX nodes                                                  | `3`
`hpa.targetCPUUtilizationPercentage`| CPU utilization percentage for activate scale                              | `300`


Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml stable/geth
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The geth image stores the geth node data (Blockchain and wallets) and configurations at the `/root` path of the container.

By default a PersistentVolumeClaim is created and mounted into that directory. In order to disable this functionality
you can change the values.yaml to disable persistence and use an emptyDir instead.

> *"An emptyDir volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on that node. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever."*

!!! WARNING !!!

Please NOT use emptyDir for production cluster! Your wallets will be lost on container restart!

