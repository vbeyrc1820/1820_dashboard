ROS Communication files
=========================


msgRosIot.action
-----------------

Description : An action message file used by ROS Actions for communication. It has goal, result and feedback messages.

Topic : /action_iot_ros

#goal 
protocol (string) : Type of network protocol used (http or mqtt)
mode (string) : Can be publisher (pub), subscriber (sub) or NA.
topic (string) : Topic name onto which it publishes/subscribes to (can be NA).
message (string) : The information that has to be carried. 

#result
flag_success (bool) : Flags success or faliure using Boolean if the goal is achieved or failed.

#feedback
percentage_complete (int8) : Feedback of the processing goal.




CameraImage.msg
-----------------

Description : Sends a list of colours from the node_camera1.

Topic : /camera1/image

ColourList (string[]) : A list of strings containing the colours recognised in order. 





LogicalCameraImage.msg
-----------------

Description : Sends the Pose of the object and the model names detected.

Topic : /eyrc/vb/logical_camera_2

models (Model[]) : A list of models detected contains type (string) and pose (geometry_msgs/Pose)
pose (geometry_msgs/Pose) : Pose of the object (poses in the frame of the camera)




robot_goal.msg
-----------------

Description : Sends the Package information to the Ur5_2 

Topic : box_position

Goal (int32) : The packsge number.
colour (string) : The colour of the package
pose (geometry_msgs/Pose) : The pose of the Package.




Orders.yaml
-----------------
Description : Contains parameter lists like order_list, order_details,order_shipping_details
	      which contain the details of the orders incoming, left to process and the ones in process
order_list: Nested list containing elements like : [COLOUR,PRIORITY]
order_details : Nested list containing elements like : [COLOUR,PRIORITY,ORDERID,CITY,ITEM,PRIORITY,QUANTITY,COST]
orders_shipping_details : Nested list containing elements that are deleted from order_details.


config_pyiot.yaml
-----------------

Description : Contains the server, subscription topic and publisher topic details for pyiot module.


ConveyorBeltControl.srv
-----------------


Description : Request/ reply structure to control the power of conveyor belt. 

Service : /eyrc/vb/conveyor/set_power

#request
power (float64) : Takes in the power value the conveyor has to be set on.

#reply
bool (success)  : Uses Boolean to flag success. 



vacuumGripper
-----------------

Description : 
#request
activate_vacuum_gripper(bool) : Takes the True/False value to activate/deactivate gripper.

#reply
result(bool) : Uses Boolean to flag success.


 








 