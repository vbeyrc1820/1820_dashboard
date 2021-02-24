API documentation
====================

1. Actionclient:
--------------------
class Actionclient:
    """ Class definition for Action Client, it sends
    goals to perform pub-sub MQTT tasks to the Action Server.
    """

    def __init__(self):
        """Constuctor for Actionclient.
        Attributes:
            _ac : Initialize Action Client.
            _goal_handles : Dictionary to store all the goal handles.
            param_config_iot : Read and Store IoT Configuration data from Parameter Server.
            _config_mqtt_pub_topic : Stores the publisher topic from the parameter file.
            message :
        """

    def feedback_callback(self, feedback):
        """ Sends Feedback regarding the status of goal.
        Args:
            feedback : The feedback on the goal status that has to be logged.
        """
    def on_transition(self, goal_handle):
        """This function will be called when there is a change of state
        in the Action Client State Machine. \n
        Args :
            goal_handle : Contains goal information which includes protocol, mode, topic and message
        """
    def send_goal(self, arg_protocol, arg_mode, arg_topic, arg_message):
        """This function is used to send Goals to Action Server.
        Args:
            arg_protocol : The protocol of the ntwork has to use.
            arg_mode : Mode, Publisher or Subscriber.
            arg_topic : Topic on which we need to publish or subscibe from.
            arg_message : The data that has to be transmitted.
        Returns:
            Returns a goal handle which contains goal information which includes protocol, mode, topic and message to be used by Action Server.
        """



2. node_action_server_ros_iot_bridge
--------------------
class IotRosBridgeActionServer:
    """ Class definition of IoTRosBridgeActionServer an action server acts as a
    bridge between ROS and MQTT,
    takes in goals to publish/subscribe to
        MQTT topics.
    """
    def __init__(self):
        """ Constructor for IotRosBridgeActionServer.
        Attributes:
            _as : Initialize the action server.
            param_config_iot : Read and Store IoT Configuration data from Parameter Server
            _config_mqtt : Stores the url, port, sub_topic, pub_topic and
            Qos from the parameter file.
            _orderid : Stores order id.
            ret : Subscribe to MQTT Topic (/eyrc/vb/Aradmama/orders)
            which is defined in 'config_iot_ros.yaml'.
            ac : Object to initialize Actionclient() methods.
        """

    def on_goal(self, goal_handle):
        """This function will be called when Action Server receives a Goal. \n
        Args:
            goal_handle : Contains goal information which includes protocol, mode, topic and message
        """

    def process_goal(self, goal_handle):
        """ This function is called is a separate thread to process Goal. \n
        Args:
            goal_handle : Contains goal information which includes protocol, mode, topic and message
        """

    def google_sheet(self, goal):
        """ Send GET request to populate entries on Google sheets with goal messages. \n
        Args:
            goal : Contains the data to be input to the sheets separated by delimiter.
        """

    def on_cancel(self, goal_handle):
        """This function will be called when Goal Cancel request is send to the Action Server.\n
        Args:
            goal_handle : Contains goal information which includes protocol, mode, topic and message
        """

3. node_belt_control
--------------------
class BeltControl:
    """Class definition for controlling Belt power.
    """

    def __init__(self):
        """Constructor for BeltControl\n
        Attributes:
            _conveyor_belt_power : Handle to the boltcontrol service (/eyrc/vb/conveyor/set_power)
                for invoking calls.
            _box_name : None type namespace to store box names.
            _box_pose : Representation of pose in free space, composed of position and orientation.
            _position_publisher : Registers to the topic /box_position to publish msg robot_goal
            _rotation_matrix : Contains a numpy array of the Rotation matix.
            _power : Name space for belt power.
            _object_list : List object for a list of Package model names in order.
            _temp : Temporary variable
            _box_delivered : List object for a list of Packages delivered.
            _pkg_no : Object created to store the number of packages encountered.
            _colour_map : Dictionary object created to hold package model names as keys and colours as values
        """

    def camera1_image(self, message):
        """ Callback function to process messages coming from  Camera 1
            Initializes the dictionary _colour_map

        Args:
            message : message on the topic

        Usage:
            colour : Stores the incoming list from the message
        """

    def camera_output(self, message):
        """ Callback Function to process messages from Logical Camera 2
        Args:
            message : message on the topic
        Usage:
            t : Length of the list of models in the message
            position : numpy array of the box position
            result :  numpy array to store the dot product of rotation matrix
                 with positon of box

        """

