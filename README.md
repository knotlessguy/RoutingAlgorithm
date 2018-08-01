# RoutingAlgorithm

# Structure of the repo
./RoutingAlgorithm/
./RoutingAlgorithm/sampleoutput.txt
./RoutingAlgorithm/sampleinput.txt
./RoutingAlgorithm/routing_algo.cpp
./RoutingAlgorithm/node.h
./RoutingAlgorithm/main.cpp
As you can see, the repo has five files.The main algorithm lies in the file routing_algo.cpp. The sample input and output files will help us test our code. You can compile the code as follows to create the 'rip' executable.

  g++ -std=c++11 main.cpp routing_algo.cpp -o rip
The code is written to take input from standard input (terminal), so you can redirect an input text file to standard input as follows, in order to run the code.
./rip < sampleinput.txt
Below is a sample input file provided to you.
2 
3 
A B C 
A 10.0.0.1 10.0.0.21 B 
B 10.0.0.21 10.0.0.1 A 
B 10.0.1.23 10.0.1.3 C 
C 10.0.1.3 10.0.1.23 B 
EOE 
3 
A B C 
A 10.0.0.1 10.0.0.21 B 
B 10.0.1.23 10.0.1.3 C 
B 10.0.0.21 10.0.0.1 A 
C 10.0.1.3 10.0.1.23 B 
EOE
The first line is the number of input topologies (2 topologies, both identical here, but that may not always be the case). The next line defines the number of nodes in the network for this input instance, followed by the labels for each node. Then, the next few lines (until EOE is found) will consist of the assignment of an IP address to each interface of the node along with the IP address of the node to which it is connected to. Here, in the above example at line #4, interface of A with an IP address 10.0.0.1 is connected to an interface of B with IP address 10.0.0.21. The overall topology represented by the above input is as follows.

The code in main.cpp parses the input and creates a data structure of the list of the nodes in the network, along with information about the interfaces at each node. It also initializes the routing table at each node with entries of zero cost only to itself. All the relevant datastructures are defined in node.h. Once the input has been parsed, a call is made to the function 'routingAlgo', which is implemented in the file routing_algo.cpp. You must complete this function so that it correctly computes the routing table entries at all nodes. Once this function returns, the main function prints the routing table before moving on to the next set of inputs.

The function 'routingAlgo' will run multiple iterations of the Bellman Ford update algorithm at every node in the list of nodes, in order to compute and update routing table entries. You must figure out when the routing tables have converged and return from the function, so that the routing tables can be printed out. During the execution of the routingAlgo function, a node may wish to send its routing table entries to other nodes. This is accomplished by calling the sendMsg() function in the node's object. Calling sendMsg at a node will result in a corresponding call to the recvMsg function at all its neighbors. The sendMsg function is already implemented in node.h; you must complete the recvMsg function in order to process a routing table update from a neighbor at a given node.

The recvMsg function will run the Bellman Ford update algorithm of the distance vector routing protocol, and update its routing table as required. You will find the datastructures and functions implemented in node.h useful; look over them carefully. In particular, the function isMyInterface(string eth) returns true/false depending whether the argument passed is the own interface of a node or not, and will be helpful during your route computation algorithm.

The function routingAlgo is invoked by main, and runs the distance vector routing protocol at all nodes in the network. This function causes nodes to send messages of their routing tables to their neighbors, who will process these routing tables in recvMsg. The datastructures corresponding to the messages exchanged are all defined in node.h.


If you implement the routing protocol correctly, the correct output from your program when run against the above sample input (with two similar topologies of three nodes each) should be as follows.
A: 
10.0.0.1 | 10.0.0.1 | 10.0.0.1 | 0 
10.0.0.21 | 10.0.0.21 | 10.0.0.1 | 1 
10.0.1.23 | 10.0.0.21 | 10.0.0.1 | 1 
10.0.1.3 | 10.0.0.21 | 10.0.0.1 | 2 
B: 
10.0.0.21 | 10.0.0.21 | 10.0.0.21 | 0 
10.0.1.23 | 10.0.1.23 | 10.0.1.23 | 0 
10.0.0.1 | 10.0.0.1 | 10.0.0.21 | 1 
10.0.1.3 | 10.0.1.3 | 10.0.1.23 | 1 
C: 
10.0.1.3 | 10.0.1.3 | 10.0.1.3 | 0 
10.0.0.21 | 10.0.1.23 | 10.0.1.3 | 1 
10.0.1.23 | 10.0.1.23 | 10.0.1.3 | 1 
10.0.0.1 | 10.0.1.23 | 10.0.1.3 | 2 
A: 
10.0.0.1 | 10.0.0.1 | 10.0.0.1 | 0 
10.0.0.21 | 10.0.0.21 | 10.0.0.1 | 1 
10.0.1.23 | 10.0.0.21 | 10.0.0.1 | 1 
10.0.1.3 | 10.0.0.21 | 10.0.0.1 | 2 
B: 
10.0.0.21 | 10.0.0.21 | 10.0.0.21 | 0 
10.0.1.23 | 10.0.1.23 | 10.0.1.23 | 0 
10.0.0.1 | 10.0.0.1 | 10.0.0.21 | 1 
10.0.1.3 | 10.0.1.3 | 10.0.1.23 | 1 
C: 
10.0.1.3 | 10.0.1.3 | 10.0.1.3 | 0 
10.0.0.21 | 10.0.1.23 | 10.0.1.3 | 1 
10.0.1.23 | 10.0.1.23 | 10.0.1.3 | 1 
10.0.0.1 | 10.0.1.23 | 10.0.1.3 | 2
The output consists of the label of a node, followed by the routing table entries at that node, in the format {destination ip | next hop | my ethernet interface | hop count}. This output is printed out by the call to printTable() for each node at the end of main.cpp. Note that the function printTable sorts the routing table entries so that the output is deterministic for a given input, enabling autograding at our end.
