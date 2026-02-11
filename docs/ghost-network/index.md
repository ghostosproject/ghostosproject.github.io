### Ghost Network
The Ghost Network is the first solution provided by The Ghost Os Project. The goal of The Ghost Network is to create the infrastructure for peer to peer cloud networks. We aim to create the backbone to allow anyone to setup a simple cloud using existing devices.

For a quick setup without all the details... [Getting Started Guide](getting-started/index.md)

For a deep dive into The Ghost Network, continue below!

The basic building blocks for The Ghost Network are as follows:
- Ghost Port Mapper Daemon (GPMD)
- Ghost Virtual Machine (GVM)

We are currently in Phase One of the project.

#### What is Phase One?
Phase One of the Ghost Network project is to allow for local cloud computing on a single device. We decided to get all of the functionality running on a single device and then expand to multiple devices that interconnect with eachother. From the developers perspective, there is no difference between the network running on one device or one-thousand. We will explain how we accomplish this in later documentation.

##### [Ghost Port Mapper Daemon (GPMD)](gpmd/index.md)

The main role of GPMD in Phase One is to orchestrate communication between local GVM Nodes. When a GVM is created, it will reach out to GPMD and register itself. If the registration is successfull, the GVM becomes a node on the local network. The Node is now able to be discover by other nodes on the network as well as be discovered. 
GPMD will orchestrate the connection between these two nodes. GPMD only orchestrates the connection. Once a conection is established, all communication is directly between the two nodes.


##### [Ghost Virtual Machine (GVM)](gvm/index.md)

The GVM is the basic building block for our network. It is a lightweight virtual machine that runs WASM ByteCode. GVM runs WASM modules and functions in the format Module.function(). If a command Module is run, the GVM runs the Wasm Module from it's _start function. if a command Module.function(args...) is run, GVM will run only the specified function within the module and return the results.

A GVM becomes a Node when it registers with GPMD. This allows it to discover and be discovered by other nodes within the network. This is important because it allows for communication between nodes. Specifically the ability to run Modules and Functions on other nodes. Although this is not especially useful at the moment, when we expand to Phase Two and connect multiple devices, spreading your workloads across multiple devices may have its benefits. 

