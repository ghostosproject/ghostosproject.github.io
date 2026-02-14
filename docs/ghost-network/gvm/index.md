### Ghost Virtual Machine (GVM)

The Ghost Virtual Machine is at the center of The Ghost Network. At it's core, it is a simple WebAssembly Virutal Machine. In reality, it is an incredible machine that, when used to it's fullest, has amazing potential. 

GVM is still in its infancy, but when combined with GPMD and other GVM Nodes, it creates a network of very useful primatives for building useful software.

The source code can be found at [https://github.com/ghostosproject/gvm](https://github.com/ghostosproject/gvm)

##### How Does GVM Work?
GVM can be started simply by running the "gvm" command in your terminal. GVM is created with a unique name and id for identification within the network.

```
$: gvm
GVM has been created node-5488 (217acd5a-63b4-44d4-9245-445509f7df02)
(node-5488) >
```

When starting a GVM it automatically tries to create a connection with GPMD on the local device. If GPMD is not running on the local device, GVM fails with a GPMD_NOT_RUNNING error.

The reason that GVM does not run without GPMD is because of the nature of The Ghost Network including the architecture of how resources are managed works within the network.

More information about how connections work from the GPMD side can be found [here](../gpmd/index.md#how-does-gpmd-work-with-gvm).

***Connecting to another node***
Connecting Nodes is one of the main advantages to using GVMs inside The Ghost Network. This allows for spreading compute across many GVMs and devices out of the box. 

The Node.discover() command, sends a request to GPMD asking for all of the discoverable nodes on the network. A list of node names is returned including the current node in use.

```
(node-5651) > Node.discover()
node-5651,node-8399
```

The Node.connect({node-name}) is used to create a connection with another node in the network.

First the node sends a request to GPMD asking for information about the node it wants to connect to. 
If the node exists, GPMD will respond back to the requesting node with the address, port, id, and name of the requested node.
The requesting GVM will then reach out to the GVM to create a direct connection. The requested GVM will respond either confirming or denying the connection.
Once a connection is established, both nodes have the ability to run modules and functions on eachother.

```
(node-5651) > Node.connect(node-8399)
node-8399
```

The Node.list() command show all connected nodes on that specific GVM.
```
(node-5488) > Node.list()
node-8399
```

The Node.run({node-name}, {module.function}) command allows for running Modules & Functions on another GVM. More details can be found below in the Running WASM Modules section.

The Node.disconnect({node-name}) command will disconnect from the requested node.

***Running WASM Modules***
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