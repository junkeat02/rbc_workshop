# ROS 2 Workshop


## Create publisher node

### Using terminal
1. This will be your directory to create the python node for ROS2 after the package is created.

    ````
    cd ~/<your workspace's name>/src/<your package's name>/<your package's name>
    ````
2. Create your first python ROS2 node file

    ```
    touch <node_name>.py
    nano <node_name>.py
    ```
    At this case the node_name will be publisher.<br>
    Inside the empty file write your first ROS2 node's code

    ```
    import rclpy
    from rclpy.node import Node
    from std_msgs.msg import String

    class MyNode(Node):
        def __init__(self):

            # node name is "Publisher"
            super().__init__("Publisher")  # change based on own preference
            
            # timer 
            self.create_timer(1, self.timer_callback)

            # datatype is String
            # topic name here is "testing_topic"
            self.publisher = self.create_publisher(String, "testing_topic", 10)  # change based on own preference
            
        def timer_callback(self):
            self.get_logger().info("Hi")
            msg = String()
            msg.data = "Hello from publisher"

            # Sending msg here
            self.publisher.publish(msg)
            
    def main(args=None):
        rclpy.init(args=args)
        node = MyNode()

        # to set the nod keep alive
        rclpy.spin(node)
        rclpy.shutdown()
        
        
    if __name__ == "__main__":
        main()

    ```

3. Return to the package root directory

    ```
    cd ../
    ```

4. Make sure you are in the package root directory and modify the setup.py file

    ```
    nano setup.py
    ```
    A writable file is then pop up and add 
    ```
    "node_name = package_name.node_name:main"
    ```
    into the setup() function with parameter of entry_points. <br>
    For example:
    ```
    entry_points={
    'console_scripts': [
        "node_name = package_name.node_name:main",
    ```

5. Rebuild the package

    ```
    cd ~/<your workspace>
    colcon build --symlink-install --packages-select <your package>
    ```

## Create listener

1. The procedure are the same as creating the publisher previously but the different is the code for creating a listener or subscriber in python.

    ```
    import rclpy
    from rclpy.node import Node
    from std_msgs.msg import String

    class MinimalSubscriber(Node):
        def __init__(self):
            super().__init__('minimal_subscriber')
            self.subscription = self.create_subscription(
                String,
                'testing_topic',
                self.listener_callback,
                10)
            self.subscription  # prevent unused variable warning

        def listener_callback(self, msg):
            self.get_logger().info(f'I heard: "{msg.data}"')

    def main(args=None):
        rclpy.init(args=args)
        minimal_subscriber = MinimalSubscriber()

        try:
            rclpy.spin(minimal_subscriber)
        except KeyboardInterrupt:
            pass
        finally:
            minimal_subscriber.destroy_node()
            rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```


## Create Service

1. Node setup first like previous two sections
2. Code for a service server:
    ### Use built-in service

    ```
    import rclpy
    from rclpy.node import Node
    from example_interfaces.srv import AddTwoInts

    class AddTwoIntsService(Node):
        def __init__(self):
            super().__init__('add_two_ints_service')
            self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_two_ints_callback)

        def add_two_ints_callback(self, request, response):
            response.sum = request.a + request.b
            self.get_logger().info(f'Received request: a={request.a}, b={request.b}, sum={response.sum}')
            return response

    def main(args=None):
        rclpy.init(args=args)
        node = AddTwoIntsService()

        try:
            rclpy.spin(node)
        except KeyboardInterrupt:
            pass
        finally:
            node.destroy_node()
            rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```

    Create another node file for the service client.
    
    ```
    import rclpy
    from rclpy.node import Node
    from example_interfaces.srv import AddTwoInts

    class AddTwoIntsClient(Node):
        def __init__(self):
            super().__init__('add_two_ints_client')
            self.client = self.create_client(AddTwoInts, 'add_two_ints')
            while not self.client.wait_for_service(timeout_sec=1.0):
                self.get_logger().info('Waiting for service to become available...')
            self.request = AddTwoInts.Request()

        def send_request(self, a, b):
            self.request.a = a
            self.request.b = b
            self.future = self.client.call_async(self.request)

    def main(args=None):
        rclpy.init(args=args)
        node = AddTwoIntsClient()
        a = int(input("a: "))
        b = int(input ("b: "))
        node.send_request(a, b)

        while rclpy.ok():
            rclpy.spin_once(node)
            if node.future.done():
                try:
                    response = node.future.result()
                    node.get_logger().info(f'Result: {response.sum}')
                except Exception as e:
                    node.get_logger().error(f'Service call failed: {str(e)}')
                break

        node.destroy_node()
        rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```

