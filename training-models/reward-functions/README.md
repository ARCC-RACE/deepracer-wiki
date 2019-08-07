# Reward Function
###### By Caelin Sutch


The reward function describes immediate feedback (as a score for reward or penalty) when the vehicle takes an action to move from a given position on the track to a new position. Its purpose is to encourage the vehicle to make moves along the track to reach its destination quickly. The model training process will attempt to find a policy which maximizes the average total reward the vehicle experiences.

In this function, you write the brain of the car itself, that learns from rewarding itself for good behavior. This is part of the basics of Reinforcement learning. You encourage the car to behave a certain way by encouraging it with reward. This is similar to training a dog for example, where you may get your dog to sit or lay down by providing a treat. In the same way, the car is encouraged to drive fast on the track by getting reward every time it does something correct. As the developer, you're asked to define what behaviors the car is rewarded for. 

For the DeepRacer, the reward function is formatted as a function with the input dictionary `params` that returns a float `reward`.


### Parameters

The reward function input parameters (params) are passed in as a dictionary object, specifying a given state (params["x"], params["y"], params["all_wheels_on_track"], params["distance_from_center"], etc.) the agent is in and a given action (params["speed"] and params["steering"]) the agent takes. You manipulate one or more of the input parameters to create a customized reward function most appropriate for your solution.


#### all_wheels_on_track
###### boolean

A boolean flag to indicate if the vehicle is on-track or off-track. The vehicle is off-track (False) if all of its wheels are outside of the track borders. It's on-track (True) if any of the wheels is inside the two track borders.

#### x
######float

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
######integer

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
######(integer, integer)

The zero-based indices of the two neighboring waypoints closest to the vehicle's current position of (x, y). The distance is measured by the Euclidean distance from the center of the vehicle.

### Basic Examples

Three basic examples are provided by Amazon.

The line follower simply attempts to get the car to follow the center line:

```$python
def reward_function(params):
    '''
    Example of rewarding the agent to follow center line
    '''
    
    # Read input parameters
    track_width = params['track_width']
    distance_from_center = params['distance_from_center']
    
    # Calculate 3 markers that are at varying distances away from the center line
    marker_1 = 0.1 * track_width
    marker_2 = 0.25 * track_width
    marker_3 = 0.5 * track_width
    
    # Give higher reward if the car is closer to center line and vice versa
    if distance_from_center <= marker_1:
        reward = 1.0
    elif distance_from_center <= marker_2:
        reward = 0.5
    elif distance_from_center <= marker_3:
        reward = 0.1
    else:
        reward = 1e-3  # likely crashed/ close to off track
    
    return float(reward)
```

This example simply gives high rewards if the agent stays inside the borders and lets the agent figure out what is the best path to finish a lap. It is easy to program and understand, but will be likely to take longer time to converge.

```$xslt
def reward_function(params):
    '''
    Example of rewarding the agent to stay inside the two borders of the track
    '''
    
    # Read input parameters
    all_wheels_on_track = params['all_wheels_on_track']
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']
    
    # Give a very low reward by default
    reward = 1e-3

    # Give a high reward if no wheels go off the track and
    # the agent is somewhere in between the track borders
    if all_wheels_on_track and (0.5*track_width - distance_from_center) >= 0.05:
        reward = 1.0

    # Always return a float value
    return float(reward)
```

This example incentivizes the agent to follow the center line but penalizes with lower reward if it steers too much, which will help prevent zig-zag behavior. The agent will learn to drive smoothly in the simulator and likely display the same behavior when deployed in the physical vehicle.
```$xslt
def reward_function(params):
    '''
    Example of penalize steering, which helps mitigate zig-zag behaviors
    '''
    
    # Read input parameters
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']
    steering = abs(params['steering_angle']) # Only need the absolute steering angle

    # Calculate 3 markers that are at varying distances away from the center line
    marker_1 = 0.1 * track_width
    marker_2 = 0.25 * track_width
    marker_3 = 0.5 * track_width

    # Give higher reward if the agent is closer to center line and vice versa
    if distance_from_center <= marker_1:
        reward = 1
    elif distance_from_center <= marker_2:
        reward = 0.5
    elif distance_from_center <= marker_3:
        reward = 0.1
    else:
        reward = 1e-3  # likely crashed/ close to off track

    # Steering penality threshold, change the number based on your action space setting
    ABS_STEERING_THRESHOLD = 15

    # Penalize reward if the agent is steering too much
    if steering > ABS_STEERING_THRESHOLD:
        reward *= 0.8

    return float(reward)
```
