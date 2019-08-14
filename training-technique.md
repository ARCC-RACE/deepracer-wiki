# Training Technique

## Training Process

## Training Methods

### Official Cloud

### Local Cloud

### Local Local

## Reward Functions
The reward function describes immediate feedback (as a score for reward or penalty) when the vehicle takes an action to move from a given position on the track to a new position. Its purpose is to encourage the vehicle to make moves along the track to reach its destination quickly. The model training process will attempt to find a policy which maximizes the average total reward the vehicle experiences.

In this function, you write the brain of the car itself, that learns from rewarding itself for good behavior. This is part of the basics of Reinforcement learning. You encourage the car to behave a certain way by encouraging it with reward. This is similar to training a dog for example, where you may get your dog to sit or lay down by providing a treat. In the same way, the car is encouraged to drive fast on the track by getting reward every time it does something correct. As the developer, you're asked to define what behaviors the car is rewarded for. 

For the DeepRacer, the reward function is formatted as a function with the input dictionary `params` that returns a float `reward`.


### Parameters

The reward function input parameters (params) are passed in as a dictionary object, specifying a given state (params["x"], params["y"], params["all_wheels_on_track"], params["distance_from_center"], etc.) the agent is in and a given action (params["speed"] and params["steering"]) the agent takes. You manipulate one or more of the input parameters to create a customized reward function most appropriate for your solution.


#### all_wheels_on_track
###### boolean

A boolean flag to indicate if the vehicle is on-track or off-track. The vehicle is off-track (False) if all of its wheels are outside of the track borders. It's on-track (True) if any of the wheels is inside the two track borders.

#### x
###### float

Location in meters of the vehicle center along the x axis of the simulated environment containing the track. The origin is at the lower-left corner of the simulated environment.

#### y
###### float

Location in meters of the vehicle center along the y axis of the simulated environment containing the track. The origin is at the lower-left corner of the simulated environment.

#### distance_from_center
###### float [0, ~track_width/2]

Distance from the center of the track, in unit meters. The observable maximum displacement occurs when any of the agent's wheels is outside a track border and, depending on the width of the track border, can be slightly smaller or larger than half of track_width.

#### is_left_of_center
###### boolean

A Boolean flag to indicate if the vehicle is on the left side to the track center (True) or on the right side (False).

heading
float (-180, 180]

Heading direction in degrees of the vehicle with respect to the x-axis of the coordinate system.

#### progress
###### float [0, 100]

Percentage of the track complete.

#### steps
###### integer

Number of steps completed. One step is one (state, action, next state, reward tuple).

#### speed
###### float [0.0, 8.0]

The observed speed of the vehicle, in meters per second (m/s).

#### steering_angle
###### float [-30, 30]

Steering angle, in degrees, of the front wheels from the center line of the vehicle. The negative sign (-) means steering to the right and the positive (+) sign means steering to the left.

#### track_width
###### float

Track width in meters.

#### waypoints
###### List of (float, float)

An ordered list of milestones along the track center. Each milestone is described by a coordinate of (x, y). A list of waypoints for each track is found in the resources section

#### closest_waypoints
###### (integer, integer)

The zero-based indices of the two neighboring waypoints closest to the vehicle's current position of (x, y). The distance is measured by the Euclidean distance from the center of the vehicle.

### Examples
I've compiled a list of examples that I've found all over the web for reward functions. Note that not all of these functions may work with a straight copy and past, as the parameter names may have changed. 