4. node_camera1 
--------------------
class Camera1:
    """Class definition to for Camera 1.
    """
    # constructor

    def __init__(self):
        """ Constructor for the Camera1
        Attributes:
          bridge : An instancve for using conversion between ROS and openCV images.
          image_sub : Suscribing from RGB camera and getting raw image.
          _dt : Getting date and time.
          _colour : array to store colour of packages when required.
          run : for publishing once to google sheet.
          _colour_publisher : publishing color array.
          ac = An instance for the use of imported Actionclient.
          """

    def callback(self, data):
        """ Callback function for Camera1
        Processes Image and returns a sorted array of colours of packages

        Args :
            data: Contains the message array

        Usage:
            cv_image : Converted image.
            cv2.threshold : Converting (Processing) opencv bgr
            8bit image to greyscale and then applying threshold function
            rect, thresh1 : for outline on QR detection.
            decode : Decoding all barcode from processed image.
            cv.2 rectangle : bounding box surrounding the barcode on the image
            barcode.data.decode("utf-8") : converting the barcode data to utf-8
            encoding for our understanding.
            cv2.putText : print the barcode type and data to the terminal, no need makes process slow.
            Converting ROS image to bgr 8bit open CV image.
        """
5. node_ur_1_control
--------------------
class Ur5_1():
    ''' Class definition to control UR5_1 robot arm
    '''

    def __init__(self):
        ''' Constructor for the Ur5_2Control
        Attributes:
            _init_node : Initializes the ros node
            _robot_ns : Namespace of the robot
            _planning_group : Stores the string "manipulator"
            _commander : Initializes the moveit_commander
            _robot : Robot Commander object, provides information of the kinematic model and
                the current joint states of the robot.
            _scene : PlanningSceneInterface, a remote interface for getting, setting, and
                updating the robot’s internal understanding of the surrounding world.
            _group: MoveGroupCommander, interface for the robot planning_group, "manipulator" in this case.
            _display_trajectory_publisher : ROS Publisher to display trajectories in Rviz.
            _execute_trajectory_client : initializes a Simple Action Client to execute trajectories
            _planning_frame : Get the frame of reference in which planning is done.
            _eef_link : end effector link object
            _group_names: object for group names
            _vacuum_srv : An instance for the VaccumGripper service.
            _computed_plan : object as empty string to store the computed path
            _current_state : object that gets the current state of the robot
            _pkg_path : Uses RosPack (ROS package management tool) to get the path fot the saved config files.
            _file_path : Object to store file path
            _ordered_package : Object  created to store ordered package model number
            _object_list : List object containing package model names
            _colour_list : List object created to contain all sorted package colours
            _i : An object for a temporary variable
            _ordered_package_colour : Object created to store the colour of the package ordered
            ac = An instance for the use of imported Actionclient.
        '''

    def camera1_image(self, message):
        '''Callback function for subscriber of the topic /camera1/image

            Args:
                message: The message that is sent on the topic

            Usage:
                Appends the sorted list of colours into the _colour_list object
                _i : Temporary variable that ensures the colour array only gets appended once
                colour : variable to store the list in the message
        '''

    def add_box(self):
        ''' Adds box to the Rviz planning scene.\n
        Usage:
            box_pose : uses PoseStamp() to give a Pose with reference coordinate frame and timestamp.
            box_pose.header .frame_id: sets the frame of reference to "world"
            box_pose.pose.position : gives the (x,y,z) position the box to be added.
            _box_name : defines the name of the box added.
            _scene.add_box() : Takes in _box_name an box_pose along with the size as
                 input for adding box to the planning scene.
        '''

    def attach_box(self):
        '''Attches the object to the end effector link.\n
        Usage:
            touch_links : Gets the links that make up a group, with no group name
                specified all the links in the robot model are returned.

            _scene.attach_box : Attaches the box to the end effector link,
                this ensures that the object is considered in the collision matrix.

            _vacuum_result : Gives the flag (bool) to activate the vacuumGripper.
        '''

    def detach_box(self):
        '''Detaches the box from the end effector of the robot in thr planning scene.\n
        Usage:
            _scene.remove_attached_object() : Passes in the eef link name and the name of the box object to be detached.
            vacuumGripper srv is set (False) to detach.
        '''

  def remove_box(self):
        '''Deletes the object from the planning scene.\n
        Usage:
            _scene.remove_world_object() : Takes in the box_name argument to delete the box from the planing scene.
        '''


    def moveit_play_planned_path_from_file(self, arg_file_path, arg_file_name):
        '''Plays the saved trajectories from the config files.\n
        Args:
            arg_file_path : The path to the .yaml files containing the saved trajectories.
            arg_file_name : Name of the file of a specific trajectory.
        '''

    def moveit_hard_play_planned_path_from_file(
            self, arg_file_path, arg_file_name, arg_max_attempts):
        '''Makes multiple attempts to plan and play the saved trajectories.\n
        Args:
            arg_file_path : The path to the .yaml files containing the saved trajectories.
            arg_file_name : Name of the file of a specific trajectory.
            arg_max_attempts : Maximum number of attempts.
        '''

   def __del__(self):
        '''Destructor to the Ur5_2Control class.
        '''

    def ur5_control(self, orders, order_details):
        ''' Function to control ur5_1

        Args :
            orders : Nested list of the orders sorted based on PRIORITY [[COLOUR,PRIOIRITY]]
            order_details : Nested sorted list of the details of orders

        Usage :
            This function recognises which package is to be picked according to the order
            and executes the order. It also resets the _object_list and _colour_list by
            removing details of the dispatched package,sends goal to action server to be updated
            on the google sheet.

            ind :  stores the index of the ordered package in the list
            goal_list : Stores the goal to be sent to RosIOT bridge as list items
            orders_dispatched : stores the parameter list order_shipping_details
            dispatchtime : variable to store the datetime
            goal : a string that contains comma separated values of the elements in the goal_list
            ac.send_goal : sends the goal to the action server

        '''

