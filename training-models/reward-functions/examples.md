# Examples
###### By Caelin Sutch


On this page, I've compiled a list of examples that I've found all over the web for reward functions. Note that not all of these functions may work with a straight copy and past, as the parameter names may have changed. 


|Iteration|Model Name|Strategy| 
| :---: |:---:|:-----|
|1|["Center Line Square Root"](#center-line-square-root)|A Pure Pursuit algorithm inspired approach|
|2|["Waypoint System"](#waypoint-system)|Use waypoints and lane preference to encourage a racing line|
|3|["Pure Pursuit"](#pure-pursuit)|Add an exponential speed component|
|4|["Self Motivator"](#self-motivator)|Simply encourage getting around the track in as few steps as possible|
|5|Add Yours!||

## Center Line Square Root
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

## Waypoint System
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

## Pure Pursuit

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

## Self Motivator
[source](https://github.com/scottpletcher/deepracer/blob/master/iterations/v4-SelfMotivator.md)
```python
def reward_function(params):

    if params["all_wheels_on_track"] and params["steps"] > 0:
        reward = ((params["progress"] / params["steps"]) * 100) + (params["speed"]**2)
    else:
        reward = 0.01
        
    return float(reward)
```