|Iteration|Model Name|Strategy| 
| :---: |:---:|:-----|
|1|["Center Line Square Root"](#center-line-square-root)|A Pure Pursuit algorithm inspired approach|
|2|["Waypoint System"](#waypoint-system)|Use waypoints and lane preference to encourage a racing line|
|3|["Pure Pursuit"](#pure-pursuit)|Add an exponential speed component|
|4|["Self Motivator"](#self-motivator)|Simply encourage getting around the track in as few steps as possible|
|5|Add Yours!||

#### Center Line Square Root
[Source](https://medium.com/proud2becloud/deepracer-our-journey-to-the-top-ten-257ff69922e)

```python
import math
def reward_function(params):
    '''
    Use square root for center line
    '''
    track_width = params['track_width']
    distance_from_center = params['distance_from_center']
    speed = params['speed']
    progress = params['progress']
    all_wheels_on_track = params['all_wheels_on_track']
    SPEED_TRESHOLD = 6
    reward = 1 - (distance_from_center / (track_width/2))**(4)
    if reward < 0:
        reward = 0
    if speed > SPEED_TRESHOLD:
        reward *= 0.8
    if not (all_wheels_on_track):
        reward = 0
    if progress == 100:    
        reward += 100
    return float(reward)
```

#### Waypoint System
[Source](https://medium.com/proud2becloud/deepracer-our-journey-to-the-top-ten-257ff69922e)

```python
import math
def reward_function(params):
    
    track_width = params['track_width']
    distance_from_center = params['distance_from_center']
    steering = abs(params['steering_angle'])
    direction_stearing=params['steering_angle']
    speed = params['speed']
    steps = params['steps']
    progress = params['progress']
    all_wheels_on_track = params['all_wheels_on_track']
    ABS_STEERING_THRESHOLD = 15
    SPEED_TRESHOLD = 5
    TOTAL_NUM_STEPS = 85
    
    # Read input variables
    waypoints = params['waypoints']
    closest_waypoints = params['closest_waypoints']
    heading = params['heading']
    
    reward = 1.0
        
    if progress == 100:
        reward += 100
    
    # Calculate the direction of the center line based on the closest waypoints
    next_point = waypoints[closest_waypoints[1]]
    prev_point = waypoints[closest_waypoints[0]]
    # Calculate the direction in radius, arctan2(dy, dx), the result is (-pi, pi) in radians
    track_direction = math.atan2(next_point[1] - prev_point[1], next_point[0] - prev_point[0]) 
    # Convert to degree
    track_direction = math.degrees(track_direction)
    # Calculate the difference between the track direction and the heading direction of the car
    direction_diff = abs(track_direction - heading)
    # Penalize the reward if the difference is too large
    DIRECTION_THRESHOLD = 10.0
    
    malus=1
    
    if direction_diff > DIRECTION_THRESHOLD:
        malus=1-(direction_diff/50)
        if malus<0 or malus>1:
            malus = 0
        reward *= malus
    
    return reward
```

#### Pure Pursuit

[Source](https://github.com/scottpletcher/deepracer/blob/master/iterations/v1-PurePursuit.md)

```python
  def reward_function(self, on_track, x, y, distance_from_center, car_orientation, progress, steps,
                        throttle, steering, track_width, waypoints, closest_waypoints):
        
        reward = 1e-3
        
        rabbit = [0,0]
        pointing = [0,0]
            
        # Reward when yaw (car_orientation) is pointed to the next waypoint IN FRONT.
        
        # Find nearest waypoint coordinates
        
        rabbit = [waypoints[closest_waypoints+1][0],waypoints[closest_waypoints+1][1]]
        
        radius = math.hypot(x - rabbit[0], y - rabbit[1])
        
        pointing[0] = x + (radius * math.cos(car_orientation))
        pointing[1] = y + (radius * math.sin(car_orientation))
        
        vector_delta = math.hypot(pointing[0] - rabbit[0], pointing[1] - rabbit[1])
        
        # Max distance for pointing away will be the radius * 2
        # Min distance means we are pointing directly at the next waypoint
        # We can setup a reward that is a ratio to this max.
        
        if vector_delta == 0:
            reward += 1
        else:
            reward += ( 1 - ( vector_delta / (radius * 2)))

        return reward
```

#### Self Motivator
[source](https://github.com/scottpletcher/deepracer/blob/master/iterations/v4-SelfMotivator.md)
```python
def reward_function(params):

    if params["all_wheels_on_track"] and params["steps"] > 0:
        reward = ((params["progress"] / params["steps"]) * 100) + (params["speed"]**2)
    else:
        reward = 0.01
        
    return float(reward)
```
## Action Spaces

## Hyperparameters

Hyperparameters are variables to control your reinforcement learning training. They can be tuned to optimize the training time and your model performance.

### Gradient descent batch size
The number of recent vehicle experiences sampled at random from an experience buffer and used for updating the underlying deep-learning neural network weights. Random sampling helps reduce correlations inherent in the input data. Use a larger batch size to promote more stable and smooth updates to the neural network weights, but be aware of the possibility that the training may be longer or slower.

The batch is a subset of an experience buffer that is composed of images captured by the camera mounted on the AWS DeepRacer vehicle and actions taken by the vehicle.


### Number of epochs
The number of passes through the training data to update the neural network weights during gradient descent. The training data corresponds to random samples from the experience buffer. Use a larger number of epochs to promote more stable updates, but expect a slower training. When the batch size is small, you can use a smaller number of epochs.


### Learning rate
During each update, a portion of the new weight can be from the gradient-descent (or ascent) contribution and the rest from the existing weight value. The learning rate controls how much a gradient-descent (or ascent) update contributes to the network weights. Use a higher learning rate to include more gradient-descent contributions for faster training, but be aware of the possibility that the expected reward may not converge if the learning rate is too large.


### Entropy
The degree of uncertainty used to determine when to add randomness to the policy distribution. The added uncertainty helps the AWS DeepRacer vehicle explore the action space more broadly. A larger entropy value encourages the vehicle to explore the action space more thoroughly.


### Discount factor
The discount factor determines how much of future rewards are discounted in calculating the reward at a given state as the averaged reward over all the future states. The discount factor of 0 means the current state is independent of future steps, whereas the discount factor 1 means that contributions from all of the future steps are included. With the discount factor of 0.9, the expected reward at a given step includes rewards from an order of 10 future steps. With the discount factor of 0.999, the expected reward includes rewards from an order of 1000 future steps.


### Loss type
The type of the objective function to update the network weights. A good training algorithm should make incremental changes to the vehicle’s strategy so that it gradually transitions from taking random actions to taking strategic actions to increase reward. But if it makes too big a change then the training becomes unstable and the agent ends up not learning. The Huber and Mean squared error loss types behave similarly for small updates. But as the updates become larger, the Huber loss takes smaller increments compared to the Mean squared error loss. When you have convergence problems, use the Huber loss type. When convergence is good and you want to train faster, use the Mean squared error loss type.


### Number of experience episodes between each policy-updating iteration
The size of the experience buffer used to draw training data from for learning policy network weights. An episode is a period in which the vehicle starts from a given starting point and ends up completing the track or going off the track. Different episodes can have different lengths. For simple reinforcement-learning problems, a small experience buffer may be sufficient and learning will be fast. For more complex problems which have more local maxima, a larger experience buffer is necessary to provide more uncorrelated data points. In this case, training will be slower but more stable. 


## Execution

## Notebook Analysis