6. node_ur5_2_control
--------------------
class Ur5_2Control:
    """  Class definition to control UR5_2 robot arm.

    """

    # Constructor
    def __init__(self):
        """ Constructor for the Ur5_2Control
        Attributes:
            _robot : Robot Commander object, provides information of the kinematic model
                and the current joint states of the robot.

            _scene : PlanningSceneInterface, a remote interface for getting, setting,
                and updating the robot’s internal understanding of the surrounding world.

            _group: MoveGroupCommander, interface for the robot planning_group, "manipulator" in this case.
            _display_trajectory_publisher : ROS Publisher to display trajectories in Rviz.
            _planning_frame : Get the frame of reference in which planning is done.
            _pkg_path : Uses RosPack (ROS package management tool) to get the path fot the saved config files.
            _srv : An instance for the VaccumGripper service.
            _box_name : An instance for box name as a str.
            _box_colour : An instance for box colour as a str.
            _curr_state : Stores the current state of the robot.
            _home_pose : A list of joint angles for the home pose of the ur5_2 robot.
            _red_bin : A list of joint angles defining the drop pose for the red bin.
            _yellow_bin : A list of joint angles defining the drop pose for the yellow bin.
            _green_bin : A list of joint angles defining the drop pose for the green bin.
            _bin_dict : A dictionary with <key,value> pair of the colors and their drop joint angles.
            _home_to_bin : A dictionary with <key, valuse> pair of the colors and their .yaml
                files containing the saved trajectories, from home to bin.
            _bin_to_home : A dictionary with <key, valuse> pair of the colors and their .yaml
                files containing the saved trajectories, from bin to home.
            _pose :  Representation of pose in free space, composed of position and orientation.
            ac = An instance for the use of imported Actionclient.
        """

    def moveit_play_planned_path_from_file(self, arg_file_path, arg_file_name):
        """ Plays the saved trajectories from the config files.\n
        Args:
            arg_file_path : The path to the .yaml files containing the saved trajectories.
            arg_file_name : Name of the file of a specific trajectory.
        """

    def moveit_hard_play_planned_path_from_file(
            self, arg_file_path, arg_file_name, arg_max_attempts):
        """ Makes multiple attempts to plan and play the saved trajectories.\n
        Args:
            arg_file_path : The path to the .yaml files containing the saved trajectories.
            arg_file_name : Name of the file of a specific trajectory.
            arg_max_attempts : Maximum number of attempts.
        """

    def box_position_callback(self, message):
        """Callback for the 'box_position' topic subscription.\n
        Args:
            message : the robot_goal message contains the Goal(int32), colour(str)
                and the pose (geometry_msgs/Pose) of the Packages.
        Usage:
            _colour : stores the box colour information.
            _pose : takes the position of the box form the message.
            orders_shipped : uses rospy.get_param to get the order_shipping_details using ROS parameter.
            add_box() : Adds the box in planning scene.
            attach_box() : Attaches the box to the eef in the planning scene.
            moveit_hard_play_planned_path_from_file() : Plays the saved trajectories
                from the saved files with a specified number of attempts.
            detach_box() : Detaches the box from the end effector link.
            remove_box() : Deletes the box from the planning scene.
            ac.send_goal() : Uses action client to send Date time information.
        """

    def add_box(self):
        """Adds box to the Rviz planning scene.\n
        Usage:
            box_pose : uses PoseStamp() to give a Pose with reference coordinate frame and timestamp.
            box_pose.header .frame_id: sets the frame of reference to "world"
            box_pose.pose.position : gives the (x,y,z) position the box to be added.
            _box_name : defines the name of the box added.
            _scene.add_box() : Takes in _box_name an box_pose along with the size as
                 input for adding box to the planning scene.
        """

    def attach_box(self):
        """Attches the object to the end effector link.\n
        Usage:
            touch_links : Gets the links that make up a group, with no group
                 name specified all the links in the robot model are returned.

            _scene.attach_box : Attaches the box to the end effector link, this
                 ensures that the object is considered in the collision matrix.

            _vacuum_result : Gives the flag (bool) to activate the vacuumGripper.
        """

    def detach_box(self):
        """Detaches the box from the end effector of the robot in thr planning scene.\n
        Usage:
            _scene.remove_attached_object() : Passes in the eef link name and the name
                of the box object to be detached.

            vacuumGripper srv is set (False) to detach.
        """

    def remove_box(self):
        """Deletes the object from the planning scene.\n
        Usage:
            _scene.remove_world_object() : Takes in the box_name argument to delete
                the box from the planing scene.
        """

    def __del__(self):
        """Destructor to the Ur5_2Control class.
        """



