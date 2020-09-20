# 3D Motion Planning

## Overview

In this project we will be plan a path through an urban environment. This project was based in this [repo](https://github.com/udacity/FCND-Motion-Planning) provided by Udacity. First, please read the previous repository as it contains important information about the environment setup, a simulator walkthrough, the tasks and evaluation. It is a continuation from Project 1 - Backyard Flyer.

# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
These scripts contain a basic planning implementation that includes...


### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
First, we read the file ```colliders.csv``` and extracted the information from the first line: ```lat0 37.792480, lon0 -122.397450```. We set this coordinates as global home.

#### 2. Set your current local position
We just use the function ```global_to_local``` with ```global_home``` and ```global_position``` as arguments.

#### 3. Set grid start position from local position
We define the variables ```north_start```, ```easth_start``` and ```grid_start```.

#### 4. Set grid goal position from geodetic coords
We set ```goal_lon``` and ```goal_lat``` as the goal location, and based on that we use again ```global_to_local()``` and set the grid goal.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
In order to add the diagonal cost we introduce the code:
```
NORTH_WEST = (-1, -1, np.sqrt(2))
NORTH_EAST = (-1, 1, np.sqrt(2))
SOUTH_WEST = (1, -1, np.sqrt(2))
SOUTH_EAST = (1, 1, np.sqrt(2))
```
</br>
And to review the diagonal motion:

```if (x - 1 < 0 or y - 1 < 0) or grid[x - 1, y - 1] == 1:
    valid_actions.remove(Action.NORTH_WEST)
if (x - 1 < 0 or y + 1 > m) or grid[x - 1, y + 1] == 1:
    valid_actions.remove(Action.NORTH_EAST)
if (x + 1 > n or y - 1 < 0) or grid[x + 1, y - 1] == 1:
    valid_actions.remove(Action.SOUTH_WEST)
if (x + 1 > n or y + 1 > m) or grid[x + 1, y + 1] == 1:
    valid_actions.remove(Action.SOUTH_EAST)
```


#### 6. Cull waypoints 
A collinearity test was used in this method within the function ```prune_path()```. The determinant of the matrix containing these three points (p1,p2,p3) must be equal to zero in 3 dimensions. If in two dimensions, the z coordinate is set to one and the determinant is zero, that's sufficient for collinearity.

### Execute the flight
#### 1. Does it work?
It works!
You can see the video [here](https://drive.google.com/file/d/10rTnAqgG4Ue5qbIvtLLq5fJBzUvLsvBD/view?usp=sharing).


[//]: # (References)
[simulation]: ./img/simulation.gif

