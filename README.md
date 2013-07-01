###2013Spring-ROS-TeamThree-FinalProject#
####ROS Middleware Final Project By Team Three#
author Legend Lee</br>
author Ken Luo</br>
author Yiran Fei</br>

#####Introduction#
Kobuki\_keyop originally was a node provide keyboard tele-operation for controlling a robot. This node can be used in both simulator and real world kobuki. But since there's no kobuki model for simulation, turtlebot model was used by the developer of kobuki in simulation instead. 
The change we made for keyop was to change the way of controlling. In the original version, the up/down/left/right key was to increase/decrease the speed of moving forward/backward/left/right, our version is that when i/k/j/l key was hit, the model will move forward/backward/turn left/turn right a little bit.

#####Methods#
* In configuration of the package, we basically followed (the tutorial of kobuki)[http://www.ros.org/wiki/kobuki/Tutorials/Simulated%20Kobuki%20navigation%20in%20perfect%20world]
* In coding area, We use ecl package and publisher in ROS to accomplish this task. For each i/k/j/l, when the key was hit, it will publish a new speed with the varible, type of MotorPower in kobuki\_msgs, and publish it. After that, the thread will sleep for 0.03s, after that, an zero cmd will be published, which will stop the model.

#####Results#
After debugging and trying for several times, the response was now sensitive, which means when the key was hit, the model will give a right move.

#####Discussion#
* The first part struggle us was to find a suitable project in ROS. We tried every paper recommanded in (ROS papers)[www.ros.org/wiki/Papers] but didn't get any advances. Then we tried many robots recommanded in (ROS robots)[www.ros.org/wiki/Robots] and found kobuki. Actually there were lots of errors when the first time we boot up kobuki. After some discussion, we found that it may be the problem of permission error when installing python. Then we got into root permission and everything goes successfully.
* The hardest part in this was to process the input of keyboard. Though the original give some examples, all the actions are synchronous, which means each key input you process should wait for the publish of the msg. And it's a significant problem for controlling in our way, which the whole controling node were designed in synchronous way. At first we didn't have the sleep method, which makes the result of moving very difficult to observe. Then we use methods provided by ctime.h, but the effect was horrible. The response was very not sensitive since it just provide second-level of sleep method. Thus we found ecl package provided in ROS which can have million-second level of sleep method.
We then want to make a new thread to handle the process (one thread for receiving keyboard input and one for processing the command), but found that normal pthread privided in linux can't be used due to lack knowledge of changing compile option in rosmake. Then we want to use spin provided in ROS. That will be our fuerte work.
Besides adding a thread for processing, the collision detect was bad in the origin code in the simulation. It looks like that the collision detect can't be used in our trials. In our future work, we will try the collision detection and add collision avoidance in our code.
#####Conclusion#
By changing compling and trying packages and topics in ROS, we learn a lot in ROS. We now know much more projects running in ROS world. Also, after the series of trying and coding for a simple node, we know much about the Spin class and the ecl package. ROS have lots of packages for us to used but we didn't know that before. By doing this project, we got lots more in coding for ROS.
