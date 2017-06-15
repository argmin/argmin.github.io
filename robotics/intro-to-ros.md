## What is ROS
ROS is a a middleware that lives between OS and your code, it provides these following services
* Communication layer, 
* device driver to communicate with hardware, 
* simulation / logging / visualization tools, 
* capabilities for control, perception, motion, planning etc.

## Components of ROS

### ROS Master
* Provides naming and resgitration service and manages communication between nodes.
* Enables node to discover other nodes.
* Nodes register with `master` during startup.
* Tracks topic and services.

```sh
# Start up
# export ROS_MASTER_URI=http://hostname:port-number/ # Use only when -p is added
roscore -p port-number
```

### ROS Node
* ROS Node is fined grained process that has single purpose, example move an arm
* The nodes are individually compiled and managed 
* Fault tolerances are isolated to single node,less code complexity
* Node are organized as packages

```sh
rosrun package_name node_name # Run a ROS node
rosnode list # Get list of ROS node
rosnode info node_name # Get information about node
rosnode kill node_name # Kill a node
rosnode ping node_name # Ping a node
```

### ROS Topics
* Nodes communicate over topics which named unidirectional buses via Pub-Sub model.
* 1 Publisher per topic, & n Subscribers
* Used for continuous data stream like camera images etc
 
```sh
rostopic list # list topic
rostopic echo /topic_name # Print messages being published to topic
rostopic info /topic_name # Print information about a topic
```

### ROS Messages
* ROS node cummunicate with each othe rby publishing messages.
* Messages are data structures that define the type of a topic.
* Standard primitive types and their arrays are supported.
* Defined in text files `.msg`

Example:
```sh
fieldtype1 fieldname1
fieldtype2 fieldname2
fieldtype3 fieldname3
```

```sh
rostopic type /topic_name # Print type of messages
```

### References
* [ETHZ ROS tutorial](https://www.ethz.ch/content/dam/ethz/special-interest/mavt/robotics-n-intelligent-systems/rsl-dam/ROS2017/lecture1.pdf)
* [ROS Wiki](http://wiki.ros.org/)

