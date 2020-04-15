<p align="center"> 
<img src="fssim_doc/img/fssim_logo.png">
</p>

# FSSIM 
FSSIM is a vehicle simulator dedicated for Formula Student Driverless Competition. It was developed for autonomous software testing purposes and not for gaming. A version of this simulator was used to predict **lap time of *gotthard* at FSG 2018** trackdrive with **1% accuracy**. 

This simulator is developed and tested on **Ubuntu 18.04 and ROS Melodic** and both are assumed to be already installed. If not, please see [below](https://github.com/yongjiajun/fssim#ROS-Melodic-Installation) for installation guide.

The more extensive tutorial can be found under [Wiki](fssim_doc/index.md)

FSSIM is developed by [Juraj Kabzan](https://www.linkedin.com/in/juraj-kabzan-143698a1/) as part of our work at [AMZ-Driverless](http://driverless.amzracing.ch/).

# How to Run It in your Workspace
0. Install [ROS Melodic and FSSIM dependencies](https://github.com/yongjiajun/fssim#ROS-Melodic-Installation). If installed, ensure `python-catkin-tools`, `python-rosdep`, `python-shapely`, `checkinstall` and `python-wstool` are installed with `apt-get`.

1. Clone this repository to an existing **ROS Workspace's** `src` folder initialized with `catkin init`

   by running `git clone https://github.com/yongjiajun/fssim.git`.

2. Run `cd src/fssim` from the workspace.

3. `source /opt/ros/melodic/setup.bash` to ensure ROS commands can be used.

4. Run `./update_dependencies.sh`, you will need to approve multiple packages to be installed

5. Go back to the root of your workspace and run `catkin_make -j1'.

6. Source the workspace by running `source devel/setup.bash`. 

7. After successful building, run the simulator with `roslaunch fssim auto_fssim.launch`. RVIZ window will start. NOTE: You might need to untick and tick `FSSIM Track` and `RobotModel` in RVIZ in order to load the STL files. NOTE: This ` [Wrn] [ModelDatabase.cc:339] Getting models from[http://gazebosim.org/models/]. This may take a few seconds.` might take up to a minute when starting for the first time.

8. The terminal will inform you what is happening. The loading time takes around 20 seconds. When `Sending RES GO` will show up in the terminal, you can start controlling the vehicle with `/fssim/cmd` topic.

# Combine it with simple FSD skeleton Framework and drive autonomously

- See [fsd_skeleton repo](https://github.com/yongjiajun/fsd_skeleton) for details.

# Features
* This simulator is targeted for FSD competition, thus it contains some of the real-car safety features
  * **RES (Remote Emergency Stop)**: The vehicle will not be able to be controlled if a `/fssim/res_state/push_button = true` is not sent. On the other side, if  `/fssim/res_state/emergency = true` is send, the vehicle will stop immediately. If you start FSSIM with `roslaunch fssim auto_fssim.launch` or through `fssim_interface` this is done automatically.
  * **Leaving Track**: If the simulation is started with `auto_fssim.launch`, an automated RES person is launched. This means, if the vehicle exists the track with all four wheels, RES-emergency will be send and the simulation will exit itself
* FSSIM does not simulate the RAW sensors! It uses a **cone-sensor-model** instead. This means a cone observations around the vehicle are simulated with numerous noise-models.  The configuration file for this sensors can be found in [fssim/fssim_description/cars/gotthard/config/sensors.yaml](fssim_description/cars/gotthard/config/sensors.yaml). Thanks to this simplification it is real-time capable
* FSSIM does not use GAZEBO Physics Engine to simulate the vehicle. Instead, it uses a basic **vehicle model** which is discretized with Euler Forward discretization and overwrites the model pose. This feature allows the simulated model to match closely the real world car.
* **Automated Run** of multiple scenarios with different configurations and save logs as well as report.
* Lap-time counter

# Known problems
* The update rate of TF between wheels and main chassis is too low or too rough. This causes jumps of wheels at higher speeds in RViZ. This influences only the visualization and not the functionality. 

# Cite
* If using this project for scientific publications, please cite this [paper](https://arxiv.org/abs/1905.05150):

Juraj Kabzan, Miguel de la Iglesia Valls, Victor Reijgwart, Hubertus Franciscus Cornelis Hendrikx, Claas Ehmke, Manish Prajapat, Andreas Bühler, Nikhil Gosala, Mehak Gupta, Ramya Sivanesan, Ankit Dhall, Eugenio Chisari, Napat Karnchanachari, Sonja Brits, Manuel Dangel, Inkyu Sa, Renaud Dubé, Abel Gawel, Mark Pfeiffer, Alexander Liniger, John Lygeros, Roland Siegwart, "**AMZ Driverless: The Full Autonomous Racing System**", arXiv preprint arXiv:1905.05150

```latex
@misc{1905.05150,
Author = {Juraj Kabzan and Miguel de la Iglesia Valls and Victor Reijgwart and Hubertus Franciscus Cornelis Hendrikx and Claas Ehmke and Manish Prajapat and Andreas Bühler and Nikhil Gosala and Mehak Gupta and Ramya Sivanesan and Ankit Dhall and Eugenio Chisari and Napat Karnchanachari and Sonja Brits and Manuel Dangel and Inkyu Sa and Renaud Dubé and Abel Gawel and Mark Pfeiffer and Alexander Liniger and John Lygeros and Roland Siegwart},
Title = {AMZ Driverless: The Full Autonomous Racing System},
Year = {2019},
Eprint = {arXiv:1905.05150},
}
```

# Example
<p align="center"> 
<img src="fssim_doc/img/fssim_demo.gif" width="700" />
</p>
# ROS Melodic Installation

To install **ROS Melodic** and install dependencies for **FSSIM on Ubuntu 18.04 Bionic, please run the following commands in your terminal:

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros-latest.list'
sudo -E apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt-get update
sudo apt-get -y install ros-melodic-desktop-full python-catkin-tools
sudo apt-get -y install python-rosdep
sudo rosdep init
rosdep update
sudo apt-get install -y python-shapely
source /opt/ros/melodic/setup.bash
sudo apt install checkinstall
sudo apt install python-wstool
```

A note: this is a public copy of a private version. The public version might have some internal functionality removed.
FSSIM was developed by

<p align="center"> 
<img src="fssim_doc/img/driverless-amzracing.png">
</p>