3. Custom service and msg <br>
Reference:
https://docs.ros.org/en/crystal/Tutorials/Custom-ROS2-Interfaces.html



## Creating action

1. The code of an example action

    ### Built-in action server

    ```
    import rclpy
    from rclpy.node import Node
    from example_interfaces.action import Fibonacci
    from rclpy.action import ActionServer

    class FibonacciActionServer(Node):
        def __init__(self):
            super().__init__('fibonacci_action_server')
            self._action_server = ActionServer(
                self,
                Fibonacci,
                'fibonacci',
                self.execute_callback)

        async def execute_callback(self, goal_handle):
            self.get_logger().info(f'Received goal: {goal_handle.request.order}')
            feedback_msg = Fibonacci.Feedback()
            feedback_msg.partial_sequence = [0, 1]

            for i in range(2, goal_handle.request.order):
                feedback_msg.partial_sequence.append(
                    feedback_msg.partial_sequence[i - 1] + feedback_msg.partial_sequence[i - 2])
                self.get_logger().info(f'Feedback: {feedback_msg.partial_sequence}')
                goal_handle.publish_feedback(feedback_msg)
                await self.sleep(1)  # Simulate processing time

            goal_handle.succeed()
            result = Fibonacci.Result()
            result.sequence = feedback_msg.partial_sequence
            return result

    def main(args=None):
        rclpy.init(args=args)
        node = FibonacciActionServer()

        try:
            rclpy.spin(node)
        except KeyboardInterrupt:
            pass
        finally:
            node.destroy_node()
            rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```
    ### Action server client:
    ```
    import rclpy
    from rclpy.node import Node
    from example_interfaces.action import Fibonacci
    from rclpy.action import ActionClient

    class FibonacciActionClient(Node):
        def __init__(self):
            super().__init__('fibonacci_action_client')
            self._action_client = ActionClient(self, Fibonacci, 'fibonacci')

        def send_goal(self, order):
            self.get_logger().info(f'Sending goal: order={order}')
            goal_msg = Fibonacci.Goal()
            goal_msg.order = order
            self._action_client.wait_for_server()
            self._send_goal_future = self._action_client.send_goal_async(
                goal_msg,
                feedback_callback=self.feedback_callback)
            self._send_goal_future.add_done_callback(self.goal_response_callback)

        def goal_response_callback(self, future):
            goal_handle = future.result()
            if not goal_handle.accepted:
                self.get_logger().info('Goal rejected.')
                return
            self.get_logger().info('Goal accepted.')
            self._get_result_future = goal_handle.get_result_async()
            self._get_result_future.add_done_callback(self.get_result_callback)

        def feedback_callback(self, feedback_msg):
            self.get_logger().info(f'Feedback received: {feedback_msg.feedback.partial_sequence}')

        def get_result_callback(self, future):
            result = future.result().result
            self.get_logger().info(f'Result: {result.sequence}')

    def main(args=None):
        rclpy.init(args=args)
        node = FibonacciActionClient()
        node.send_goal(10)  # Example goal

        try:
            rclpy.spin(node)
        except KeyboardInterrupt:
            pass
        finally:
            node.destroy_node()
            rclpy.shutdown()

    if __name__ == '__main__':
        main()

    ```


