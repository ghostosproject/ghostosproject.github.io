#### Getting Started With Ghost Network
The Ghost Network consists of two programs
- Ghost Virtual Machine (GVM)
- Ghost Port Mapper Daemon (GPMD)

You will need to install both of these programs to run The Ghost Network.

Installation is easy. The only prerequisite is to have Go 1.24 installed.

**Setting Up GPMD**
Installation

```
go install github.com/ghostosproject/gpmd@latest
```

Starting GPMD
```
gpmd start
```

Adding WASM Modules
```
gpmd wasm upload --name=math --version=v0.0.1 --file=/Users/ghostosproject/ghost-network/math.wasm
Module Uploaded Successfully!
```

Stopping GPMD
```
gpmd kill
```

**Setting Up GVM**
Installation
```
go install github.com/ghostosproject/gvm@latest
```

Starting GVM
```
gvm
GVM has been created (node-5651) (df0c35f5-bea5-4a60-bbe2-aad3e4954c8c)
Listening for nodes on port 55695
(node-5651) > 
```

Running WASM Modules & Functions
There are two ways to run Modules and Functions in a GVM. Locally and Remote.

Running Modules

<u>Locally</u> - running a module locally is as simple as typing in the name of the module into the console. Running just the module, runs the main function of the module.
```
(node-5651) > math
Reponse from the math module...
```

<u>Remote</u> - running a module on a remote node is a little more complicated. You must first establish a connection to the node before running the Node.run() command.

```
(node-5651) > Node.run(node-8399, math)
Reponse from the math module...
```

Running Functions

<u>Locally</u> - running a function locally is as simple as typing in the name of the module with the function and arguments into the console. Running just the module, runs the main function of the module.
```
(node-5651) > math.add(1,2)
Adding Numbers From Wasm...

3
```

<u>Remote</u> - running a module on a remote node is a little more complicated. You must first establish a connection to the node before running the Node.run() command.

```
(node-5651) > Node.run(node-8399,math.add(34,34))
ID:  9e731370-d42c-4545-8a93-c31a74b7b2e9
Adding Numbers From Wasm...
68
```

Ending GVM
```
(node-5651) > exit
Goodbye :(
```


**Below are the list of built in Node Commands**

Node.discover()
- lists all of the nodes on a network
- Asks gpmd which nodes are on the network and available for connection

```
(node-5651) > Node.discover()
node-5651,node-8399
```

Node.connect({node-name})
- requests a connection to gpmd for {node-name}

```
(node-5651) > Node.connect(node-8399)
node-8399
```

Node.list() 
- lists all the open connections to other nodes
```
(node-5488) > Node.list()
node-8399
```


Node.run({node-name}, {module.function})
- runs a command (wasm module or wasm function) on another machine
- for now all results will be returned to the calling Node
    - eventually, when the different types of VMs are implemented, node.run can be updated to distribute long running processes on other VMs

```
(node-5651) > Node.run(node-8399,math.add(34,34))
ID:  9e731370-d42c-4545-8a93-c31a74b7b2e9
Adding Numbers From Wasm...
68
```
Node.disconnect({node-name})
- disconnects from a connected node gracefully

```
(node-5651) > Node.disconnect(node-8399)
Client disconnected.
```
