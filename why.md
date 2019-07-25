## Why Choose the DeepRacer

Our organization, ARCC, has studied alternatives for learning and teaching the AWS platform. 

### Pros

- Integrated Approach
- Simulation Based - No expensive car or track required
- Abstracts most of the software: Allows the developer to focus just on the RL model
- Competition - Unifies community into large scale competition
- Active development community
- Standardized car, race is just about software
- Uses ROS on backend

### Cons

- AWS platform can get expensive
- Not much flexibility in ML models, you're stuck with PPO

## AWS DeepRacer Alternatives
There are a few other alternatives to the AWS DeepRacer if you're looking to race autonomous cars

### Donkey Car
The [Donkey Car](https://www.donkeycar.com/) is a community project to opensource self driving cars. They also offer kits to build the car seperately. All in all, the car cost around $250, which prices it lower than the DeepRacer. Theres also no standardization in cars, every car has a different chasis and motors, meaning hardware matters just as much as software. Additionally, DonkeyCar isn't supported by an organization like Amazon, so it's a little more spread out and not as organized compared to DeepRacer.

#### Pros
- Actually cheaper
- Completely Open Sourced
- Pretty Large Community

#### Cons
- Community isn't as big as AWS DeepRacer
- Competitions are spread out
- No simulator to test and develop in, requires large track

### Custom Built Car
The group I work with, [ARCC](https://arcc.ai), built a self driving car from scratch using a Traxaas RC car, NVIDIA Jetson TX2, and Intel Realsense camera. This was our first project in autonomous cars, and we still develop on it with alternative algorithms and methods.

#### Pros
- Fast
- Much more powerful hardware
- Fully customizable -  Choose whatever hardware or software stack you want

#### Cons
- Hardware problems to debug :(
- Much more expensive
- No community support as it's a oneoff
- Too big for most tracks, we race it on a 400m running track
