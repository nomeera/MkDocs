---
tags:
  - ROS
  - C++
  - Python

---

# 🤖 ROS 2 Core Components

Welcome to the world of ROS 2! This guide uses emojis to make concepts easy and fun to understand. Let’s break down the core components:

1. 🧩 **Nodes**

   - The building blocks of ROS 2! Each node is a process that performs a specific task (e.g., controlling a motor, processing sensor data).
2. 🔗 **Topics**

   - For asynchronous communication. Nodes publish (📤) and subscribe (📥) to topics to exchange messages.
3. 🛠️ **Services**

   - Enable synchronous request-response (☎️). A client node sends a request, and a server node replies.
4. 🏃 **Actions**

   - For long-running tasks with feedback (like moving a robot arm and getting progress updates 🦾).
5. ⚙️ **Parameters**

   - Tune your nodes at runtime! Parameters let you configure values (like thresholds, rates) without changing code.

---

> 🧩 **What is a Node?**
>
> A ROS 2 node is a single unit of computation within the ROS 2 graph. All components (publishers, servers, clients, etc.) are implemented inside nodes, making them the foundation of any ROS 2 system.
>
> In this guide, we’ll create a **CustomNode** blueprint in both Python 🐍 and C++ 💻 to show how flexible and reusable nodes are!
>
> ![Node Illustration](./services//Resources/node.drawio.png)

## 🐍 Implementing a ROS 2 Node (Python)

Let's explore the design of our custom node from a software architecture viewpoint.

### First: Importing Required Dependencies

```python
import rclpy
from rclpy.node import Node
```

These imports provide the fundamental building blocks for ROS 2 Python development. The `rclpy` package contains the core functionality for ROS 2 Python programming, while the `Node` class from `rclpy.node` serves as the abstract base class that we'll extend to create our custom node.

### Creating Our Custom Node Class

We'll inherit from the abstract `Node` class provided by the ROS 2 Python API (rclpy). This inheritance-based approach allows us to use the methods and attributes provided by the base class to create and customize our node.

The `__init__` method (constructor) plays a crucial role as it's the primary mechanism for initializing our node with all the features we want exposed within the ROS 2 ecosystem. such as registering the node with the ROS 2 system, setting up communication layers, parameter handling, and initializing logging mechanisms. Without calling the `Node`  (Base Class) constructor, these features will not be initialized, and your node will lack critical functionality.

```python

class CustomNode(Node):
    def __init__(self):  # 'self' is a reference to the current object instance
        super().__init__('node_name')  # 'super()' creates a temporary object of the base class to call its constructor
  
        # At this point, our node is created and initialized in the ROS 2 system
  
        # We can now use any attributes or methods inherited from the base class
   	    # Here you would typically add:
        	# - Publishers for sending data by create_publisher()
        	# - Subscribers for receiving data by create_subscription()
        	# - Services for request/response patterns by create_service() and create_client()
            # - Action server, client, and goal by create_server<AcnT>() and create_client<AcnT>()
        	# - Timers for periodic execution by create_timer()
            # - Parameters for configurable settings by declare_parameter() and get_parameter()
            # - and so on.
  
	# example method
        self.get_logger().info('Hello, ROS 2!')  # This method prints a message to the console/log
```

The constructor method first calls the parent class's constructor using `super().__init__('node_name')`, which registers our node in the ROS 2 system with the specified name. After this initialization, we gain access to all the functionality of a ROS 2 node and can begin customizing it to suit our specific needs.

### Main Function for Node Execution

```python
def main(args=None):
    rclpy.init(args=args)  # Initialize the ROS 2 Python client library with command-line arguments
  
    node = CustomNode()  # Create an instance of our custom node class
  
    rclpy.spin(node)  # Process callbacks and keep the node alive until shutdown
  
    # Clean up resources
    node.destroy_node()  # Release node-specific resources
    rclpy.shutdown()  # Shut down the ROS 2 client library entirely

if __name__ == '__main__':
    main()
```

After creating our `CustomNode`, the next step is to execute it within the ROS 2 ecosystem. The `rclpy` library provides several functions that facilitate the execution of nodes in a reliable and efficient manner. These functions not only ensure smooth operation but also include features for managing the node's lifecycle,handle errors, interact with the ROS 2 ecosystem, configure (QoS) Polices, and cleaning up resources when the node is no longer needed.

1. **`rclpy.init(args=args)`**: initializes the ROS 2 communication layer, which is responsible for managing all interactions between nodes, topics, services, and other ROS 2 entities. This step is **mandatory** before creating any nodes or interacting with the ROS 2 system. arguments passed to the program. This initialization must happen before creating any nodes.
   1. `rclpy.spin(node)` enters an event loop that keeps the node running and processing callbacks (for subscribers, timers, services, etc.) until the program receives a shutdown signal.
2. `node.destroy_node()` properly cleans up node-specific resources such as publishers, subscribers, and service clients. This should be called before shutting down ROS 2 to ensure proper resource management
3. `rclpy.shutdown()` completely shuts down the ROS 2 client library, releasing all associated resources. This should be called at the end of your program.

The sequence matters: initialize ROS 2, create nodes, process node callbacks (spin), clean up nodes, and finally shut down ROS 2 completely.

We have now created a blueprint node that can be adapted to any communication model by simply adding the relevant methods (e.g., publisher, server, client, etc.) within the node class.

Alternatively, nodes can also be created using the `rclpy.create_node(...)` function, which provides a simple, functional approach—ideal for quick scripts or debugging. However, the object-oriented approach, where a custom class inherits from the `Node` class (`class CustomNode(Node): ...`), is the recommended and scalable method for writing ROS 2 nodes.

## 💻 Implementing a ROS 2 Node (C++)

Let's explore the design of our custom node from a software architecture viewpoint. We will go to the implementation as the concepts is the same we explained previous. and I will explain only the differences

### First: Importing Required Dependencies

```Cpp
#include <rclcpp/rclcpp.hpp>
```

like `rclpy` The `rclcpp` package contains the core functionality for ROS 2 C++ programming and its contain a Node class that we will inherit our custom node from it

### Creating Our Custom Node Class

```Cpp
class CustomNode : public  rclcpp::Node
{
  public:
    CustomNode() : Node("node_name") {
    
      // At this point, our node is created and initialized in the ROS 2 ecosystem
  
        // We can now use any attributes or methods inherited from the base class
   	    // Here you would typically add:
        	// - Publishers for sending data by create_publisher<MsgT>()
        	// - Subscribers for receiving data by create_subscription<MsgT>()
        	// - Services for request/response patterns by create_service<SrvT>() and create_client<SrvT>()
          // - Action server, client, and goal by create_server<AcnT>() and create_client<AcnT>()
        	// - Timers for periodic execution by create_timer()
          // - Parameters for configurable settings by declare_parameter() and get_parameter()
          // - and so on.
    };



  private:

};
```

Like python code we inherit a custom class from the base class and initialize base class constructor with node_name.

Note

> Node base class definition is:
>
> ```
> class Node : public std::enable_shared_from_this
> ```
>
> This mean that its inherits from `std::enable_shared_from_this, Allows the node to safely generate a shared_ptr of itself (used for managing memory in ROS 2 callbacks). So we will safely use shared pointer when define the methods we needed for the best practice`

### Main Function for Node Execution

```Cpp
int main(int argc, char** argv)
{
  rclcpp::init(argc, argv);  // initializes the ROS 2 communication layer,
  auto node = std::make_shared<CustomNode> (); // create a shared pointer that point to CustomNode 
  rclcpp::spin(node);   // enters an event loop that keeps the node running and processing callbacks
  rclcpp::shutdown(); // completely shuts down the ROS 2 client library, releasing all associated resources.
  return 0;
} 
```

## 🌐 Common Features in ROS 2 Components (Publisher, Subscriber, Client, Server, Parameter, Action, etc.)

<img align="right" width="50%" src="./services/Resources/Base Node.png">

Since all ROS 2 components (publishers, subscribers, services, etc.) are created and managed through the `rclcpp::Node` class, they share several **common design patterns and mechanisms**. Below is a breakdown of these similarities:
Now we have blueprint CustomNode in python and C++, let's use it to build our ROS 2 system component.
Let's discuss the last generic point in creating our blueprint CustomNode which is the common features that we will creating it in most ROS 2 components

---

### 🧩 1. Dependency on `rclcpp::Node`

All ROS 2 components **must be created from a `Node` instance**, meaning:

- They **require an initialized `Node`** to interact with the ROS 2 graph.
- Example:

  ```cpp
  // C++
  auto node = std::make_shared<rclcpp::Node>("my_node");
  auto pub = node->create_publisher<...>("topic", qos);  // Publisher
  auto sub = node->create_subscription<...>("topic", qos, callback);  // Subscriber
  auto client = node->create_client<...>("service");  // Client
  ```

  ```python
  # Python
  node = Node("my_node")
  pub = node.create_publisher(MessageType, "topic", qos)
  sub = node.create_subscription(MessageType, "topic", callback, qos)
  client = node.create_client(ServiceType, "service")
  ```

---

### 🔄 2. Callback-Based Execution

Most ROS 2 components **rely on callbacks** for handling events:

| Component                | Callback Usage                             |
| ------------------------ | ------------------------------------------ |
| **Subscriber**     | Executes when a new message arrives.       |
| **Timer**          | Runs at a fixed frequency.                 |
| **Service Server** | Executes when a request is received.       |
| **Action Server**  | Handles goal, cancel, and result requests. |

- Example (Subscriber Callback):

  ```cpp
  // C++
  // Search about lambada function in C++ to understand the bellow function 
  auto callback = [node](const std_msgs::msg::String::SharedPtr msg) {
      RCLCPP_INFO(node->get_logger(), "Received: %s", msg->data.c_str());
  };
  auto sub = node->create_subscription<std_msgs::msg::String>("topic", 10, callback);
  ```

  ```python
  # Python
  def callback(msg):
    node.get_logger().info(f"Received: {msg.data}")
  sub = node.create_subscription(MessageType, "topic", callback, qos)
  ```

---

### 📶 3. Quality of Service (QoS) Settings

Most communication components **support QoS policies**, which control:

- **Reliability** (`RELIABLE` vs. `BEST_EFFORT`)
- **Durability** (`TRANSIENT_LOCAL` for late-joining subscribers)
- **Deadlines & Lifespans** (message expiration)

Example (Setting QoS for a Publisher):

```cpp
// C++
rclcpp::QoS qos(10);  // Depth = 10
qos.reliable();       // Ensure message delivery
auto pub = node->create_publisher<std_msgs::msg::String>("topic", qos);
```

```python
# Python
qos = QoSProfile(depth=10)
qos.reliability = ReliabilityPolicy.RELIABLE
pub = node.create_publisher(String, "topic", qos)
```

---

### 🧬 4. Use of Templates

Most component-creation methods **use templates** to support different message types in `C++`, while `Python` uses dynamic typing:

| Component            | Template Usage                         |
| -------------------- | -------------------------------------- |
| **Publisher**  | `create_publisher<MessageT>(...)`    |
| **Subscriber** | `create_subscription<MessageT>(...)` |
| **Service**    | `create_service<ServiceT>(...)`      |
| **Client**     | `create_client<ServiceT>(...)`       |

Example (Service with a Custom Message):

```cpp
// C++
auto srv = node->create_service<my_interfaces::srv::Compute>("compute", 
    [](const Request::SharedPtr req, Response::SharedPtr res) {
        res->result = req->a + req->b;
    });
```

### Generic Template Structure

    Most Node methods follow this pattern:

```Cpp
// C++
template<
    typename MessageT,                     // Primary message/service type
    typename AllocatorT = std::allocator<void>,  // Memory allocator (default)
    typename ComponentT = DefaultComponentType<MessageT, AllocatorT>,  // Component type (Publisher/Subscriber/etc.)
    typename OptionsT = OptionsWithAllocator<AllocatorT>  // Configuration options
>
std::shared_ptr<ComponentT> create_component(
    const std::string &name,               // Topic/service name
    const QoS &qos,                        // Quality of Service settings
    CallbackT &&callback = nullptr,        // Callback (if applicable)
    const OptionsT &options = OptionsT()   // Additional options
);
```

---

### ⏰ 5. Time Handling (`node->now()`)

- Nodes provide **time access** (`node->now()`), which respects:
  - **Wall time** (real-world clock).
  - **Simulated time** (if `/use_sim_time` is `true`).

Example (Using Node Time in a Timer):

```cpp
// C++
auto timer = node->create_wall_timer(500ms, [node]() {
    auto now = node->now();  // ROS 2 time
    RCLCPP_INFO(node->get_logger(), "Current time: %f sec", now.seconds());
});
```

```python
# Python
def timer_callback():
    now = node.get_clock().now()
    node.get_logger().info(f"Current time: {now.seconds_nanoseconds()[0]} sec")

timer = node.create_timer(0.5, timer_callback)  # 500ms = 0.5 seconds
```

### 🧹 6. Lifecycle Management

- All components **automatically clean up** when the node is destroyed.
- **Manual cleanup** is rarely needed (thanks to smart pointers).

---

---

## 🎯 Key Takeaways

1. 🧩 **`rclcpp::Node` is the central hub** for all ROS 2 components.
2. 🔄 **Callbacks + QoS + Templates** are fundamental across publishers, subscribers, services, etc.
3. ⏰ **Time, threading, and parameters** are consistently managed by the node.
4. 🧹 **Smart pointers & RAII** ensure safe destruction.

Now you can use your **CustomNode** blueprint to build any ROS 2 component! Assemble publishers, subscribers, services, clients, and more like pieces of a puzzle 🧩 for a modular, scalable, and reusable ROS 2 system.
