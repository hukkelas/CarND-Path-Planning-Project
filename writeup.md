# Implementation details
We followed the implementation from the Q&A video to make smooth, jerk-free trajectories and follow the car in front. Then we started to develop the lane-changing algorithm.

## Follow car / generating trajectories.
First to generate trajectories, we generate a couple of waypoints. We create waypoints for 30m a time by using frenet coordinates (line 246-248). To ensure that our new waypoints are smooth between the previous path and new path, we find the tangent between the previous path and the current path (line 215-242). 

When we have the waypoints, we generate a smooth path by using spline. We set the new plan with the remaining path from the previous path (line 279-282), and then find the new points from the new path (line 285-315). We find the new path by computing the spline based on our computed waypoints (line 272), then we split the path up to several points to generate a smooth path following the computed spline. We split each path into 50 points.

## Lane changing logic
Instead of using finite state machines or A* hybrid, we find a path with a very greedy algorithm. We only change lane in cases there is a car in front of us is closer than 30m. 

When we want to change lane, we check if its possible to change lane to the left or right (line 17 - 88). We assume that it is safe to change lane if there is no car within 20m in front or back of our car (line 51, 59-60, 69-70). If it is safe to change lane both left and right, we take the lane that has the maximum distance to the next car (line 76-81). If there is no safe lane change, we keep our current lane (line 88). 

# Valid trajectories


## 4.32 miles without incident
We tried the code several times and it does not have a incident.


## Speed limit.
The car drives as fast as possible, as long as there is no car in front of it. Its maximum speed is currently 49.5

## Max accelleration and jerk is not exceeded. 
The max acceleration is not exceeded as we maximumly increase the speed by .224 each .02 seconds. This was beneficial to be below the acceleration limit.

By implementing the trajectory with spline, and using a tangent function to find the path between a previous path and the new path, we are able to achieve a smooth and jerk-free ride.

## Car does not have collision
We ensure that a car never changes lane into a car and that it does not drive into the car in front. We did not check behind the car, but from running the simulator a couple of times it did not seem necesarry.

## The car stays in its lane
The trajectory we compute with spline has an end path in the middle of the lane each time.

## Car is able to change lane
We change lane each time we are behind a car that drives slower than 49.5, as long as its possible. To change lane, we check that it is safe to drive first.

# Reflection
The trajectory generated from the spline is very smooth and jerk free. Improvements to our method would be in our lane changing / path planning algorithm. We use a very greedy algorithm, that only changes lanes if there is a car in front of it (by 30m) and when its changing lane its only looking at the adjacent lanes. For example, if its in the rightmost lane and there are two cars within 20m in the current lane and the lane to the left, it would just keep following the car no matter what speed they are going. The leftmost lane might be free, and the optimal path might be to slow down and change lanes twice. However, our algorithm is greedy and never evaluates that option. Finite state machines would be a better option for this. 