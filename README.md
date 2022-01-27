# big_bot_description
Big bot is a six-wheeled robot used to generate a 3D map simulation  of a V-shaped cave structure


# 1. Prerequisites
* teleop_twist_keyboard package; source: http://wiki.ros.org/teleop_twist_keyboardhttp://wiki.ros.org/teleop_twist_keyboard
* ouster_example package; source: https://github.com/ouster-lidar/ouster_example
* octomap_server package; source: https://github.com/OctoMap/octomap_mapping


# 2. Installing the Package
* git clone the package into your catkin_ws/src (or whatever you named your ros workspace) using the following line in the terminal

  ```git clone https://github.com/AntarCreates/big_bot_description.git```
* Build the workspace using
  ```catkin_make```
* Source the bash file using ```source devel/setup.bash```

# 3. Verify Installation
* Run the following coomand to bring up Big bot in gazebo

  ``` roslaunch big_bot_description gazebo.launch```

##                                                                  And you are pretty much set!

# 4. Setting-up the Environment
* Press the "insert" option in gazebo
* If you have everything set up adequately, you should find two objects by the following names under the "models used" folder on the left plane inside gazebo
  * Cave Corner 01 Type B
  * cave_straight_01
* Drag and drop the models to generate your desired shape. I went for a V-shaped one like the image attached below:

![Screenshot from 2022-01-28 00-38-33](https://user-images.githubusercontent.com/81281780/151383566-bc73c040-ba0a-41c3-af31-48fd1b3f81d8.png)

# 5. 3D Map Generation
The procedure can be subdivided into five (5) separate steps
  1. Initialize visualization
  2. Editing the octomap_mapping.launch file
  3. Running the octomap node to start generating 3D map
  4. Running the teleop_twist_keyboard.py node to drive the bot as the map is being generated, and
  5. Saving the generated 3D map

## 5.1 Initialize visualization
* To viaualize the process, we'll use RViz, the default ROS visualizer. Open a new tab in the terminal and run the following line:

  ```roslaunch big_bot_description display.launch```
  
  This should being-up RViz, and you should find various options like robot model, Tf, etc. I recommend to uncheck the robot model box, as doing so keeps the map visualization          cleaner.
  
  *note: In case you don't have your system set up for removing the irritating need for sourcing it each time a new terminal is opened, you must run ```source devel/setup.bash``` every time you open a new tab in the terminal, which you will be doing a lot in the next few steps*
  
 ## 5.2 Editing the octomap_mapping.launch file
 
 * Find the octomap_server package in your system using ```rospack find octomap_server``` . It should give you a path to its file location.
 * copy the path and open the file location (GUI) using  ```nautilus <your copied path>```
 * Find and open the octomap_mapping.launch file in the launch folder and edit the following lines as below:
 
  ```<param name="frame_id" type="string" value="odom" />```
  
  ```<remap from="cloud_in" to="/depth/points" />```
 
 
 
 ## 5.3 Running the Octomap node to start generating 3D map
 *Now this is quite a critical phase so be a bit more cautious*
 
 * Start the octomapping process by running the following line in a separate terminal tab ( Don't forget to source the bash!)
 
    ```roslaunch octomap_server octomap_mapping.launch```
 * In RViz press "Add" then "Add by topic" and press on "MarkerArray" as depicted in the snap below and set the fixed frame to be "odom":
 
 ![Inkedrviz_LI](https://user-images.githubusercontent.com/81281780/151390372-c5f5ee0e-689a-439f-83ea-f69d3fac0eb3.jpg)

  
  * By this time you should see blue cubes forming up along the red lidar rays near the big bot in the RViz window. That means the octomap is running! Good job!

*note: The octomap terminal may give you a warning with something like "octree is empty.Nothing to publish". You can ignore that as long as your mapping process is running.*

## 5.4 Running the teleop_twist_keyboard.py node to Drive the bot as the Map is being generated

* Now all you have to do is drive the bot through your desired path using keyboard inputs
* Run ```rosrun teleop_twist_keyboard teleop_twist_keyboard.py``` in a separate terminal tab to drive the robot using keyboard inputs (e.g., "i" for forward, "," for backward, etc.)

If everything works as expected, you should see a similar process as depicted in the following clip:

https://user-images.githubusercontent.com/81281780/151400685-dbd324c9-f284-4c19-aa43-688df832c775.mp4


## 5.5 Saving the generated 3D map
* If you made it this far, you are basically done! Congratulations! Now just save the map using the following line in a separate terminal in a desired directory ( don't forget to source the bash):

  ```rosrun octomap_server octomap_saver -f <name of your map>.bt``` 
  
# 6. Loading the Map

* To load or open the saved map, close the RViz using ``` Ctrl+c```  and run the following command in the terminal tab and add MarkerArray with the fixed frame set to "Odom"

  ``` rosrun octomap_server octomap_server_node <name of your map>.bt``` 
  
 * This should bring up the map in RViz window that you can rotate as you wish

  ![Screenshot from 2022-01-28 00-40-53](https://user-images.githubusercontent.com/81281780/151402730-6bed9415-6d21-4002-b4a8-8ed784c249ad.png)

![Screenshot from 2022-01-28 00-40-03](https://user-images.githubusercontent.com/81281780/151402740-00a44ab8-594a-43c9-b873-f4ca73d46129.png)
![Screenshot from 2022-01-28 00-38-33](https://user-images.githubusercontent.com/81281780/151403892-ebb06c55-7582-4e07-ab1a-127711689488.png)
  


# ....AAAnd that's it! Pat yourself on the back! You just made a pretty dope 3D map!
