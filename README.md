# Week5README

# Writing a simple publisher and subscriber (Python)

# #1 create a package

Recalling  that package should be created in the src directory, not the root of the workspace. So, navigate into **ros2_ws/src**, and run the package creation command:

```pyhton
ros2 pkg create --build-type ament_python py_pubsub
```

the output should be as below

![w1](https://user-images.githubusercontent.com/90182787/192686880-9b5e519f-d2ff-4851-8e0c-2edd49f9fd9d.jpg)

# #2 Writing the publisher node

Downloading the example talker code by entering the following command:

```pyhton
wget https://raw.githubusercontent.com/ros2/examples/humble/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

![w2](https://user-images.githubusercontent.com/90182787/192687253-f1f91eee-f50f-4bf5-b8d0-d2718e586ecb.jpg)

## #2.1 Examing the code

you can open the **pulisher_member_function.py** and examine it. you can notice from the code that node classes can be used because it imports **rclpy**
Next, a timer is created with a callback to execute every 0.5 seconds. **self.i** is a counter used in the callback.

```python
import rclpy
from rclpy.node import Node

from std_msgs.msg import String


class MinimalSubscriber(Node):

    def __init__(self):
        super().__init__('minimal_subscriber')
        self.subscription = self.create_subscription(
            String,
            'topic',
            self.listener_callback,
            10)
        self.subscription  # prevent unused variable warning

    def listener_callback(self, msg):
        self.get_logger().info('I heard: "%s"' % msg.data)


def main(args=None):
    rclpy.init(args=args)

    minimal_subscriber = MinimalSubscriber()

    rclpy.spin(minimal_subscriber)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_subscriber.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

## #2.2 adding dependencies

We have to navigate one level back to the **ros2_ws/src/py_pubsub** directory, where the **setup.py**, **setup.cfg**, and **package.xml** files have been created for us

Need to open **package.xml**

Make sure to fill **description**, **maintainer** and **license** tags

Add the following dependencies corresponding to your node’s import statements

```python
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```

![package](https://user-images.githubusercontent.com/90182787/192692561-d97d3ff0-6aa7-40bf-94b3-6ef777f0018b.jpg)

Make sure to save the file

# #3 Write the subscriber node

Return to **ros2_ws/src/py_pubsub/py_pubsub** to create the next node. Enter the following code in your terminal

```pyhton
wget https://raw.githubusercontent.com/ros2/examples/humble/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```
Now the directory should have the following

```c++
__init__.py  publisher_member_function.py  subscriber_member_function.py
```

![publisher1](https://user-images.githubusercontent.com/90182787/192693717-777cc400-d275-4af5-b2ae-bd720b7b5df5.jpg)

# #3.1 Add an entry point

Open **setup.py** and add the entry point for the subscriber and the publisher. The *entry_points* field should now look like this

```python
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
                'listener = py_pubsub.subscriber_member_function:main',
        ],
},
```

