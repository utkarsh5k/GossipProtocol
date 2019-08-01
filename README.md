# GossipProtocol

Implementation of a naive gossip protocol. I implemented this project as a part of the course "Cloud Computing Concepts: Part 1" on Coursera. 

Main logic resides in MP1Node.cpp and MP1Node.h. 

The gossip protocol has a well known introducer, which every node contacts to join the group. Post the join, each node runs the gossip protocol to detect failures and constantly update their membership list. 

The algorithm is standard, and in this project, every node chooses 20% of its neighbors to send a gossip message to. However, if this selected number is less than 4, we either send to 4 neighbors, or total number of neighbors in the member list, whichever is smaller. 

There are three kinds of messages: 

1) JOINREQ: Each node on startup sends a JOINREQ message to the well known peer (introducer), asking to be included in the group. It provides its data (ID, PORT, HEARTBEAT) with this JOINREQ message. 
2) JOINREP: The well known peer, on receiving a JOINREQ message, creates a response with its copy of the membership list, and sends back the the fresh node, so that it gets the complete membership list. 
3) GOSSIP: In each round of the protocol, each node selects a few neighbors randomly and forwards its membership list, and we execute the standard gossip protocol. 

### Future Optimisation

Currently the peer maintains the list of all its neighbors, but I realise that with millions of nodes, this could clearly overwhelm the peer if it decided to maintain the complete membership list. The best solution would be to only have a list of a fixed size that would be sent to a requesting node. 

The question then obviously is how we choose the k members to be maintained in the list. Off the top of my head, I would prefer to keep the oldest members in the list, because if the sooner the older members find out about a new joinee, they might select it to forward their membership list. After sufficient rounds, we would expect that the oldest members have converged on the complete membership table, so it would mean that if they were to forward their gossip message to the new joinees, the new joinees will converge on the membership table soon enough. 

Ofcourse, the selection of gossip recipients only has to be semi-random, a peer might as well decide to send a fresh neighbor its membership list. 

All of this is still midnight ramblings, if anyone is interested in discussing this and improving the implementation, open an issue. :) 

### Running the code 

Run `make`
Run `./Application testcases/<any_testcase>`

You can find the run logs in dbg.log. Coursera provided us with a grader script `Grader.sh` which you can use to judge the implementation on the metrics of Accuracy and Completeness, both in case of joins and failures. 
