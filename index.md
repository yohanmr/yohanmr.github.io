### Social Navigation
## A brief introduction
My project for GSoC 2017 revolves around Social Navigation. Unlike other normal navigational algorithms my job was to incorporate certain social norms into the existing RRT PRM based navigation system. These norms make the robot navigate more human like so that humans feel more at ease with robots movings around them. The reason why this is important is that more and more robots are being introduced into human life, and designing these bots to meet human expectations allows a common ground to be formed between the robot and the humans. Some of the norms I introduced are, variable personal space, pass on right and obstacle buffer. Apart from this I have also added more dynamic and robust human simulations to the working environment.

This project needed a good knowledge of navigational algoriths as well a good mathematics background to calculate trajectory, and also to estimate the personal space via assymetric Gaussians.

### Project Overview
This project consists of the following modules which have made. 

# Fake Human Agent
This component when deployed is responsible for the generation of multiple humans. It also deploys a human controller UI, which can do a number of tasks such as change velocity, position as well as engage auto pilot mode. More details about this is given in my post 2 which is linked below.

# Social Navigation Gaussian
This is the component that recieves the list of humans and depending on the fact that they are moving or not two things happen. Firstly if they are moving a variable Gaussain is drawn over each human with varied deviations as per his velocity, then to encorporate the pass on right feature a special gaussian is drawn around each human such that the path to the right is chosen by removing the nodes to the left. This is followed by Density Function which adds all the gaussians to give a new required shape for the human space. This is followed by generation of polylines via a convex hull which then gives a set of points which basically are the polylines. 

# Social Naivgation Agent
This is the main component into which changes were made so that the person data is read from the fake human component and this data, depending on the position and motion status of the human different sequences of humans are passsed into the gaussian component where Gaussians are drawn as per the sequence passed. This returns back a vector of polylines which consist of points which were genrated as described. These sets of polylines are then passed to the trajectory component where a number of changes which I am about to describe will be made to enforce these rules. 

# Trajctory Robot 2D
This component recieves the set of polylines from the navigation component and then adds a plane to the innermodel. What this plane in turn does is remove the path joining two nodes in the graph, thus reoving the path from the robot. After this a wall is added in place of the plaes and this can be physically seen in the trajectort components window.

### How to use
Firstly robocomp needs to be installed **with FCL support**, the instructions to whuch can be found [here](https://github.com/robocomp/robocomp).
# Pre requisites:
1. Clone this [repository](https://github.com/ljmanso/AGM) in your home(~) directory, and run the shell scripts by typing **sh instDep.sh** and **sh compile.sh**. Say yes to support RoboComp.
2. In your home directory create a .rcremote file using gedit and type the line **localhost#robocomp** into it and save it.

# Steps:
1. Navigate to robocomp/components and in there clone my [repository](https://github.com/yohanmr/robocomp-shelly.git)
2. Go to the following components and run **cmake ..** in them. 
- Fake Human Agent
- trajectoryrobot2d
- socialnavigationagent
- socialnavigationgaussian
- localization
- testtrajectoryrobot2d
3. Then open a new terminal and type **rcreoteserver** this starts the server.
4. In a separate terminal navigate to the AGM folder and run **agglplannerserver**
5. Go to robocomp-shelly which was my repository and then into etcSim, there run rcmanagersimple **deployment.xml**.
6. A graph of nodes open up.
7. Turn on executive followed by base in the graph.
8. Turn on the fake human agent and enter number of humans, and manipulate them as per your needs using the UI.
9. Turn on the trajectory, followed by social navigation gaussian and social navigation agent.
10. Now the gaussians should appear in the trjectory window.
11. To test the trajectory run the test trajectory component and just click anywhere in the map and the robot will follow the trajectory.

### Current drawbacks and future work
Currently due to the excessive features added to the innermodel, copying the innermodel everytime a change is made to it by our funcations have created a lot of lag, especially if there are multiple humans. This is being worked on by Araceli and will be fixed soon. 
As for my work as soon as this is resolved I will be adding the human intention feature where depending on the human intention the robot approches the human to interact with him.
I also plan on adding the feature of human companion where the robot accompanies a human while maintaining his personal space. 
Both these features need the lag to be fixed which will be done soon.

### Code
My repository can be found [here](https://github.com/yohanmr/robocomp-shelly)

My commits can be found [here](https://github.com/yohanmr/robocomp-shelly/commits/yohanbranch)

My posts explaining my work and its features in detail can be found [here](https://github.com/robocomp/web/tree/master/gsoc/2017/yohan)

## Credits
I would like to thank my mentor Pedro and Araceli without whom I could not have completed my project, and Google for giving me this opprtunity. I would also like to thank Luis and Pablo who helped me out when I was stuck.
