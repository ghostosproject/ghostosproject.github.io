### Ghost Port Mapper Daemon (GPMD)

The Ghost Port Mapper Daemon is a central piece of the Ghost Network. GPMD is responsible for the orchestration of connections between nodes in a network as well as the distribution of resources such as WASM Modules. 
In this document, we will discuss how GPMD works, how it works in conjunction with GVM, and how to use it.

##### How Does GPMD Work?
GPMD starts by running the "gpmd start" command.
```
$: gpmd start
GPMD is running in the background.
```
GPMD opens two ports locally port 1111 & 5989

**Port 5989** is the local port used by all GVM nodes within a device for communication. Through port 5989, GVMs can
- register with GPMD
- request connections with other nodes
- request resources like Wasm Modules

All communication between GVM nodes and GPMD uses the Ghost Message Protocol (GMP)

**Port 1111** is the port used to run gpmd commands. The reason we used a port for communication is two fold.
1. Having gpmd run in the background, we need to run commands to communicate with gpmd.
2. Since our goal is to create a distributed system infrastructure, having a port able to accept commands allows for commands to be executed from outside the targeted device. 

Current gpmd commands:
- ***gpmd start [-nd non-detached]*** - starts gpmd (default runs detached)
    - -nd: runs gpmd in non detached mode
- ***gpmd kill*** - stops gpmd
- ***gpmd wasm upload --name=node --version=v0.0.1 --file=/Users/ghost/node-v0.0.1.wasm*** - 
    - --name - the name of the module being uploaded
    - --version - the version of the module being uploaded
    - --file - where the file is being uploaded from

##### How Does GPMD Work With GVM?
While technically gvm can run as a single instance, it is far less powerful than when it works in conjunction with other gvm nodes. This is the main reason why we have designed GVM to require GPMD to be running, so that the GVM will connect automatically and have access to all of the resources provided by default. We talk more about the internals of GVM and how it works in another [section](../gvm/index.md).

**Node Registration & Connection**

When a GVM comes online it will automatically reach out to GPMD and register itself. GPMD saves the node name, id, and port. This is used when a GVM wants to communicate with another node.

When a GVM requests to discover nodes on the network, GPMD responds with the name of each discoverable node.

When a GVM requests a connection to another node on the network, GPMD responds with the name, id, address, and port for the corresponding node. The calling node will use that information to request a connection with the requested node. From that point forward, those nodes are connected and can communicate directly. Direct communication between local nodes is discussed in another [section](../gvm/index.md).

***Remote Node Communication***
Communication between nodes on remote devices will be implemented in Phase Two.

**Handling Resources**
To keep GVMs as small and lightweight as possible, we keep all relevant resources in a central place on each device. This means that each GVM has bare minimum functionality and does not come with any Modules by default. This leads into the Resource Handling functionality of GPMD.

Currently the only resource handled by GPMD is the WASM Modules used by the GVMs. 

***Handling WASM Modules***

Modules are stored as WASM ByteCode in a hidden folder on the device. The list of Modules on device are listed in the ghost.modules.json file in the same hidden folder. The user does not need to modify any of these files manually, but they can be viewed for reference if desired.

<u>Uploading WASM Modules</u>

In order for Modules to be available for GVMs to use, they must be uploaded to GPMD. Currently we only support adding Modules to GPMD from local sources. 

```
$: gpmd wasm upload --name=node --version=v0.0.1 --file=/Users/ghost/node-v0.0.1.wasm
```

--name  - the name of the module being uploaded
--version - the version of the module being uploaded
--file - where the file is being uploaded from

When a Module is uploaded a copy is added to the hidden folder on device and the information saved in the ghost.modules.json file.

<u>WASM Module Request</u>

A GVM Node will request a WASM Module using the GPMD connection established when the GVM comes online. The GVM will request a Module with its name and version (if the version is left blank, gpmd responds with the latest version available).

If the Module is found, GPMD will respond with the Module name, version, hash, and binary as a base64 encoded string.

More information on how GVM handles WASM Modules including requests is located in another [section](../gvm/index.md).