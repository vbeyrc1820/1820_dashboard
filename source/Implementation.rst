Implementation-  what we did
==============================
This section describes the implementation of Task-5 in the sequence of operations or complexities that were faced by us.


1. 2D Camera:
--------------------
The node node_camera_1 takes an image of the boxes on the shelf and converts it into greyscale. Then it scans the image for barcodes and appends the scan results in a nested list "[[COLOUR,X,Y]]". This nested list is sorted with respect to x coordinates and then y coordinates. This makes the order of the list in the order of the package positions. This list is then published over the topic "/camera1/image".


2. Recieving orders:
---------------------
The orders are received by the node_action_server_ros_iot_bridge from the MQTT topic "/eyrc/vb/Aradmama/orders".
These orders need to be processed and sorted so that the highest priority package gets sent as a goal first. This is accomplished by a config file called orders.yaml. The orders are appended in the parameter list "order_list" in the form of "[COLOUR,PRIORITY]" where PRIORITY is 1 for Red, 2 for Yellow and 3 for Green. The details of the orders are appended in a parameter list called "order_details" in the form of "[COLOUR,PRIORITY,ORDERID,CITY,ITEM,PRIORITY,QUANTITY,COST]".


3. UR5 1:
--------------------
UR5 1 is controlled by the node: "node_ur5_1_control"
After the initialization of UR5 1, it moves to the drop position of packages and waits for orders to be placed.It subscribes to the topic "/camera1/image" and appends the sorted list of colours of the packages. As soon as the parameter nested lists "order_list" and "order_details" exist, they are sorted based on the PRIORITY element. The first list item is the order to be executed. After the order is appended in a temporary variable, it is popped from the parameter lists and,object lists and the colour list. The order details of the dispatched order is appended to a parameter list called "order_shipping_details" to be used by UR5 2 when it ships the package. All the trajectories of UR5 1 are prerecorded which bypasses the need of its calculation in every run, speeding up the execution


4. Conveyor Belt:
--------------------
The node "node_belt_control" is responsible for the conveyor belt and logical_camera_2. As soon as the initialization is done, the service for conveyor belt is flagged active. When the logical_camera_2 detects a model that exists in the object list (containing the package model names), it stops the conveyor belt and publishes the pose of the package on the topic "/box_position". As soon as the box is moved out of the frame of the camera, the conveyor is started at full speed.


5. UR5 2:
--------------------
UR5 2 is controlled by the node: "node_ur5_2_control".
After the initialization, the robot moves to the pickup position near the conveyor belt and waits for packages to arrive.It subscribes to the topic "/box_posititon". The message sent on this topic is the "robot_goal" message. It includes a serial number, the colour of the package and the pose of the box. The serial number ensures that the subscription callback actions for one box is executed only once.
This is done by cross referencing the serial number with a temporary serial number initialized in the node. When the serial number is equal to the temporary number, the actions are executed and the temporary serial number is incremented by one. The colour of the box determines the bin in which it is supposed to be placed. All the trajectories for the UR5 2 are prerecorded and played while execution which makes the process faster. The trajectories are optimized for the robot to reach the end goal position in minimum time. The pose from the message is used to add box in RVIZ. To prevent the chance of failure of the controller, we positioned the UR5 2 in a position that is always in the vicinity of the package and can pick it up easily. Since the packages arrive in the same file, the position of each package does not alter by a lot when the conveyor is stopped. This helps in speeding up the execution as the robot does not have to calculate any trajectories while execution.
