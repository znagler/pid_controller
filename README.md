# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 


| Coefficient        | Value   | 
|:-------------:|:-------------:| 
| Kp   | -0.25     | 
| Ki       |  0      |
| Kd    | 2   |


| Speed    | Set - throttle   | 
|:-------------:|:-------------:| 
| < 25 mph   | 0.3     | 
| >= 25 mph       |  0      |



###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

One major issue was related to something I mentioned above regarding the dataset.  I was spending a lot of time tuning my HOG parameters and relying on my accuracy score to tell me if I was improving.  Since I was doing different train-test-split shuffling each run, it's likely that my changes were coming more from random shuffles than they were from the changes, given the fact that many images may have appeared in both the training and test sets.  It probably would have been smarter to disregard the accuracy score and only care about the video results.

I also had issues getting the heatmap to illuminate cars enough before I added 3 full window sizes and 60% overlap.  I didn't realize it was needed at first, because I was easily seeing some rectangles around the cars in the images with just one window size, but by the time I reached the video, a ton of inaccuracies and noise showed up.  In particular, when the road changed color, the bounding box completely disappeared around the car.  After making the change, the box gets a bit smaller at that point, but doesn't disappear– a clear improvement.

This pipeline and model could fail without high quality, diverse training data.  Vehicles come in many shapes and sizes, and new car models are constantly coming out.  It may not detect abnormal-looking vehicles, or vehicles strangely lit up, since gradients and colors are relied on heavily.  It could also probably be fooled by images of cars, since it's relying on a visible light camera.  Better vehicle detection would combine RADAR or LIDAR. 
