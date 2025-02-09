# Launch files

## Command-line launch commands
Launch files are ideal to start multiple ROS nodes at once. Launch files can include other launch files. The roslaunch command cleverly checks whether ROS is already running. You have already seen this because you used the launch command twice:  
`$ roslaunch mirte_workshop mirte_workshop.launch` started the basic nodes for driving control etc.  
`$ roslaunch mirte_teleop teleopkey.launch` started the node that allowed keyboard control for driving.

The command structure is roslaunch package_name file_name. It cleverly knows to find the package folder and to check in the 'launch' folder inside that package folder. Verify the above commands with the folder structure on the left side of the vscode screen.

## Create your own launch file
In the folder ~/mirte_ws/src/mirte_workshop/launch, create a new file and call it 'arm_and_gripper.launch'  
Copy the following code in it:

    <launch>
        <node pkg="mirte_workshop" name="arm_server" type="arm_server.py" output="screen"/>
    </launch>

Save the file and test it with `$ roslaunch mirte_workshop arm_and_gripper.launch`  
Now add the gripper_server.py node to the launch file and test it. Make sure that nodes are not started twice, note that other team members may start nodes from their computer.  

## Roslaunch trumps rosrun
Roslaunch is at the top of the food chain. It is the most complete and robust way to launch nodes. It is better than the quick command that your team members use:  
`rosrun mirte_workshop arm_server.py`, so it is advisable to make nice launch files for them.

By the way, the rosrun command is still better than `$ python3 arm_server.py`. For rosrun and roslaunch to work, you must tell Linux that your python files are executable:  
`$ chmod +x python_file_name.py`  

## Include other launch files
Code that works well, does not need to be started separately; you can have it launch together with all the other nodes. Just leave your new arm_and_gripper.launch file as it is, and add the following line to mirte_workshop.launch

    <include file="$(find mirte_workshop)/launch/arm_and_gripper.launch"/>

Or, vice versa, you could include the mirte_workshop.launch file in your own.

## Alias
For an even faster start, you can create an 'alias' in Linux. 
Open the file `~/.bashrc` in the editor. Add the following line at the bottom of the file:  

    alias go='roslaunch mirte_workshop mirte_workshop.launch'

Save the file. All new terminals will now execute the roslaunch command if you type `$ go`, but existing terminals won't, unless you `$ source ~/.bashrc` 

## Create an integrated system
Discuss with your team members which files need to be started automatically. Some, like keyboard control, are better started separately in their own terminal. Create your complete application!

It is advised to analyze the mirte_workshop.launch file thoroughly, and request explanation from ChatGPT about any unclear code in that file. It is nice to check the content of the launch file with `/rosnode list`. 