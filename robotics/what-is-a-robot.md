---
layout: default 
---

### What is a robot ?
---
Although many definitions of robot exists and one can get philosophical about it, we consider a robot as an entity that does following three things.
* Perception
* Decision making
* Action

The above characteristics are similar to agent & enviroment analogy from reinforcement learning, where the agent which recieves an observation from it's environment, recieves / computes reward and selects an action based on the reward.


#### Perception
Robots receives the observations from it's environment with the use of sensors, few examples would be images taken from it's camera, IR sensors, microphone, GPS, barometer etc. 

Consider a robot to be an autonomous car, with a camera mounted on it, the camera feeds in images from the front of the car.

#### Decision making
From the observations recieved from it's environment, the robot/agent computes the reward / punishment of it's earlier action (assume we are at time step _t_ in the series), evaluates the reward and expected reward and chooses an action based on a policy (this is distribution over an action). 


Extending the above example of autonomous car, the camera feeds indicate an obstacle ahead of the car, the reward function of the agent computes the expected reward which could end up being negative, based on it it selects and action to slow down the car.

#### Action
Based on the action selected in the decision making step the robot executes the action, from the above example of autonomous car the RL agent makesa decision to slow down, and the robot initiates braking sequence to get the car to a stop.


