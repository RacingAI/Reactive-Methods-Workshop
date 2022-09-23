 # Disparity Extender Workshop

 Welcome to the second half of the workshop. Now that the boring part is out of the way, the real fun can begin. In this section you will be taking everything you learned over the past while and implement your own disparity extender algorithm! You will then race your algorithms against each other in a virtual environment and the winner will get to do something special at the end. But enough preamble, let’s get started!

 ## Setup
 ### Required

 * [Python](https://realpython.com/installing-python/)
 
 ### Recommended

 * [Visual Studio Code](https://www.toolsqa.com/blogs/install-visual-studio-code/)
 * [git](https://www.atlassian.com/git/tutorials/install-git)

 ### Getting Started

 To get started, first clone this repo by typing the following command into the terminal and entering the directory

 ```bash
 $ git clone https://github.com/RacingAI/Reactive-Methods-Workshop.git
 $ cd Reactive-Methods-Workshop
 ```

 
 Next, we will clone the RacingAI Quickstart Gym
 ```bash
 $ git clone https://github.com/RacingAI/racingai-gym-quickstart
 $ cd racingai-gym-quickstart
 ```

 Follow the set up steps in the gym's README. Everything is set up correctly if a window pops up with the AI Grand Prix track.

 Once this is done, switch to the workshop branch by typing the following into the terminal

 ```bash
 $ git checkout reactive-methods-workshop
 ```

 ### Importing the Driver
 
 To import a driver, go to the file racingai-gym-quickstart/src/simulator.py and find the line like

 ```python
 from drivers.starting_point import SimpleDriver
 ```

 You can import a driver class by following this format. We will import the DiparityExtender class by adding the following line. 

 ```python 
 from drivers.disparity_start import DisparityExtender
 ```

 Next ensure that the driver list has your imported class in it. It should look like this 
 ```python
 drivers = [DisparityExtender()]
 ```

 ## Disparity Extender
 The now that all the setup is done, you can start coding your own disparity extender! The rest of this document will just be a refresher of whatever was mentioned in the PowerPoint. You can refer to here if you ever get stuck. However, don’t just copy the steps below. At each step, you should consider “Why am I doing this step?” or “What happens if I change this or maybe don’t include that”. Approach this with a healthy level of inquisitiveness and if you can make any improvements to the algorithm, then go ahead! We want to see what sort of innovations you can come up with!
*The challenges are optional*
 ### Step 1: Limiting the FOV
 Limiting our field of view is important for two reasons. Firstly, we can get rid of information we don’t care about. For example, we don’t care about the LiDAR scans behind us since our main goal is to go forward. We also don’t care about the LiDAR scans at our 9 and 3 o’clock positions and a little bit around them. The reason being, if there was a disparity in that region, we wouldn’t be able to turn quickly enough to face that disparity. In reality, every autonomous car as a non-zero turning circle that we have to be aware of. Secondly, limiting our FOV improves processing speed. If we don’t have as many LiDAR scans to consider, our algorithm becomes faster. While this is not so important in our virtual simulator, in real races shaving a second or two off your algorithm could be the difference between winning and losing. 
 *Hint 1: This is done in the preprocess_lidar function*
 *Hint 2: Maybe using python array slicing might be useful here*

 ### Step 2: Finding Disparities by Thresholding Differences
 The next step in the algorithm is to find all the disparities by thresholding the differences.  Remember the differences being negative doesn’t help us. So how would we ensure that the differences are always positive?
 *Hint 1: For this you need to implement the get_differences and get_disparities*
 *Hint 2: Maybe the abs function might be useful*
 *Challenge: Can you find the index of the disparities using numpy and one line of code?*

 ### Step 3: “Extending” the Disparities

 But remember we can’t just pick any disparity we find. As shown in the PowerPoint we still run the risk of running one of the walls. So, we need a way of finding “safe” disparities that we can choose. We can do this by taking a disparity and looking at its immediate neighbors. There will be one “closer” and one “farther” scan. Starting at the closer scan we calculate how many lidar scans we need to overwrite so that it covers half the width of the car from the distance you’re at. Once you figure out this angle you can find out how many lidar scans that equates to. Then overwrite those many scans with the “closer” distance. You can overwrite “further” distances with “closer” ones. But never “closer” ones with “farther” ones. 

 *Hint 1:  You need to implement get_num_points_to_cover, cover_points and use both of those in extend_disparities to finish this step*

 ### Step 5: Run the Algorithm
 Now that you have coded all the helper functions, you need to implement the “run_algorithm” function. Here you place the helper functions you coded before and return two numbers, the speed and then the steering angle. It would also be wise here to figure out how many radians there are per LiDAR point as this number is heavily used throughout your code.

 *Hint 1: You need the functions preprocess_lidar, get_differences, get_disparities, extend.disparities, get_steering_angle and get_speed*