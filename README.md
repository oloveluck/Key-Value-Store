This is a distributed key-value store that implements RAFT

The key-value store can be accessed by sending a get or put request to the program, and this data is stored in memory within a python dictionary. A potential extension could be adding some sort of persistant storage for key-value pairs. 

In our implementation we have different classes for each type of RPC and RPC response. 
These are constructed with an RPCFactory and are called with the handleResponse() function that also takes in a server. 
A Server will have a list of replicas, as well as all of the data described in the Raft paper: https://raft.github.io/raft.pdf, and a base class called Controller, that is one of Follower, Candidate, and Leader.  

For performence and reliability our implementation sends requests to clients and and other replicas asyncronously using an event loop. This allows for us to pipeline the election and log replication process, and decreases the chance of complications when a Leader dies.

Additionally in order to reduce the amount of packets sent during an election we deviated from the paper slightly, adding a cache for received client messages during the candidate state. Without this cache, it is common to run into a packet storm during an election. 

  
